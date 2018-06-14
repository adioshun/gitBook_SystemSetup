# Autoware

[Github](https://github.com/CPFL/Autoware), [Homepage](https://www.autoware.ai/)

## 1. 설치

```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116

sudo apt-get update
```

### 1.1 Indigo \(Ubuntu 14.04\)

```
sudo apt-get install ros-indigo-desktop-full ros-indigo-nmea-msgs ros-indigo-nmea-navsat-driver ros-indigo-sound-play ros-indigo-jsk-visualization ros-indigo-grid-map ros-indigo-gps-common

sudo apt-get install ros-indigo-controller-manager ros-indigo-ros-control ros-indigo-ros-controllers ros-indigo-gazebo-ros-control ros-indigo-sicktoolbox ros-indigo-sicktoolbox-wrapper ros-indigo-joystick-drivers ros-indigo-novatel-span-driver

sudo apt-get install libnlopt-dev freeglut3-dev qtbase5-dev libqt5opengl5-dev libssh2-1-dev libarmadillo-dev libpcap-dev gksu libgl1-mesa-dev libglew-dev software-properties-common libyaml-cpp-dev python-flask python-requests

sudo add-apt-repository ppa:mosquitto-dev/mosquitto-ppa; apt-get update

sudo apt-get install libmosquitto-dev
```

NOTE: Please **do not** install `ros-indigo-velodyne-pointcloud` package. Please uninstall it if you already installed.

### 1.1 Kinetic \(Ubuntu 16.4\)

```
sudo apt-get install ros-kinetic-desktop-full ros-kinetic-nmea-msgs ros-kinetic-nmea-navsat-driver ros-kinetic-sound-play ros-kinetic-jsk-visualization ros-kinetic-grid-map ros-kinetic-gps-common ros-kinetic-controller-manager ros-kinetic-ros-control ros-kinetic-ros-controllers ros-kinetic-gazebo-ros-control ros-kinetic-joystick-drivers libnlopt-dev freeglut3-dev qtbase5-dev libqt5opengl5-dev libssh2-1-dev libarmadillo-dev libpcap-dev gksu libgl1-mesa-dev libglew-dev python-wxgtk3.0 software-properties-common libmosquitto-dev libyaml-cpp-dev python-flask python-requests

sudo rosdep init

rosdep update

# ENVIRONMENT SETUP
echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

### 1.1 소스 설치

PCL소스 설치법으로 사전 설치 필요 (VTK, etc.)


```python


$ cd $HOME
$ git clone https://github.com/CPFL/Autoware.git --recurse-submodules
$ cd ~/Autoware/ros/src
$ catkin_init_workspace
$ cd ../
$ rosdep install -y --from-paths src --ignore-src --rosdistro $ROS_DISTRO
$ ./catkin_make_release

$ cd $HOME/Autoware/ros
$ ./run
```


| Error Code | Solution |
| --- | --- |
| Could NOT find GLEW | `sudo apt-get install libglew-dev` |
| gconf\_value\_free: assertion 'value != NULL' failed | `ssh -X localhost`, `./run` |

### 1.2 Docker 설치

```
docker pull autoware/autoware:1.7.0-kinetic
docker pull kunfengchen/u14-indigo-autoware 
docker pull nownicked/autoware-x11running7
```

## 2. 실행

### 2.1 소스설치시

### 2.2 Docker 설치시

* xmanager - 연결 설정 - Connection/SSH/Tunneling/X11 Forwarding 체크, xManager체크

* 도커 실생   `docker run -it --rm --net host --env="DISPLAY" --privileged -v /tmp/.X11-unix:/tmp/.X11-unix --volume "$HOME/.Xauthority:/root/.Xauthority:rw" --name "x11" adioshun/ubuntu16:Open3D /bin/bash`

* 도커 내에서 `cd /src/Autoware/ros/ && ./run`



