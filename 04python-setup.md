
apt-get install python-pip python-dev build-essential 


> [주피터설정하기](https://github.com/adioshun/Blog_Jekyll/blob/master/2017-07-18_Jupyter_setup.md)

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

