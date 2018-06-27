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

rosrun camera_calibration cameracalibrator.py --size 8x6 --square 0.025 image:=/cameara/image_raw camera:=/camera --no-service-check
```

![](http://library.isr.ist.utl.pt/docs/roswiki/attachments/camera_calibration%282f%29Tutorials%282f%29MonocularCalibration/mono_0.png)

위 GUI 상에서 다음 [Youtube](https://www.youtube.com/watch?v=yAYqt3RpT6c)처럼 칼리브레이션 작업 수행하여 `CALIBRATE`버튼 활성화 시 `SAVE`버튼 클릭

칼리브레이션 파일은 `/tmp/calibarationdata.tar.gz\`에 보통 저장 & `~/.ros/camera_info/camera.yaml`

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



