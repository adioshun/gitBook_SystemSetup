# 초기 설정



## Editor

* leafpad : `sudo apt install leafpad`

* code : 'https://go.microsoft.com/fwlink/?LinkID=760868'

## Terminal 

* 멀티창 : `sudo apt-get install terminator`

* guake : `sudo apt-get install guake`  #/usr/bin/guake ㅅㅣㅈㅏㄱㅁㅔㄴㅠ ㅊㅜㄱㅏ 




## Chrome

```
snap install chrome
```

```
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb 
sudo apt-get install libxss1 libgconf2-4 libappindicator1 libindicator7
sudo dpkg -i google-chrome-stable_current_amd64.deb
# sudo dpkg -P google-chrome-stable #삭제 
```

## VScode

https://code.visualstudio.com/


---



### 멀티 부팅 
- [GRUB](http://programmingskills.net/archives/190) 설정
- GUI Tool : `sudo add-apt-repository ppa:danielrichter2007/grub-customizer` - `sudo apt-get install grub-customizer`

추천 로고 : https://www.windowslatest.com/wp-content/uploads/2017/07/Ubuntu-on-Windows-10-696x348.jpg

## 테마 

> [paper 테마](https://snwh.org/paper)

```python
sudo add-apt-repository -u ppa:snwh/ppa
sudo apt-get update
sudo apt-get install paper-icon-theme paper-gtk-theme paper-icon-theme
#추천 배경 : http://www.technocrazed.com/wp-content/uploads/2015/12/Linux-Wallpaper-32.png
```

gnome-tweak를 통해 변경 


## apt-source list update 

[Ubuntu Sources List Generator](https://repogen.simplylinux.ch/index.php)


---



# 추천 유틸



- 동영상 포맷 변환 : handbrake.fr


## 공간 환보

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
apt-get purge linux-headers-xx linux-headers-xx-heneric linux-image-xx-generic linux-image-extra-xx-genric 
#boot공간 확보 
```


- 디스크공간 `apt-get install ncdu`

![](https://static.makeuseof.com/wp-content/uploads/2017/08/muo-linux-diskusagetools-qdirstat.png)

```
sudo add-apt-repository ppa:nathan-renniewaldock/qdirstat
sudo apt-get update
sudo apt install qdirstat
```



## 멀티 부팅 USB 만들기

* [MultiBootUSB](http://multibootusb.org/page_download/) : [설명](https://itsfoss.com/multiple-linux-one-usb/), `wget https://github.com/mbusb/multibootusb/releases/download/v8.8.0/python3-multibootusb_8.8.0-1_all.deb`


\[ 우분투 16.04에서 obs로 화면 녹화하기\)\([http://www.kwangsiklee.com/ko/2017/12/우분투-16-04에서-obs로-화면-녹화하기/](http://www.kwangsiklee.com/ko/2017/12/우분투-16-04에서-obs로-화면-녹화하기/)\)

googler 71 : 터미널에서 구글 검색을 할 수 있게 해줍니다.  
stacer 31 : 리눅스의 시스템 최적화 툴 입니다.  
fzf 26 : 파일찾기에 유용한 툴 입니다.  
tmux 21 : 터미널 실수로 꺼도 똑같은 상태로 다시 킬 수 있는 툴 입니다.

> [리눅스에서 터미널 생활 즐기기.](http://black7375.tistory.com/15), [개발자들을 위한 툴 리스트](https://www.codentalks.com/t/topic/181)


## shortcut 

```
$ sudo apt-get install --no-install-recommends gnome-panel
$ sudo gnome-desktop-item-edit /usr/share/applications/ --create-new

#Type: 	   Application 
#Name: 	   FreeFileSync
#Command:   /opt/FreeFileSync/FreeFileSync		
#Comment:   Folder Comparison and Synchronization
```


