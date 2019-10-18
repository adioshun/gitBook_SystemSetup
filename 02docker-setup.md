# Docker 

> 참고: [Docker Official Hub](https://hub.docker.com/), [Docker Material Links](http://documents.docker.co.kr/ ), [eBook](http://www.pyrasis.com/private/2014/11/30/publish-docker-for-the-really-impatient-book),[Tips](http://newsight.tistory.com/228): 명령어, 팁들

```python
docker run --gpus all -it --privileged --network=host --volume /workspace:/workspace --name 'Ubuntu' <image> /bin/bash

docker run --gpus all -it --privileged --network=host -v /tmp/.X11-unix:/tmp/.X11-unix --volume="$HOME/.Xauthority:/root/.Xauthority:rw" -e DISPLAY --volume /workspace:/workspace --name 'Ubuntu' <image> /bin/bash

#docker run --runtime=nvidia -it --privileged --network=host -u $(id -u):$(id -g) -v /tmp/.X11-unix:/tmp/.X11-unix --volume="$HOME/.Xauthority:/root/.Xauthority:rw" -e DISPLAY --volume /workspace:/workspace --name 'Ubuntu' <image> /bin/bash

#docker run --runtime=nvidia -it --privileged --network=host -v /tmp/.X11-unix:/tmp/.X11-unix --volume="$HOME/.Xauthority:/root/.Xauthority:rw" -e DISPLAY --volume /workspace:/workspace --name 'Ubuntu' <image> /bin/bash



```




## 1. Install


```python 
# GPU (Ubuntu 16.04/18.04, Debian Jessie/Stretch/Buster)
$ distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
$ curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
$ curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

$ sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
$ sudo systemctl restart docker

> https://github.com/NVIDIA/nvidia-docker

# CPU
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh
sudo usermod -a -G docker $USER

> https://docs.docker.com/install/linux/docker-ce/ubuntu/


```

### 1.1 [docker-nvidia for ubuntu 18](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04)

```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
sudo apt update
apt-cache policy docker-ce
sudo apt install docker-ce
sudo systemctl status docker
sudo apt-get install -y nvidia-docker2
sudo pkill -SIGHUP dockerd
docker run --runtime=nvidia --rm nvidia/cuda nvidia-smi
```

### 1.2 docker 설치 \(CPU지원 도커\)

```
# 이전 docker 삭제 
sudo apt-get purge docker.io 

# 최신 docker 설치
sudo wget -qO- https://get.docker.com/ | sh
# sudo apt install docker.io #sudo apt-get update; sudo apt-get install docker-engine 

sudo apt-get upgrade docker

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

Add User : sudo usermod -aG docker adioshun

---

### 1.3 nvidia-docker 설치 \(GPU지원 도커\)

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

## 2. How to use




## Image search on the server
```
docker search ubuntu
```

## Image Download
```
docker pull ubuntu:latest
```
Remove the Image `docker rmi ubuntu:latest`


## Image make by Dockerfile
```
docker build --tag gingertea/centos-nginx:0.1 .
```
> docker build <옵션><Dockerfile 경로> : `docker build -t kunfengchen/u14-indigo-autoware` . #-t tag옵션


## Image rename 

```
docker tag server:latest myname/server:latest
docker tag d583c3ac45fd myname/server:latest
```

## List the DL images  
```
docker images
```

> 컨테이너 이름 변경 : `docker remane {원래이름} {변경 이름}`

## Create Container 

```python
# 일반 실행 
docker run -i -t -p 2222:22 -p 8585:8888 --volume /workspace:/workspace -h <name> --name 'Ubuntu' <image> /bin/bash 

#x11
xhost + 
sudo docker run -it --privileged -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY -p 1122:22 -p 1188:8888 -v /workspace:/workspace -h <name> --name "docker" <image> /bin/bash

#ROS용 실행 (net)
docker run --runtime=nvidia -it --privileged --network=host -v /tmp/.X11-unix:/tmp/.X11-unix --volume="$HOME/.Xauthority:/root/.Xauthority:rw" -e DISPLAY --volume /workspace:/workspace --name 'Ubuntu' <image> /bin/bash
docker run -it --net=host --volume /workspace:/workspace --name 'Ubuntu' <image> /bin/bash 




# --runtime=nvidia  #For Nvidia Docker2, Not or 1 
```

~~--volume "$HOME/.Xauthority:/root/.Xauthority:rw"~~


## Check the running status 
```
docker ps -a
```

## Start the Container
```
docker start Ubuntu
```

## Connect/Use the Container 
```
docker attach Ubuntu
docker exec -it [컨테이너명] bash # 쉘 접속 
docker exec -it [컨테이너명] su - ubuntu  #exec 로 접속 했을땐 접속을 해제 하여도 도커 컨테이너가 중지되지 않습니다.
```



## Exit the container

- 컨테이너 서비스를 중지하고 빠져나오기 `Ctrl + C`
- 컨테이너 서비스를 중지하지 않고 빠져나오기 : `Holding Ctrl+p,q`


## Stop the Container 
```
docker stop Ubuntu
```

## 컨테이너 삭제 

- Remove the Container `docker rm Ubuntu`

- 중지된 모든 컨테이너 삭제 : `docker container prune`

- 모든 컨테이너 삭제 : `docker rm $(docker ps -a -q)` # -q 컨테이너 ID만 출력 



## Copy file from Docker image
```
sudo docker cp hello-nginx:/etc/nginx/nginx.conf ./
#docker cp <컨테이너 이름>:<경로> <호스트 경로> 형식입니다.
```

# commit
```
docker commit -m "opencv3+tensorflow" 0c98bd5a193e adioshun/deeplearning:opencv3
# docker commit <exiting-container> <hub-user>/<repo-name>[:<tag>] 
```

> commit전에 불필요한 파일 삭제하여 용량줄이기 : 

# push 
```
sudo docker login
docker push adioshun/deeplearning:opencv3
# docker push <hub-user>/<repo-name>:<tag>

```



# Docker 저장 위치 변경 [[참고]](https://sanenthusiast.com/change-default-image-container-location-docker/)

```bash
docker info #저장 위치 확인
sudo systemctl stop docker
sudo mkdir /etc/systemd/system/docker.service.d # 폴더 없으면 생성 
sudo vi /etc/systemd/system/docker.service.d/docker.conf # 설정 파일 생성 & 아래 참고 하여 수정
sudo systemctl daemon-reload 
sudo systemctl start docker
```

설정 파일
```
# docker.conf
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd --graph="/mnt/new_volume" --storage-driver=devicemapper 
```


# Docker 이미지 복사 

```
docker save -o example.tar docker-image-name:tag 
docker load -i example.tar 
```

---

## 3.Tips

### 3.1 Webcam 연결

1. USB연결후 `lsusb`하여 연결 확인
2. Docker RUN시 `--privileged` 옵션 추가 
3. Docker 내에서 `/dev/video0` 확인
4. x11이 된다면 `apt-get install guvcview`를 설치 하여 웹캠 작동 확인 가능 

```
import numpy as np
import cv2

cap = cv2.VideoCapture(0)

while(True):
    # Capture frame-by-frame
    ret, frame = cap.read()

    # Our operations on the frame come here
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

    # Display the resulting frame
    cv2.imshow('frame',gray)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

# When everything done, release the capture
cap.release()
cv2.destroyAllWindows()
```

### 3.2 모니터링 

#### A. Docker Manager 
- [홈페이지](https://github.com/kevana/ui-for-docker)

```
docker run -d -p 9000:9000 --privileged -v /var/run/docker.sock:/var/run/docker.sock uifd/ui-for-docker
http://<dockerd host ip>:9000

```

![](https://github.com/kevana/ui-for-docker/raw/master/containers.png)


> Kubernetes 

---
https://github.com/remotty/documents.docker.co.kr

https://gist.github.com/nacyot/8366310




#### B. [docker_cli_dashboard](https://github.com/goody80/docker_cli_dashboard)

![](https://raw.githubusercontent.com/goody80/docker_cli_dashboard/master/sample01.png)

```
curl -sL bit.ly/ralf_dcs -o ./dcs
sudo chmod 755 ./dcs
sudo mv ./dcs /usr/bin/dcs
dcs
```


#### C. [NexClipper](https://github.com/TheNexCloud/NexClipper)

```python
sudo docker pull nexclipper/nexclipper;

sudo docker run \
	     --detach=true \
	     --name NexClipper \
	     -p 10001:9001 \
	     --volume /var/run/docker.sock:/var/run/docker.sock \
	     --volume /var/lib/docker:/var/lib/docker \
	     nexclipper/nexclipper;

# NexClipper is now running (in the background) on http://localhost:10001.
```


###  3.3 Docker에서 GUI(x11) 실행하기 

> [중요] xmamager 사용시 하기 설정 필요 없이 실행 가능 

[Using GUI's with Docker](http://wiki.ros.org/docker/Tutorials/GUI)

#### A. Script로 실행 

```bash
## to start this script: ./run_ros.sh
export IMAGE=nvidia/cuda:8.0-devel-ubuntu14.04
xhost +
nvidia-docker run -it \
    -e http_proxy="http://xx.xx.xx.xx:xx<if you are using proxy>" \
    -e https_proxy="https://xx.xx.xx.xx:xx" \
    -e ftp_proxy="ftp://xx.xx.xx.xx:xx" \
    \
    -e NO_AT_BRIDGE=1 \
    -e QT_X11_NO_MITSHM=1\
    \
    -e DISPLAY=:0 \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -v /tmp/.docker.xauth:/tmp/.docker.xauth \
    -e XAUTHORITY=/tmp/.docker.xauth \
    \
    -v /root/share:/root/share <if you are using share drive> \ 
    \
    $IMAGE  \
    /bin/bash
printf '\nclosing script now !!!\n'
exit

```

```
docker run -it --rm \
   --net host \
   -p 2222:2222 \
   -p 8585:8585 \
   --env="DISPLAY" \
   --name 'Ubuntu' \
   --volume "$HOME/.Xauthority:/root/.Xauthority:rw" \
   --volume "$HOME/docker_autoware:/sharefolder" \
   {image_name} /bin/bash
```
- 명령어로 실행 

```
# 실행전 
xhost +


docker run -it --net host --env="DISPLAY" -p 2222:2222 -p 8585:8585 --volume /home/adioshun/docker:/root --name 'Ubuntu' --volume "$HOME/.Xauthority:/root/.Xauthority:rw" {image_name} /bin/bash

nvidia-docker run -it --privileged -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY <image>
```




> `Error: Can't open display: :0` 에러시 : `xhost +` 실행  [[x11_Connection]](https://github.com/adioshun/System_Setup/wiki/x11_Connection)

---

![](https://github.com/wagoodman/dive/raw/master/.data/demo.gif)

도커이미지 탐색 : [dive](https://github.com/wagoodman/dive/blob/master/README.md)



---

### 3.4 추천 Docker Image

####A ##. [ROS-Kinetic with VNC](https://hub.docker.com/r/ct2034/vnc-ros-kinetic-full)

Docker image to provide HTML5 VNC interface to access ROS kinetic on Ubuntu 16.04 with the LXDE desktop environment.

```bash
docker pull ct2034/vnc-ros-kinetic-full  #1GB
docker run -it --rm -p 6080:80 ct2034/vnc-ros-kinetic-full
Browse http://127.0.0.1:6080/
```

> [docker-ubuntu-vnc-desktop](https://github.com/fcwu/docker-ubuntu-vnc-desktop)

## 2. [Tor Browser](https://hub.docker.com/r/hkjn/tor-browser)

```sh
docker pull hkjn/tor-browser #250M

docker run -it --rm --name tor-browser \
           -v /tmp/.X11-unix:/tmp/.X11-unix:ro \
           -e DISPLAY=unix$DISPLAY \
           hkjn/tor-browser
```

## 3. pcl

docker pull adioshun/ros16:python23-pcl\_opend3d\_161226

## 4. [CUDA](https://hub.docker.com/r/nvidia/cuda/tags)

| Ubuntu | CUDA/CuDNN | tag | Size | etc |
| --- | --- | --- | --- | --- |
| 16.4 | 9.0/7 | `docker pull nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04` | 720M |  |
| 16.4 | 8.0/ |  |  |  |
| 16.4 | 7.0/ |  |  |  |
| 18.4 | 9.2/7 | `docker pull nvidia/cuda:9.2-cudnn7-devel-ubuntu18.04` | 2GB |  |
| 18.04 | 10.0/7 | `docker pull nvidia/cuda:10.0-cudnn7-devel-ubuntu18.04` |  |  |
| 16.04 | 10.0/7 | `docker pull pytorch/pytorch:1.0.1-cuda10.0-cudnn7-devel` | 6.17G | pytorch 1.0.1- |

* base: starting from CUDA 9.0, contains the bare minimum \(libcudart\) to deploy a pre-built CUDA application.
  Use this image if you want to manually select which CUDA packages you want to install.
* runtime: extends the base image by adding all the shared libraries from the CUDA toolkit.
  Use this image if you have a pre-built application using multiple CUDA libraries.
* **devel**: extends the runtime image by adding the compiler toolchain, the debugging tools, the headers and the static libraries. Use this image to compile a CUDA application from sources.

설치전 CUDA NVIDIA Driver버젼 확인 필요

![](https://i.imgur.com/Eqsmxsu.png)

---

# Dockerfile

> \[추천\] [build ROS development environment with docker using ssh, GUI](https://ssaru.github.io/tip/2019/04/12/docker_ssh.html?fbclid=IwAR1zM8TMNkLX5UzrEK5DQyBxRPhI2BIGCSsvJjt0jgU2AFtgIFi8NjyThK0)

docker build --tag hello:0.1 .

ref : [https://rampart81.github.io/post/dockerfile\_instructions/](https://rampart81.github.io/post/dockerfile_instructions/)

## 1. 자동 실행 /데몬

```
FROM adioshun/pcls:pcl


EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]

EXPOSE 8888

CMD ["jupyter notebook --allow-root", "-D"]

ENTRYPOINT ["/entrypoint.sh"]
```

## 2. ssh 샘플

```
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

## 3. conda

```
FROM debian:latest
MAINTAINER Conda Development Team <conda@continuum.io>

RUN apt-get -qq update && apt-get -qq -y install curl bzip2 \
    && curl -sSL https://repo.continuum.io/miniconda/Miniconda2-latest-Linux-x86_64.sh -o /tmp/miniconda.sh \
    && bash /tmp/miniconda.sh -bfp /usr/local \
    && rm -rf /tmp/miniconda.sh \
    && conda install -y python=2 \
    && conda update conda \
    && apt-get -qq -y remove curl bzip2 \
    && apt-get -qq -y autoremove \
    && apt-get autoclean \
    && rm -rf /var/lib/apt/lists/* /var/log/dpkg.log \
    && conda clean --all --yes

ENV PATH /opt/conda/bin:$PATH
```

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

    Run example
    $ sudo docker run -d -P --name test_sshd rastasheep/ubuntu-sshd:16.04
    $ sudo docker port test_sshd 22
      0.0.0.0:49154

    $ ssh root@localhost -p 49154
    # The password is `root`
    root@test_sshd $



---

## Valgrant

[Vagrant Hub](https://app.vagrantup.com/boxes/search)



> [Setting Up Vagrant Environment on Ubuntu Platform](https://github.com/saasbook/courseware/wiki/Setting-Up-Vagrant-Environment-on-Ubuntu-Platform), [VirtualBox와 Vagrant의 기본 사용법](https://junistory.blogspot.kr/2017/08/virtualbox-vagrant.html)

### 1. Install

`apt-get install virtualbox`
`apt-get install vagrant`


### 2. 확인
```
VBoxManage -v
vagrant -v
vagrant status
```

### 3. 초기 설정
```
mkdir -p ~/vagrant/vm_01; cd ~/vagrant/vm_01
vagrant init shadowrobot/ros-indigo-desktop-trusty64
```
또는

```
vagrant init
vi Vagrantfile
- L15라인을 수정 `config.vm.box = "base"` -> `config.vm.box = "puphpet/centos65-x64"`

### 추천 설정
config.vm.provider "virtualbox" do |vb|
# # Display the VirtualBox GUI when booting the machine
vb.gui = true
#
# # Customize the amount of memory on the VM:
vb.memory = "2048"
end
###
```



### 5. 가상머신 실행 (Vagrant 실행)

vagrant up --provider virtualbox

### 6. 가상머신 접속 (로그인)

vagrant ssh
- Username : vagrant
- Password : vagrant

---

# singularity 

##  install
```bash
VERSION=2.2.1
wget https://github.com/singularityware/singularity/releases/download/$VERSION/singularity-$VERSION.tar.gz
tar xvf singularity-$VERSION.tar.gz
cd singularity-$VERSION
./configure --prefix=/usr/local
make
sudo make install
```
Check release : [github](https://github.com/singularityware/singularity/releases)

## create img from Docker(docker hub)
```bash
sudo singularity create --size 1000 analysis.img
sudo singularity import analysis.img docker://ubuntu:latest
singularity shell analysis.img
```
## Run 
```bash
singularity exec analysis.img cat /etc/lsb-release #/bin/bash
```

## convert Docker img to singularity img 
```
docker run -v /var/run/docker.sock:/var/run/docker.sock -v \root:/output --privileged -t --rm filo/docker2singularity adioshun/deeplearning:Full
```
- \root : 저장위치
- adioshun/deeplearning:Full : 변환할이미지



