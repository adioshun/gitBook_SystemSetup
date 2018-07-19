
# 초기 설정


2. Nvidia Driver (ubuntu 16, 18)

```
#sudo apt-get install ubuntu-drivers-common
ubuntu-drivers devices

ubuntu-drivers autoinstall
apt install nvidia-settings
#apt install nvidia-340
```

> [How to install the NVIDIA drivers on Ubuntu 18.04 Bionic Beaver Linux ](https://linuxconfig.org/how-to-install-the-nvidia-drivers-on-ubuntu-18-04-bionic-beaver-linux)



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

- 동영상 포맷 변환 : handbrake.fr

# Tip

## 공간 환보

```
apt-get clean

pip clean?
```

## 멀티 부팅 USB 만들기

- [MultiBootUSB](http://multibootusb.org/page_download/) : [설명](https://itsfoss.com/multiple-linux-one-usb/), `wget https://github.com/mbusb/multibootusb/releases/download/v8.8.0/python3-multibootusb_8.8.0-1_all.deb`


## 테마 

### [paper 테마 ](https://snwh.org/paper)



```python
sudo add-apt-repository -u ppa:snwh/ppa

sudo apt-get install paper-icon-theme
```


