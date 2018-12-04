# [YOLO설치전 Opencv3.2 설치](http://pgmrlsh.tistory.com/3)

# 1. OpenCV 설치 
 
 apt-get install libopencv-dev libcv-bridge-dev
 
 
## 2. python-opencv 설치  
 
pip install opencv-python
 
## 3. ROS용 cv-bridge설치 
 
 
sudo apt-get install ros-(ROS version name)-cv-bridge

sudo apt-get install ros-(ROS version name)-vision-opencv

## 3-1 ROS용 cv-bridge설치 for python3


> ROS + python3에서 cv-bridge문제 있는 듯 


```python
# `python-catkin-tools` is needed for catkin tool
# `python3-dev` and `python3-catkin-pkg-modules` is needed to build cv_bridge
# `python3-numpy` and `python3-yaml` is cv_bridge dependencies
# `ros-kinetic-cv-bridge` is needed to install a lot of cv_bridge deps. Probaply you already have it installed.
sudo apt-get install python-catkin-tools python3-dev python3-catkin-pkg-modules python3-numpy python3-yaml ros-kinetic-cv-bridge


# Create catkin workspace
mkdir catkin_workspace
cd catkin_workspace
catkin init

# Instruct catkin to set cmake variables
catkin config -DPYTHON_EXECUTABLE=/usr/bin/python3 -DPYTHON_INCLUDE_DIR=/usr/include/python3.5m -DPYTHON_LIBRARY=/usr/lib/x86_64-linux-gnu/libpython3.5m.so

# Instruct catkin to install built packages into install place. It is $CATKIN_WORKSPACE/install folder
catkin config --install

# Clone cv_bridge src
git clone https://github.com/ros-perception/vision_opencv.git src/vision_opencv

# Find version of cv_bridge in your repository
apt-cache show ros-kinetic-cv-bridge | grep Version
    Version: 1.12.8-0xenial-20180416-143935-0800

# Checkout right version in git repo. In our case it is 1.12.8
cd src/vision_opencv/
git checkout 1.12.8
cd ../../

# Build
catkin build cv_bridge

# Extend environment with new package
source install/setup.bash --extend
```

## 에러처리 

###### [Could not find the following Boost libraries: boost_python3](https://github.com/andrewssobral/bgslibrary/issues/96)

```
/usr/lib/x86_64-linux-gnu# ln -s libboost_python-py35.so libboost_python3.so

```
  
###### [`ImportError: /opt/ros/kinetic/lib/python2.7/dist-packages/cv2.so: undefined symbol: PyCObject_Type`](https://stackoverflow.com/questions/43019951/after-install-ros-kinetic-cannot-import-opencv)

```python 
~/.bashrc : PYTHONPATH="/usr/lib/python3.5/site-packages:$PYTHONPATH" 
```

```python
import sys
print(sys.path)

find '/opt/ros/kinetic/lib/python2.7/dist-packages'

remove it : `sys.path.remove('/opt/ros/kinetic/lib/python2.7/dist-packages')`
```

mv /opt/ros/kinetic/lib/python2.7/dist-packages/cv2.so /opt/ros/kinetic/lib/python2.7/dist-packages/cv2.so~



###### [`ImportError: dynamic module does not define module export function (PyInit_cv_bridge_boost)`](https://stackoverflow.com/questions/49221565/unable-to-use-cv-bridge-with-ros-kinetic-and-python3)


- python3에서 `from cv_bridge.boost.cv_bridge_boost import getCvType`시 발생


