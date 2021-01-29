# Docker Network (HostPC)

```
$ docker network create --gateway 172.19.0.1 --subnet 172.19.0.0/21 mynet
$ docker network ls
$ docker network inspect mynet
$ docker run -it --privileged --hostname server01.hadoop.com --network mynet --ip 172.19.0.101 --volume /workspace:/workspace --name 'u101' cloudera:20210127  /bin/bash
```

> https://blog.leocat.kr/notes/2016/12/24/docker-assign-ip-on-docker-container



# Docker Hosts setup (In each docker)



```
vi /etc/hosts
"""
127.0.0.1 localhost server01
172.19.0.101 server01.hadoop.com server01
172.19.0.102 server02.hadoop.com server02
172.19.0.103 server03.hadoop.com server03
"""

vi /etc/hostname
"""
server01.hadoop.com
"""
```

- root log in : vi /etc/ssh/sshd_config -> passwd setup


---

# [Ambari ](http://ambari.apache.org/install.html)

```
$ wget -O /etc/apt/sources.list.d/ambari.list http://public-repo-1.hortonworks.com/ambari/ubuntu18/2.x/updates/2.7.3.0/ambari.list
$ apt-key adv --recv-keys --keyserver keyserver.ubuntu.com B9733A7A07513CAD
$ apt-get update

$ apt-get install ambari-server
$ ambari-server setup
#Username : Ambari
#Password : bigdata
$ service ambari-server start
https://hostname:8080/ #admin/admin
```



---

# Claudera


# Manager Setup (Docker #1 = server01)
```
$ cd ~
$ apt-get install psmisc ssh locales apt-transport-https ca-certificates #없으면 설치시 에러 발생생
$ env | grep LANG # LANG=en_US.UTF-8 확인 -> dpkg-reconfigure locales -> 149 - 3
$ wget http://archive.cloudera.com/cm5/installer/latest/cloudera-manager-installer.bin   #ubuntu 16.04까지만 지원 
$ chmod +x cloudera-manager-installer.bin
$ ./cloudera-manager-installer.bin
$ service cloudera-scm-server status

```
삭제 
$ sudo /usr/share/cmf/uninstall-cloudera-manager.sh



== web으로 CM 접속 후 구성

1. 172.19.0.101:7180 
2. admin/admin
3. CDH 클러스터 설치에 대한 호스트를 지정
```
server01.hadoop.com
server02.hadoop.com
server03.hadoop.com
```





== 기타

#cloudera manager 서비스 중지하기

service cloudera-scm-server stop


> `Cloudera에서는 /proc/sys/vm/swappiness를 최대 10으로 설정할 것을 권장합니다.` 
> `$ vi /etc/sysctl.conf` ` sysctl vm.overcommit_memory=1` Run `sysctl -p`


---
== 사용 계정에 sudo 암호 물어보지 않도록 하기. 
```python
http://stackoverflow.com/questions/28171755/cloudera-installation-failed-to-detect-root-privileges-on-centos

sudo vi /etc/sudoers

#추가
"""
feisia ALL =(ALL) NOPASSWD: ALL
"""
```
