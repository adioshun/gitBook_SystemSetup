```
$ docker network create --gateway 172.19.0.1 --subnet 172.19.0.0/21 mynet
$ docker network ls
$ docker network inspect mynet
$ docker run -it --privileged --hostname server01.hadoop.com --network mynet --ip 172.19.0.101 --volume /workspace:/workspace --name 'u101' ubuntu16:20210125  /bin/bash
```

> https://blog.leocat.kr/notes/2016/12/24/docker-assign-ip-on-docker-container



----
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

server01
```
$ cd ~
$ wget http://archive.cloudera.com/cm5/installer/latest/cloudera-manager-installer.bin   #ubuntu 16.04까지만 지원 
$ chmod +x cloudera-manager-installer.bin
$ ./cloudera-manager-installer.bin
```
삭제 
$ sudo /usr/share/cmf/uninstall-cloudera-manager.sh



== repository 추가
```
# http://www.cloudera.com/documentation/enterprise/latest/topics/cm_ig_install_path_b.html
$ wget https://archive.cloudera.com/cm5/ubuntu/trusty/amd64/cm/cloudera.list
$ cp cloudera.list /etc/apt/sources.list.d/
$ apt-get update
```


== web으로 CM 접속 후 구성

` 172.19.0.101:7180`







== 기타

#cloudera manager 서비스 중지하기

service cloudera-scm-server stop



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
