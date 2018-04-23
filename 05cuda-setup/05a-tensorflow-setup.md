# Tensorflow Setup

> GPU설치시 [CUDA installation](https://github.com/adioshun/Blog_Jekyll/blob/master/2017-07-18-CUDA_CuDNN_Installation.md)

## 1. pip 이용 설치

```bash
$ pip install tensorflow      # Python 2.7; CPU support (no GPU support)
$ pip3 install tensorflow     # Python 3.n; CPU support (no GPU support)
$ pip install tensorflow-gpu  # Python 2.7;  GPU support
$ pip3 install tensorflow-gpu # Python 3.n; GPU support 
``` 
## 2. Conda 이용 설치


```bash
conda install tensorflow 
conda install tensorflow-gpu
# conda install -c conda-forge tensorflow
``` 


## 8. 설치 확인 
```
import tensorflow as tf
sess = tf.Session()
print(tf.__version__) # version more than v1. 
#sess = tf.Session(config=tf.ConfigProto(log_device_placement=True))
```


## 9. 에러 처리 
`apt-get install -y libcupti-dev`  #에러발생 "/sbin/ldconfig.real: /usr/lib/nvidia-375/libEGL.so.1 is not a symbolic link"

libcudnn.so.5: cannot open shared object file 
- `LD_LIBRARY_PATH="/usr/local/cuda-8.0/targets/x86_64-linux/lib` #ldconfig -v 로해당라이브러리위치확인
- `export LD_LIBRARY_PATH` # 확인 : `echo $LD_LIBRARY_PATH`