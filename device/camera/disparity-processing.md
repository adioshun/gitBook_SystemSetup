# [ROS stereo_image_proc](http://wiki.ros.org/stereo_image_proc)

## 1. 설치 
- `sudo apt-get install ros-indigo-stereo-image-proc`
- `sudo apt-get install ros-melodic-stereo-image-proc`


## 2. 카메라 설정 

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

각 launch 파일 실행 

### 2.2 camera_calibration 후 image_rect_color를 활용 하므로 

- `$ ~/.ros/camera_info/18060111.yaml & 18060129.yaml`
- `$ ROS_NAMESPACE=stereo/left rosrun image_proc image_proc`
- `$ ROS_NAMESPACE=stereo/right rosrun image_proc image_proc`

> `rostopic echo /stereo/right/camera_info`로 확인 


### 2.2 pointgrey 제공 stereo.launch 실행 

```xml
<launch>
  <!-- Common parameters -->
  <arg name="camera_name" default="stereo" />
  <arg name="frame_rate" default="10" />

  <arg name="left_camera_serial" default="18060129" />
  <arg name="left_camera_calibrated" default="0" />

  <arg name="right_camera_serial" default="18060111" />
  <arg name="right_camera_calibrated" default="0" />
```


### 3. 실행 

```python
$ ROS_NAMESPACE=stereo rosrun stereo_image_proc stereo_image_proc


#시각화 
## rosrun image_view stereo_view stereo:=<stereo namespace> image:=<image topic identifier>
$ rosrun image_view stereo_view stereo:=/stereo image:=image_rect_color _queue_size:=20 _approximate_sync:=True

```

rivz이용 pintcloud2도 확인 가능 


## [Choosing Good Stereo Parameters](http://wiki.ros.org/stereo_image_proc/Tutorials/ChoosingGoodStereoParameters)