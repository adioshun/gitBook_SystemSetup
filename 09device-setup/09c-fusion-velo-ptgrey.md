## 3. Lidar + Camear Calibration 

### 3.1 Ankitdhall의 방법

> 출처 : [LiDAR-Camera Calibration using 3D-3D Point correspondences, 2017](https://arxiv.org/abs/1705.09785):  [깃허브](https://github.com/ankitdhall/lidar_camera_calibration), [ROS(Kinetic)_lidar_camera_calibration](http://wiki.ros.org/lidar_camera_calibration)

장비 : 카메라(Point Gray Blackfly or ZED camera) + Lidar (VLP-16 +)

목적 : The package finds a rotation and translation that transform all the points in the LiDAR frame to the (monocular) camera frame.

설치 요구 사항 (Git의 `dependencies `폴더 참고)
- [aruco_ros](http://wiki.ros.org/aruco_ros) :  It provides real-time marker based 3D pose estimation using AR markers
- Slightly modified aruco_mapping 


```
cd ~
cp -r lidar_camera_calibration/dependencies/aruco_ros/ catkin_ws/src/
cd catkin_ws/
catkin_make
cp -r ~/lidar_camera_calibration/dependencies/aruco_mapping/ ~/catkin_ws/src/
catkin_make
cp -r ~/lidar_camera_calibration ~/catkin_ws/src/
catkin_make
catkin_make install 
```
> /root/catkin_ws/devel/lib/aruco_mapping/aruco_mapping

### 3.2. Hector 방식 (2017)

[ROS packages related to calibration of robots and subsystems (cameras, LIDAR, kinematics)](https://github.com/tu-darmstadt-ros-pkg/hector_calibration)

### 3.2 swyphcosmo의 방법 

> 출처 : Camera and LIDAR Calibration and Visualization in ROS: [깃허브](https://github.com/swyphcosmo/ros-camera-lidar-calibration), [youtube](https://www.youtube.com/watch?v=Zc4Ev_ggHIA)

Vagrant제공 

### 3.3 but의 방법

> 출처 : Calibration of RGB Camera with Velodyne LiDAR, 2014: [깃허브](https://github.com/robofit/but_velodyne), [ROS(Hydro)_but_calibration_camera_velodyne](http://wiki.ros.org/but_calibration_camera_velodyne)

요구사항
- ROS Hydro
- PCl 1.7 and
- OpenCV 2.x library