![image](https://user-images.githubusercontent.com/17797922/179491014-89dce7cb-24e5-4858-b962-83dda2c1d8b9.png)


- kibana : 일라스틱 서치 전용 시각화툴, 다른것은 안됨, 일라스틱의 내용은 풍부하게 시각화 


![image](https://user-images.githubusercontent.com/17797922/179491779-127ef151-bfb9-47a8-a1aa-97e81313ca00.png)


# 0. 사전 작업 

### 오류1) File Descriptor 오류 해결
[1]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65535]
file descriptor 갯수를 증가시켜야 한다.

> 에러 : [1]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65535]
https://www.elastic.co/guide/en/elasticsearch/reference/current/setting-system-settings.html#limits.conf

```
> sudo vi /etc/security/limits.conf
# 아래 내용 추가 
* hard nofile 70000
* soft nofile 70000
root hard nofile 70000
root soft nofile 70000

# 적용을 위해 콘솔을 닫고 다시 연결한다. (console 재접속)
# 적용되었는지 확인
> ulimit -a
core file size          (blocks, -c) 0
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 59450
max locked memory       (kbytes, -l) 64
max memory size         (kbytes, -m) unlimited
open files                      (-n) 70000  #--> 정상적으로 적용됨을 확인함
```

### 오류2) virtual memory error 해결
시스템의 nmap count를 증가기켜야 한다.

> 에러 : [2]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html

```
# 0) 현재 설정 값 확인
> cat /proc/sys/vm/max_map_count
65530

# 아래 3가지 방법 중 1가지를 선택하여 적용 가능
# 1-1) 현재 서버상태에서만 적용하는 방식
> sudo sysctl -w vm.max_map_count=262144

# 1-2) 영구적으로 적용 (서버 재부팅시 자동 적용)
> sudo vi /etc/sysctl.conf
# 아래 내용 추가
vm.max_map_count = 262144

# 1-3) 또는 아래 명령어 실행 
> echo vm.max_map_count=262144 | sudo tee -a /etc/sysctl.conf

# 3) 시스템에 적용하여 변경된 값을 확인
> sudo sysctl -p
vm.max_map_count = 262144
```


---

# 1. Elasticsearch 설치 

```
> curl -LO https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-oss-7.10.2-linux-x86_64.tar.gz
> tar -xzf elasticsearch-oss-7.10.2-linux-x86_64.tar.gz
> rm -rf elasticsearch-oss-7.10.2-linux-x86_64.tar.gz
```

설정
```
vi config/elasticsearch.yml
node.name : node-1 #/etc/hosts 에도 
## Master Node의 후보 서버 목록을 적어준다. 후보가 없더라도 작성 필요 (여기서는 1대 이므로 본인의 서버 정보만)
cluster.initial_master_nodes: ["kakfa-monitoring"]  # IP로 적으면 에러 발생 

```

실행 
```
$ ./bin/elasticsearch 
$ ./bin/elasticsearch -d -p elastic_pid -> (종료) pkill -F elastic_pid

```

확인 
```
curl -X GET "localhost:9200/?pretty"
```

---

# 2. Kibana 설치 

설치 
```
$ curl -OL https://artifacts.elastic.co/downloads/kibana/kibana-oss-7.10.2-linux-x86_64.tar.gz
```

설정 
```
## 외부에서 접속 할 수 있도록 설정한다. 
> vi config/kibana.yml

## 아래 내용 추가 (0.0.0.0은 모든 ip에서 접근이 가능한 설정. 운영환경에서는 특정 IP로 제한 필요)
server.host: "0.0.0.0"
```

실행
```
$./bin/kibana
#확인 http://localhost:5601
```

---

# logstash 설치 

## 사전 준비 
1. JMX 설치 및 port 설정

## 다운로드

```
$ cd ~
$ curl -LO https://artifacts.elastic.co/downloads/logstash/logstash-oss-7.15.0-linux-x86_64.tar.gz
$ tar xvf logstash-oss-7.15.0-linux-x86_64.tar.gz  
$ rm -rf logstash-oss-7.15.0-linux-x86_64.tar.gz
$ cd logstash-7.15.0

## install jmx plugin(플러그인 설치)
> bin/logstash-plugin install logstash-input-jmx

```


### 실습 (JMX)
```
> mkdir ~/jmx_conf
> vi ~/jmx_conf/broker01.conf

{
  "host" : "broker-01", 
  "port" : 9999,
  "alias" : "broker01",
  "queries" : [
  {
    "object_name" : "kafka.server:type=BrokerTopicMetrics,name=MessagesInPerSec",
    "attributes" : [ "OneMinuteRate" ],
    "object_alias" : "${type}.${name}"
  }
 ]
}

```

