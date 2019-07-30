# ROS

> !!! 아나콘다 삭제후 진행 anaconda is not recommended [\[Solution\]](https://github.com/udacity/RoboND-Python-StarterKit/blob/master/doc/linux_ros_anaconda_warning.md)

## 1. ROS 설치 

### 1.1 스크립트 설치 

```
melodic 18: https://raw.githubusercontent.com/ROBOTIS-GIT/robotis_tools/master/install_ros_melodic.sh
kinetic 16: https://raw.githubusercontent.com/ROBOTIS-GIT/robotis_tools/master/install_ros_kinetic.sh
```


### 1.2 Line-by-Line cmd 설치 

```python 
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
sudo apt-get install ros-melodic-desktop-full cd ~/

sudo apt-get install ros-kinetic-vision-opencv #ros-kinetic-opencv3 ros-kinetic-cv-bridge 

# INITIALIZE ROSDEP
sudo rosdep init
rosdep update #rosdep fix-permissions && rosdep update

# ENVIRONMENT SETUP
echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc

source ~/.bashrc

# Dependencies for building packages
sudo apt-get install python-rosinstall python-rosinstall-generator python-wstool build-essential -y
sudo apt-get install python-catkin-tools python3-dev python3-catkin-pkg-modules python3-numpy python3-yaml -y


pip install PyYAML rospkg catkin_pkg && pip3 install PyYAML rospkg catkin_pkg


# 머신러닝 관련 패키지
pip install scikit-learn && pip3 install scikit-learn
pip install scipy && pip3 install scipy
```

> Ref :[Ronny](http://ronny.rest/blog/post_2017_03_29_ros/)


환경 설정 확인 :`printenv | grep ROS`
---




## 5.Tip

* 여러 컴퓨터에 설치 

```
vi /etc/host 
192.168.0.65 jetson
192.168.0.17 lidar0
```

```
export ROS_HOSTNAME=jetson
export ROS_MASTER_URI=http://lidar0:11311
```



