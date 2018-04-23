

## 5. X11 Forwarding


|      |     | 서버                                                                                                             | 클라이언트                                   | 클라이언트                                                                                                       |
|------|-----|------------------------------------------------------------------------------------------------------------------|----------------------------------------------|------------------------------------------------------------------------------------------------------------------|
|      |     | 리눅스                                                                                                           | 윈도우                                       | 리눅스                                                                                                           |
| 설치 | 1.1 | apt-get install xauth                                                                                            | xmanager                                     | apt-get install auth                                                                                             |
|      | 1.2 | apt-get   install x11-xserver-utils #xhost                                                                       |                                              |                                                                                                                  |
| 설정 | 2.1 | tourch   ~/.Xauthority      <br>(권한 문제로 새로 만드는것이 속편함)                                                 |                                              |                                                                                                                  |
|      | 2.2 | /etc/ssh/sshd_config        <br>. x11forwarding = yes                                                                | 연결-SSH-터널링-X11포워딩     . Xmanager선택 | /etc/ssh/sshd_config        <br>. x11forwarding = yes                                                                |
|      | 2.3 | /etc/ssh/ssh_config   또는 ~/.ssh/config     <br>. ForwardAgent yes     <br>. ForwardX11 yes     <br>. ForwardX11Trusted yes |                                              | /etc/ssh/ssh_config   또는 ~/.ssh/config     <br>. ForwardAgent yes     <br>. ForwardX11 yes     <br>. ForwardX11Trusted yes |
|      | 2.4 | export   DISPLAY=localhost:0.0                                                                                   |                                              |                                                                                                                  |
|      | 2.5 | xhost   +        <br>(모든 호스트에 대해 그래픽 요청을 허용)                                                         |                                              |                                                                                                                  |
|      | 3.1 | rm   ~/.Xauthority-{c}                                                                                           |                                              |                                                                                                                  |
|      | 3.2 | echo   $DISPLAY    <br> . 예상 결과 : localhost:0.0                                                                  |                                              |                                                                                                                  |
|      | 3.3 | xhost <br>. 예상 결과 : Access control disabled                                                                      |                                              |                                                                                                                  |
| 접속 | 4   |                                                                                                                  |                                              | ssh -X   [IP주소]                                                                                                |
| 에러 | 5.1 | Can't   open display      <br>. /etc/profile 에 export Display하지 않기, 동적으로 할당되기 떄문에 강제 설정          |                                              | The   remote SSH server rejected X11 forwarding request.                                                         |
|      | 5.2 | "X11   connection rejected because of wrong authentication"     <br>. tourch ~/.Xauthority                           |                                              |                                                                                                                  |
|      |     |                                                                                                                  |                                              |                                                                                                                  |



> [중요] **xmanager**사용시 하기 설정 상관 없이 GUI실행 가능 

> Docker에서 x11접속은 [여기](https://github.com/adioshun/System_Setup/wiki/3_Docker-Setup#docker%EC%97%90%EC%84%9C-guix11-%EC%8B%A4%ED%96%89%ED%95%98%EA%B8%B0)랑 [여기ROS Wiki: Dokcer Tutorial/GUI](http://wiki.ros.org/docker/Tutorials/GUI)참고

## 도커 실행 
```bash
- docker run -it --rm \
   --net host \ 
   --env="DISPLAY" \
   -p 2233:2233 \
   --volume "$HOME/.Xauthority:/root/.Xauthority:rw" \
   {Docker Image ID} /bin/bash

docker run -it --net host --env="DISPLAY" -e USER=root --privileged -p 1122:1122 -p 1188:1188 --volume "$HOME/.Xauthority:/root/.Xauthority:rw" -v /workspace/docker_{}:/workspace {Docker Image ID} /bin/bash
```

> SSH접속을 위해서는 도커 컨테이너의 port번호를 22에서 다른것으로 변경 후 dock를 실행한 서버에서 ssh -X -p 2233 root@IP 로 접속 시도  

