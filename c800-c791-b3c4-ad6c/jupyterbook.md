1. 원하는 템플릿 fork 
2. mkdir /workspace/jupyterbook
3. 포크하여 생성한 주소 활용 git clone [https://github.com/hunjung-lim/PCL\_For\_All.git](https://github.com/hunjung-lim/PCL_For_All.git)
4. pip3 install bs4 tqdm numpy pyyaml nbformat nbclean jupyter\_contrib\_nbextensions
5. vi Makefile : python -&gt; python3로 변경 
6. make book 

가상서버 설치   
1. sudo apt-get install ruby ruby-dev build-essential \#ruby-full

echo '\# Install Ruby Gems to ~/gems' &gt;&gt; ~/.bashrc  
echo 'export GEM\_HOME=$HOME/gems' &gt;&gt; ~/.bashrc  
echo 'export PATH=$HOME/gems/bin:$PATH' &gt;&gt; ~/.bashrc  
source ~/.bashrc

gem install jekyll bundler  
gem install budle  
sudo gem update --system  
gem update bundler  
bundle install  
make serve

---

댓글 상자 : [https://devmjun.github.io/archive/addComments](https://devmjun.github.io/archive/addComments)

[https://www.google.co.kr/search?q=github+disqus+사용법&oq=disqus+github&aqs=chrome.1.69i57j0l3.15105j0j4&client=tablet-android-samsung&sourceid=chrome-mobile&ie=UTF-8](https://www.google.co.kr/search?q=github+disqus+사용법&oq=disqus+github&aqs=chrome.1.69i57j0l3.15105j0j4&client=tablet-android-samsung&sourceid=chrome-mobile&ie=UTF-8)



jupyter theme : https://github.com/nsonnad/base16-ipython-notebook/blob/master/ipython-3/output/base16-google-light.css


https://github.com/transcranial/jupyter-themer


https://github.com/yirending/Jupyter-blog

https://github.com/yirending/Jupyter-blog