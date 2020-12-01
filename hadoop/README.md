# [Hadoop: Setting up a Single Node Cluster.](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html)


- 출처: https://private-space.tistory.com/42 [티끌모아 산을 쌓아보자]
- https://hhgg.tistory.com/5
- https://dksshddl.tistory.com/entry/Hadoop-Hadoop-%EC%84%A4%EC%B9%98-wordcount-%EC%98%88%EC%A0%9C
- https://phoenixnap.com/kb/install-hadoop-ubuntu
- https://private-space.tistory.com/42
- https://it-sunny-333.tistory.com/79 
- https://datacodingschool.tistory.com/30

---

# 사용자 추가후 작업 추천
```
$sudo adduser hdoop
$su - hdoop
```

## 사전 작업 
```
$sudo apt install software-properties-common wget vim openssh-server pdsh
```

## open jdk 설치
```
$add-apt-repository ppa:openjdk-r/ppa 
$apt-get update  
$apt-get install openjdk-8-jdk 
$java -version 
```

## 하둡 설치 
```
wget http://mirror.navercorp.com/apache/hadoop/common/hadoop-3.2.1/hadoop-3.2.1.tar.gz
wget https://downloads.apache.org/hadoop/common/hadoop-3.2.1/hadoop-3.2.1.tar.gz
tar zxvf hadoop-3.2.1.tar.gz
```

## 설정 (단독 모드)
하기 파일들 수정 필요
- bashrc
- hadoop-env.sh
- core-site.xml
- hdfs-site.xml
- mapred-site-xml
- yarn-site.xml


### 1. bashrc수정 
```
$ vi ~/.bashrc 
"""
export HADOOP_HOME=/home/hdoop/hadoop-3.2.1
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
export HADOOP_OPTS"-Djava.library.path=$HADOOP_HOME/lib/nativ"
"""
$ source ~/.bashrc 
```

## 아래의 명령어를 입력하여 output 디렉토리에 출력이 기록되도록 한다.

```
cd ~/hadoop-3.2.1
mkdir input
cp etc/hadoop/*.xml input
bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.1.jar grep input output 'dfs[a-z.\+'
```

### 2. hadoop-env.sh
```
$vi $HADOOP_HOME/etc/hadoop/hadoop-env.sh
"""
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64  #L54
"""

$vi $HADOOP_HOME/etc/hadoop/core-site.xml
"""
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
"""
```

### 3. hdfs-site.xml

```
$vi $HADOOP_HOME/etc/hadoop/hdfs-site.xml
# The properties in the hdfs-site.xml file govern the location for storing node metadata, fsimage file, and edit log file. Configure the file by defining the NameNode and DataNode storage directories.

# Additionally, the default dfs.replication value of 3 needs to be changed to 1 to match the single node setup.

"""
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>
"""

```

### 4. mapred-site.xml

```
$vi $HADOOP_HOME/etc/hadoop/mapred-site.xml
#define MapReduce values
"""
<configuration> 
<property> 
  <name>mapreduce.framework.name</name> 
  <value>yarn</value> 
</property> 
</configuration>
"""
```

### 5. yarn-sit

```
$vi $HADOOP_HOME/etc/hadoop/yarn-site.xml
# The yarn-site.xml file is used to define settings relevant to YARN. It contains configurations for the Node Manager, Resource Manager, Containers, and Application Master.

```


## 네임노드 포맷 & 실행

```
$ hdfs namenode -format
$ start-dfs.sh
```

---



#비번 없이 접속 되도록 설정
$ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa 
$cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys 
$chmod 0600 ~/.ssh/authorized_keys 
$ssh localhost   # 비번없이 접속 되는지 확인
$sudo vi /etc/pdsh/rcmd_default
"""
ssh
""" 
```
