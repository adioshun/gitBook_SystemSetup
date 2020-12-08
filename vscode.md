# VSCODE

download : https://code.visualstudio.com/docs/?dv=linux64_deb


sudo dpkg -i ~/Downloads/code_*.deb; sudo apt -f install


- python PATH : file -> preferences -> settings (User Settings)


```
#/home/adioshun/.vscode/extensions/ms-python.python-2018.6.0/node_modules/vscode-uri/.vscode/settings.json
{
    "python.pythonPath":"/home/cvr/.pyenv/shims/python"
}
```

Press Ctrl+Shift+P to hit “task” to find Tasks: Configure Default Build Task, modify the contents of 
tasks.json (task.json)
    "command":"/home/cvr/.pyenv/shims/python"
    
    
> http://robotslam.blogspot.com/2018/03/open3d-modern-library-for-3d-data.html


## ssh이용 원격 접속 

비밀번호 인증을 지원하지 않는다고 함 
```
$ ssh-keygen -t rsa -b 4096
$ scp -P 22 <유저홈디렉터리패스>/.ssh/id_rsa.pub 서버계정명@서버주소

#리모트 서버에서 
$ mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat ~/tmp.pub >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys && rm -f ~/tmp.pub
```
출처: https://noooop.tistory.com/entry/VS-Code-Remote-사용하기-SSH-방식 [잡동사니]

## [[Python] Visual Studio Code를 파이썬 IDE 로 이용해 보기](http://egloos.zum.com/mcchae/v/11262544)

---

## GIST 연동 

https://github.com/settings/tokens -> [Generate New Token] -> "gist" 체크 


VS Code 
1. gist확장프로그램 설치 (gist, Ken howard)
2. Command+Shift+p -> [Gist: select profile] -> 토큰 입력 
3. Command+Shift+p -> [Gist:Insert Text From Gist File] -> 사용할 snippet 지정 
4. https://github.com/mre/vscode-snippet  /gistpad 
eda5663d223b154e240b23302f4f7244ba5df11c