```
#broker01.conf에 정의된 metric 들을 수집하여,
#elasticsearch로 전송하는 logstash 설정

> mkdir ~/logstash_conf
> vi ~/logstash_conf/logstash_jmx.conf

> cd ~/logstash-7.10.2/
> bin/logstash -f ~/logstash_conf/logstash_jmx.conf


input {
 jmx {
  path => "/home/freepsw.10/jmx_conf"
  polling_frequency => 1
 }
}

output{
 stdout {
  codec => rubydebug
 }
 elasticsearch {
   hosts => "localhost:9200"
   index => "kafka_mon"
 }
}
```
> https://github.com/freepsw/kafka-metrics-monitoring/tree/master/code/ch.04/01.setup_elasticstack


실행 확인 
```
curl -X GET "localhost:9200/_cat/indices/"
```

### 실습(file)

1. logstash설정
```
$ vi config/logstash.yml
## kafka producer로 동작하기 위해서, 일정 주기로 일정 메세지를 전달하도록 설정 필요.
## logstash를 기본으로 사용하면, 전송 성능을 위해서 정의된 batch size 만큼을 보내도록 설정됨.
## 따라서 본 실습을 위해서는 1개의 thread가 한번에 1개의 메세지만 보내도록 설정한다.

# logstash에서 동시에 실행 가능한 thread
pipeline.workers: 2
# logstash에서 한번에 전송할 batch size
pipeline.batch.size: 125
```

1. producer 설정 파일 저장 폴더 만들기 
```
$ mkdir logstash_conf
$ vi logstash_conf/producer.conf

input {
    file {
        path=>"/tmp/tracks*.csv"
        start_position => "beginning"
    } 
}

filter {
    sleep {
        time => "1"   # Sleep 1 second
        every => 1   # on every 1th event
    }
}

output {
    stdout{
        codec => rubydebug
    }

    kafka { 
        bootstrap_servers => "broker-01:9092"
        topic_id =>  "kafka-mon"
        codec => plain {
            format => "%{message}"
        }
        # compression_type => "snappy"
    }
}
```

실행 
```
## thread 1개, batch size 1개로 데이터를 kafka로 전송
$ ~/logstash-7.15.0/bin/logstash -w 1 -b 1 -f ~/logstash_conf/producer.conf
```

2. consumer 설정 파일 저장 폴더 만들기 

```
$ vi logstash_conf/producer.conf

input {
    kafka {
      bootstrap_servers => "broker-01:9092"
      group_id => "consumer_group_1"
      topics => ["kafka-mon"]
      consumer_threads => 1
    }
}

filter {
    sleep {
        time => "1"   # Sleep 1 second
        every => 1   # on every 1th event
    }
}

output {
    stdout{
        codec => rubydebug
    }
}
```

```
## thread 1개, batch size 1개로 데이터를 kafka로 전송
## logstash 실행시 -b (batch.size) 옵션을 1로 설정해야, 
## broker에 데이터가 1건이라로 도착하면, logstash에서 consumer thread로 데이터를 전송하고, 이를 화면에 출력한다. 
## -b 128로 하면, broker에서 128건을 가져올 때 까지 기다린 후 화면에 출력함. 
## path.data 옵션 : 각 logstash 프로세스에서 내부적으로 관리하기 위한 데이터를 저장하기 위한 공간 
## 이전 producer에서 이미 default 경로(./data)를 사용하고 있으므로, consumer용 logstash에서 사용하기 위한 경로를 지정한다. 
> mkdir ~/data
> ~/logstash-7.15.0/bin/logstash -w 1 -b 1 --path.data ~/data/consumer_data -f ~/logstash_conf/consumer.conf
```


3. JMX 설정 

```
> export LS_JAVA_OPTS='-Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.authenticate=false 
  -Dcom.sun.management.jmxremote.ssl=false 
  -Dcom.sun.management.jmxremote.port=9998 
  -Dcom.sun.management.jmxremote.rmi.port=9998 
  -Djava.rmi.server.hostname=34.64.185.183'

```



# TIP
LS_JAVA_OPT 설정을 logstash 시작시에 기본으로 적용하는 방식
```
> vi logstash-7.15.0/config/jvm.options

## 위 파일 내용에 아래 jmx 관련된 내용을 LS_JAVA_OPTS에 추가
-Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.port=9998 -Dcom.sun.management.jmxremote.rmi.port=9998 -Djava.rmi.server.hostname=34.64.239.87
```
