? **ROS inclune PCL ???**


# 1. PCL for C++ 설치 

## 1.1 Source 설치 


```
sudo apt-get install git
cd
git clone https://github.com/PointCloudLibrary/pcl.git
cd pcl
mkdir build
cd build
cmake ..
make
make install 

#checkinstall #apt-get install checkinstall
```


|Error Code | Solution|
|-|-|
|package 'eigen3' not found | apt-get install libeigen3-dev|
|package 'flann>=1.7.0' not found|apt-get install libflann-dev|





## 1.1 apt 설치 
```
add-apt-repository ppa:v-launchpad-jochen-sprickerhof-de/pcl
apt-get update
apt-get install libpcl-all
```

|Error Code | Solution|
|-|-|
|add-apt-repository command not found | `apt-get install software-properties-common python-software-properties`|
|Unable to locate package libpcl-all|apt-get install libpcl1|
|pip10, ImportError: cannot import name main|Downgrade: `python2 -m pip install --user --upgrade pip==9.0.3`|




## 1.2 deb 설치 
```
wget https://www.dropbox.com/s/9llzm20pc4opdn9/PCL-1.8.0-Linux.deb?dl=0 #파일명 에서 `?dl=0`제거 
dpkg -i PCL-1.8.0-Linux.deb
```

> 출처 : [POINT CLOUD LIBRARY ON UBUNTU 16.04 LTS](https://larrylisky.com/2016/11/03/point-cloud-library-on-ubuntu-16-04-lts/)

> [PLC for Ubuntu 16.4 & 17.10](https://askubuntu.com/questions/916260/how-to-install-point-cloud-library-v1-8-pcl-1-8-0-on-ubuntu-16-04-2-lts-for)



###### PCl Tools

`sudo apt-get install pcl-tools` [[github]](https://github.com/ryanfb/pcl-tools)
- pcd2ply - convert PCD files to PLY files
- ply2pcd - convert PLY files to PCD files
- statistical_removal - statistical outliers removal
- pcd_viewer - PCL PCD viewer with added 
- pcdnormal2ply - convert PCD files with normals to PLY files
- normal_estimation_omp - estimate normals in a PCD using PCL's OpenMP implementation



# 2. PCL for Python 설치 
- [Python bindings for the Point Cloud Library](http://pointclouds.org/news/tags/python), [[Code Sample]](https://github.com/strawlab/python-pcl/tree/master/examples)
- 요구 사항 : `Python 2.7.6, 3.4.0, 3.5.2`, `pcl 1.7.0`, `Cython`

## 2.1 apt 설치

```
apt-get install build-essential
pip install cython==0.25.2
pip install numpy
git clone https://github.com/strawlab/python-pcl.git
cd python-pcl

sudo python setup.py clean
sudo make clean

sudo make all 
sudo python setup.py install

# Or
python setup.py build
python setup.py install
```

> 참고 : [python-pcl, python 3, ubuntu 14.04](http://adamsteer.blogspot.kr/2016/01/python-pcl-python-3-ubuntu-1404.html)


|에러코드|해결책|원인|
|-|-|-|
|fatal error: pcl/features/cppf.h: No such file or directory|`sudo add-apt-repository ppa:v-launchpad-jochen-sprickerhof-de/pcl;sudo apt-get update;sudo apt-get upgrade libpcl-features-dev libpcl-io-1.7 libpcl-io-1.7-dev`|ubnutu 14 떄문인|


### 2.2 conda 설치 (ROS충돌 위험) 
```
conda install -c https://conda.anaconda.org/ccordoba12 python-pcl
```


### 2.3 Python-pcl docker



```
#Dockerfile

FROM ubuntu:14.04

RUN apt-get install -y software-properties-common
RUN add-apt-repository ppa:v-launchpad-jochen-sprickerhof-de/pcl
RUN apt-get update && \
    apt-get install -y libpcl-all

RUN apt-get install -y python-pip git
RUN apt-get install -y python-dev
RUN pip install cython
RUN pip install numpy
RUN pip install git+https://github.com/strawlab/python-pcl.git#egg=pcl
```


###### pcl_helper.py

[github](https://github.com/udacity/RoboND-Perception-Exercises#documentation-for-pcl_helperpy), [Code](https://github.com/udacity/RoboND-Perception-Exercises/tree/master/Exercise-2/sensor_stick/scripts)



`random_color_gen()` : Generates a random set of r,g,b values
- Return: a 3-tuple with r,g,b values (range 0-255)

`ros_to_pcl(sensor_msgs/PointCloud2)` : Converts sensor_msgs/PointCloud2 to XYZRGB Point Cloud
- Return: pcl.PointCloud_PointXYZRGB

`pcl_to_ros(pcl.PointCloud_PointXYZRGB)`: Converts XYZRGB Point Cloud to sensor_msgs/PointCloud2
- Return: sensor_msgs/PointCloud2

`XYZRGB_to_XYZ(XYZRGB_cloud)`: Converts XYZRGB Point Cloud to XYZ Point CLoud
- Return: pcl.PointCloud

`XYZ_to_XYZRGB(XYZ_cloud, color)`:Takes a 3-tuple as color and adds it to XYZ Point Cloud
- Return: pcl.PointCloud_PointXYZRGB

`rgb_to_float(color)`:Converts 3-tuple color to a single float32
- Return: rgb packed as a single float32

`get_color_list(cluster_count)` : Creates a list of 3-tuple (rgb) with length of the list = cluster_count
- Return: get_color_list.color_list





## 3. PCL for ROS

### 3.1 Source 설치 (ubuntu 14)

```
## Download
cd ~/catkin_ws/src
git clone https://github.com/ros-perception/perception_pcl
rosdep install --from-paths ./ --ignore-src --rosdistro indigo -y

## Register {launch} with ROS via catkin_make.
cd ~/catkin_ws
catkin_make 
source ./devel/setup.sh

## Check
rospack profile
rospack list | grep {velodyne}
rospack find {}
```

## 3. VALIDATION 

### 3.1 PCL for C++

```
cd ~
mkdir pcl-test && cd pcl-test
```

Create a CMakeLists.txt file:

```
cmake_minimum_required(VERSION 2.8 FATAL_ERROR)
project(pcl-test)
find_package(PCL 1.2 REQUIRED)

include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})

add_executable(pcl-test main.cpp)
target_link_libraries(pcl-test ${PCL_LIBRARIES})

SET(COMPILE_FLAGS "-std=c++11")
add_definitions(${COMPILE_FLAGS})
```

Create a main.cpp file:

```
#include <iostream>

int main() {
    std::cout << "hello, world!" << std::endl;
    return (0);
}
```

Compile:

```
mkdir build && cd build
cmake ..
make
```

Test: 

```
./pcl-test
``` 

Output -> hello, world!

### 3.2 PCL for Python

```
python
import pcl
```
