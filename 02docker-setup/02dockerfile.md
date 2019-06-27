# Docker Image

## 1. [ROS-Kinetic with VNC](https://hub.docker.com/r/ct2034/vnc-ros-kinetic-full)

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
| 16.04 | 10.0/7 | 'docker pull pytorch/pytorch:1.0.1-cuda10.0-cudnn7-devel\` | 6.17G | pytorch 1.0.1- |

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



