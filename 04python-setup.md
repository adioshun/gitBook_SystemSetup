
apt-get install python-pip python-dev build-essential 

# Python 설치 


## apt-get 이용 설치 

```python
$ sudo apt-get install python3 # 3.5설치 

## 3.6설치 
$ sudo apt-get install software-properties-common
$ sudo add-apt-repository ppa:deadsnakes/ppa
$ sudo apt-get update
$ sudo apt-get install python3.6


```
> Ubuntu리눅스 16.04에는 기본 3.5가 설치 되어 있음

[[3.6을 기본으로 바꾸기]](https://unipro.tistory.com/237)

```
# 등록
sudo rm /usr/bin/python
sudo update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.5 2
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.6 3

#선택
sudo update-alternatives --config python

```


---

# Python IDE

### 3.2 Debugger 



#### A. [PuDB](http://heather.cs.ucdavis.edu/~matloff/pudb.html)
![](http://heather.cs.ucdavis.edu/~matloff/pudb1.png)
```
easy_install pudb
```

#### B. [better-exceptions](https://github.com/Qix-/better-exceptions)

![](https://github.com/Qix-/better-exceptions/raw/master/screenshot.png)

```
$ pip install better_exceptions
import better_exceptions
```

--- 

## 4. Spyter
### 4.1 Spyder remote kernel connection
1. In jupyter run `%connect_info'
2. save it as `kernel.json`
3. Run spyder -

참고 : http://stackoverflow.com/questions/39007571/running-jupyter-with-multiple-python-and-ipython-paths

## 5. Pycharm

- 설치 : `snap install pycharm-community --classic`
- 제거 : `snap remove pycharm-community pycharm-professional`

OR

```
add-apt-repository ppa:mystic-mirage/pycharm
apt-get update
apt-get install pycharm-community
# apt-get install pycharm
```

### 5.1 Run line-by-line 

- `Execute Selection in console`

- Settings - go to Keymap - Search for `Execute Selection in Console` -  `Crl+Enter`

### 5.2 Tip

한글 입력 : 코드 상단에 `# -*- coding: utf-8 -*-`

- [PyCharm 5.x CE 에서 알아두면 좋을 기능들 정리.](http://blog.naver.com/passion053/220684364208)
- [Tip - pycharm 에서 tensorflow 라이브러리 import error ](http://suseok.egloos.com/4272210)
- PATH설정 : `sys.path.append('/workspace/mv3d/src')`


## 디버깅 

#### 가. Traccer()()이용 

- 디버깅 시작 하고자 하는 부분에 추가 `from IPython.core.debugger import Tracer; Tracer()() `
- 디버깅 모드 진입후 다음 명령어들 사용 
    - n(ext) line and run this one 
    - c(ontinue) running until next breakpoint
    - q(uit) the debugger

- 추가 : `from IPython.core.debugger import set_trace; set_trace()`
- 추가(쥬피터 외): ` import pdb; pdb.set_trace()`

#### 나. 쥬피터 노트북 셀 상단에 %%debug

#### 다. 함수 앞에 `%debug {함수}` 
    - `%debug test(10)`


--- 
## pyqt & QT Designer

```
apt-get install python-qt4 # conda install pyqt
apt-get install qt4-designer
./designer-qt4
```

conda이용 설치
```
conda install -c dsdale24 pyqt5 #python2.7 용
```
---

[Vim-Python-IDE](https://github.com/jarolrod/vim-python-ide)

