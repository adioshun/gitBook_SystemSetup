# 1. PCL for C++ 설치 

## 1.1 apt 설치 [[공식 설치 가이드]](http://pointclouds.org/downloads/linux.html) 
```
add-apt-repository ppa:v-launchpad-jochen-sprickerhof-de/pcl
apt-get update
apt-get install libpcl-all
```
> [에러] add-apt-repository command not found -> `apt-get install software-properties-common python-software-properties`

## 1.2 deb 설치 
```
wget https://www.dropbox.com/s/9llzm20pc4opdn9/PCL-1.8.0-Linux.deb?dl=0 #파일명 에서 `?dl=0`제거 
dpkg -i PCL-1.8.0-Linux.deb
```

> 출처 : [POINT CLOUD LIBRARY ON UBUNTU 16.04 LTS](https://larrylisky.com/2016/11/03/point-cloud-library-on-ubuntu-16-04-lts/)

> [PLC for Ubuntu 16.4 & 17.10](https://askubuntu.com/questions/916260/how-to-install-point-cloud-library-v1-8-pcl-1-8-0-on-ubuntu-16-04-2-lts-for)


# 2. PCL for Python 설치 
- [Python bindings for the Point Cloud Library](http://pointclouds.org/news/tags/python)
- 요구 사항 : `Python 2.7.6, 3.4.0, 3.5.2`, `pcl 1.7.0`, `Cython`

## 2.1 apt 설치

```
apt-get install build-essential
pip install cython==0.25.2
pip install numpy
git clone https://github.com/strawlab/python-pcl.git
cd python-pcl

## For python2
python setup.py build_ext -i
python setup.py install

## re-install 
sudo python setup.py clean
sudo make clean
sudo make all
sudo python setup.py install


## For python3
python3 setup.py build
python3 setup.py install
```

> 참고 : [python-pcl, python 3, ubuntu 14.04](http://adamsteer.blogspot.kr/2016/01/python-pcl-python-3-ubuntu-1404.html)

### 2.2 conda 설치 (ROS충돌 위험) 
```
conda install -c https://conda.anaconda.org/ccordoba12 python-pcl
```

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

## 4. VALIDATION 

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
