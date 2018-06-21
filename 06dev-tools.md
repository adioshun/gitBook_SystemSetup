


1. Home 디렉토리의 .profile 또는 /etc/.profile 에 라이브러리를 추가

* .profile은 숨김파일로 ls -al 옵션을 함께 줘야 보여진다.

 

2. 쉘에서 Path 지정.

- LD_LIBRARY_PATH=/home/user/lib:$LD_LIBRARY_PATH

- export LD_LIBRARY_PATH

 

3. /etc/enviroment에 입력하거나, bash가 시작될때 불러올 수 있도록 home 디렉토리의 .bashrc에 입력



끝으로 우분투에서 필요한 공유 라이브러리들은 ldconfig를 사용해서 설정하는 것을 권장.

/etc/ld.so.conf.d/ 디렉토리 아래에 .conf 확장자를 가진 파일들을 적절한 이름으로 만들어주고.

그안에 가져올 라이브러리의 경로를 적어주면,

/sbin/ldconfig.real이 conf 파일들을 모아서 라이브러리 경로를 추가해준다.

 

라이브러리 경로는 LD_LIBRARY_PATH=/usr/~ 이런식으로 쓰는게 아니라.

파일 시작에 바로 경로가 나와야 한다.

 

제대로 라이브러리를 가져오는지 알아보려면

sudo ldconfig -v | grep {이름}

 
