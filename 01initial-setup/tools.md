## 시스템 모니터링 

| ![](https://raw.githubusercontent.com/oguzhaninan/Stacer/native/screenshots/header.png) | ![](https://extensions.gnome.org/extension-data/screenshots/screenshot_1320_zgXAduX.png) |  |
| --- | --- | --- |
| sudo add-apt-repository ppa:oguzhaninan/stacer<br> sudo apt-get install stacer| [NVIDIA GPU Stats Too](https://extensions.gnome.org/extension/1320/nvidia-gpu-stats-tool/) |  |




## CPU/GPU Monitoring

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


```pythn 
#Ubuntu disco (19.04)
sudo apt install nvtop 

#Ubuntu Older
sudo apt install cmake libncurses5-dev libncursesw5-dev git
git clone https://github.com/Syllo/nvtop.git
mkdir -p nvtop/build && cd nvtop/build
cmake ..

# If it errors with "Could NOT find NVML (missing: NVML_INCLUDE_DIRS)"
# try the following command instead, otherwise skip to the build with make.
cmake .. -DNVML_RETRIEVE_HEADER_ONLINE=True

make
make install # You may need sufficient permission for that (root)
```

|![](https://1.bp.blogspot.com/-kUlBy_kIszQ/XROIGBi-0HI/AAAAAAAAC-4/d1wfWb2Y7546ICSJedmrT8DLygUZ6fyFwCLcBGAs/s640/nvtop-screenshot.png)||
|-|-|
|nvtop||


> [GPU Useage를 편하게 모니터 해보자!](https://eungbean.github.io/2018/08/29/gpu-monitor-with-byobu/?fbclid=IwAR3Rv0iPd1PJjEogujyxWBWjJyLknu_QLxexY_OfIyrOTaLsAADEzFagpRE)

## Netwkrok 

### 1 총 사용량 

![](https://i.imgur.com/YULmu8j.png)

```python 
$ sudo apt-get install vnstat
$ sudo vnstat -u -i <interface>  #initialize a database first
$ sudo systemctl start vnstat.service


# https://askubuntu.com/questions/15836/how-to-track-the-total-network-data-in-a-month
```
```
sudo add-apt-repository ppa:teejee2008/ppa
sudo apt-get update
sudo apt-get install conky-manager
```




### 2프로그램별 사용량 


![](https://cloud.githubusercontent.com/assets/6733770/16925237/a1363dd0-4cf9-11e6-81aa-83a52e72c549.png)
https://github.com/mbfoss/gnethogs

### 3 통합 
![](https://2.bp.blogspot.com/-9gAsuNMF_P0/WKsaGcx_DgI/AAAAAAAAMk4/kiIw6OIkTa0VOCCofNsNT6W5hgHeV2yLwCLcB/s1600/bmon.png)



* bmon : 'apt install bmon`



### 3 대역폭 

![](https://cdn.clien.net/web/api/file/F01/10702478/c7ac83f4a1429.png)

https://www.clien.net/service/board/lecture/15489366


## 학습 모니터링

* 앱기반 : [Hyperdash](https://hyperdash.io/), `pip install hyperdash && hyperdash login`
* 웹기반 : [LabNotebook](https://github.com/henripal/labnotebook)

| ![](http://i.imgur.com/QCEGtYx.png) | ![](https://github.com/henripal/labnotebook/raw/master/nbs/img/labnotebook.gif) |
| --- | --- |
| Hyperdash | LabNotebook |



## 모니터링 by-your-own

### 1. [termui](https://github.com/gizak/termui)

![](https://github.com/gizak/termui/raw/master/_assets/demo.gif)

---

## 공간 모니터링 




- 디스크공간(CLI) `apt-get install ncdu`



```
sudo add-apt-repository ppa:nathan-renniewaldock/qdirstat
sudo apt-get update
sudo apt install qdirstat
```

![](https://static.makeuseof.com/wp-content/uploads/2017/08/muo-linux-diskusagetools-qdirstat.png)




--- 


## 공간 확보 

> https://www.omgubuntu.co.uk/2016/08/5-ways-free-up-space-on-ubuntu

자동 제거 툴 : BleachBit #sudo apt install bleachbit

```
apt-get clean
pip clean?

#용량큰 폴더는 
$ cd /
$ sudo du -ckx | sort -n > /tmp/duck-root
```

- 이전 커널 삭제 
```
uname -r #현커널 확인
dpkg --list|grep linux-image #설치된 커널 확인 
apt-get purge linux-headers-xx linux-headers-xx-heneric linux-image-xx-generic linux-image-extra-xx-genric 
#이전 버젼 만 제거 # apt-get purge linux-image-
```

```python 
journalctl --disk-usage
sudo vi /etc/systemd/journald.conf #SystemMaxUse=50M
cd /var/log/journal/
```

[좋은도커이미지만들기](https://dayone.me/1740z5r)

### 1. Clean the APT Cache (And Do It Regularly)

```
sudo apt-get autoremove
#sudo apt-get autoclean
#sudo apt-get clean
#du -sh /var/cache/apt/archives
```

## 2. Remove Old Kernels (If No Longer Required)
```
sudo apt-get autoremove --purge
```








