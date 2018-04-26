# ROS kinetic - in ubuntu 16.04 

- !!! 아나콘다 삭제후 진행 anaconda is not recommended 

> docker : sudo docker pull osrf/ros:kinetic-desktop-full

```bash


sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116

sudo apt-get update

sudo apt-get install ros-kinetic-desktop-full #OR ros-kinetic-desktop-base 
sudo apt-get install ros-kinetic-catkin

sudo rosdep init

rosdep update

# ENVIRONMENT SETUP
echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
source ~/.bashrc

# GET ROSINSTALL
sudo apt-get install python-rosinstall


sudo apt-get install python-rosinstall  #Create a ROS Workspace까지해야import ros가능

```

> Ref :[ROS Wiki](http://wiki.ros.org/kinetic/Installation/Ubuntu)



# ROS INDIGO - in ubuntu 14.04


```bash 
# INSTALL ROS INDIGO - in ubuntu 14.04

# ADD TO APT REPOSITORY LIST
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu trusty main" > /etc/apt/sources.list.d/ros-latest.list'

# SETUP KEYS 
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116

# UPDATE APT PACKAGE LIST
sudo apt-get update

# INSTALL THE DESKTOP FULL VERSION
sudo apt-get install ros-indigo-desktop-full

# BARE BONES VERSION - NO GUI 
# sudo apt-get install ros-indigo-ros-base

# INITIALIZE ROSDEP
sudo rosdep init
rosdep update

# ENVIRONMENT SETUP
echo "source /opt/ros/indigo/setup.bash" >> ~/.bashrc
source ~/.bashrc

# GET ROSINSTALL
sudo apt-get install python-rosinstall

```

> Ref :[Ronny](http://ronny.rest/blog/post_2017_03_29_ros/)

# Setup

ROS_ROOT and ROS_PACKAGE_PATH 확인  : `printenv | grep ROS`

## Create a ROS Workspace

```bash 
# SOURCING  ENVIRONMENT - for kinetic version
source /opt/ros/kinetic/setup.bash

# CREATING A ROS CATKIN WORKSPACE
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/
catkin_make
#Run "make -j4 -l4" in "/workspace/ros/build"


# SOURCING  CATKIN ENVIRONMENT - and automatically get it to source from now on
echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
source ~/catkin_ws/devel/setup.bash; source ~/.bashrc
echo $ROS_PACKAGE_PATH

```
> 확인 : `echo $ROS_PACKAGE_PATH` 

## 설치 확인 

```
1st terminal : roscore
2nd terminal : rosrun turtlesim turtlesim_node
3rd terminal : rosrun turtlesim turtle_teleop_key
4th terminal : rosrun rqt_graph rqt_graph
```

## 권장 개발 에디터 (qtcreator)

```
sudo apt-get install qtcreator 
qtcreator
```


---

