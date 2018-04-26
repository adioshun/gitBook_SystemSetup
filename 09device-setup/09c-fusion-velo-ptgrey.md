# Lidar + Camear Calibration (Camera lidar alignment)


## 0. Autoware의 방법 

> ref : [How to Calibrate Camera](https://github.com/CPFL/Autoware/wiki/Calibration), [Youtube](https://www.youtube.com/watch?v=pfBmfgHf6zg)


- Lidar 부분 배경 색상 변경 : `b`  (Cloud point도 검은색이라 보이지 않음)

---

## 0. preritj의 방법  

[Lidar-camera calibration for DiDi competition](https://github.com/preritj/lidar-camera-calibration) 
- 간단한 코드로 구성 되어 원리 이해하기 좋을듯 
- [jupyter & python](http://nbviewer.jupyter.org/github/preritj/lidar-camera-calibration/blob/master/Calibration.ipynb) 이용

---

## Sensor Box (2017)

- [깃허브](https://github.com/h3ct0r/sensor_box_itv)




---

## 1. Ankitdhall의 방법

> 출처 : [LiDAR-Camera Calibration using 3D-3D Point correspondences, 2017](https://arxiv.org/abs/1705.09785):  [깃허브](https://github.com/ankitdhall/lidar_camera_calibration), [ROS\(Kinetic\)\_lidar\_camera\_calibration](http://wiki.ros.org/lidar_camera_calibration)

장비 : 카메라\(Point Gray Blackfly or ZED camera\) + Lidar \(VLP-16 +\)

목적 : The package finds a rotation and translation that transform all the points in the LiDAR frame to the \(monocular\) camera frame.

### 1.1 설치

```bash
cd ~
git clone https://github.com/ankitdhall/lidar_camera_calibration.git 
cp -r lidar_camera_calibration/dependencies/aruco_ros/ catkin_ws/src/
cd catkin_ws/
catkin_make
cp -r ~/lidar_camera_calibration ~/catkin_ws/src/
catkin_make
catkin_make install
```

### 1.2 설정

> /root/catkin\_ws/devel/lib/aruco\_mapping/aruco\_mapping

실행 : `roslaunch lidar_camera_calibration find_transform.launch`

#### A `lidar_camera_calibration.yaml` 파일 수정

참고 해야 하는 카메라와 라이다 TOPIC명시 `Contains name of camera and velodyne topics that the node will subscribe to.`

```python
lidar_camera_calibration:
camera_frame_topic: /image_raw # [중요] 반드시 변경
camera_info_topic: /camera_info # [중요] 반드시 변경
velodyne_topic: /velodyne_points
```

> `lidar_camera_calibration/launch/find_transform.launch` 을이용하여 값 찾을수 있음

#### B. `config_file.txt` 파일 수정

```python
480 640                 # image_width image_height
-2.5 2.5                 # x- x+, 불필요한 포인트 제거를 위한 필터, mark the board edges를 쉽게 함
-4.0 4.0                 # y- y+, 불필요한 포인트 제거를 위한 필터, mark the board edges를 쉽게 함
0.0 2.5                 # z- z+, 불필요한 포인트 제거를 위한 필터, mark the board edges를 쉽게 함
0.05 # cloud_intensity_threshold # Intensity가 낮는 포인트 제거를 위한 필터의 기준
2                 # number_of_markers
1                 # use_camera_info_topic? # `camera_info topic`을 사용하려면 `1` / '0'은 `config_file.txt`의 값 사용
611.651245 0.0 642.388357 0.0 # fx 0 cx 0
0.0 688.443726 365.971718 0.0 # 0 fy cy 0
0.0 0.0 1.0 0.0 # MAX_ITERS
                # After MAX_ITERS, the node outputs an average translation vector (3x1) and an average rotation matrix (3x3).
                # Averaging the translation vector is trivial;
                # the rotations matrices are converted to quaternions and averaged,
                # then converted back to a 3x3 rotation matrix
1.57 -1.57 0.0 # initial_rot_x initial_rot_y initial_rot_z
                # 카메라대비 Lidar의 초기 회전 여부, Initial orientation of the lidar with respect to the camera, in radians.
                # The default values are for the case when both the lidar and the camera are both pointing forward.
                # The final transformation that is estimated by the package accounts for this initial rotation.
```

#### C. `find_transform.launch` 파일 수정

`aruco_mapping`노드에게 필요한 정보들 `Parameters are required for the aruco_mapping node and need to be specfied here.`

* Ensure that the topics are mapped correctly for the node to function. Other parameters required are:
* calibration\_file\(.ini format\)
* num\_of\_markers
* marker\_size\(in meters\)

```xml
<?xml version="1.0"?>
<launch>
<!-- <param name="/use_sim_time" value="true"/> -->


<!-- ArUco mapping -->

<node pkg="aruco_mapping" type="aruco_mapping" name="aruco_mapping" output="screen">  #Uncomment
<remap from="/image_raw" to="/image_raw"/> ## Must

<param name="calibration_file" type="string" value="$(find aruco_mapping)/data/geniusF100.ini" /> # 칼리브레이션 파일(ini), ptgrey.ini
<param name="num_of_markers" type="int" value="2" /> # 마커 갯수
<param name="marker_size" type="double" value="0.24"/> # 마커 크기 (m)
<param name="space_type" type="string" value="plane" />
<param name="roi_allowed" type="bool" value="false" />


</node>


<rosparam command="load" file="$(find lidar_camera_calibration)/conf/lidar_camera_calibration.yaml" />
<node pkg="lidar_camera_calibration" type="find_transform" name="find_transform" output="screen">
</node>
</launch>
```

#### D. `marker_coordinates.txt` 파일

The ArUco markers are stuck on the board such that when it is hung from a corner, the ArUco marker is on the left side of the board.

Notice how the axis are aligned.

y-axis should point outwards, x-axis along the breadth \(s2\) and z-axis along the length \(s1\).

The markers are also arranged so that the ArUco id are in ascending order. \(마커의 ArUco ids는 오름차순으로 있어야 한다.\)

```
# cm 단위로 표기
2 # 'N' the number of boards
24.0 # length (s1)
24.0 # breadth (s2)
2.0 # border_width_along_length (b1)
2.0 # border_width_along_breadth (b2)
13.6 # edge_length_of_ArUco_marker (e)
24.0 # s1
24.0 # s2
2.0 # b1
2.0 # b2
13.6 # e
```

> [Aruco Marker Generator](https://terpconnect.umd.edu/~jwelsh12/enes100/markergen.html): ID 25, 582 , Marker Size = 205\(e\), padding = 50 \(b1, b2\)

![](https://github.com/ankitdhall/lidar_camera_calibration/raw/master/images/board_dim_label.jpg)

![](https://github.com/ankitdhall/lidar_camera_calibration/raw/master/images/aruco_axis.png)

## 3. 실행

> 동영상 설명 : [Youtube](https://youtu.be/SiPGPwNKE-Q)

```
roslaunch velodyne_pointcloud VLP16_points.launch
roslaunch pointgrey_camera_driver camera.launch
roslaunch lidar_camera_calibration find_transform.launch
```

An initial \[R\|t\] between the camera and the various ArUco markers will be estimated. Following this, a filtered point cloud \(according to the specifications in the config\_file.txt\) will be displayed. The user needs to mark each edge of the rectangular board.

Each board will have 4 line segments and need to be marked from leftmost board to the rightmost board. Marking a line segment is quite straight-forward, one needs to draw a quadrilateral around the line being marked. Click and press a key to confirm the corner of the quadrilateral. Once 4 points are clicked, each followed by a key-press, the program will move on to the next line segment. Continue marking the line segments for all boards until complete. Line segments for each board are to be marked in clock-wise order starting from the top-left.

After marking all the line-segments, the rigid-body transformation between the camera and the LiDAR frame will be displayed.

결과 값들은 `conf/points.txt`에 저장 된다.

###### `conf/points.txt`파일

```
8 # num_of_sensors(2로 고정)*num_of_markers*points_per_board(4개, 코너)

# viewed from the **lidar** origin (3D co-ordinates in meters)
## 4 points represent the 1st board
-0.956904 -0.341421 1.41743
-0.585524 -0.0215598 1.39599
-0.895218 0.348724 1.4129
-1.27033 0.0542636 1.43673
## 4 points represent the 2nd board
-0.118234 -0.381686 1.41175
0.250142 -0.0487747 1.48413
-0.0822171 0.314878 1.50676
-0.425345 -0.00302494 1.44502

# viewed from the camera origin (3D co-ordinates in meters)
## 4 points represent the 1st board
-0.180255 -0.389331 1.59946
0.116911 -0.102827 1.37893
-0.154424 0.277022 1.50679
-0.451589 -0.00948233 1.72732
## 4 points represent the 2nd board
0.59118 -0.439573 1.17482
0.945849 -0.134896 1.15468
0.63982 0.229148 1.27265
0.28515 -0.0755282 1.2928
```

---

## 2. Hector 방식 \(2017\)

[ROS packages related to calibration of robots and subsystems \(cameras, LIDAR, kinematics\)](https://github.com/tu-darmstadt-ros-pkg/hector_calibration)

---

## 3 swyphcosmo의 방법

> 출처 : Camera and LIDAR Calibration and Visualization in ROS: [깃허브](https://github.com/swyphcosmo/ros-camera-lidar-calibration), [youtube](https://www.youtube.com/watch?v=Zc4Ev_ggHIA)

Vagrant제공

---

## 4. but의 방법

> 출처 : Calibration of RGB Camera with Velodyne LiDAR, 2014: [깃허브](https://github.com/robofit/but_velodyne), [ROS\(Hydro\)\_but\_calibration\_camera\_velodyne](http://wiki.ros.org/but_calibration_camera_velodyne)

요구사항

* ROS Hydro
* PCl 1.7 and
* OpenCV 2.x library

---

## The Laser-Camera Calibration Toolbox (2017 or 2006)

- 실행 툴을 이용한 간편한 작업 가능 
- [깃허브-2017](https://github.com/zhixy/Laser-Camera-Calibration-Toolbox), [홈페이지-2006](http://www.cs.cmu.edu/~ranjith/lcct.html)

---

## Extrinsic Calibration of a 3D Lidar and Camera (2017)

- "Automatic Targetless Extrinsic Calibration of a 3D Lidar and Camera by Maximizing Mutual Information." in AAAI, special track on robotics, July 2012, Toronto, Canada.
- [깃허브](https://github.com/anlif/mi-extrinsic-calib), [홈페이지](http://robots.engin.umich.edu/SoftwareData/ExtrinsicCalib)

---

## 0. ~~Kitti Calibration Server 이용 방법~~ 

> [Kitti 칼리브레이션 파일 설명](https://stackoverflow.com/questions/29407474/how-to-understand-kitti-camera-calibration-files): `calib_cam_to_cam.txt`

[사용법](https://github.com/dsqx71/lidar-cam-calibration)
- prepare calibration data
- upload calibration data to kitti calibration server
- download the calibration result
- use this tool and rectify new data.

[현재는 서버 동작 안하는 듯]

