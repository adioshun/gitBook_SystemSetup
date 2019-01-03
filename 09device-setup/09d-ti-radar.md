1. 롬(??) 업데이트
- 업데이터 툴 설치 : [다운로드](http://processors.wiki.ti.com/index.php/Category:CCS_UniFlash)
- bin 다운로드 : [sdk 설치시](http://www.ti.com/tool/MMWAVE-SDK) 자동 다운로드 
- 설명서 : [UniFlash v4 User Guide for mmWave Devices](http://processors.wiki.ti.com/images/f/f5/Mmwave_uniflash_user_guide_v1.0.pdf)
---


[TI 포럼](https://e2e.ti.com/support/sensors/f/1023)

[mmWave SDK EVM Out-of-Box Demo](https://training.ti.com/mmwave-sdk-evm-out-box-demo)

[mmWave software development kit (SDK)](http://www.ti.com/tool/MMWAVE-SDK)
- 다운로드(Linus) : http://software-dl.ti.com/ra-processors/esd/MMWAVE-SDK/lts-latest/exports/mmwave_sdk_02_01_00_04-Linux-x86-Install.bin
- chmod +x *.bin
- [User Guide](file:///home/adioshun/Downloads/mmwave_sdk_user_guide.pdf)

[웹앱 갤러리](http://dev.ti.com/gallery/)
- [mmWave Demo Visualizer 메뉴얼 ](http://www.ti.com/lit/ug/swru529b/swru529b.pdf): pdf, 
- [mmWave Demo Visualizer 웹앱](https://dev.ti.com/gallery/view/mmwave/mmWave_Demo_Visualizer/ver/3.1.0/)

[DCA1000EVM Data Capture Card](http://www.ti.com/lit/ug/spruij4/spruij4.pdf): pdf

[TI mmWave ROS Driver Setup Guide](http://dev.ti.com/tirex/content/mmwave_training_1_6_1/labs/lab0006-ros-driver/lab0006_ros_driver_pjt/TI_mmWave_ROS_Driver_Setup_Guide.pdf) : pdf, 
- DCA1000EVM(http://www.ti.com/tool/DCA1000EVM)은 TI mmWave EVM과 연결하여 Raw ADC 데이터 캡쳐를 할 수 있게 하는 인터페이스 보드입니다.

[ROS driver for TI mmwave sensor boards](https://github.com/ibcn-cloudlet/ti_mmwave_rospkg) : 깃허브 

[TI mmWave ROS Package (Customized)](https://github.com/radar-lab/ti_mmwave_rospkg): 깃허브 

[TI Resouce Explorer](http://dev.ti.com/tirex/#/)


- [Traffic Monitoring Overview](http://dev.ti.com/tirex/#/?link=Software%2FmmWave%20Sensors%2FIndustrial%20Toolbox%2FLabs%2FTraffic%20Monitoring)

Configuration (.cfg) File Format : [SDK 메뉴얼](file:///home/adioshun/Downloads/mmwave_sdk_user_guide.pdf) 14page

---

# Code Composer Studio(CCS)

Download the latest CCS : http://processors.wiki.ti.com/index.php/Download_CCS


---

# mmWaveLib
- [SDK 메뉴얼](file:///home/adioshun/Downloads/mmwave_sdk_user_guide.pdf) 50page
- mmWaveLib is a collection of algorithms that provide basic functionality needed for FMCW radar-cube processing. 
- This component is available for **xWR16xx** only and contains optimized library routines for C674 DSP architecture only. 




---

- ttyS0 is the device for the first UART serial port on x86 and x86_64 architectures. If you have a PC motherboard with serial ports you'd be using a ttySn to attach a modem or a serial console.
- ttyUSB0 is the device for the first USB serial convertor. If you have an USB serial cable you'd be using a ttyUSBn to connect to the serial port of a router.
- ttyAMA0 is the device for the first serial port on ARM architecture. If you have an ARM-based TV box with a serial console and running Android or OpenELEC, you'd be using a ttyAMAn to attach a console to it.