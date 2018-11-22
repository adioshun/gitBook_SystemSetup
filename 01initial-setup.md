# 초기 설정

## Driver 

1. Nvidia Driver \(ubuntu 16, 18\)

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

* leafpad : `sudo apt install leafpad`

## Terminal 

* 멀티창 : `sudo apt-get install terminator`



## 테마 

> [paper 테마](https://snwh.org/paper)

```python
sudo add-apt-repository -u ppa:snwh/ppa
sudo apt-get update
sudo apt-get install paper-icon-theme paper-gtk-theme paper-icon-theme
#추천 배경 : http://www.technocrazed.com/wp-content/uploads/2015/12/Linux-Wallpaper-32.png
```

gnome-tweak를 통해 변경 


## Chrome

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

추천 로고 : https://www.windowslatest.com/wp-content/uploads/2017/07/Ubuntu-on-Windows-10-696x348.jpg



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

* 동영상 포맷 변환 : handbrake.fr


## 공간 환보

```
apt-get clean

pip clean?
```

## 멀티 부팅 USB 만들기

* [MultiBootUSB](http://multibootusb.org/page_download/) : [설명](https://itsfoss.com/multiple-linux-one-usb/), `wget https://github.com/mbusb/multibootusb/releases/download/v8.8.0/python3-multibootusb_8.8.0-1_all.deb`


\[ 우분투 16.04에서 obs로 화면 녹화하기\)\([http://www.kwangsiklee.com/ko/2017/12/우분투-16-04에서-obs로-화면-녹화하기/](http://www.kwangsiklee.com/ko/2017/12/우분투-16-04에서-obs로-화면-녹화하기/)\)

googler 71 : 터미널에서 구글 검색을 할 수 있게 해줍니다.  
stacer 31 : 리눅스의 시스템 최적화 툴 입니다.  
fzf 26 : 파일찾기에 유용한 툴 입니다.  
tmux 21 : 터미널 실수로 꺼도 똑같은 상태로 다시 킬 수 있는 툴 입니다.

> [리눅스에서 터미널 생활 즐기기.](http://black7375.tistory.com/15), [개발자들을 위한 툴 리스트](https://www.codentalks.com/t/topic/181)


