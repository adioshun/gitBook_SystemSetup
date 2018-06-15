# ROS kinetic - in ubuntu 16.04 

!!! 아나콘다 삭제후 진행 anaconda is not recommended [[Solution]](https://github.com/udacity/RoboND-Python-StarterKit/blob/master/doc/linux_ros_anaconda_warning.md)


```bash
apt-get install lsb-release

# ADD TO APT REPOSITORY LIST
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu trusty main" > /etc/apt/sources.list.d/ros-latest.list'

# SETUP KEYS 
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116

# UPDATE APT PACKAGE LIST
sudo apt-get update

# INSTALL THE DESKTOP FULL VERSION
sudo apt-get install ros-indigo-desktop-full
sudo apt-get install ros-kinetic-desktop-full ros-kinetic-catkin -y
sudo apt-get install ros-melodic-desktop-full
#apt-cache search ros-melodic

# INITIALIZE ROSDEP
sudo rosdep init
rosdep update #rosdep fix-permissions && rosdep update

# ENVIRONMENT SETUP
echo "source /opt/ros/{배포판}/setup.bash" >> ~/.bashrc
source ~/.bashrc

# Dependencies for building packages
sudo apt-get install python-rosinstall python-rosinstall-generator python-wstool build-essential -y

```

> Ref :[Ronny](http://ronny.rest/blog/post_2017_03_29_ros/)


## Create a ROS Workspace

```python
# SOURCING  ENVIRONMENT - for kinetic version
source /opt/ros/kinetic/setup.bash

# CREATING A ROS CATKIN WORKSPACE
mkdir -p ~/catkin_ws/src && cd ~/catkin_ws/
catkin_make
cd ./build 
make -j4 -l4


# SOURCING  CATKIN ENVIRONMENT - and automatically get it to source from now on
echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
source ~/catkin_ws/devel/setup.bash; source ~/.bashrc
echo $ROS_PACKAGE_PATH

```

## 설치 확인 

```
1st terminal : roscore
2nd terminal : rosrun turtlesim turtlesim_node
3rd terminal : rosrun turtlesim turtle_teleop_key
4th terminal : rosrun rqt_graph rqt_graph
```

---

#### [패키지 설치시 에러처리] Could not find a package configuration file provided by

```
Could not find a package configuration file provided by "Qt5Core"
  (requested version 5.0) with any of the following names:

    Qt5CoreConfig.cmake
    qt5core-config.cmake

  Add the installation prefix of "Qt5Core" to CMAKE_PREFIX_PATH or set
  "Qt5Core_DIR" to a directory containing one of the above files.  If
  "Qt5Core" provides a separate development package or SDK, be sure it has
  been installed.
```

run `apt-file search Qt5CoreConfig.cmake`

review the result

```
qtbase5-dev: /usr/lib/x86_64-linux-gnu/cmake/Qt5Core/Qt5CoreConfig.cmake
qtbase5-gles-dev: /usr/lib/x86_64-linux-gnu/cmake/Qt5Core/Qt5CoreConfig.cmake
```

Install the missing package `sudo apt install qtbase5-dev`

> ref [What package do I need to build...](https://askubuntu.com/questions/374755/what-package-do-i-need-to-build-a-qt-5-cmake-application/374775)


