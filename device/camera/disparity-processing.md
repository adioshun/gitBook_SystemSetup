# [ROS stereo_image_proc](http://wiki.ros.org/stereo_image_proc)

## 1. 설치 
- `sudo apt-get install ros-indigo-stereo-image-proc`
- `sudo apt-get install ros-melodic-stereo-image-proc`


## 2. 카메라 설정 

### 2.1 두개의 camera.launch 실행 

```python 
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


### 2.2 pointgrey 제공 stereo.launch 실행 


### 3. 실행 

```
$ ROS_NAMESPACE=stereo rosrun stereo_image_proc stereo_image_proc

#시각화 
$ rosrun image_view image_view image:=/stereo/left/image_rect_color
$ rosrun image_view stereo_view stereo:=/stereo image:=image_rect_color
```

rivz이용 pintcloud2도 확인 가능 


## [Choosing Good Stereo Parameters](http://wiki.ros.org/stereo_image_proc/Tutorials/ChoosingGoodStereoParameters)