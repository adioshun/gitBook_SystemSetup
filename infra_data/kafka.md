
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

docker에 설정 하기 
```
# https://blog.soga.ng/story/31/
    environment:
      JMX_PORT: 9093 # JMX(Java Management Extension)를 사용할 포트 지정
      KAFKA_JMX_OPTS: -Dcom.sun.management.jmxremote=true 
                      -Dcom.sun.management.jmxremote.authenticate=false 
                      -Dcom.sun.management.jmxremote.ssl=false 
                      -Djava.rmi.server.hostname={카프카 컨테이너의 아이피 주소} 
                      -Dcom.sun.management.jmxremote.rmi.port=9393 
                      -Djava.net.preferIPv4Stack=true
```

### 카프카 서버 설정시 JMX 지정 하여 실행 하기 
```
$ env JMX_PORT=9999 bin/kafka-server-start.sh config/server.properties
$ env JMX_PORT=9999 bin/kafka-server-start.sh -daemon config/server.properties
$ jconsole
```
![image](https://user-images.githubusercontent.com/17797922/179473800-018ad172-b626-40f2-b46f-486f99ca7833.png)

---

# JMXTerm
![image](https://user-images.githubusercontent.com/17797922/179484025-f278f151-7de5-45e2-bbfd-2c2a73b3e59b.png)

## 설치 

```
> curl -OL https://github.com/jiaqi/jmxterm/releases/download/v1.0.2/jmxterm-1.0.2-uber.jar
```

## jmxterm 실행
```
> java -jar jmxterm-1.0.2-uber.jar
Welcome to JMX terminal. Type "help" for available commands.

## broker jmx port에 접속
$>open localhost:9999

## 조회 가능한 mbeans domain(metric 그룹)을 확인한다. 
$>domains

## 위 domain 중에 조회할 domain을 선택한다. 
$>domain kafka.server

## "kafka.server" domain에서 제공하는 beans 목록을 확인한다. 
$>beans

## bean 중에서 "MessageInPerSec" metric을 선택한다. 
$>bean kafka.server:name=MessagesInPerSec,type=BrokerTopicMetrics

## "MessageInPerSec"에서 조회 가능한 값의 유형을 선택한다.
$>info

## info에서 출력된 목록 중 원하는 값의 유형을 선택한다. 
$>get Count

$>get MeanRate

## JMXTERM 종료
$>bye
#bye
```

