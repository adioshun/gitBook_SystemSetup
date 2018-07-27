\[ 우분투 16.04에서 obs로 화면 녹화하기\)\([http://www.kwangsiklee.com/ko/2017/12/우분투-16-04에서-obs로-화면-녹화하기/](http://www.kwangsiklee.com/ko/2017/12/우분투-16-04에서-obs로-화면-녹화하기/)\)

googler 71 : 터미널에서 구글 검색을 할 수 있게 해줍니다.  
stacer 31 : 리눅스의 시스템 최적화 툴 입니다.  
fzf 26 : 파일찾기에 유용한 툴 입니다.  
tmux 21 : 터미널 실수로 꺼도 똑같은 상태로 다시 킬 수 있는 툴 입니다.

> [리눅스에서 터미널 생활 즐기기.](http://black7375.tistory.com/15), [개발자들을 위한 툴 리스트](https://www.codentalks.com/t/topic/181)

| ![](https://raw.githubusercontent.com/oguzhaninan/Stacer/native/screenshots/header.png) | ![](https://extensions.gnome.org/extension-data/screenshots/screenshot_1320_zgXAduX.png) |  |
| --- | --- | --- |
| sudo add-apt-repository ppa:oguzhaninan/stacer<br> sudo apt-get install stacer| [NVIDIA GPU Stats Too](https://extensions.gnome.org/extension/1320/nvidia-gpu-stats-tool/) |  |

Terminator

Chrome install \(Ubuntu 16\)

```
snap install chrome
```

```
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb 
sudo apt-get install libxss1 libgconf2-4 libappindicator1 libindicator7
sudo dpkg -i google-chrome-stable_current_amd64.deb
```

멀티 부팅 
- [GRUB](http://programmingskills.net/archives/190) 설정
- GUI Tool : `sudo add-apt-repository ppa:danielrichter2007/grub-customizer` - `sudo apt-get install grub-customizer`



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



