## 1. Velodyne

[velodyne](http://wiki.ros.org/velodyne)


$ Veloview 

### 1.2 Velodyne for ROS  

#### A. ROS dependencies 설치 

`$ sudo apt-get install ros-kinetic-velodyne`  # kinetic버젼 용


#### B. 드라이버 설치 

```
cd 
git clone https://github.com/ros-drivers/velodyne.git
cd velodyne
mkdir src
cp -r velodyne* ./src
catkin_make -j8
source ./devel/setup.bash
```
OR 

```
# INDIGO VERSION
cd ~/catkin_ws/src/ && git clone https://github.com/ros-drivers/velodyne.git
cd velodyne/
rosdep install --from-paths ./ --ignore-src --rosdistro indigo -y
cd ~/catkin_ws/ && catkin_make
```

> ref : [velodyne-ROS wiki](http://wiki.ros.org/velodyne/Tutorials/Getting%20Started%20with%20the%20Velodyne%20VLP16)

#### C. 확인 
```
# Velodyne 노드 실행 & 데이터 수집 
$ roslaunch velodyne_pointcloud VLP16_points.launch
# Now, the necessary nodes are running. You can check this with the following command

# 실행 노드 확인 
$ rosnode list
# You'll can see the messages being published and subscribed in the following topic:

# 메시지 출력 
$ rostopic echo /velodyne_points
# After That, launch rviz, with the "velodyne" as a fixed frame:

# rviz이용 시각화 
$ rosrun rviz rviz -f velodyne
# In the "displays" panel, click "Add", then select "Point Cloud2", then press "OK".
# In the "Topic" field of the new "Point Cloud2" tab, enter "/velodyne_points"
# Congratulations. Now, your Velodyne is ready to builds the "real" world inside your system. Enjoy it.
```

### 1.3 Calibration in ROS



we need to calibrate generate calibration data in YAML file. 

벨로다인에서 제공하는 XML포맷을 YAML포맷으로 변경 `$ rosrun velodyne_pointcloud gen_calibration.py 32db.xml`
- The following command will generate the calibration data in a YAML file from the standard Velodyne XML file: 
- 파일 위치(??) : `catkin_ws/src/velodyne/velodyne_pointcloud/params/VLP16db.yaml`

`my_calibration.yaml`이름으로 저장 ` $ rosrun velodyne_pointcloud gen_calibration.py 32db.xml my_calibration.yaml`
- Save generated 32E calibration data in my_calibration.yaml.

런쳐 실행시 칼리브레이션 파일 지정  `$ roslaunch velodyne_pointcloud 32e_points.launch calibration:=/home/robot/32db.yaml`
- We have to mention the generated calibration YAML file along with the launch file:

rviz 실행 : `$ rosrun rviz rviz -f velodyne`


