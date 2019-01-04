# TI mmwave RADAR 

## 초기 설치 (윈도우 권장)

> DCA1000EVM대신 IWR 1642에 직접 연결 후 작업, [[Ref Tutorial]](https://training.ti.com/mmwave-sdk-evm-out-box-demo) 

1. usb 드라이버 다운로드 : [IWR1642: stand alone XDS110 drivers](https://downloads.ti.com/downloads/dsps/dsps_public_sw/sdo_ccstudio/emulation/ti_emupack_setup_8.0.903.4_win_32.exe?__gda__=1547020113_3fbdeab41d0759de936f41c4cc3e13e7)

    

2. 롬(??) 업데이트 [[UniFlash v4 User Guide for mmWave Devices]](http://processors.wiki.ti.com/images/f/f5/Mmwave_uniflash_user_guide_v1.0.pdf)

    - 업데이터 툴 설치 : [다운로드](http://processors.wiki.ti.com/index.php/Category:CCS_UniFlash)
    -  xwr16xx_mmw_demo.bin 다운로드 : [sdk 설치시](http://www.ti.com/tool/MMWAVE-SDK) 자동 다운로드 (`C:\ti\mmwave_sdk_02_01_00_04\packages\ti\demo\xwr16xx\mmw`)
    - 점퍼 설정 : SOP2 연결 -> 재부팅 
    - 툴 실행 
    - 윈도우 장치 관리자 에서 포트 번호 확인 후 `XDS110 Class Application/User UART`를 설정에 입력
    - 점퍼 원복 
3. 확인 : [mmWave Demo Visualizer](https://dev.ti.com/gallery/view/mmwave/mmWave_Demo_Visualizer/ver/3.1.0/), [[메뉴얼 ]](http://www.ti.com/lit/ug/swru529b/swru529b.pdf)



## ROS 연계 (ubuntu)

> [TI mmWave ROS Driver Setup Guide: [pdf](http://dev.ti.com/tirex/content/mmwave_training_1_6_1/labs/lab0006-ros-driver/lab0006_ros_driver_pjt/TI_mmWave_ROS_Driver_Setup_Guide.pdf) , [깃허브](https://github.com/ibcn-cloudlet/ti_mmwave_rospkg), [깃허브-개선버젼](https://github.com/radar-lab/ti_mmwave_rospkg) 

```python 
sudo apt-get install ros-kinetic-pcl-ros ros-kinetic-pcl-msgs ros-kinetic-pcl-conversions ros-kinetic-perception-pcl
cd ~/catkin_ws/src
git clone https://github.com/radar-lab/ti_mmwave_rospkg.git
git clone https://github.com/wjwwood/serial.git
cd ~/catkin_ws
catkin_make

sudo chmod 666 /dev/ttyACM0
sudo chmod 666 /dev/ttyACM1

roslaunch ti_mmwave_rospkg 1642es2_short_range.launch

rostopic echo /ti_mmwave/radar_scan
```

> 에러 발생시 참고 : https://github.com/radar-lab/ti_mmwave_rospkg#troubleshooting

---

### DCA1000EVM
- DCA1000EVM(http://www.ti.com/tool/DCA1000EVM)은 TI mmWave EVM과 연결하여 Raw ADC 데이터 캡쳐를 할 수 있게 하는 인터페이스 보드입니다.
- [홈페이지](http://www.ti.com/tool/dca1000evm)
- [Quick Start Guide](http://www.ti.com/lit/ml/spruik7/spruik7.pdf)
- [User guide](http://www.ti.com/lit/ug/spruij4/spruij4.pdf)
- [드라이버/mmWave studio](https://downloads.ti.com/downloads/ra-processors/esd/MMWAVE-STUDIO/latest/mmwave_studio_02_00_00_02_win32.exe?__gda__=1547022129_a6064df5c19ef3383b7510adfe062512)



---



## [TI Resouce Explorer](http://dev.ti.com/tirex/#/)
- [Traffic Monitoring Overview](http://dev.ti.com/tirex/#/?link=Software%2FmmWave%20Sensors%2FIndustrial%20Toolbox%2FLabs%2FTraffic%20Monitoring)
- [ROS Point Cloud Visualizer](http://dev.ti.com/tirex/#/?link=Software%2FmmWave%20Sensors%2FIndustrial%20Toolbox%2FLabs%2FROS%20Point%20Cloud%20Visualizer)
- People Counting Demo
- Zone Occupancy Detection 
- Gesture Recognition 

> 사전 설치 : [Matlab runtime_R2017a (9.2)](https://kr.mathworks.com/products/compiler/matlab-runtime.html)

Configuration (.cfg) File Format : [SDK 메뉴얼](file:///home/adioshun/Downloads/mmwave_sdk_user_guide.pdf) 14page


---
## [mmWave software development kit (SDK)](http://www.ti.com/tool/MMWAVE-SDK)
- 다운로드(Linus) : http://software-dl.ti.com/ra-processors/esd/MMWAVE-SDK/lts-latest/exports/mmwave_sdk_02_01_00_04-Linux-x86-Install.bin
- chmod +x *.bin
- [User Guide](https://e2e.ti.com/cfs-file/__key/communityserver-discussions-components-files/1023/mmwave_5F00_sdk_5F00_user_5F00_guide-_2800_1_2900_.pdf)




---

# Code Composer Studio(CCS)

Download the latest CCS : http://processors.wiki.ti.com/index.php/Download_CCS


---

# mmWaveLib
- [SDK 메뉴얼](file:///home/adioshun/Downloads/mmwave_sdk_user_guide.pdf) 50page
- mmWaveLib is a collection of algorithms that provide basic functionality needed for FMCW radar-cube processing. 
- This component is available for **xWR16xx** only and contains optimized library routines for C674 DSP architecture only. 




---

- [TI 포럼](https://e2e.ti.com/support/sensors/f/1023)

- ttyS0 is the device for the first UART serial port on x86 and x86_64 architectures. If you have a PC motherboard with serial ports you'd be using a ttySn to attach a modem or a serial console.
- ttyUSB0 is the device for the first USB serial convertor. If you have an USB serial cable you'd be using a ttyUSBn to connect to the serial port of a router.
- ttyAMA0 is the device for the first serial port on ARM architecture. If you have an ARM-based TV box with a serial console and running Android or OpenELEC, you'd be using a ttyAMAn to attach a console to it.