> 참고: [Docker Official Hub](https://hub.docker.com/), [Docker Material Links](http://documents.docker.co.kr/ ), [eBook](http://www.pyrasis.com/private/2014/11/30/publish-docker-for-the-really-impatient-book), 

> [Tips](http://newsight.tistory.com/228): 명령어, 팁들 


# Install


## 1. docker 설치 (CPU지원 도커) 

```
# 이전 docker 삭제 
sudo apt-get purge docker.io 

# 최신 docker 설치
sudo wget -qO- https://get.docker.com/ | sh
# sudo apt install docker.io #sudo apt-get update; sudo apt-get install docker-engine 

docker 버전 확인
# docker version
##  ~17.12 : nvidia-docker 1
##  17.12~ : nvidia-docker 2 

```

Dependency 에러시 특정 패키지 버젼 지정하여 설치 하기 
```
The following packages have unmet dependencies:
 nvidia-docker2 : Depends: docker-ce (= 17.12.0~ce-0~ubuntu) but 18.02.0~ce-0~ubuntu is to be installed or
                           docker-ee (= 17.12.0~ee-0~ubuntu) but it is not installable
E: Unable to correct problems, you have held broken packages.

## Solution
root@ubuntu16:/tmp# sudo apt-get install docker-ce={17.12.0~ce-0~ubuntu}
```






## 2. nvidia-docker 설치 (GPU지원 도커) 

####### 버젼 1

```python
# nvidia-docker 다운로드
wget -P /tmp https://github.com/NVIDIA/nvidia-docker/releases/download/v1.0.1/nvidia-docker_1.0.1-1_amd64.deb

# 패키지 설치
sudo dpkg -i /tmp/nvidia-docker*.deb && rm /tmp/nvidia-docker*.deb

# nvidia docker 테스트
nvidia-docker run --rm nvidia/cuda nvidia-smi 

```
> Ndivia-docker binary : [Download](https://github.com/NVIDIA/nvidia-docker/releases), [Manual](https://github.com/NVIDIA/nvidia-docker)

####### 버젼 2 

```python

# If you have nvidia-docker 1.0 installed: we need to remove it and all existing GPU containers
docker volume ls -q -f driver=nvidia-docker | xargs -r -I{} -n1 docker ps -q -a -f volume={} | xargs -r docker rm -f
sudo apt-get purge -y nvidia-docker

# Add the package repositories
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
  sudo apt-key add -

distribution=$(. /etc/os-release;echo $ID$VERSION_ID)

curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list

sudo apt-get update

# Install nvidia-docker2 and reload the Docker daemon configuration
sudo apt-get install -y nvidia-docker2
sudo pkill -SIGHUP dockerd

# Test nvidia-smi with the latest official CUDA image
docker run --runtime=nvidia --rm nvidia/cuda nvidia-smi

```




---

## 3. Dockerfile

```
# SSH enabled docker 

FROM       ubuntu:16.04
MAINTAINER Aleksandar Diklic "https://github.com/rastasheep"

RUN apt-get update

RUN apt-get install -y openssh-server
RUN mkdir /var/run/sshd

RUN echo 'root:root' |chpasswd

RUN sed -ri 's/^#?PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config

RUN mkdir /root/.ssh

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

EXPOSE 22

CMD    ["/usr/sbin/sshd", "-D"]
```


docker pull rastasheep/ubuntu-sshd:16.04

```
Run example
$ sudo docker run -d -P --name test_sshd rastasheep/ubuntu-sshd:16.04
$ sudo docker port test_sshd 22
  0.0.0.0:49154

$ ssh root@localhost -p 49154
# The password is `root`
root@test_sshd $
```
