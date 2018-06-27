# web-cam

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

---

