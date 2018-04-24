## 2. PointGrey

BFLY-PGE-31S4C-C

### 2.1 Installation

> ref :[Getting Started with FlyCapture 2.x and Linux](https://www.ptgrey.com/KB/10548)

1. Download : flycapture2-2.9.3.43-amd64-pkg.tgz [\[Link\]](https://www.ptgrey.com/support/downloads)

2. Dependent Package

   * Ubuntu 16.04: `sudo apt-get install libraw1394-11 libgtkmm-2.4-1v5 libglademm-2.4-1v5 libgtkglextmm-x11-1.2-dev libgtkglextmm-x11-1.2 libusb-1.0-0`
   * Ubuntu 14.04: `sudo apt-get install libraw1394-11 libgtkmm-2.4-1c2a libglademm-2.4-1c2a libgtkglextmm-x11-1.2-dev libgtkglextmm-x11-1.2 libusb-1.0-0 libglademm-2.4-dev`
   * Ubuntu 12.04: `sudo apt-get install libraw1394-11 libgtk2.0-0 libgtkmm-2.4-dev libglademm-2.4-dev libgtkglextmm-x11-1.2-dev libusb-1.0-0 libglademm-2.4-dev`

3. raw1394  
   After installing raw1394 you may need to add the raw1394 module to be loaded on start.  
   To do this just add "raw1394" to the `/etc/modules` file.

4. `sudo sh install_flycapture.sh`

5. Validation

   ```
   cp -R /usr/src/flycapture ~
   cd ~/flycapture/src/FlyCapture2Test
   make
   FlyCapture2Test
   ```

1. Run 
   * Applications -&gt; Point Grey Research -&gt; FlyCapture
   * `$ FlyCap2`

> [Getting Started with FlyCapture 2.x and Linux](https://www.ptgrey.com/tan/10548)

### 2.2 PointGrey for Python

pip install pyflycap2

> [PyFlyCap2](https://matham.github.io/pyflycap2/index.html)
>
> error : Failed to start with error: PointGreyCamera::start Failed to start capture \| FlyCapture2::ErrorType 33 Error starting isochronous stream. --&gt; [reducing the frame rate](https://stackoverflow.com/questions/12070778/trouble-in-driving-point-grey-grasshoper-cameras)
>
> error : IMAGE\_CONSISTENCY\_ERRORS -&gt;  [sudo sysctl -w net.core.rmem\_max=1048576 net.core.rmem\_default=1048576](http://www.ptgrey.com/KB/10016), Packet size 중간, Packet delay 높게, Jumbo packet\(MTU\) 최대

### 2.4 PointGrey for ROS

#### A. Apt install

apt-get install ros-indigo-pointgrey-camera-driver

#### B. Source install

[pointgrey\_camera\_driver](http://wiki.ros.org/pointgrey_camera_driver)

```
# INDIGO VERSION
sudo apt-get install ros-indigo-pointgrey-camera-driver
cd ~/catkin_ws/src/ && git clone https://github.com/ros-drivers/pointgrey_camera_driver.git
cd pointgrey_camera_driver
rosdep install --from-paths ./ --ignore-src --rosdistro indigo -y
cd ~/catkin_ws/ && catkin_make
```

List detected cameras:`rosrun pointgrey_camera_driver list_cameras`

Launch camera: `roslaunch pointgrey_camera_driver camera.launch`

### 2.3 Calibration

#### A. 패키지 설치 및 최초 설정값 확인

```
$ sudo apt-get install ros-indigo-camera-calibration
  # rosdep install camera_calibration
  # rosmake camera_calibration
$ rosrun uvc_camera uvc_camera_node
$ rostopic echo /camera_info  #칼리브레이션 정보가 없으므로 0으로 표기
```

#### B. 칼리브레이션 수행

1m 크기 \[체스 보드\]\([http://library.isr.ist.utl.pt/docs/roswiki/attachments/camera\_calibration\(2f\)Tutorials\(2f\)MonocularCalibration/check-108.pdf](http://library.isr.ist.utl.pt/docs/roswiki/attachments/camera_calibration%282f%29Tutorials%282f%29MonocularCalibration/check-108.pdf)\) 준비 : 8x6의 체스 보드\(가로 9개 네모, 교차점 8개, 세로로 7개 세모, 교차점 6개\)

```
$ rosrun camera_calibration cameracalibrator.py \
--size 8x6 \ #체스보드 가로,세로 크기
--square 0.108 \ #체스보드 네모의 실체 크기 
image:=/my_camera/image \ # 
camera:=/my_camera
```

!\[\]\([http://library.isr.ist.utl.pt/docs/roswiki/attachments/camera\_calibration\(2f\)Tutorials\(2f\)MonocularCalibration/mono\_0.png](http://library.isr.ist.utl.pt/docs/roswiki/attachments/camera_calibration%282f%29Tutorials%282f%29MonocularCalibration/mono_0.png)\)  
위 GUI 상에서 다은 [Youtube](https://www.youtube.com/watch?v=yAYqt3RpT6c)처럼 칼리브레이션 작업 수행하여 `CALIBRATE`버튼 활성화 시 `SAVE`버튼 클릭

* /tmp/calibarationdata.tar.gz\`에 보통 저장 

#### C. ROS용 카메라 파라미터 파일\(\*.yaml\)로 변환

```
cd /tmp; tar zvfx calibarationdata.tar.gz
mv ost.txt ost.ini (??)
rosrun camera_calibration_parsers convert ost.ini camera.yaml
mv camera.yaml ~/.ros/camera_info/camera.yaml
```

#### D. 실행

```
rosrun uvc_camera uvc_camera_node
rostopic echo /camera_info  #칼리브레이션 정보 D,K,R,P 확인
```

> 참고 : \[ROS 로봇 프로그래밍, 표윤석, 217p\], [\[How to Calibrate a Monocular Camera\]](http://wiki.ros.org/camera_calibration/Tutorials/MonocularCalibration), [\[ROS\_camera\_calibration\]](http://wiki.ros.org/camera_calibration)

[http://wiki.ros.org/camera\_calibration/Tutorials/MonocularCalibration](http://wiki.ros.org/camera_calibration/Tutorials/MonocularCalibration)

---

## web-cam

uvc\(USB Video device Class\) package install

```
apt-get install ros-indigo-uvc-camera
```

Image relative package install

```
apt-get install ros-indigo-image-*
apt-get install ros-indigo-rqt-image-view
```

RUN

```
roscore
rosrun uvc_camera uvc_camera_node
```

VIEW

```
rosrun image_view image_view image:=/image_raw
rqt_image_view image:=/image_raw
rviz -> image -> /image_raw
```



