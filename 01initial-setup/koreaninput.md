# Korean Input

## ubuntu 18

> 한글 입력 [패키지 설정](http://awesometic.tistory.com/87), [단축키 설정](https://steemkr.com/ubuntu/@sjc333/18-04)

1. `Gnome Tweaks` - `Additional Layout Options` - `Korean Hangul/Hanja Keys` $ `Hardware Hangul/Hanja keys`
2. `Ubuntu 설정` - `Region & Language` - `Manage Installed Languages` - Install `ibus-hangul` 
3. `Input sources` + `+` + `Korean (hangul)` #Korean\(hangul\)이 안보이면 reboot
4. `English (US)` 는 `-` 버튼으로 없애고, `Korean (Hangul)` 하나만 남게
5. "Hangul toggle key" 에서 "Hangul" 하나만 남게 "Add", "Remove"

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile9.uf.tistory.com%2Fimage%2F9982113359F6992C214038)

```
# Manual Install [ibus-hangul]
sudo add-apt-repository ppa:createsc/3beol
sudo apt-get update; sudo apt-get install ibus ibus-hangul
ibus-setup #ibus-setup-hangul
```

[uim이용 설정](http://progtrend.blogspot.com/2018/06/ubuntu-1804-uim.html)

## ubuntu 16

ibus : [http://techlog.gurucat.net/288](http://techlog.gurucat.net/288)

```
sudo add-apt-repository ppa:createsc/3beol
sudo apt-get update; sudo apt-get install ibus ibus-hangul
```

키 맵핑

```
System Settings에서 Keyboard 실행
Shortcuts 탭 선택한 후 Typing 항목 선택
모든 항목(Switch to next source, Switch to previous source, Alternative Characters Key)을 Disabled로 설정(Back 키를 누르면 Disabled가 됨)
Compose Key 항목을 Right Alt로 변경
```

fcitx : [http://androidtest.tistory.com/52](http://androidtest.tistory.com/52), [http://snowdeer.github.io/linux/2018/01/21/ubuntu-16p04-install-korean-keyboard/](http://snowdeer.github.io/linux/2018/01/21/ubuntu-16p04-install-korean-keyboard/)

---

## NIMF

```
sudo add-apt-repository ppa:hodong/nimf
sudo apt-get update
sudo apt install nimf nimf-libhangul

https://ncube.net/13986
```

# 한글키다 동작 하지 않을경우 

> 일부 키보드의 경우 [hangul]키를 [alt_R]로 인식 하여서 발생 

```c
xmodmap -e 'remove mod1 = Alt_R' # Alt_R의 기본 키 매핑 제거
xmodmap -e 'keycode 108 = Hangul' # Alt_R을 Hangul 키로 매핑

# 키 매핑 영구 저장
xmodmap -pke > ~/.Xmodmap

#출처 : https://kwonnam.pe.kr/wiki/linux/xmodmap
```

- [우분투 리눅스 한영전환/한자키(오른쪽 Alt Ctrl) 안 될 때](https://jimnong.tistory.com/939)

