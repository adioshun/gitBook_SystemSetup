# Python Setup

## 1. Pip

### 1.1 Installation

apt 이용 설치 

```bash
apt-get install -y python3-pip python3-dev
pip3 install --upgrade pip
```

스크립트 이용 설치 

```
wget https://bootstrap.pypa.io/get-pip.py
python get-pip.py

# Upgrade
pip install -U pip
```

### 1.2 Viretual Env.


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

## 2. Anaconda

### 2.1 Installation

```bash
# Python 3
wget https://repo.continuum.io/archive/Anaconda3-4.4.0-Linux-x86_64.sh
bash Anaconda3-4.4.0-Linux-x86_64.sh 

# Python 2
wget https://repo.continuum.io/archive/Anaconda2-4.4.0-Linux-x86_64.sh
bash Anaconda2-4.4.0-Linux-x86_64.sh 
```

### 2.2 Viretual Env.

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





Anaconda 삭제

```
conda install anaconda-clean   # install the package anaconda clean
anaconda-clean --yes           # clean all anaconda related files and directories 
rm -rf ~/anaconda3             # removes the entire anaconda directory

rm -rf ~/.anaconda_backup       # anaconda clean creates a back_up of files/dirs, remove it 
                                # (conda list; cmd shouldn't respond after the clean up)

```







