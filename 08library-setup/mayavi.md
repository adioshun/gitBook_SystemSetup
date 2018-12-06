###### Pip 설치 

```python
$ sudo apt-get install vtk6 libvtk6-dev #tcl-vtk python-vtk   #python3-vtk는 없음 
$ python3 -c "import vtk"

$ pip3 install numpy mayavi
$ pip3 install pyqt5==5.7.1 sip==4.19  #mayavi backend로 추후 필요 #ubuntu 16.04, pyqt5
$ python3 -c "import mayavi.mlab as mlab"


# Jupyter시각화시 필요 
$ jupyter nbextension install --py mayavi --user
$ jupyter nbextension enable --py mayavi --user
```

###### Conda 설치  

- conda install -c anaconda vtk 

- python2 : `conda install -c anaconda mayavi`
- python3 : `conda install -c clinicalgraphics vtk=7.1.0; pip install mayavi`



## 에러 처리 

ImportError: Could not import backend for traits : `conda install -c conda-forge pyside=1.2.4 ` OR `conda install pyqt=4`

qt.qpa.plugin: Could not load the Qt platform plugin "xcb" in "" even though it was found : 




```bash

# ImportError: libGLU.so.1: cannot open shared object file: No such file or director
## -> cp -r /usr/lib/python2.7/dist-packages/vtk /opt/anaconda3/envs/python2_gpu/lib/python2.7/site-packages/


# echo "INSTALLING GUI BACKEND FOR MAYAVI"
# pip install mayavi[TraitsBackendQt]
sudo apt-get install build-essential git cmake libqt4-dev libphonon-dev python2.7-dev libxml2-dev libxslt1-dev qtmobility-dev libqtwebkit-dev
pip install pyside
sudo apt-get install python-pyqt4 for pyqt4

```


---

I suspect libvtk6-dev is installed by ROS because ROS Kinetic requires PCL 1.7.x, which is provided by package libpcl-dev, which depends on libvtk6-dev.

http://www.ros.org/reps/rep-0003.html#kinetic-kame-may-2016-may-2021
