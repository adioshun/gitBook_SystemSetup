# Driver & CUDA install script 

|참고 : 설치 후 재 부팅 |
|-|

Ubuntu 16.04 LTS or 16.10 - CUDA 8 with latest driver:
```bash
#!/bin/bash
echo "Checking for CUDA and installing."
# Check for CUDA and try to install.
if ! dpkg-query -W cuda; then
  # The 16.04 installer works with 16.10.
  curl -O http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_8.0.61-1_amd64.deb
  dpkg -i ./cuda-repo-ubuntu1604_8.0.61-1_amd64.deb
  apt-get update
  apt-get install cuda -y
fi
```
Ubuntu 14.04 LTS - CUDA 8 with latest driver:
```bahs 
#!/bin/bash
echo "Checking for CUDA and installing."
# Check for CUDA and try to install.
if ! dpkg-query -W cuda; then
  curl -O http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1404/x86_64/cuda-repo-ubuntu1404_8.0.61-1_amd64.deb
  dpkg -i ./cuda-repo-ubuntu1404_8.0.61-1_amd64.deb
  apt-get update
  apt-get install cuda -y
  apt-get install linux-headers-$(uname -r) -y
fi
```

---

## 1. CUDA설치 



### 1.0 사전 작업 

> NVIDIA 드라이버 설치 확인 : `cat /proc/driver/nvidia/version`  미 설치시 하단 설명 참고

```
sudo apt-get update
sudo apt-get install build-essential
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
sudo apt-get install --no-install-recommends libboost-all-dev
sudo apt-get update
sudo apt-get install linux-generic (VM 사용시 설치) 

```

### 1.1 CUDA by apt-get install 

```
sudo apt-get install cuda #This will install the latest toolkit  and the latest drive
```

### 1.1 CUDA 8 for Ubuntu16.04 x86
```
## Network 버젼 
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_8.0.61-1_amd64.deb

## Local 버젼 
wget https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda-repo-ubuntu1604-8-0-local-ga2_8.0.61-1_amd64-deb` 1.9G

sudo dpkg -i cuda-repo-ubuntu1604_8.0.61-1_amd64.deb
sudo apt-get update
#sudo apt-get install cuda
echo "export PATH=/usr/local/cuda/bin/:\$PATH; export LD_LIBRARY_PATH=/usr/local/cuda/lib64/:\$LD_LIBRARY_PATH; " >>~/.bashrc && source ~/.bashrc
```


```
export PATH="/usr/local/cuda/bin:$PATH"  
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/cuda/lib64"
export CUDA_HOME=/usr/local/cuda
source ~/.bashrc
```
### 1.2 CUDA 8 for Ubuntu14.04 x86

```
## Network 버젼
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1404/x86_64/cuda-repo-ubuntu1404_8.0.61-1_amd64.deb

sudo dpkg -i cuda-repo-ubuntu1404_8.0.61-1_amd64.deb
sudo apt-get update
sudo apt-get install cuda
echo "export PATH=/usr/local/cuda/bin/:\$PATH; export LD_LIBRARY_PATH=/usr/local/cuda/lib64/:\$LD_LIBRARY_PATH; " >>~/.bashrc && source ~/.bashrc
```

### 1.3 cuda 설치 확인
 `nvcc --version`

### 1.4 삭제 

`apt-get purge cuda`

`/usr/local/cuda/uninstall.xx.sh`
---
## 2. cuDNN 설치 

### 2.1 소스코드 설치 cuDNN v6.0 Library for Linux
- Download : https://developer.nvidia.com/cudnn ->  cuDNN 5.1 (August 10, 2016) for CUDA 8.0




```
wget http://developer.download.nvidia.com/compute/redist/cudnn/v6.0/cudnn-8.0-linux-x64-v6.0.tgz
tar cvzpf cudnn-8.0-linux-x64-v5.1.tgz ./

sudo cp -P cuda/include/*.h /usr/local/cuda/include
sudo cp cuda/lib64/*.so* /usr/local/cuda/lib64  
#OR cp -P cuda/lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
sudo apt-get install libcupti-dev
```

에러 : "/sbin/ldconfig.real: /usr/lib/nvidia-375/libEGL.so.1 is not a symbolic link"
```
sudo mv /usr/lib/nvidia-375/libEGL.so.1 /usr/lib/nvidia-375/libEGL.so.1.org
sudo mv /usr/lib32/nvidia-375/libEGL.so.1 /usr/lib32/nvidia-375/libEGL.so.1.org
sudo ln -s /usr/lib/nvidia-375/libEGL.so.375.39 /usr/lib/nvidia-375/libEGL.so.1
sudo ln -s /usr/lib32/nvidia-375/libEGL.so.375.39 /usr/lib32/nvidia-375/libEGL.so.1
```

## 2.2 삭제 

해당 볼더 제거 

---
### [참고] 드라이버 설치 

- GPU 정보를 확인합니다. :`$ lspci | grep -i nvidia`

- nvidia graphic driver [설치](http://www.nvidia.com/Download/index.aspx?lang=en-us) 

- [Nvidia Driver Instalation](https://goo.gl/kfzWfJ) 

- [nouveau 해제](https://gist.github.com/haje01/f13053738853f39ce5a2#nouveau-해제): 오픈소스 드라이버입니다. 이것이 NVIDIA 드라이버의 커널 모듈과 충돌 `sudo apt-get --purge remove xserver-xorg-video-nouveau`

```
sudo add-apt-repository -y ppa:xorg-edgers/ppa -y
sudo apt-get update
sudo apt-get install nvidia-current

sudo apt purge nvidia-*
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update
sudo apt install nvidia-381
# cd /usr/lib/nvidia-xxxx 로 확인 가능 
```


### 0.1 For Ubuntu 14.04
```
CUDA_REPO_PKG=http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1404/x86_64/cuda-repo-ubuntu1404_8.0.61-1_amd64.deb
ML_REPO_PKG=http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1404/x86_64/nvidia-machine-learning-repo-ubuntu1404_4.0-2_amd64.deb
```
### 0.2 For Ubuntu 16.04
```
CUDA_REPO_PKG=http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_8.0.61-1_amd64.deb
ML_REPO_PKG=http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64/nvidia-machine-learning-repo-ubuntu1604_1.0.0-1_amd64.deb
```
### 0.3 Install repo packages
```
wget "$CUDA_REPO_PKG" -O /tmp/cuda-repo.deb && sudo dpkg -i /tmp/cuda-repo.deb && rm -f /tmp/cuda-repo.deb
wget "$ML_REPO_PKG" -O /tmp/ml-repo.deb && sudo dpkg -i /tmp/ml-repo.deb && rm -f /tmp/ml-repo.deb
```
