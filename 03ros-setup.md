# ROS

!!! 아나콘다 삭제후 진행 anaconda is not recommended [\[Solution\]](https://github.com/udacity/RoboND-Python-StarterKit/blob/master/doc/linux_ros_anaconda_warning.md)

```bash
apt-get install lsb-release

# ADD TO APT REPOSITORY LIST
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
# sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu trusty main" > /etc/apt/sources.list.d/ros-latest.list'

# SETUP KEYS
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116

# UPDATE APT PACKAGE LIST
sudo apt-get update

# INSTALL THE DESKTOP FULL VERSION
sudo apt-get install ros-kinetic-desktop-full

sudo apt-get install ros-kinetic-vision-opencv #ros-kinetic-opencv3 ros-kinetic-cv-bridge 

# INITIALIZE ROSDEP
sudo rosdep init
rosdep update #rosdep fix-permissions && rosdep update

# ENVIRONMENT SETUP
echo "source /opt/ros/$ROS_DISTRO/setup.bash" >> ~/.bashrc
source ~/.bashrc

# Dependencies for building packages
sudo apt-get install python-rosinstall python-rosinstall-generator python-wstool build-essential -y
sudo apt-get install python-catkin-tools python3-dev python3-catkin-pkg-modules python3-numpy python3-yaml ros-kinetic-cv-bridge


pip install PyYAML rospkg catkin_pkg && pip3 install PyYAML rospkg catkin_pkg


# 머신러닝 관련 패키지
pip install scikit-learn && pip3 install scikit-learn
pip install scipy && pip3 install scipy


```

> Ref :[Ronny](http://ronny.rest/blog/post_2017_03_29_ros/)

## Create a ROS Workspace

```python
# SOURCING  ENVIRONMENT - for kinetic version
source /opt/ros/$ROS_DISTRO/setup.bash

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

## [Create a ROS Workspace 2](http://korearosnews.blogspot.com/2015/03/catkin-catkintools.html)

```python
#apt-get install python-catkin-tools

mkdir ~/catkin_workspace
cd ~/catkin_workspace
mkdir src
catkin init
```


---

## 2. 패키지 설치 방법

### 2.1 소스 설치

```python
# INDIGO VERSION
cd ~/catkin_ws/src/ && git clone https://github.com/ros-drivers/velodyne.git
cd velodyne/
rosdep install --from-paths ./ --ignore-src --rosdistro $ROS_DISTRO -y
cd ~/catkin_ws/ && catkin_make
```

### 2.2 apt설치

```
apt-get install ros-$ROS_DISTRO-velodyne
```

#### \[패키지 설치시 에러처리\] Could not find a package configuration file provided by

- apt-get install apt-file && apt-file update
  ```python
  Could not find a package configuration file provided by "Qt5Core"
    (requested version 5.0) with any of the following names:

      Qt5CoreConfig.cmake
      qt5core-config.cmake

    Add the installation prefix of "Qt5Core" to CMAKE_PREFIX_PATH or set
    "Qt5Core_DIR" to a directory containing one of the above files.  If
    "Qt5Core" provides a separate development package or SDK, be sure it has
    been installed.
```

- run `apt-file search Qt5CoreConfig.cmake`
  - review the result

  ```python
  qtbase5-dev: /usr/lib/x86_64-linux-gnu/cmake/Qt5Core/Qt5CoreConfig.cmake
  qtbase5-gles-dev: /usr/lib/x86_64-linux-gnu/cmake/Qt5Core/Qt5CoreConfig.cmake
  ```

- Install the missing package `sudo apt install qtbase5-dev`

> ref [What package do I need to build...](https://askubuntu.com/questions/374755/what-package-do-i-need-to-build-a-qt-5-cmake-application/374775)
