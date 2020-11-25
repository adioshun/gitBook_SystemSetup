
출처: https://private-space.tistory.com/42 [티끌모아 산을 쌓아보자]
https://hhgg.tistory.com/5

https://dksshddl.tistory.com/entry/Hadoop-Hadoop-%EC%84%A4%EC%B9%98-wordcount-%EC%98%88%EC%A0%9C
https://phoenixnap.com/kb/install-hadoop-ubuntu
https://private-space.tistory.com/42
https://it-sunny-333.tistory.com/79
https://datacodingschool.tistory.com/30

---

$sudo apt install software-properties-common wget vim openssh-server pdsh
``


ssh 설정 
```
$sudo vi /etc/ssh/sshd_config 
"""
PermitRootLogin yes로 변경
"""

$sudo service sshd restart 
$ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa 
$cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys 
$chmod 0600 ~/.ssh/authorized_keys 
$sudo vi /etc/pdsh/rcmd_default
"""
ssh
""" 
$ssh localhost



```


2. open jdk 설치 (컨테이너) 
add-apt-repository ppa:openjdk-r/ppa 
apt-get update  
apt-get install openjdk-8-jdk 
java -version 


3. 하둡 설치 (컨테이너)
wget http://mirror.navercorp.com/apache/hadoop/common/hadoop-3.2.1/hadoop-3.2.1.tar.gz
tar zxvf hadoop-3.2.1.tar.gz


4. bashrc수정 
```
$ vi ~/.bashrc 
"""
#Hadoop settings
export HADOOP_HOME=/home/adioshun/hadoop-3.2.1   #수정 
export HADOOP_CONFIG_HOME=$HADOOP_HOME/etc/hadoop 
export HADOOP_MAPRED_HOME=$HADOOP_HOME 
export HADOOP_COMMON_HOME=$HADOOP_HOME 
export HADOOP_HDFS_HOME=$HADOOP_HOME 
export YARN_HOME=$HADOOP_HOME

export PATH=$PATH:$HADOOP_HOME/bin 
export PATH=$PATH:$HADOOP_HOME/sbin 

export HDFS_NAMENODE_USER="adioshun"  #수정
export HDFS_DATANODE_USER="adioshun" 
export HDFS_SECONDARYNAMENODE_USER="adioshun" 
export YARN_RESOURCEMANAGER_USER="adioshun" 
export YARN_NODEMANAGER_USER="adioshun"

출처: https://private-space.tistory.com/42 [티끌모아 산을 쌓아보자]


"""
$ source ~/.bashrc 
```

아래의 명령어를 입력하여 output 디렉토리에 출력이 기록되도록 한다.
```
cd ~/hadoop-3.2.1
mkdir input
cp etc/hadoop/*.xml input
bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.1.jar grep input output 'dfs[a-z.\+'
```

6. 관련 파일 수정 
$vi ~/hadoop-3.2.1/etc/hadoop/hadoop-env.sh
"""
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64  #L54
"""

$vi ~/hadoop-3.2.1/etc/hadoop/core-site.xml

"""
<configuration> <property> <name>fs.defaultFS</name> <value>hdfs://localhost:9000</value> </property> </configuration>
"""


$vi ~/hadoop-3.2.1/etc/hadoop/hdfs-site.xml
"""
<configuration> <property> <name>dfs.replication</name> <value>1</value> </property> </configuration>
"""

7. 네임노드 포맷 & 실행

```
$ hdfs namenode -format
$ start-dfs.sh
```

