Link OpenCV 4 into your Python 3 virtual environment 

```python 
# Let’s create a symbolic link from the OpenCV install directory to our virtual environment:

$ cd ~/.virtualenvs/cv/lib/python3.5/site-packages/
$ ln -s /usr/local/lib/python3.5/site-packages/cv2.cpython-35m-x86_64-linux-gnu.so cv2.so
$ cd ~
```

OSError: libcuda.so.1: cannot open shared object file:

`export LD_LIBRARY_PATH="/path/to/sdk/lib"`



## [Python Lib. 문제 ](https://ssaru.github.io/tip/2019/04/11/opencv_with_virtualenv.html?fbclid=IwAR38eKCpyR_dG9MbpxG5eKsoQwlV9uDuPPDQKPuUy9LWyjRX2CydveEnGcY)

python3 패키지가 설치되는 경로를 확인할 수 있다.

```python
import site
site.getsitepackages()
>> ['/usr/local/lib/python3.6/dist-packages', '/usr/lib/python3/dist-packages', '/usr/lib/python3.6/dist-packages']

```

패지명.so 파일 복사 `cv2.cpython-36m-x86_64-linux-gnu.so`