# Docker Image 

## 1. [ROS-Kinetic with VNC](https://hub.docker.com/r/ct2034/vnc-ros-kinetic-full)

Docker image to provide HTML5 VNC interface to access ROS kinetic on Ubuntu 16.04 with the LXDE desktop environment.

```bash
docker pull ct2034/vnc-ros-kinetic-full  #1GB
docker run -it --rm -p 6080:80 ct2034/vnc-ros-kinetic-full
Browse http://127.0.0.1:6080/
```


## 2. Tor Browser

```sh
docker pull hkjn/tor-browser #250M
docker run -it --rm --name tor-browser -v /tmp/.X11-unix:/tmp/.X11-unix:ro -e DISPLAY=unix$DISPLAY hkjn/tor-browser

```







---

# dockerfile 

docker build --tag hello:0.1 .

ref : https://rampart81.github.io/post/dockerfile_instructions/



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