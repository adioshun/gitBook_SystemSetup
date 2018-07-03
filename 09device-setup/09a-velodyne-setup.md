## 1. Velodyne


### Veloview 

[Linux 64bit](http://www.paraview.org/paraview-downloads/download.php?submit=Download&version=v5.1&type=app&os=win32&downloadFile=VeloView-3.5.0-Linux-64bit.sh)

### 1.2 Velodyne for ROS  

> [ROS_velodyne](http://wiki.ros.org/velodyne)

#### A. ROS dependencies 설치 

`$ sudo apt-get install ros-$ROS_DISTRO-velodyne`  


#### B. 드라이버 설치 

```
cd ~/catkin_ws/src/ && git clone https://github.com/ros-drivers/velodyne.git
cd velodyne/
rosdep install --from-paths ./ --ignore-src --rosdistro $ROS_DISTRO -y
cd ~/catkin_ws/ && catkin_make
```

> ref : [velodyne-ROS wiki](http://wiki.ros.org/velodyne/Tutorials/Getting%20Started%20with%20the%20Velodyne%20VLP16)

#### C. 확인 

Velodyne 노드 실행 & 데이터 수집 : `$ roslaunch velodyne_pointcloud VLP16_points.launch`


메시지 출력 : `$ rostopic echo /velodyne_points --noarr`


rviz이용 시각화 : `$ rosrun rviz rviz -f velodyne`

```
$ roslaunch velodyne_pointcloud VPL16_points.launch pcap:=/home/soowon/Documents/County_Fair.pcap
$ rosrun velodyne_pointcloud cloud_node _calibration:=/home/velodyne_pointcloud/params/VLP16db.yaml
```

### 1.3 Calibration in ROS

After the installation, you will need to create a calibration file for the device.

복사만 하면 됨 `cp /ros/indigo/share/velodyne_pointcloud/params/VLP16db.yaml ~/`




we need to calibrate generate calibration data in YAML file. 

벨로다인에서 제공하는 XML포맷을 YAML포맷으로 변경 `$ rosrun velodyne_pointcloud gen_calibration.py 32db.xml`
- The following command will generate the calibration data in a YAML file from the standard Velodyne XML file: 
- 파일 위치(??) : `catkin_ws/src/velodyne/velodyne_pointcloud/params/VLP16db.yaml`

`my_calibration.yaml`이름으로 저장 ` $ rosrun velodyne_pointcloud gen_calibration.py 32db.xml my_calibration.yaml`
- Save generated 32E calibration data in my_calibration.yaml.

런쳐 실행시 칼리브레이션 파일 지정  `$ roslaunch velodyne_pointcloud 32e_points.launch calibration:=/home/robot/32db.yaml`
- We have to mention the generated calibration YAML file along with the launch file:

rviz 실행 : `$ rosrun rviz rviz -f velodyne`


---

# Sample data 

- [HDL-32/VLP-16](https://midas3.kitware.com/midas/community/29)
    
    - PCAP: all received packet data.
    - CSV: one frame data that is reflection intensity and distance of 0-359.9 degree.
    
- [Velodyne HDL-64E LiDAR, LiDAR Velodyne HDL-32e LiDAR, Velodyne VLP-16 Puck](http://masc.cs.gmu.edu/wiki/MapGMU)

- [VLP16](https://goo.gl/MJDfWA)
            
# VeloView 뷰어 (paraview??)

- [Wiki](https://www.paraview.org/Wiki/VeloView), [Homepage](https://www.paraview.org/VeloView/), [Github](https://github.com/Kitware/VeloView)
- `.pcap` files 재생 
- csv/txt files로 저장 가능

## Point Cloud Library with Velodyne LiDAR

- Point Cloud Library (PCL) have `Grabber` for input data from Velodyne LiDARs.

- `.pcap`파일을 사용 하기 위해서는, You need build PCL with WITH_PCAP option. (Pre-built library is disabled this option.)
    - HDLGrabber : This Grabber for HDL-64E/HDL-32E.
    - VLPGrabber : This Grabber for VLP-16.


## Article 

- [Parse pcap from HDL-32E Sensor](https://github.com/ritzalam/velodyne-lidar-parser): python, Github

- [ROS support for Velodyne 3D LIDARs](https://github.com/ros-drivers/velodyne)

- [Point cloud conversions for Velodyne 3D LIDARs.](http://wiki.ros.org/action/fullsearch/velodyne_pointcloud?action=fullsearch&context=180&value=linkto%3A%22velodyne_pointcloud%22)

- [Velodyne Lidar Python Library](https://github.com/esrlabs/velodyne): HDL 64E S2 only.

- [The Velodyne High Definition LiDAR (HDL) Grabber](http://pointclouds.org/documentation/tutorials/hdl_grabber.php): pcap설정, cpp

---

## `pcap`파일 읽어 Play하기 
- [pcap_reader.py](https://gist.github.com/gerkey/bf749775e6bc600368b97ce3d9f113e5): Read a .pcap file full of UDP packets from a velodyne and play them back to

```python 
#!/usr/bin/env python

# Read a .pcap file full of UDP packets from a velodyne and play them back to
# localhost:2368, for consumption by the velodyne driver.
#
# TODO: error-checking and options (looping, etc.)

import dpkt
import sys
import socket
import time

UDP_IP = "localhost"
UDP_PORT = 2368

def parse(fname):
    lasttime = -1
    sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    i = 0
    with open(fname, 'rb') as f:
        pcap = dpkt.pcap.Reader(f)
        for ts, buf in pcap:
            eth = dpkt.ethernet.Ethernet(buf)
            ip = eth.data
            udp = ip.data
            velodata = udp.data
            if lasttime > 0 and (ts-lasttime) > 0:
                time.sleep(ts-lasttime)
            lasttime = ts
            print('[%d] [%s] sending %d-byte message'%(i,`ts`,len(velodata)))
            sock.sendto(velodata, (UDP_IP, UDP_PORT))
            i += 1

if __name__ == '__main__':
    if len(sys.argv) < 2:
        print('ERROR: must supply pcap filename')
        sys.exit(1)
    while True:
        parse(sys.argv[1])
```

> To prevent exception: UnicodeDecodeError: 'utf-8' codec can't decode byte 0xd4 in position 0: invalid continuation byte we need to use binary mode for open file: dpkt.pcap.Reader(open(filename,'rb'))


---

## pcap to rosbag 변환 
서로 다른 터미널에서 실행 
```
roscore
rosrun velodyne_driver velodyne_node _model:=VLP16 _pcap:=/your/pcap/path/data.pcap _read_once:=true
rosrun rosbag record -O your_vlp16_070815.bag /velodyne_packets
```



