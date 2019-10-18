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



---


## [gcc default include path 확인 방법](http://jinyongjeong.github.io/2016/06/06/gcc_default_include_path_confirm/)
> echo | gcc -v -x c -E - 
> echo | gcc -v -x c++ -E - 


## 제대로 라이브러리를 가져오는지 알아보려면

sudo ldconfig -v

---

## 9. 에러 처리 
`apt-get install -y libcupti-dev`  #에러발생 "/sbin/ldconfig.real: /usr/lib/nvidia-375/libEGL.so.1 is not a symbolic link"

libcudnn.so.5: cannot open shared object file 
- `LD_LIBRARY_PATH="/usr/local/cuda-8.0/targets/x86_64-linux/lib` #ldconfig -v 로해당라이브러리위치확인
- `export LD_LIBRARY_PATH` # 확인 : `echo $LD_LIBRARY_PATH`



`for fn in libpcl_*.so.1.9.1; do sudo ln -s $fn ${fn%.1.9.1}.1.7; done`