# 추천 프로그램 

- [MobaXterm](ttps://mobaxterm.mobatek.net) : X11 server, tabbed SSH client, network tools

# FTP Server 

- [윈도우 FTP 서버](https://multicore-it.com/4)

# Ubuntu on Windows10

`Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux`

MS Stroe -> 선택 
> [참고](https://docs.microsoft.com/ko-kr/windows/wsl/install-win10)

# Fortforwarding


1. 모든 포트 포워딩 보기 (show port proxy) : netsh interface portproxy show all

2. 포트 포워딩 추가 ( add port proxy ) : netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=8082 connectaddress=192.168.99.252 connectport=8082
 > TCP 8082 포트를 Listen 한다음, 내부 네트워크의 192.168.99.252 서버의 TCP 8082 포트로 포워딩하도록 설정하는 명령어

3. 포트 포워딩 삭제 : 'netsh interface portproxy delete v4tov4 listenaddress=0.0.0.0 listenport=8082`

4. 방확벽 해제


---
# WSL

### [윈도우 Insider 참여 ](https://insider.windows.com/ko-kr/getting-started#flight)


### [WSL](https://docs.microsoft.com/ko-kr/windows/wsl/install-win10)


### [WSL2 초기 셋업부터 도커로 서버 실행까지](https://www.44bits.io/ko/post/wsl2-install-and-basic-usage)



# [windows -> ubuntu 원격 접속 (VNC)](https://tolovefeels.tistory.com/20)

```
$ sudo apt install vino -y
#설정 - 공유 - 비번 설정 
$ gsettings set org.gnome.Vino require-encryption false
```
