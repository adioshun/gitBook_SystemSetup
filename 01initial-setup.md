# 초기 설정


1. 한글 입력 [패키지 설정](http://awesometic.tistory.com/87), [단축키 설정](https://steemkr.com/ubuntu/@sjc333/18-04) 

> Korean(hangul)이 안보이면 reboot 

```
sudo add-apt-repository ppa:createsc/3beol
apt-get update; sudo apt-get install ibus ibus-hangul
ibus-setup #ibus-setup-hangul
```

2. Nvidia Driver (ubuntu 16, 18)

```
#sudo apt-get install ubuntu-drivers-common
ubuntu-drivers devices

ubuntu-drivers autoinstall
apt install nvidia-settings
#apt install nvidia-340
```

> [How to install the NVIDIA drivers on Ubuntu 18.04 Bionic Beaver Linux ](https://linuxconfig.org/how-to-install-the-nvidia-drivers-on-ubuntu-18-04-bionic-beaver-linux)


3. Chrome install (Ubuntu 16)

```
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb sudo apt-get in
sudo apt-get install libxss1 libgconf2-4 libappindicator1 libindicator7
sudo dpkg -i google-chrome-stable_current_amd64.deb
```


4. 멀티 부팅 [GRUB](http://programmingskills.net/archives/190) 설정 



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

---

## Editor

* [TOast UI](https://nhnent.github.io/tui.editor/)

---




## github 키 등록

```
    1  ssh-keygen -t rsa -b 4096 -C "hunjung.lim@samsung.com"
    2  eval $(ssh-agent -s)
    3  ssh-add ~/.ssh/id_rsa
    4  clip < ~/.ssh/id_rsa.pub  ## 복사된 내용을 킷허브 설정 페이지에서 키 추가
```

## git 폴더 연동

```
#로컬 폴더로 이동 
$ git init
$ git remote add origin {https://github....}

# 소스 가져 오기 
$ git pull origin master #remote의 소스 가져 오기 

# 소스 업로드 하기 
$ git add *
$ git commit -m ""
$ git push --set-upstream origin master
```

## [git 자동 완성 트릭](https://github.com/progit/progit/blob/master/ko/02-git-basics/01-chapter2.markdown#자동완성)

# 추천 유틸

## 1. 디스크공간

```
apt-get install ncdu
```

# Tip

## 공간 환보

```
apt-get clean

pip clean?
```

## 멀티 부팅 USB 만들기

- [MultiBootUSB](http://multibootusb.org/page_download/) : [설명](https://itsfoss.com/multiple-linux-one-usb/), `wget https://github.com/mbusb/multibootusb/releases/download/v8.8.0/python3-multibootusb_8.8.0-1_all.deb`

