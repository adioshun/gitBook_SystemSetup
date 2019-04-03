# [ROS stereo_image_proc](http://wiki.ros.org/stereo_image_proc)

> 이론 설명은 [[요기]](https://eehoeskrap.tistory.com/103)로 

## 1. 설치 
- `sudo apt-get install ros-indigo-stereo-image-proc`
- `sudo apt-get install ros-melodic-stereo-image-proc`


## 2. 실행 

### 2.1 두개의 camera.launch 실행 

```xml
cp camera.launch left_camera.launch
cp camera.launch right_camera.launch
vi left_camera.launch 
"""
   <!-- Determine this using rosrun pointgrey_camera_driver list_cameras.
       If not specified, defaults to first camera found. -->
  <arg name="camera_name" default="stereo/left" />  #stereo namespace필수 
  <arg name="camera_serial" default="18060129" />
  <arg name="calibrated" default="0" />
"""
```

1. 각 launch 파일 실행 

2. image_rect_color를 활용 하므로 
  - `$ ~/.ros/camera_info/18060111.yaml & 18060129.yaml`
  - `$ ROS_NAMESPACE=stereo/left rosrun image_proc image_proc` 
  - `$ ROS_NAMESPACE=stereo/right rosrun image_proc image_proc`
  - `rostopic echo /stereo/right/camera_info`로 확인 
3. `$ ROS_NAMESPACE=stereo rosrun stereo_image_proc stereo_image_proc _approximate_sync:=True`





### 2.2 pointgrey 제공 stereo.launch 실행 (추천)

수정 항목 
- camera_name : camera -> stereo 
- frame_rate : 10 -> 1
- camera_serial


```xml
<launch>
  <!-- Common parameters -->
  <arg name="camera_name" default="stereo" />
  <arg name="frame_rate" default="1" />

  <arg name="left_camera_serial" default="18060129" />
  <arg name="left_camera_calibrated" default="0" />

  <arg name="right_camera_serial" default="18060111" />
  <arg name="right_camera_calibrated" default="0" />
```

1. roslaunch pointgrey_camera_driver stereo.launch
2. `<image_rect_color>` 자동 생성 
3. `<stereo_image_proc>` 자동 실행 with _approximate_sync:=True 



## 3. 시각화 

출력 
- disparity images (stereo/disparity) 
- point clouds (stereo/points2).

### 3.1 Image


```python 
## rosrun image_view stereo_view stereo:=<stereo namespace> image:=<image topic identifier>
$ rosrun image_view stereo_view stereo:=/stereo image:=image_rect_color _queue_size:=20
# `rosnode info /stereo_view_{TAB}` 으로 확인 
```

rivz이용 pintcloud2도 확인 가능 


## [Choosing Good Stereo Parameters](http://wiki.ros.org/stereo_image_proc/Tutorials/ChoosingGoodStereoParameters)

Dynamic Reconfiture 활용 : `rosrun rqt_reconfigure rqt_reconfigure`
- `stereo_image_proc` 선택


### 설정 값 

Disparity pre-filtering
- _prefilter_size (int, default: 9) : Normalization window size, pixels.
- _prefilter_cap (int, default: 31) : Bound on normalized pixel values.

Disparity correlation
- ~correlation_window_size (int, default: 15) : Edge size (pixels) of the correlation window for matching. Values must be odd, in the range 5 to 255 (but with an extra performance hit for values larger than 21). Larger values have smoother disparity results, but smear out small features and depth discontinuities.
_ ~min_disparity (int, default: 0) : Minimum disparity, or the offset to the disparity search window. By setting to a positive value, the cameras become more "cross-eyed" and will find objects closer to the cameras. If the cameras are "verged" (inclined toward each other), it may be appropriate to set min_disparity to a negative value. When min_disparity is greater than 0, objects at large distances will not be found.
- ~disparity_range (int, default: 64) : The size of the disparity search window (pixels). Together with min_disparity, this defines the "horopter," the 3D volume that is visible to the stereo algorithm.

Disparity post-filtering
- ~uniqueness_ratio (double, default: 15.0) : Filters disparity readings based on a comparison to the next-best correlation along the epipolar line. Matches where uniqueness_ratio > (best_match - next_match) / next_match are filtered out.
- ~texture_threshold (int, default: 10) : Filters disparity readings based on the amount of texture in the correlation window.
- ~speckle_size (int, default: 100) : Filters disparity regions that are less than this number of pixels. For example, a value of 100 means that all regions with fewer than 100 pixels will be rejected.
- ~speckle_range (int, default: 4) : Groups disparity regions based on their connectedness. Disparities are grouped together in the same region if they are within this distance in pixels.
```

## 설정 예시 


What's this junk on the tabletop?
|![](https://i.imgur.com/KVTKhlV.png)|![](https://i.imgur.com/cZpBbpR.png)|


|-|-|

What's this junk on the tabletop?
|

speckle filter controlled by 
- `speckle_size` is the number of pixels below which a disparity blob is dismissed as "speckle." 
- `speckle_range` controls how close in value disparities must be to be considered part of the same blob. 
- eg. In this case the objects in the scene are relatively large, so we can crank speckle_size up to 1000 pixels:
|![](https://i.imgur.com/KVTKhlV.png)|![](https://i.imgur.com/cZpBbpR.png)|
|-|-|

|![](https://i.imgur.com/KVTKhlV.png)|![](https://i.imgur.com/cZpBbpR.png)|
|-|-|



But where are the table and object?

