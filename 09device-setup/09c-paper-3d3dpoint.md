|논문명/저자/소속|LiDAR-Camera Calibration using 3D-3D Point correspondences|
|-|-|
|저자(소속)| Ankit Dhall |
|학회/년도| arXiv2017, [논문](https://arxiv.org/pdf/1705.09785.pdf)|
|키워드| VLP-16, Point Gray Blackfly, ZED stereo camera|
|데이터셋/모델||
|참고|[저자홈페이지](https://ankitdhall.github.io/), [ROS Package](http://wiki.ros.org/lidar_camera_calibration)|
|코드|[깃허브](), [tutorial.ytb](https://youtu.be/SiPGPwNKE-Q), [comparison.ytb](https://youtu.be/AbjRDtHLdz0), [Stereo_demo.ytb](https://youtu.be/Om1SFPAZ5Lc) |

# Lidar - Camera 3D-3D

We propose a novel pipeline and experimental setup to find accurate `rigid-body transformation` for extrinsically calibrating a LiDAR and a camera.

> 강체변환(Rigid Body Transformation)은 이동변환과, 회전변환으로만 나타낸 것으로 물체 자체의 모습은 불변한다. [[출처]](http://x21999.blog.me/220514294651)


The pipeling uses 3D-3D point correspondences in LiDAR and camera frame and gives a closed form solution.

We further show the accuracy of the estimate by fusing point clouds from two stereo cameras which align perfectly with the rotation and translation estimated by our method, confirming the accuracy of our method’s estimatesboth mathematically and visually.

## 1 Introduction

[2]의 방식도 있지만 해당 방식은 고사양 Lidar를 기반으로 하여 저사양(eg. 16채널)에는 적당하지 않다. `Marker based[2] as well as automatic calibration for LiDAR and cameras has been proposed but methods and experiments discussed in these use the high-density, more expensive LiDAR and do not extend very well when a lower-density LiDAR, such as the VLP-16 is used.`

```
2. Martin Velas, Michal Spanel, Zdenek Materna, Adam Herout Calibration of RGB Camera With Velodyne LiDAR. https://www.github.com/robofit/but velodyne/2, 8
```

We propose a very accurate and repeatable method to estimate extrinsic calibration parameters in the form of 6 degrees-of-freedom between a camera and a LiDAR.

> 6 degrees-of-freedom : 모든 동작 요소. 즉, X(수평), Y(수직), Z(깊이), 피치(pitch), 요(yaw), 롤(roll)을 말한다.
> 3자유도(3DOF)는 X, Y, Z만을 말한다.

## 2 Sensors and General Setup

필요 선행 작업 : The `intrinsic parameters of the camera` should be known before starting the LiDAR-camera calibration process.

카메라는 전면만 센신하고, Lidar는 360도를 센싱한다. 매순간 데이터가 수집될때 두 센서는 서로 임의의 간격을 두고 있다. `Each time data was collected, the LiDAR and camera were kept at arbitrary(임의의) distance in 3D space.`

두 센서간의 `transformation`는 manual하게 측정한다. `The transformation between them was measured manually. `

비록 테이프 측정방식이 조잡해 보이지만 여러 알고리즘으로 얻은 값들을 sanity check할수 있게 한다. `Although, the tape measurement is crude, it serves as a sanity check for values obtained using various algorithms. `

`translation`을 측정하는것은 `회전`을 측정하는것 보다 쉽다. `Measuring translation is easier than rotation. `

회전이 작으면 0으로 가적하면 된다. 반대로 회적이 크면 거리를 측정하여 삼각법으로 앵글을 유추 한다. `When the rotations were minimal, we assumed them to be zero, in other instances, when there was considerable rotation in the orientation of the sensors, we measured distances and estimated the angles roughly using trigonometry.`

> 언급된 파라미터 : transformation, translation, rotation

## 3 Using 2D-3D correspondences

제안 방식인 3D-3D을 살펴 보기 전에 2D-3D방식을 살펴 보겠다.

이를 위해서는 속이 빈 사각형이 필요 하다. `The setup involves markers of a specific type: hollow rectangular cardboards`

![](https://i.imgur.com/v1g3tQf.png)
```
Fig. 2. Experimental setup with rectangular cardboard cutouts using 2D-3D correspondences.
```

이 방식을 이용하면 2D-3D correspondences(대응?)를 매칭 하는 방식을 이용하여 6-DoF를 찾을수 있다. `This method involves finding the 6-DoF between the camera and the LiDAR by the means of matching 2D-3D point correspondences.`

2D correspondences은 이미지의 특징점에 마킹을 함드로써 쉽게 얻을수 있다. `2D correspondences can be easily obtained by manually marking feature points in an image with an accuracy of 3-4 pixels. `

대응하는 3D point를 얻는것은 위 방법으로 되지 않는다. `Obtaining corresponding 3D points is not that straightforward.`
  - 원인 : For one reason LiDARs does not give a high density point cloud and with increasing distance (away from the LiDAR center) the point cloud becomes more and more sparse.

평면 보드는 4개의 점 즉,4 point correspondences를 가지고 있다. `A planar cardboard can provide 4 corner points i.e. 4 point correspondences.`
 - In 3D these points are obtained by line-fitting(직선 조정) followed by line-intersection(교차선) and their 2D correspondences can be obtained by marking pixel co-ordinates.

뚤린 보드를 사용하면 **8 3D-2D point correspondences** 를 제공한다. `If a hollowed out rectangular cardboard is used, it provides 8 3D-2D point correspondences`:
- 4 corners on the outer rectangle and
- 4 corners on the inner rectangle;

2배 많은 correspondences로 인해 더 좋다. `doubling the correspondences, allowing for more data points with lesser number of boards. `

Such a setup allows to have enough data to run a **RaNSaC** version of **PnP** algorithms and also will help reduce noisy data, in general.

We use rectangular (planar cardboard) markers.

- 문제 : If in the experimental setup,the markers are kept with one of their sides parallel to the ground, due to the horizontal nature of the LiDAR’s scan lines one can obtain the vertical edges, but not necessarily the horizontal ones.

- 해결법 : To overcome this, we tilt the board to make approximately 45 degrees between one of the edges and the ground plane.

- 산출물 : With such a setup we always obtain points on all four edges of the board.

RanSaC을 이용하여 Lidar의 point들 직선 조정을 하였다. `RanSaC is used to fit lines on the points from the LiDAR.`

![](https://i.imgur.com/zlCQxup.png)
```
Fig. 3. Marking line segments in the 3D-pointcloud.
The ROS node allows for manually marking segments by drawing polygons around each line segment and also calculate their intersections.
```

가장 중요한 특징은 **코너** 이다. `The most prominent feature on the marker is the corner.`

코너는 찾기도 쉽고 Line공식을 이용하여 3D 교차점 계산이 가능하다. `It can be marked with relative easy on the image and since we have quite accurate line equations for the four edges, their intersection is calculated in 3D.`

다시 말하지만, 이 선들이 정확히 교차 하진 않겠지만 거의 비슷 하다. `Again, these lines may not actually intersect, but come very close.`

We approximate the corner to the midpoint on the shortest line-segment between the two lines.

As, a check, that this point is indeed a very close approximation to the actual corner, we calculated the length of the shortest line-segment.

Also, since we know the dimensions of the cardboard marker, the length of the opposite sides should be very close to each other and also to the actual length measure by tape.

We consistently observed that the distance between two line-segments was of the order 10^{−4} meters and the error between the edge lengths was about 1centimeter on average.

Collecting data over multiple experiments, we observed that the edges are extremely close and the corner and intersection are at most off by 0.68 mm onaverage.

An average absolute deviation of 1cm is observed between the expected and estimated edge lengths of the cardboard markers.

With the above two observations one can conclude that the intersections are indeed a very accurate approximation of the corner in 3D.

Using hollowed out markers, we obtained 20 corner points:
- 2 hollow rectangular markers (8+8 points) and
- one solid rectangular marker (4 points) increasing the number of point correspondences from our initial experiments.

PnP를 이용하여서 2D-3D correspondences 셋들 사이에서 rigid-body transformation를 찾을수 있다. `Perspective n-Point (PnP) finds the rigid-body transformation between a set of 2D-3D correspondences. `

Equation 1 shows how the 3D points are projected after applying the [R|t] which is estimated by PnP.

Equation 2 represents the general cost-function to solve such a problem.


> 중간 생략

## 4 Using 3D-3D correspondences

2D-3D correspondences방법은 본 논문의 실험에서 좋은 결과가 나오지 않았다. `The 2D-3D correspondences method did not seem to work very well in our experimental setup. `

부정확한 2D 포인트 마킹 또는 PnP를 수행시 노이즈 데이터로 인해 에러율을 조금씩 올랐다. `Error could have crept up(서서히 오르다) due to not-so-accurate marking of 2D points (in pixels by looking at the image) or noisy data points to perform PnP.`

back-projection에러는 최소화 되었지만 transformation값은 테이프로 측정된 값과 비슷하지 않았따. `The back-projection error seemed to get minimized, however, the transformation values did not seem to be in agreement with the values measured by tape.`

Setups being used in real-time require extrinsic calibration to be quite accurate and produce minimal error.

퓨전은 외부 칼리브레이션 파라미터의 정확도를 시각화 하는 방법 중 하나이다. `Fusion is one way to visualize the accuracy of the extrinsic calibration parameters. `

Bad calibration can result in fused data to have hallucinations in the form of duplication of objects in the fused point
clouds due to bad alignment.

One such application requiring real-time fusion from multiple sensors is autonomous driving.

Bad calibration can result in erroneous fused data, which can be fatal for the car as well as nearby cars, pedestrians and property.


This part of involves using augmented-reality (AR) tags and the LiDAR point cloud to find the extrinsic calibration parameters.
- Multiple versions of AR tags have been released by the open-source community [7] [5].
- The method proposed here uses the ArUco tags [5].



```
5. ArUco Markers. https://github.com/SmartRoboticSystems/aruco mapping http://docs.opencv.org/3.1.0/d5/dae/tutorial aruco detection.html 8, 9
7. April Tags. https://april.eecs.umich.edu/wiki/AprilTags 8
```

To find the transformation between the camera and Velodyne, we need two sets of 3D points:
- one in the camera frame and
- another in the Velodyne frame.

Once, these point correspondences are found, one can try to solve for [R|t] between the two sensors.

### 4.1 Experimental Setup

![](https://i.imgur.com/ANF2pFw.png)
```
Fig. 6. Experimental setup with rectangular cardboard and ArUco markers using 3D-3D correspondences.
```

보드 크기 : 45-55cm `The boards used in the experiments had length/breadth ranging between 45.0-55.0 centimeters.`

Lidar 거리 : 2m `Keeping the LiDAR about 2.0 meters away from board with these dimensions`



---

# 칼리브레이션

## 1. 사전 준비

## 2. 파일 설정

### 2.1  `lidar_camera_calibration.yaml` 파일

참고 해야 하는 카메라와 라이다 TOPIC명시 `Contains name of camera and velodyne topics that the node will subscribe to.`

```
lidar_camera_calibration:
camera_frame_topic: /frontNear/left/image_raw   # [중요] 반드시 변경
camera_info_topic: /frontNear/left/camera_info  # [중요] 반드시 변경
velodyne_topic: /velodyne_points
```


> `lidar_camera_calibration/launch/find_transform.launch` 을이용하여 값 찾을수 있음

### 2.2  `config_file.txt` 파일

```
1280 720 # image_width image_height
-2.5 2.5 # x- x+, 불필요한 포인트 제거를 위한 필터,  mark the board edges를 쉽게 함
-4.0 4.0 # y- y+, 불필요한 포인트 제거를 위한 필터,  mark the board edges를 쉽게 함
0.0 2.5 # z- z+, 불필요한 포인트 제거를 위한 필터,  mark the board edges를 쉽게 함
0.05 # cloud_intensity_threshold # Intensity가 낮는 포인트 제거를 위한 필터의 기준  
2 # number_of_markers
0 # use_camera_info_topic? # `camera_info topic`을 사용하려면 `1` / '0'은 `config_file.txt`의 값 사용
611.651245 0.0 642.388357 0.0 # fx 0 cx 0
0.0 688.443726 365.971718 0.0 # 0 fy cy 0
0.0 0.0 1.0 0.0 # MAX_ITERS
                # After MAX_ITERS, the node outputs an average translation vector (3x1) and an average rotation matrix (3x3).
                # Averaging the translation vector is trivial;
                # the rotations matrices are converted to quaternions and averaged,
                # then converted back to a 3x3 rotation matrix
1.57 -1.57 0.0  # initial_rot_x initial_rot_y initial_rot_z
                # 카메라대비 Lidar의 초기 회전 여부, Initial orientation of the lidar with respect to the camera, in radians.
                # The default values are for the case when both the lidar and the camera are both pointing forward.
                # The final transformation that is estimated by the package accounts for this initial rotation.
```

### 2.3 `marker_coordinates.txt` 파일


The ArUco markers are stuck on the board such that when it is hung from a corner, the ArUco marker is on the left side of the board.


Notice how the axis are aligned.

y-axis should point outwards, x-axis along the breadth (s2) and z-axis along the length (s1).

The markers are also arranged so that the ArUco id are in ascending order. (마커의 ArUco ids는 오름차순으로 있어야 한다.)




```
# cm 단위로 표기
2     # 'N' the number of boards
48.4  # length (s1)
46.8  # breadth (s2)
4.0   # border_width_along_length (b1)
5.0   # border_width_along_breadth (b2)
20.5  # edge_length_of_ArUco_marker (e)
49.0  # s1
46.8  # s2
4.0   # b1
5.0   # b2
20.5  # e
```
![](https://github.com/ankitdhall/lidar_camera_calibration/raw/master/images/board_dim_label.jpg)

![](https://github.com/ankitdhall/lidar_camera_calibration/raw/master/images/aruco_axis.png)



###### `find_transform.launch` 파일

`aruco_mapping`노드에게 필요한 정보들 `Parameters are required for the aruco_mapping node and need to be specfied here.`
- Ensure that the topics are mapped correctly for the node to function. Other parameters required are:
  - calibration_file(.ini format)
  - num_of_markers
  - marker_size(in meters)
```
<?xml version="1.0"?>
<launch>
  <!-- <param name="/use_sim_time" value="true"/> -->


  <!-- ArUco mapping -->
 <!--  <node pkg="aruco_mapping" type="aruco_mapping" name="aruco_mapping" output="screen">
    <remap from="/image_raw" to="/frontNear/left/image_raw"/>

    <param name="calibration_file" type="string" value="$(find aruco_mapping)/data/zed_left_uurmi.ini" />  # 칼리브레이션 파일(ini)
    <param name="num_of_markers" type="int" value="2" />  # 마커 갯수
    <param name="marker_size" type="double" value="0.205"/> # 마커 크기
    <param name="space_type" type="string" value="plane" />
    <param name="roi_allowed" type="bool" value="false" />


  </node>  -->


  <rosparam command="load" file="$(find lidar_camera_calibration)/conf/lidar_camera_calibration.yaml" />
  <node pkg="lidar_camera_calibration" type="find_transform" name="find_transform" output="screen">
  </node>
</launch>
```

## 3. 실행

> 동영상 설명 : [Youtube](https://youtu.be/SiPGPwNKE-Q)

```
roslaunch lidar_camera_calibration find_transform.launch
```

An initial [R|t] between the camera and the various ArUco markers will be estimated. Following this, a filtered point cloud (according to the specifications in the config_file.txt) will be displayed. The user needs to mark each edge of the rectangular board.

Each board will have 4 line segments and need to be marked from leftmost board to the rightmost board. Marking a line segment is quite straight-forward, one needs to draw a quadrilateral around the line being marked. Click and press a key to confirm the corner of the quadrilateral. Once 4 points are clicked, each followed by a key-press, the program will move on to the next line segment. Continue marking the line segments for all boards until complete. Line segments for each board are to be marked in clock-wise order starting from the top-left.

After marking all the line-segments, the rigid-body transformation between the camera and the LiDAR frame will be displayed.

결과 값들은 `conf/points.txt`에 저장 된다.

###### `conf/points.txt`파일


```
8  # num_of_sensors(2로 고정)*num_of_markers*points_per_board(4개, 코너)

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
