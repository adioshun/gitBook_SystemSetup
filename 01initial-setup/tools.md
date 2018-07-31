

| ![](https://raw.githubusercontent.com/oguzhaninan/Stacer/native/screenshots/header.png) | ![](https://extensions.gnome.org/extension-data/screenshots/screenshot_1320_zgXAduX.png) |  |
| --- | --- | --- |
| sudo add-apt-repository ppa:oguzhaninan/stacer<br> sudo apt-get install stacer| [NVIDIA GPU Stats Too](https://extensions.gnome.org/extension/1320/nvidia-gpu-stats-tool/) |  |




## Monitoring

* CPU

  * htop : `sudo apt-get install htop`
  * nmon : `sudo apt-get install nmon`

* GPU Monitoring :

  * `nvidia-smi -l 2` : `Failed to initialize NVML: Driver/library version mismatch` 에러시 `Reboot`
  * 한줄로보기, gpustat : `sudo pip install gpustat`, `watch --color -n1.0 gpustat`[\[참고\]](https://github.com/wookayin/gpustat)
  * htop처럼 보기, glances : `sudo pip install glances[gpu]`, `sudo glances` \(n, z, d\)
    * `wget -O- https://bit.ly/glances | /bin/bash` [\[홈페이지\]](https://pypi.python.org/pypi/Glances)
  * intel-gpu-tools : `sudo apt-get install intel-gpu-tools`, `intel_gpu_top`

![](http://i.imgur.com/XjyHkIF.png)

## 학습 모니터링

* 앱기반 : [Hyperdash](https://hyperdash.io/), `pip install hyperdash && hyperdash login`
* 웹기반 : [LabNotebook](https://github.com/henripal/labnotebook)

| ![](http://i.imgur.com/QCEGtYx.png) | ![](https://github.com/henripal/labnotebook/raw/master/nbs/img/labnotebook.gif) |
| --- | --- |
| Hyperdash | LabNotebook |

* [Gnome Shell extension for making and uploading screenshots](https://github.com/OttoAllmendinger/gnome-shell-screenshot)



