> 참고: [Docker Official Hub](https://hub.docker.com/), [Docker Material Links](http://documents.docker.co.kr/ ), [eBook](http://www.pyrasis.com/private/2014/11/30/publish-docker-for-the-really-impatient-book), 

> [Tips](http://newsight.tistory.com/228): 명령어, 팁들 


# Install


## 1. docker 설치 (CPU지원 도커) 

```
# 이전 docker 삭제 
sudo apt-get purge docker.io 

# 최신 docker 설치
sudo apt install docker.io #sudo apt-get update; sudo apt-get install docker-engine 
sudo wget -qO- https://get.docker.com/ | sh

docker 버전 확인
# docker version
```

## 2. nvidia-docker 설치 (GPU지원 도커) 

1. Docker(CPU) 설치 

2. [GPU Driver,CUDA install](https://github.com/adioshun/System_Setup/wiki/4_CUDA_CuDNN-Setup)

3. nvidia Docker설치

3.1 버젼 1

```bash
# nvidia-docker 다운로드
wget -P /tmp https://github.com/NVIDIA/nvidia-docker/releases/download/v1.0.1/nvidia-docker_1.0.1-1_amd64.deb

# 패키지 설치
sudo dpkg -i /tmp/nvidia-docker*.deb && rm /tmp/nvidia-docker*.deb

# nvidia docker 테스트
nvidia-docker run --rm nvidia/cuda nvidia-smi 

```
> Ndivia-docker binary : [Download](https://github.com/NVIDIA/nvidia-docker/releases), [Manual](https://github.com/NVIDIA/nvidia-docker)

3.2 버젼 2

```
# If you have nvidia-docker 1.0 installed: we need to remove it and all existing GPU containers
docker volume ls -q -f driver=nvidia-docker | xargs -r -I{} -n1 docker ps -q -a -f volume={} | xargs -r docker rm -f
sudo apt-get purge -y nvidia-docker

# Add the package repositories
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
  sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/ubuntu16.04/amd64/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update

# Install nvidia-docker2 and reload the Docker daemon configuration
sudo apt-get install -y nvidia-docker2
sudo pkill -SIGHUP dockerd

# Test nvidia-smi with the latest official CUDA image
docker run --runtime=nvidia --rm nvidia/cuda nvidia-smi

```

Dependency 에러시 특정 패키지 버젼 지정하여 설치 하기 
```
The following packages have unmet dependencies:
 nvidia-docker2 : Depends: docker-ce (= 17.12.0~ce-0~ubuntu) but 18.02.0~ce-0~ubuntu is to be installed or
                           docker-ee (= 17.12.0~ee-0~ubuntu) but it is not installable
E: Unable to correct problems, you have held broken packages.
```
> root@ubuntu16:/tmp# sudo apt-get install docker-ce={17.12.0~ce-0~ubuntu}



# How to use 


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


## List the DL images  
```
docker images
```

> 도커 이름 변경 : `docker remane {원래이름} {변경 이름}`

## Create Container 
```
docker run -i -t --name Ubuntu ubuntu /bin/bash
sudo docker run -i -t -p 2222:2222 -p 8585:8585 --volume /home/adioshun/docker:/root --name 'Ubuntu' ubuntu /bin/bash 
sudo nvidia-docker run -i -t -p 2222:2222 -p 8585:8585 --volume /home/adioshun/docker:/root --name 'Ubuntu' ubuntu /bin/bash 
sudo docker --runtime=nvidia -it 
```

> nvidia 도커 실행시 `nvcc --version`확인후 `echo "export PATH=/usr/local/cuda/bin/:\$PATH; export LD_LIBRARY_PATH=/usr/local/cuda/lib64/:\$LD_LIBRARY_PATH; " >>~/.bashrc && source ~/.bashrc` 설정

> VM에서실행시 `firewall ingress/egress` 설정필요, [[AWS 포트 개방하기]](http://blog.naver.com/alice_k106/220340515017)


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

## Webcam 연결

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

---



# Docker Manager 
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




### [docker_cli_dashboard](https://github.com/goody80/docker_cli_dashboard)

![](https://raw.githubusercontent.com/goody80/docker_cli_dashboard/master/sample01.png)

```
curl -sL bit.ly/ralf_dcs -o ./dcs
sudo chmod 755 ./dcs
sudo mv ./dcs /usr/bin/dcs
dcs
```

---

# Docker에서 GUI(x11) 실행하기 

> [중요] xmamager 사용시 하기 설정 필요 없이 실행 가능 

[Using GUI's with Docker](http://wiki.ros.org/docker/Tutorials/GUI)

## Script로 실행 

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

---
[좋은도커이미지만들기](https://dayone.me/1740z5r)

### 1. Clean the APT Cache (And Do It Regularly)

```
sudo apt-get autoremove
#sudo apt-get autoclean
#sudo apt-get clean
#du -sh /var/cache/apt/archives
```

## 2. Remove Old Kernels (If No Longer Required)
```
sudo apt-get autoremove --purge
```




