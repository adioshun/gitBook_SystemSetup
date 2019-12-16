### 스크린샷 GIF 생성

- [Peek - an animated GIF recorder](https://github.com/phw/peek): [설명글](https://www.omgubuntu.co.uk/2016/08/peek-desktop-gif-screen-recorder-linux)

```
sudo add-apt-repository ppa:peek-developers/stable
sudo apt update && sudo apt install peek
```


### mp4 to gif

```
$ ffmpeg -i da-play-full.mp4 -vf scale=1024:-1 -ss 00:00:04 -to 00:00:14 da-play-full.gif
```

## img to gif 


convert -delay 10 -loop 0 *.png animation.gif

## 녹화 

[우분투 16.04에서 obs로 화면 녹화하기](http://www.kwangsiklee.com/ko/2017/12/우분투-16-04에서-obs로-화면-녹화하기/)\)


## markdown용 스크린샷 URL생성 

[Gnome Shell extension for making and uploading screenshots](https://github.com/OttoAllmendinger/gnome-shell-screenshot)
```
sudo apt install gnome-shell-extensions
sudo apt install chrome-gnome-shell


visit : https://extensions.gnome.org/extension/1112/screenshot-tool/
```