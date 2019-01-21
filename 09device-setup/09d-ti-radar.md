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


## Uniflash 설치 

> 펌웨어(??) 설치 툴 

- linux : 'sudo apt-get install libusb-dev' -> `sudo ./uniflash_sl.4.5.0.2056.run`
- windows : 
- Webapp : [Uniflash](https://dev.ti.com/uniflash/#!/)


```
    <param name="command_port" value="/dev/ttyACM0"  />
    <param name="command_rate" value="115200"   />
    <param name="data_port" value="/dev/ttyACM1"  />
    <param name="data_rate" value="921600"   />

```

---



## [TI Resouce Explorer](http://dev.ti.com/tirex/#/)
- [Traffic Monitoring Overview](http://dev.ti.com/tirex/#/?link=Software%2FmmWave%20Sensors%2FIndustrial%20Toolbox%2FLabs%2FTraffic%20Monitoring)
- [ROS Point Cloud Visualizer](http://dev.ti.com/tirex/#/?link=Software%2FmmWave%20Sensors%2FIndustrial%20Toolbox%2FLabs%2FROS%20Point%20Cloud%20Visualizer)
- People Counting Demo : [설명자료](https://training.ti.com/kr/mmwave-sensors-people-counting-demo-english?cu=1128486)
- Zone Occupancy Detection 
- Gesture Recognition 

> 사전 설치 : [Matlab runtime_R2017a (9.2)](https://kr.mathworks.com/products/compiler/matlab-runtime.html)



---
## ROS 연계 (ubuntu)

> [TI mmWave ROS Driver Setup Guide: [pdf](http://dev.ti.com/tirex/content/mmwave_training_1_6_1/labs/lab0006-ros-driver/lab0006_ros_driver_pjt/TI_mmWave_ROS_Driver_Setup_Guide.pdf) , [공식](http://dev.ti.com/tirex/#/?link=Software%2FmmWave%20Sensors%2FIndustrial%20Toolbox%2FLabs%2FROS%20Point%20Cloud%20Visualizer), [깃허브](https://github.com/ibcn-cloudlet/ti_mmwave_rospkg), [깃허브-개선버젼](https://github.com/radar-lab/ti_mmwave_rospkg) 

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
## [mmWave software development kit (SDK)](http://www.ti.com/tool/MMWAVE-SDK)
- 다운로드(Linus) : http://software-dl.ti.com/ra-processors/esd/MMWAVE-SDK/lts-latest/exports/mmwave_sdk_02_01_00_04-Linux-x86-Install.bin
- chmod +x *.bin
- [User Guide](https://e2e.ti.com/cfs-file/__key/communityserver-discussions-components-files/1023/mmwave_5F00_sdk_5F00_user_5F00_guide-_2800_1_2900_.pdf)




---

# Code Composer Studio(CCS)

Download the latest CCS : http://processors.wiki.ti.com/index.php/Download_CCS


---

# mmWaveLib
- [SDK 메뉴얼](http://software-dl.ti.com/ra-processors/esd/MMWAVE-SDK/latest/exports/mmwave_sdk_user_guide.pdf) 50page
- mmWaveLib is a collection of algorithms that provide basic functionality needed for FMCW radar-cube processing. 
- This component is available for **xWR16xx** only and contains optimized library routines for C674 DSP architecture only. 


### DCA1000EVM
- DCA1000EVM(http://www.ti.com/tool/DCA1000EVM)은 TI mmWave EVM과 연결하여 Raw ADC 데이터 캡쳐를 할 수 있게 하는 인터페이스 보드입니다.
- [홈페이지](http://www.ti.com/tool/dca1000evm)
- [Quick Start Guide](http://www.ti.com/lit/ml/spruik7/spruik7.pdf)
- [User guide](http://www.ti.com/lit/ug/spruij4/spruij4.pdf)
- [드라이버/mmWave studio](https://downloads.ti.com/downloads/ra-processors/esd/MMWAVE-STUDIO/latest/mmwave_studio_02_00_00_02_win32.exe?__gda__=1547022129_a6064df5c19ef3383b7510adfe062512)


---

# UART Read 

http://processors.wiki.ti.com/index.php/Linux_Core_UART_User%27s_Guide


https://github.com/nsekhar/serialcheck

```
~/serialcheck$ gcc -o serialcheck serialcheck.c CROSS_COMPILE=arm-linux-gnueabihf-
~/serialcheck$ gcc -o serialstats serialstats.c CROSS_COMPILE=arm-linux-gnueabihf-

serialstats -d /dev/ttySX -i 1 &

```

## Commnad CLI 접속 

- 센서 :  (C:\ti\mmwave_sdk_02_01_00_04\packages\ti\demo\xwr16xx\mmw)로 플래싱
- 실행 : roslaunch rviz_1642_2d.launch #실행 안해도 자동으로 값 출력 
- 확인 : moserial -> data port 지정 ,  echo uncheck

---

## 설정 


![](https://i.imgur.com/EIirwPZ.png)

```
% ***************************************************************
% Created for SDK ver:02.00
% Created using Visualizer ver:2.0.0.0
% Frequency:77
% Platform:xWR16xx
% Scene Classifier:best_range_res
% Azimuth Resolution(deg):15
% Range Resolution(m):0.044
% Maximum unambiguous Range(m):9.08
% Maximum Radial Velocity(m/s):5.06
% Radial velocity resolution(m/s):0.64
% Frame Duration(msec):33.333
% Range Detection Threshold (dB):9
% Doppler Detection Threshold (dB):0
% Range Peak Grouping:disabled
% Doppler Peak Grouping:enabled
% Static clutter removal:enabled
% ***************************************************************
sensorStop
flushCfg
dfeDataOutputMode 1
channelCfg 15 3 0
adcCfg 2 1
adcbufCfg -1 0 0 1 0
profileCfg 0 77 39 7 57.14 0 0 70 1 256 5209 0 0 30
chirpCfg 0 0 0 0 0 0 0 1
chirpCfg 1 1 0 0 0 0 0 2
frameCfg 0 1 16 0 33.333 1 0
lowPower 0 1
guiMonitor -1 1 0 0 0 0 0
cfarCfg -1 0 0 8 4 4 0 3072
cfarCfg -1 1 0 4 2 3 0 3072
peakGrouping -1 1 0 1 1 255
multiObjBeamForming -1 1 0.5
clutterRemoval -1 1 #노이즈 제 
calibDcRangeSig -1 0 -5 8 256
extendedMaxVelocity -1 0
bpmCfg -1 0 0 1
lvdsStreamCfg -1 0 0 0
nearFieldCfg -1 0 0 0
compRangeBiasAndRxChanPhase 0.0 1 0 1 0 1 0 1 0 1 0 1 0 1 0 1 0
measureRangeBiasAndRxChanPhase 0 1.5 0.2
CQRxSatMonitor 0 3 5 123 0
CQSigImgMonitor 0 127 4
analogMonitor 1 1
sensorStart
```

Configuration (.cfg) File Format : [SDK 메뉴얼](http://software-dl.ti.com/ra-processors/esd/MMWAVE-SDK/latest/exports/mmwave_sdk_user_guide.pdf) 14page

[mmWave sensing Estimation](https://dev.ti.com/gallery/view/1792614/mmWaveSensingEstimator/ver/1.3.0/)툴을 이용하여 설정값 변경이 더편함
[샘플 cfg](http://dev.ti.com/tirex/content/mmwave_industrial_toolbox_3_1_1/.metadata/.html/chirps.html) 다운 받아 사용 권장 


---

# RawData 파싱 

[mmw Demo Data Structure v0.1](https://e2e.ti.com/cfs-file/__key/communityserver-discussions-components-files/1023/mmw-Demo-Data-Structure_5F00_8_5F00_16_2D00_7.pdf): Out-of-box Demo 플래슁 

```python
import struct
import sys
import time

#
# TODO 1: (NOW FIXED) Find the first occurrence of magic and start from there
# TODO 2: Warn if we cannot parse a specific section and try to recover
# TODO 3: Remove error at end of file if we have only fragment of TLV
# https://e2e.ti.com/support/sensors/f/1023/t/684872
#

def tlvHeaderDecode(data):
    tlvType, tlvLength = struct.unpack('2I', data)
    return tlvType, tlvLength

def parseDetectedObjects(data, tlvLength):
    numDetectedObj, xyzQFormat = struct.unpack('2H', data[:4])
    print("\tDetect Obj:\t%d "%(numDetectedObj))
    for i in range(numDetectedObj):
        #print("\tObjId:\t%d "%(i))
	if(struct.calcsize('3H3h') == len(data[4+12*i:4+12*i+12])):		
		rangeIdx, dopplerIdx, peakVal, x, y, z = struct.unpack('3H3h', data[4+12*i:4+12*i+12])
		print("\t\tDopplerIdx:\t%d "%(dopplerIdx))
		print("\t\tRangeIdx:\t%d "%(rangeIdx))
		print("\t\tPeakVal:\t%d "%(peakVal))
		print("\t\tX:\t\t%07.3f "%(x*1.0/(1 << xyzQFormat)))
		print("\t\tY:\t\t%07.3f "%(y*1.0/(1 << xyzQFormat)))
		print("\t\tZ:\t\t%07.3f "%(z*1.0/(1 << xyzQFormat)))

def parseRangeProfile(data, tlvLength):
    for i in range(256):
        rangeProfile = struct.unpack('H', data[2*i:2*i+2])
        print("\tRangeProf[%d]:\t%07.3f "%(i, rangeProfile[0] * 1.0 * 6 / 8  / (1 << 8)))
    print("\tTLVType:\t%d "%(2))

def parseStats(data, tlvLength):
    interProcess, transmitOut, frameMargin, chirpMargin, activeCPULoad, interCPULoad = struct.unpack('6I', data[:24])
    print("\tOutputMsgStats:\t%d "%(6))
    print("\t\tChirpMargin:\t%d "%(chirpMargin))
    print("\t\tFrameMargin:\t%d "%(frameMargin))
    print("\t\tInterCPULoad:\t%d "%(interCPULoad))
    print("\t\tActiveCPULoad:\t%d "%(activeCPULoad))
    print("\t\tTransmitOut:\t%d "%(transmitOut))
    print("\t\tInterprocess:\t%d "%(interProcess))

def tlvHeader(data):
    while data:
        
        headerLength = 36
        try:
            magic, version, length, platform, frameNum, cpuCycles, numObj, numTLVs = struct.unpack('Q7I', data[:headerLength])
        except:
            print "Improper TLV structure found: ", (data,)
            break
        print("Packet ID:\t%d "%(frameNum))
        print("Version:\t%x "%(version))
        print("TLV:\t\t%d "%(numTLVs))
        print("Detect Obj:\t%d "%(numObj))
        print("Platform:\t%X "%(platform))
	if version > 0x01000005:
	    subFrameNum = struct.unpack('I', data[36:40])[0]
	    headerLength = 40
	    print("Subframe:\t%d "%(subFrameNum))
        pendingBytes = length - headerLength
        data = data[headerLength:]

 
        for i in range(numTLVs):             
                
                tlvType, tlvLength = tlvHeaderDecode(data[:8])
                data = data[8:]
                if (tlvType == 1):
                    parseDetectedObjects(data, tlvLength)
                elif (tlvType == 2):
                    parseRangeProfile(data, tlvLength)
                elif (tlvType == 6):
                    parseStats(data, tlvLength)
                else:
                    print("Unidentified tlv type %d"%(tlvType))
                data = data[tlvLength:]
                pendingBytes -= (8+tlvLength)
        data = data[pendingBytes:]
        yield length, frameNum

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print("Usage: parseTLV.py inputFile.bin")
        sys.exit()

    fileName = sys.argv[1]
    rawDataFile = open(fileName, "rb")
    while(1):
	    rawData = rawDataFile.read()
	    #print(type(rawDataFile))
	    #rawDataFile.close()
	    magic = b'\x02\x01\x04\x03\x06\x05\x08\x07'
	    offset = rawData.find(magic)
	    rawData = rawData[offset:]
	    for length, frameNum in tlvHeader(rawData):
		print  
```                

---

# People tracking Demo 

> * C:\ti\mmwave_industrial_toolbox_3_1_1\labs\lab0011-pplcount\lab0011_pplcount_quickstart\


```
===========================================================
[IWR1642] v3.1.1
===========================================================
dfeDataOutputMode 1
channelCfg 15 3 0
adcCfg 2 1
adcbufCfg 0 1 1 1
profileCfg 0 77 30 7 62 0 0 60 1 128 2500 0 0 30
chirpCfg 0 0 0 0 0 0 0 1
chirpCfg 1 1 0 0 0 0 0 2
frameCfg 0 1 128 0 50 1 0
lowPower 0 1
guiMonitor 1 1 0 0
cfarCfg 6 4 4 4 4 16 16 4 4 50 62 0
doaCfg 600 1875 30 1
SceneryParam -6 6 0.05 6
GatingParam 4 3 2 0
StateParam 10 5 10 100 5
AllocationParam 450 0.01 25 1 2
VariationParam 0.289 0.289 1.0
PointCloudEn 1
trackingCfg 1 2 250 20 200 50 90
sensorStart

left wall: -6
R wall: 6
front wall: 6
back wall: 0
------------------
```

```
===========================================================
[IWR6843] v3.1.1
===========================================================
flushCfg
dfeDataOutputMode 1
channelCfg 15 5 0
adcCfg 2 1
adcbufCfg 0 1 1 1
profileCfg 0 60.6 30 10 62 0 0 53 1 128 2500 0 0 30
chirpCfg 0 0 0 0 0 0 0 1
chirpCfg 1 1 0 0 0 0 0 4
frameCfg 0 1 128 0 50 1 0
lowPower 0 1
guiMonitor 1 1 0 0
cfarCfg 6 4 4 4 4 16 16 4 4 50 62 0
doaCfg 600 1875 30 1 1 0
SceneryParam -6 6 0.5 6
GatingParam 4 3 2 0
StateParam 10 5 100 100 5
AllocationParam 250 250 0.25 10 1 2
AccelerationParam 1 1 1
trackingCfg 1 2 250 20 52 82 50 90
sensorStart

left wall: -6
R wall: 6
front wall: 6
back wall: 0
------------------
```



---

- [TI 포럼](https://e2e.ti.com/support/sensors/f/1023)

- ttyS0 is the device for the first UART serial port on x86 and x86_64 architectures. If you have a PC motherboard with serial ports you'd be using a ttySn to attach a modem or a serial console.
- ttyUSB0 is the device for the first USB serial convertor. If you have an USB serial cable you'd be using a ttyUSBn to connect to the serial port of a router.
- ttyAMA0 is the device for the first serial port on ARM architecture. If you have an ARM-based TV box with a serial console and running Android or OpenELEC, you'd be using a ttyAMAn to attach a console to it.