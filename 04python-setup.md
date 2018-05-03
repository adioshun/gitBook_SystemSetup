# Python Setup

## 1. Pip 설치

- apt 이용 설치 
```bash
apt-get install -y python3-pip python3-dev
pip3 install --upgrade pip
```

- 스크립트 이용 설치 

```
wget https://bootstrap.pypa.io/get-pip.py
python get-pip.py

# Upgrade
pip install -U pip
```




## 2. Anaconda 이용 설치 

- Anaconda설치
```bash
# Python 3
wget https://repo.continuum.io/archive/Anaconda3-4.4.0-Linux-x86_64.sh
bash Anaconda3-4.4.0-Linux-x86_64.sh 

# Python 2
wget https://repo.continuum.io/archive/Anaconda2-4.4.0-Linux-x86_64.sh
bash Anaconda2-4.4.0-Linux-x86_64.sh 
```
## 3. Additional SW
### 3.1 OpenCV

- OpenCv 

```bash
# python3
conda install -c https://conda.binstar.org/menpo opencv3 #conda install -c menpo opencv3=3.2.0
pip3 install opencv_python

#pytbon2
pip install opencv_python

# apt-get install libgtk2.0-0 #opencv에러시
```

- Anaconda 삭제

```
conda install anaconda-clean   # install the package anaconda clean
anaconda-clean --yes           # clean all anaconda related files and directories 
rm -rf ~/anaconda3             # removes the entire anaconda directory

rm -rf ~/.anaconda_backup       # anaconda clean creates a back_up of files/dirs, remove it 
                                # (conda list; cmd shouldn't respond after the clean up)

```




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

## 3. 가상공간 환경 사용하기 


### 3.1 Conda 

```bash
conda create -n venv

source activate venv
source deactivate venv
```


###### [Tip] [requirement.yml](https://github.com/datitran/Object-Detector-App/blob/master/environment.yml) 파일이용하여 한번에 가상환경 설치하기

- requirement.yml 파일 생성 하기 : conda env export > environment.yml

```
name: object-detection
channels: !!python/tuple
- menpo
- defaults
dependencies:
- freetype=2.5.5=2
- jbig=2.1=0
- pip:
  - backports.weakref==1.0rc1
prefix: /Users/datitran/anaconda/envs/object-detection
```

- `conda env create -f environment.yml --name python2`

### 3.2 pip 

```bash
pip install virtualenv
virtualenv -p /usr/bin/python2.7 venv
virtualenv --python=python3.3 venv
virtualenv --system-site-packages venv # global패키지 사용이 가능하다는 듯


source venv/bin/activate
deactivate
```


###### [Tip] requirements.txt 파일이용하여한번에설치하기

- requirements.txt파일 생성 하기 : pip freeze > requirements.txt

```
Cython>=0.19.2
numpy>=1.7.1
...
pyyaml>=3.10
Pillow>=2.3.0
```

- pip 
	- `for req in $(cat requirements.txt); do pip install $req | cut -d ">" -f1; done`
	- `pip install .`
    - `pip install -r requirements.txt`
- conda : `while read requirement; do conda install --yes $requirement; done < requirements.txt`

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

