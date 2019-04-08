# Autoware&ROS

## 1. 정의 

Autoware는 아래와 같은 구조를 가지고 있다. 
![](https://i.imgur.com/ptfCJXZ.png)

ROS기반으로 일본 나고야 대학에서 개발 하고 있다. 현재 깃허브에 오픈되어 있으면 다음과 같은 기능을 가지고 있다. 
- Localization
- Object detection
- Driving control
- 3D map generation and sharing

ROS는 2007년 미국의 스탠포드 대학 인공지능 연구소(AI LAB)가 진행하던 STAIR(STanford AI Robot) 프로젝트를 위해 Morgan Quigley 이 개발한 "Switchyard" 이라는 시스템에서 시작되었다. 2008년 미국의 로봇 전문 기업 윌로우게러지라는 회사가 이어받아 ROS라는 이름으로 개발하기 시작하여 2010년 1월 22일날 ROS 1.0 이라는 세상에 나왔다.

## 2. 용어 정리

> 출처 : [daddynkidsmakers](http://daddynkidsmakers.blogspot.com/2015/08/ros-2.html)

### 2.1 마스터

* 노드와 노드사이의 연결과 메시지 통신을 위한 네임 서버와 같은 역활을 한다.
* `roscore`가 실행 명령어이다.
* 실행하면, 각 노드의 이름을 등록하고, 필요에 따라 정보를 받을 수 있다.

* 통신 방식 : 마스터는 접속하는 **슬래이브**들과의 접속 상태를 유지하지 않는 HTTP 기반의 프로토콜인 **XMLRPC**를 이용하여 슬레이브들과 통신한다.

* 이로 인해, 슬레이브가 다운되더라도, 마스터는 다운되는 일이 없으며, 슬레이브의 센서 노드 등이 일부 종료하더라도, 전체 시스템이 다운되는 일은 없다.
* 심지어, 다운된 센서 노드가 다시 기동하면, 이 데이터를 받는 노드는 다시 데이터를 받아 처리하도록 되어 있다. 이는 토픽-메시지 기반 데이터 처리 방식이기 때문에 가능한 것이다. 

* 마스터는 사용자가 정해 놓은 ROS\_MASTER\_URL변수에 기재된 **URI주소**와 포트를 가진다.

* 사용자가 설정해 놓지 않으면, URI주소로 현재 로컬 IP를 사용하고, 11311포트를 이용한다.

### 2.2 노드

* ROS 최소 단위의 실행 프로세서이다.

* 노드는 생성과 함께 마스터에 노드와 퍼블리셔, 서브스크라이버, 토픽, 서비스의 각 이름, 메시지 형태, URI 주소와 포트를 등록한다.

* 이 정보들을 기반으로 각 노드는 노드끼리 토픽과 서비스를 이용해 메시지를 주고 받는다.

* 통신 방식 : 노드는 마스터와 통신할 때는 XMLRPC를 이용하고, 노드간 통신에는 XMLRPC나 TCPROS를 사용한다.

* 노드간 접속과 질의 응답은 XMLRPC를 사용하고, 메시지 통신은 노드간 직접 통신이므로 TCPPROS를 이용한다.
* URI주소는 노드가 실행중인 컴퓨터에 저장된 ROS\_HOSTNAME 환경 변수 값으로 URI주소를 사용한다.
- ROS nodes are basically programs made in ROS , 실행중인 프로세스?
- `rosnode list`
- 'rosnode infor /Obiwan`
- 'rosnode kill /topic_puvlisher`
### 2.3 패키지

* ROS 기본단위이다.

* 공식 패키지 : www.ros.org/debbuild/indigo.html
* 사용자 개발 공개 패키지 : [http://rosindex.github.io/stats/](http://rosindex.github.io/stats/)

* 메타 패키지 - 공통 목적을 지닌 여러 패키지를 모아둔 패키지 집합이다.

> ros 패키지를 설치 URL정보 : `/etc/apt/sources.list.d/ros-lastest.list`

### 2.4 메시지

* 노드 간에 데이터를 주고 받는 단위이다.

* 정수, 실수 변수의 구조체이며, 배열을 사용할 수 있다.

* 단방향 메시지 송수신 방식의 토픽과 양방향 메시지 요청/응답 방식의 서비스를 이용한다.

### 2.5 토픽

* 스토리이며, 퍼블리셔\(publisher\) 노드가 하나의 스토리에 대해, 토픽이란 이름으로 마스터에 등록한 후, 이 스토리에 대해 메시지 형태로 퍼블리쉬한다.

* 토픽을 수신받기 위해서, 서브스크라이버\(subscriber\) 노드가 마스터에 등록된 토픽의 이름에 해당하는 퍼블리셔 노드의 정보를 받는다.

* 이를 기반으로 서브스크라이버 노드와 퍼블리셔 노드가 직접 연결해 메시지를 토픽으로 송수신한다.

### 2.6 서비스

* 동기화된 메시지 처리 방식이다.** 일회성 메시지** 통신으로, 서비스 요청과 응답이 완료되면 연결된 두 노드의 접속은 끊긴다.

### 2.7 캐킨\(catkin\)

* ROS빌드 시스템이다. 기본적으로 CMake를 이용한다.

* ROS는 CMake를 ROS에 맞도록 특화된 캐킨 빌드 시스템을 만들었다.


### 2.8 Parameter Server

- ROS uses to store parameters
- This parameters can be used by Nodes at runtime and are normally used for static data, such as configure parameters
- To get a list of parameters : `rosparam list`
- To get a value of a parameter : `rospapam get <parameter_name?
- To set a value to a parameter : `rosparam set <parameter_name> <value>`




---


# bag file(rosbag)

## 1. 정의 
rosbag은 ROS의 메시지 저장 파일 포맷이다. 쉽게 작업 내용을 저장/재생 할수 있다. 


## 2. 활용 방법 

Save : `rosbag record -o chatter.bag /chatter`
- `-a` : 모두 저장
Play : `rosbag play /{path-to-file}/bagfile_name.bag`
- 반복 `-l` or `--loop`

Dislplay : roscore -> `$ rostopic echo /vehicle/gps/fix`

자르기
- rosbag filter input.bag output.bag "t.secs >= 1531425960 and t.secs <= 1531426140"

## 3. 저장 예 

```
# This should output something like this: header:

seq: 7318
stamp:
secs: 1492883544
nsecs: 965464774
frame_id: ''
status:
status: 0
service: 1
latitude: 37.4267093333
longitude: -122.07584
altitude: -42.5
position_covariance: [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0]
position_covariance_type: 0
```



---


# functions provided

ROS 에서 제공하는 주요 기능은 다음과 같다. 
* Original build system \(Catkin\)
* Image processing library \(OpenCV\)
* Data logging tool \(ROSBAG\)
* Visualization tools for data and software state \(RViz\)
* Coordinate transformation library \(TF\)
* Qt based GUI development tool \(RQT\)

ROS는 통신 기반으로 데이터를 교환 한다. 
* ROS uses message passing with **topics** of publish/subscribe form for inter-module connection/cooperation frameworks. 
* In ROS, processes `(called node)` are **launched** and each node is run independently.
* In communication between nodes, by following the publish/subscribe scheme,
* a node writes messages \(publish\) into a topic and
* another node reads the messages \(subscribe\) of the topic.

![](https://i.imgur.com/ekIY1NU.png)

---

# Available user Interface

현재 ROS는 C++과 Python 기반 사용자 Interface를 제공한다. 

본 과제에서 사용되는 Python기반  Topic데이터 송수신 예시 

## 1 Topic Publisher

* A topic is like a pipe. Nodes use topics to publish information for other nodes so they can communicate.

* `rostopic list`

* `rostopic list | grep '/{topic이름}`
* `rostopic echo /Obiwan` \# 실시간으로 정보 출력

![](https://i.imgur.com/Dln3vPe.png)

* create a Publisher that keeps publishing into the '/counter' topic a sequence of integer.

* A **Publisher** is a node that keeps publishing a message into a topic

* A **topic** is a channel that acts as a pipe, where oter ROS nodes can either publish or read information. 소켓??

* To get a list of available topics `rostopic list`

* To read the information that is being published in a topic `rostopic echo <topic_name>`

* 마지막 메시지만 출력 `rostopic echo <topic_name> -n1`
* To get information about a certain topic `rostopic info <topic_name>`

## 2 ROS Messages

* Topics handle information through messages

* Messages can be of many types.

* Messages are defined in `.msg`files, which are located inside a `msg`directory of a package.

* To get information about a message `rosmsg show <message>` , `rosmsg show std_msgs/Int32`

## 3.  Topic Subscriber

* Publisher와 반대 개념, To read information from topic

* 테스트 전에 publish를 위해 임시 실행 `rostopic pub <topic_name> <message_type> <value>` eg. `rostopic pub /count std_msgs/int32 5`

* This command will publish the message you specify with the value you specify, in the topic you specify.

![](https://i.imgur.com/k7VWK0B.png)

* create a subscriber node that listens to the /counter topic and each time it reads something it calls a function that does a print of the msg.

* 당장은 아무 반응 없음, But when you executed the rostopic pub command, you published a message into the /counter topic, so the function has printed number

## 4. Custom Topic Message Compilation

* How to prepare `CMakeLists.txt` and `package.xml` for custom topic message compilation

* Create a directory named `msg` inside your package

* Inside this directory, create a file named `Name_of_your_message.msg` eg. `Age.msg`
* Modify `CMakeLists.txt file`
* Modify `package.xml` file
* Compile
* Use in code

```
# ./src/my_publisher/msg/Age.msg

float32 years
float32 months
float32 days
```

---

# Operation steps

- Setup of DEMO DATA
- Unpacking demo data
- Launch Autoware
- Play rosbag for a few seconds and pause
- Loading and setting up RViz
- Loading PointCloud and Vector maps
- Start Localization
- Diaplay in RViz
- Start Mission Planning(1), Start Motion Planning(2)

필요 선행 작업 : The `intrinsic parameters of the camera` should be known before starting the LiDAR-camera calibration process.

카메라는 전면만 센신하고, Lidar는 360도를 센싱한다. 매순간 데이터가 수집될때 두 센서는 서로 임의의 간격을 두고 있다. `Each time data was collected, the LiDAR and camera were kept at arbitrary(임의의) distance in 3D space.`

두 센서간의 `transformation`는 manual하게 측정한다. `The transformation between them was measured manually.`

비록 테이프 측정방식이 조잡해 보이지만 여러 알고리즘으로 얻은 값들을 sanity check할수 있게 한다. `Although, the tape measurement is crude, it serves as a sanity check for values obtained using various algorithms.`

`translation`을 측정하는것은 `회전`을 측정하는것 보다 쉽다. `Measuring translation is easier than rotation.`

회전이 작으면 0으로 가적하면 된다. 반대로 회적이 크면 거리를 측정하여 삼각법으로 앵글을 유추 한다. `When the rotations were minimal, we assumed them to be zero, in other instances, when there was considerable rotation in the orientation of the sensors, we measured distances and estimated the angles roughly using trigonometry.`

> 언급된 파라미터 : transformation, translation, rotation


## 1 설치

```bash
cd ~
git clone https://github.com/ankitdhall/lidar_camera_calibration.git
cp -r lidar_camera_calibration/dependencies/aruco_ros/ catkin_ws/src/
cd catkin_ws/
catkin_make
cp -r ~/lidar_camera_calibration ~/catkin_ws/src/
catkin_make
catkin_make install
```

## 2 설정

> /root/catkin\_ws/devel/lib/aruco\_mapping/aruco\_mapping

실행 : `roslaunch lidar_camera_calibration find_transform.launch`

#### A `lidar_camera_calibration.yaml` 파일 수정

참고 해야 하는 카메라와 라이다 TOPIC명시 `Contains name of camera and velodyne topics that the node will subscribe to.`

```python
lidar_camera_calibration:
camera_frame_topic: /image_raw # [중요] 반드시 변경
camera_info_topic: /camera_info # [중요] 반드시 변경
velodyne_topic: /velodyne_points
```

> `lidar_camera_calibration/launch/find_transform.launch` 을이용하여 값 찾을수 있음

#### B. `config_file.txt` 파일 수정

```python
480 640 # image_width image_height
-2.5 2.5 # x- x+, 불필요한 포인트 제거를 위한 필터, mark the board edges를 쉽게 함
-4.0 4.0 # y- y+, 불필요한 포인트 제거를 위한 필터, mark the board edges를 쉽게 함
0.0 2.5 # z- z+, 불필요한 포인트 제거를 위한 필터, mark the board edges를 쉽게 함
0.05 # cloud_intensity_threshold # Intensity가 낮는 포인트 제거를 위한 필터의 기준
2 # number_of_markers
1 # use_camera_info_topic? # `camera_info topic`을 사용하려면 `1` / '0'은 `config_file.txt`의 값 사용
611.651245 0.0 642.388357 0.0 # fx 0 cx 0
0.0 688.443726 365.971718 0.0 # 0 fy cy 0
0.0 0.0 1.0 0.0 # MAX_ITERS
# After MAX_ITERS, the node outputs an average translation vector (3x1) and an average rotation matrix (3x3).
# Averaging the translation vector is trivial;
# the rotations matrices are converted to quaternions and averaged,
# then converted back to a 3x3 rotation matrix
1.57 -1.57 0.0 # initial_rot_x initial_rot_y initial_rot_z
# 카메라대비 Lidar의 초기 회전 여부, Initial orientation of the lidar with respect to the camera, in radians.
# The default values are for the case when both the lidar and the camera are both pointing forward.
# The final transformation that is estimated by the package accounts for this initial rotation.
```

#### C. `find_transform.launch` 파일 수정

`aruco_mapping`노드에게 필요한 정보들 `Parameters are required for the aruco_mapping node and need to be specfied here.`

* Ensure that the topics are mapped correctly for the node to function. Other parameters required are:
* calibration\_file\(.ini format\)
* num\_of\_markers
* marker\_size\(in meters\)

```xml
<?xml version="1.0"?>
<launch>
<!-- <param name="/use_sim_time" value="true"/> -->


<!-- ArUco mapping -->

<node pkg="aruco_mapping" type="aruco_mapping" name="aruco_mapping" output="screen"> #Uncomment
<remap from="/image_raw" to="/image_raw"/> ## Must

<param name="calibration_file" type="string" value="$(find aruco_mapping)/data/geniusF100.ini" /> # 칼리브레이션 파일(ini), ptgrey.ini
<param name="num_of_markers" type="int" value="2" /> # 마커 갯수
<param name="marker_size" type="double" value="0.24"/> # 마커 크기 (m)
<param name="space_type" type="string" value="plane" />
<param name="roi_allowed" type="bool" value="false" />


</node>


<rosparam command="load" file="$(find lidar_camera_calibration)/conf/lidar_camera_calibration.yaml" />
<node pkg="lidar_camera_calibration" type="find_transform" name="find_transform" output="screen">
</node>
</launch>
```

#### D. `marker_coordinates.txt` 파일

The ArUco markers are stuck on the board such that when it is hung from a corner, the ArUco marker is on the left side of the board.

Notice how the axis are aligned.

y-axis should point outwards, x-axis along the breadth \(s2\) and z-axis along the length \(s1\).

The markers are also arranged so that the ArUco id are in ascending order. \(마커의 ArUco ids는 오름차순으로 있어야 한다.\)

```
# cm 단위로 표기
2 # 'N' the number of boards
24.0 # length (s1)
24.0 # breadth (s2)
2.0 # border_width_along_length (b1)
2.0 # border_width_along_breadth (b2)
13.6 # edge_length_of_ArUco_marker (e)
24.0 # s1
24.0 # s2
2.0 # b1
2.0 # b2
13.6 # e
```

> [Aruco Marker Generator](https://terpconnect.umd.edu/~jwelsh12/enes100/markergen.html): ID 25, 582 , Marker Size = 205\(e\), padding = 50 \(b1, b2\)

![](https://github.com/ankitdhall/lidar_camera_calibration/raw/master/images/board_dim_label.jpg)

![](https://github.com/ankitdhall/lidar_camera_calibration/raw/master/images/aruco_axis.png)




