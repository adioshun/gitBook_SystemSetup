# 초기 설정



## 1. 필수 패키지 설치

```
# [Repository 변경 ](https://webdir.tistory.com/201)
# [Ubuntu Sources List Generator](https://repogen.simplylinux.ch/index.php)
$ sudo vi /etc/apt/sources.list
:%s/kr.archive.ubuntu.com/ftp.kaist.ac.kr/g
:%s/kp.archive.ubuntu.com/ftp.kaist.ac.kr/g
:%s/archive.ubuntu.com/ftp.kaist.ac.kr/g
:%s/ftp.daum.net/ftp.kaist.ac.kr/g

# leafpad :https://go.microsoft.com/fwlink/?LinkID=760868
$ sudo apt install leafpad

# Terminal 
$ sudo apt-get install terminator

# Guake
$ sudo add-apt-repository ppa:linuxuprising/guake
$ sudo apt-get update
$ sudo apt install guake
$ sudo add-apt-repository --remove ppa:linuxuprising/guake #제거 

# Double Commander 
sudo add-apt-repository ppa:alexx2000/doublecmd
sudo apt-get install doublecmd-gtk #QT버젼 doublecmd-qt


# 크롬 
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb 
sudo apt-get install libxss1 libgconf2-4 libappindicator1 libindicator7
sudo dpkg -i google-chrome-stable_current_amd64.deb

# Tor Browser
$ sudo apt install torbrowser-launcher

# VScode : https://code.visualstudio.com/
wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"
sudo apt update 
sudo apt install code

> Download Link : https://go.microsoft.com/fwlink/?LinkID=760868

# gnome-tweak


# mathpix
https://mathpix.com/

#screen Shot
설치 : ubuntu software - Add on - Shell Extensiotn - Screenshot Tool 
https://extensions.gnome.org/extension/1112/screenshot-tool/
https://github.com/OttoAllmendinger/gnome-shell-screenshot/

#원격 제어(윈도우)
$ snap install remmina

$ sudo add-apt-repository ppa:remmina-ppa-team/remmina-next
$ sudo apt-get update
$ sudo apt-get install remmina remmina-plugin-* libfreerdp-plugins-standard

#NeoVIM
$ sudo apt-get install neovim
$ sudo apt-get install python3-neovim
https://neovim.io/

# FishShell
https://fishshell.com/
https://okky.kr/article/454099
```

- 런치바 위치 변경(gsettings set com.canonical.Unity.Launcher launcher-position Bottom`

---

## 2. 멀티 부팅

* [GRUB](http://programmingskills.net/archives/190) 설정
* GUI Tool : `sudo add-apt-repository ppa:danielrichter2007/grub-customizer` - `sudo apt-get install grub-customizer`

추천 로고 : [https://www.windowslatest.com/wp-content/uploads/2017/07/Ubuntu-on-Windows-10-696x348.jpg](https://www.windowslatest.com/wp-content/uploads/2017/07/Ubuntu-on-Windows-10-696x348.jpg)

## 3. 테마

> [paper 테마](https://snwh.org/paper)

```python
sudo add-apt-repository -u ppa:snwh/ppa
sudo apt-get update
sudo apt-get install paper-icon-theme paper-gtk-theme paper-icon-theme
#추천 배경 : http://www.technocrazed.com/wp-content/uploads/2015/12/Linux-Wallpaper-32.png
```

gnome-tweak를 통해 변경

---

# 추천 유틸

* 동영상 포맷 변환 : handbrake.fr

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

#Type:        Application 
#Name:        FreeFileSync
#Command:   /opt/FreeFileSync/FreeFileSync        
#Comment:   Folder Comparison and Synchronization
```

### [FreeFileSync](https://www.tecmint.com/freefilesync-compare-synchronize-files-in-ubuntu/)

```
$ wget https://freefilesync.org/download/FreeFileSync_10.10_Linux.tar.gz
$ cd Downloads/
$ sudo tar xvf FreeFileSync_*.tar.gz -C /opt/
$ cd /opt/
$ ls
$ sudo unzip FreeFileSync/Resources.zip -d /opt/FreeFileSync/Resources/
$ ./FreeFileSync
```

> ISO to USB : [balena](https://www.balena.io/etcher/)


---

Local host Search : `nmap -n -sP 192.168.137.0/24`
