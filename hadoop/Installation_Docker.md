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

웹브라우져 : 172.19.0.101:7180
