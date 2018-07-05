# Jupyter 


### 설치 

- pip 이용 설치 : `pip install jupyter` / `pip3 install jupyter`
- conda이용 설치 : `conda install -y ipython jupyter`

### Jupyter Configuration

```bash
mkdir ~/.jupyter && cd ~/.jupyter 
wget https://gist.githubusercontent.com/adioshun/383e4f722712e94dcc9a5d8c9fda2bf4/raw/bbd7d9a23af2af47cd518261c74ef10a2c304ccd/jupyter_notebook_config.py


jupyter notebook --generate-config --allow-root
vi /root/.jupyter/jupyter_notebook_config.py
```
`jupyter_notebook_config.py` 설정파일
```
# -*- coding: utf-8 -*- 

c.NotebookApp.ip = '*'  #L158
c.NotebookApp.open_browser = False # L201 원격접속으로 활용할 것이기 때문에 비활성화 시켰다.
c.NotebookApp.port = 8888 # L213 포트를 설정해준다. 기본포트로 8888이 자동 배정된다.
c.NotebookApp.password = 'sha1:a8dee43a3a44:b18f1ad149a60efb4838da44cf127985d64a5e70' # L210 python 실행후 from notebook.auth import passwd; passwd\(\)
# c.NotebookApp.password = 'sha1:a4a97a70ed7c:a554dcc8f9c76c44129cb9bce3d954a6d8bfc1ae' #snu2018
# c.NotebookApp.password = 'sha1:834d30a0973a:3ff75a3b19f0aa52bf6402eb8778b7fa6da04537' #ubuntu

c.NotebookApp.notebook_dir = '/workspace' # L195 기본 디렉터리를 지정시켜준다.
```


> 실행:jupyter notebook --allow-root

jupyter 테마: [#1](https://github.com/powerpak/jupyter-dark-theme), [#2](http://haanjack.github.io/jupyter/theme/2016/03/08/jupyter-theme.html), [#3](https://github.com/dunovank/jupyter-themes)

### Jupyter Lab설치

Unonffical
```
# you will need jupyter notebook >= v4.2
pip3 install jupyterlab
jupyter serverextension enable --py jupyterlab --sys-prefix
jupyter lab
```
### Jupyter Extension설치
##### Official:[ref](https://docs.continuum.io/anaconda/jupyter-notebook-extensions)
```
conda install nb_conda -c conda-forge
# conda install nb_conda
# conda install -c anaconda-nb-extensions nbpresent

```
> UnsatisfiableError: [solved](https://github.com/ContinuumIO/anaconda-issues/issues/1423)

- [K3D-jupyter](https://github.com/K3D-tools/K3D-jupyter): Jupyter notebook extension for 3D visualization.

##### Unofficial:[ref](https://jupyter-contrib-nbextensions.readthedocs.io/en/latest/install.html)
```
#conda install -c conda-forge jupyter_contrib_nbextensions
pip install https://github.com/ipython-contrib/jupyter_contrib_nbextensions/tarball/master
jupyter contrib nbextension install --user
```
[참고](https://github.com/ipython-contrib/jupyter_contrib_nbextensions)

### Jupyter 다중커널설정

> [conda install nb_conda](https://blog.naver.com/ryu_0108/221198673685)

기존 Jupyter에 새 커널 추가 하기 (conda사용시)

```
jupyter kernelspec list
source activate py2torch
python -m ipykernel install --user --name py2torch --display-name "Python2(pytorch)"
```

```
# conda2 OR 3 install 

conda create -n pyton3 python=3 ipykernel

jupyter kernelspec list #설치된 커널 확인 
conda install notebook ipykernel
ipython kernel install --user
```

또는 
```
python2 -m pip install ipykernel
python2 -m ipykernel install --user
```

> - [Docker 이미지로 설치한 Jupyter에 커널 추가하기](http://mazdah.tistory.com/784)
> - [Installing the IPython kernel](http://ipython.readthedocs.io/en/stable/install/kernel_install.html)


### [jupyter-tensorboard ](https://github.com/lspvic/jupyter_tensorboard)

> 주피터 노트북에서 tensorboard를 바로 킬 수 있게 해줍니다.

설치

```
1. Be sure that tensorflow(-gpu)>=1.3.0 has been installed.
2. Install the pip package: pip(3) install jupyter-tensorboard
```

실행

```
1. jupyter로 logdir로 이동한다.
2. new - tensorboard
3. 자동으로 실행되거나 run에 가면 실행시킨 tensorboard로 이동할 수 있습니다.
```



#### 2. Python 용 R 설치 \(/w conda\)

```bash
conda install -c r r-irkernel
conda install -c r r-essentials
```

###### ipython에서 R 패지키 설치 방법 \(R 콘솔에서 실행??\)

sudo apt-get install libcurl4-openssl-dev
sudo apt-get install libssl-dev
```
#R쉘진입
install.packages(c('repr', 'IRdisplay', 'evaluate', 'crayon', 'pbdZMQ', 'devtools', 'uuid', 'digest'))
devtools::install_github('IRkernel/IRkernel')
IRkernel::installspec()
```

> 패키지설치시: install.packages("ldavis", "/home/user/anaconda3/lib/R/library")
> [메뉴얼필독](https://www.r-bloggers.com/jupyter-and-r-markdown-notebooks-with-r/amp/)

-- 
# Jupyter Tips

- [28 Jupyter Notebook tips, tricks and shortcuts](https://www.dataquest.io/blog/jupyter-notebook-tips-tricks-shortcuts/)

## [부팅시 자동 실행](https://dymaxionkim.github.io/beautiful-jekyll/2017-01-23-Jupyter/) : 중간 부분 

## 슬라이드로 만들기 

- View → Cell Toolbar → Slideshow
- jupyter nbconvert Jupyter\ Slides.ipynb --to slides --post serve