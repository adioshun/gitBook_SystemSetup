# [YOLO설치전 Opencv3.2 설치](http://pgmrlsh.tistory.com/3)


 
 
 ## Apt 설치 
 
 - apt-get install libopencv-dev libcv-bridge-dev
 
 
 ## pip install 
 
 pip install opencv-python
 
 
 에러 : `ImportError: /opt/ros/kinetic/lib/python2.7/dist-packages/cv2.so: undefined symbol: PyCObject_Type`

```python 
import sys
print(sys.path)

find '/opt/ros/kinetic/lib/python2.7/dist-packages'

remove it : `sys.path.remove('/opt/ros/kinetic/lib/python2.7/dist-packages')`
```

https://stackoverflow.com/questions/43019951/after-install-ros-kinetic-cannot-import-opencv




[Unable to use cv_bridge with ROS Kinetic and Python3](https://stackoverflow.com/questions/49221565/unable-to-use-cv-bridge-with-ros-kinetic-and-python3)