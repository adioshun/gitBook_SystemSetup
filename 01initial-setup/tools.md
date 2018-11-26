

| ![](https://raw.githubusercontent.com/oguzhaninan/Stacer/native/screenshots/header.png) | ![](https://extensions.gnome.org/extension-data/screenshots/screenshot_1320_zgXAduX.png) |  |
| --- | --- | --- |
| sudo add-apt-repository ppa:oguzhaninan/stacer<br> sudo apt-get install stacer| [NVIDIA GPU Stats Too](https://extensions.gnome.org/extension/1320/nvidia-gpu-stats-tool/) |  |




## Monitoring

* CPU

  * htop : `sudo apt-get install htop`
  * nmon : `sudo apt-get install nmon`
  * glances : CPU, Memory, Network 모니터

* GPU Monitoring :

  * `nvidia-smi -l 2` : `Failed to initialize NVML: Driver/library version mismatch` 에러시 `Reboot`
  * gpustat : `sudo pip install gpustat`, `watch --color -n1.0 gpustat`[\[참고\]](https://github.com/wookayin/gpustat)
  * glances : `sudo pip install glances[gpu]`, `sudo glances` \(n, z, d\)
    * `wget -O- https://bit.ly/glances | /bin/bash` [\[홈페이지\]](https://pypi.python.org/pypi/Glances)
  * gmonitor : GPU 현재 사용량 모니터링
  * intel-gpu-tools : `sudo apt-get install intel-gpu-tools`, `intel_gpu_top`
  * nvtop : (h)top like task monitor for NVIDIA GPU

> [GPU Useage를 편하게 모니터 해보자!](https://eungbean.github.io/2018/08/29/gpu-monitor-with-byobu/?fbclid=IwAR3Rv0iPd1PJjEogujyxWBWjJyLknu_QLxexY_OfIyrOTaLsAADEzFagpRE)

## 학습 모니터링

* 앱기반 : [Hyperdash](https://hyperdash.io/), `pip install hyperdash && hyperdash login`
* 웹기반 : [LabNotebook](https://github.com/henripal/labnotebook)

| ![](http://i.imgur.com/QCEGtYx.png) | ![](https://github.com/henripal/labnotebook/raw/master/nbs/img/labnotebook.gif) |
| --- | --- |
| Hyperdash | LabNotebook |


