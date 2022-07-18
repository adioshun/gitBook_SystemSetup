
# 사전 설치

```
sudo apt install openjdk-8-jdk
sudo vi /etc/profile
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
source /etc/profile
echo $JAVA_HOME
```

## 코드 설치 

```
$ cd ~
$ wget https://dlcdn.apache.org/kafka/3.2.0/kafka_2.13-3.2.0.tgz
$ tar –xvzf kafka_2.13-3.2.0.tgz
```

## 설정 파일
```
$ vi config/server.properties
# advertised.listeners=PLAINTEXT://broker-01:9092 #hostname 지정
```

--- 

## JMX enable
bash콘솔에서 
```
$ export KAFKA_JMX_OPTS='-Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.authenticate=false 
  -Dcom.sun.management.jmxremote.ssl=false 
  -Dcom.sun.management.jmxremote.port=9999 
  -Dcom.sun.management.jmxremote.rmi.port=9999 
  -Djava.rmi.server.hostname=192.168.48.131'
  
$ echo $KAFKA_JMX_OPTS

```
> 외부 접속을 위해 IP주소 입력

### 카프카 서버 설정시 JMX 지정 하여 실행 하기 
```
$ env JMX_PORT=9999 bin/kafka-server-start.sh config/server.properties
$ env JMX_PORT=9999 bin/kafka-server-start.sh -daemon config/server.properties
$ jconsole
```
![image](https://user-images.githubusercontent.com/17797922/179473800-018ad172-b626-40f2-b46f-486f99ca7833.png)


