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
```

> logstash도 일정의 consummer이다 

## 설정 (실습 용도)  

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
