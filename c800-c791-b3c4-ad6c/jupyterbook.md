1. 원하는 템플릿 fork 
2. mkdir /workspace/jupyterbook
3. 포크하여 생성한 주소 활용 git clone https://github.com/hunjung-lim/PCL_For_All.git
4. pip3 install bs4 tqdm numpy pyyaml nbformat nbclean jupyter_contrib_nbextensions
5. vi Makefile : python -> python3로 변경 
6. make book 

가상서버 설치 
1. sudo apt-get install ruby ruby-dev build-essential #ruby-full

echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME=$HOME/gems' >> ~/.bashrc
echo 'export PATH=$HOME/gems/bin:$PATH' >> ~/.bashrc
source ~/.bashrc


gem install jekyll bundler
gem install budle
gem update --system
gem update bundler
bundle install
make serve



---

댓글 상자 : https://devmjun.github.io/archive/addComments

https://www.google.co.kr/search?q=github+disqus+%EC%82%AC%EC%9A%A9%EB%B2%95&oq=disqus+github&aqs=chrome.1.69i57j0l3.15105j0j4&client=tablet-android-samsung&sourceid=chrome-mobile&ie=UTF-8