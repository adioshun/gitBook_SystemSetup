```
# ubuntu 16.04 tested (??)
$ pip install numpy mayavi

$ pip install PyQt5 #mayavi backend로 추후 필요 

$ jupyter nbextension install --py mayavi --user
$ jupyter nbextension enable --py mayavi --user
```

###### Conda 설치  

- python2 : `conda install -c anaconda mayavi`
- python3 : `conda install -c clinicalgraphics vtk=7.1.0; pip install mayavi`


> ImportError: Could not import backend for traits : `conda install -c conda-forge pyside=1.2.4 ` OR `conda install pyqt=4`

###### Pip 설치 


```bash
sudo apt-get install vtk6 tcl-vtk python-vtk
python -c "import vtk"
# ImportError: libGLU.so.1: cannot open shared object file: No such file or director
## -> cp -r /usr/lib/python2.7/dist-packages/vtk /opt/anaconda3/envs/python2_gpu/lib/python2.7/site-packages/
pip install mayavi
import mayavi.mlab as mlab


# echo "INSTALLING GUI BACKEND FOR MAYAVI"
# pip install mayavi[TraitsBackendQt]
sudo apt-get install build-essential git cmake libqt4-dev libphonon-dev python2.7-dev libxml2-dev libxslt1-dev qtmobility-dev libqtwebkit-dev
pip install pyside

sudo apt-get install python-pyqt4 for pyqt4
```

