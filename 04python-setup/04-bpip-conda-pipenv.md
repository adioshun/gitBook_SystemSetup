# Python Setup

## 1. Pip

SSl오류시 : `python -m pip install --trusted-host pypi.org --trusted-host files.pythonhosted.org [패키지명]`
```
vi ~/.pip/pip.conf 
[global]
trusted-host = pypi.org files.pythonhosted.org
```


### 1.1 Installation

apt 이용 설치 

```bash
apt-get install -y python3-pip python3-dev  #python3.5도 같이 설치 
pip3 install --upgrade pip
```

특정 버젼만 설치시 스크립트 이용 설치 

```
wget https://bootstrap.pypa.io/get-pip.py
python3.6 get-pip.py

# Upgrade
pip install -U pip
# Python pip 업데이트 후 from pip import main ImportError: cannot import name 'main' 에러  --> `$ hash -d pip3`
```



### 1.2 가상환경 

#### A. virtulenv

```bash
pip install virtualenv
virtualenv -p /usr/bin/python2.7 venv
virtualenv --python=python3.3 venv
virtualenv --system-site-packages venv # global패키지 사용이 가능하다는 듯


source venv/bin/activate
deactivate
```




#### B. [pipenv](https://velog.io/@doondoony/pipenv-101)

- pip와 virtualenv를 따로 쓸 필요가 없다. 동시에 사용이 된다.
- Pipenv는 Pipfile와 Pipfile.lock을 requirements.txt를 대신하여 사용한다.
- 해쉬가 자동생성된다. (보안)
- 의존성 그래프를 제공함으로서 insight를 제공한다 (e.g. $ pipenv graph).
- `.env` 파일들을 사용한 스트림라인 개발 워크플로우

```python 
# 설치 
$sudo pip install pipenv

#가상환경 생성 
pipenv --python 3.6 #python 3.6버전을 기준으로 한 프로젝트가 생성

#가상환경내 실행 
pipenv [명령어] 
pipenv install 

# 정리 
$ mkdir pipenv-test && cd pipenv-test  # 테스트해볼 폴더를 하나 만들어요
$ pipenv shell  # 이따 설명하겠지만, 가상환경 시작 명령어 입니다
(pipenv-test-tXHIoQb5) $ python --version  # 이제 더 이상 python3 --version 이 아닌점도 참고하세요
(pipenv-test-tXHIoQb5) $ pipenv --rm  # 확인했으니 지웁시다
(pipenv-test-tXHIoQb5) $ exit  # 이 쉘에서 나가서


pipenv shell --fancy  

Run `pipenv install` to create a new empty pipenv virtualenv
Run `pipenv shell`.

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


SSL오류시 : conda config --set ssl_verify false


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







