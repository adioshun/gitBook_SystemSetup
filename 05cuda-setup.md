# CUDA 활용 환경 구성

## 1. Nvidia 드라이버 설치 [[Download]](http://www.nvidia.com/Download/index.aspx?lang=en-us)

### 1.1 그래픽 카드 확인 

``` 
# ubunutu 18.04
$ ubuntu-drivers devices

# ubunut 16.04
$ lspci | grep -i nvidia #apt-get install pciutils
$ lshw -numeric -C display
$ lspci -vnn | grep VGA

```

### 1.2 [nouveau 해제](https://gist.github.com/haje01/f13053738853f39ce5a2#nouveau-해제)
> 오픈소스 드라이버입니다. 이것이 NVIDIA 드라이버의 커널 모듈과 충돌   

```python 
# Disable nouveau
$ lsmod | grep nouveau
$ sudo vi /etc/modprobe.d/blacklist-nouveau.conf
"""
blacklist nouveau
blacklist lbm-nouveau
options nouveau modeset=0
alias nouveau off
alias lbm-nouveau off
"""
  
$ echo options nouveau modeset=0 | sudo tee -a /etc/modprobe.d/nouveau-kms.conf
$ sudo update-initramfs -u
$ reboot

#$sudo apt-get --purge remove xserver-xorg-video-nouveau
```
  
### 1.3 Nvidia 드라이버 설치 

```python
#ubuntu 18.04
sudo ubuntu-drivers autoinstall

#ubunutu 16.04
sudo apt purge nvidia-* 
sudo add-apt-repository ppa:graphics-drivers/ppa #ppa:xorg-edgers/ppa
sudo apt update
sudo apt install nvidia-driver-390 #nvidia-current #ubuntu-drivers devices로 확인된 값
#modprobe nvidia
```

### 1.3  드라이버 설치 확인 : 

```
$ nvidia-smi  
$ cat /proc/driver/nvidia/version
```


![](https://camo.githubusercontent.com/578cbdc3cca2ddc2fe30e6796ec1cf404aa134d4/68747470733a2f2f692e696d6775722e636f6d2f4571736d7873752e706e67)


---

## 2. CUDA 설치 

- cuda 설치 확인: `nvcc --version`

- 삭제 : `apt-get purge cuda` or `/usr/local/cuda/uninstall.xx.sh`

### 2.1 일반적 방법 

```python
sudo apt install nvidia-cuda-toolkit  

```

### 2.2 Ubuntu 18.04 - CUDA 9



```
sudo apt update && sudo apt install nvidia-cuda-toolkit gcc-6 g++-6  #18.04
nvcc --version
reboot
```

### 2.3 Ubuntu 16.04 LTS or 16.10 - CUDA 8 with latest driver:

apt-get install curl wget

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
or

```bash
## Network 버젼 
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_8.0.61-1_amd64.deb

## Local 버젼 
wget https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda-repo-ubuntu1604-8-0-local-ga2_8.0.61-1_amd64-deb` 1.9G

sudo dpkg -i cuda-repo-ubuntu1604_8.0.61-1_amd64.deb
sudo apt-get update
#sudo apt-get install cuda
echo "export PATH=/usr/local/cuda/bin/:\$PATH; export LD_LIBRARY_PATH=/usr/local/cuda/lib64/:\$LD_LIBRARY_PATH; " >>~/.bashrc && source ~/.bashrc


export PATH="/usr/local/cuda/bin:$PATH"  
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/cuda/lib64"
export CUDA_HOME=/usr/local/cuda
source ~/.bashrc
```



### 2.4 Ubuntu 14.04 LTS - CUDA 8 with latest driver:

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
or 
```
## Network 버젼
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1404/x86_64/cuda-repo-ubuntu1404_8.0.61-1_amd64.deb

sudo dpkg -i cuda-repo-ubuntu1404_8.0.61-1_amd64.deb
sudo apt-get update
sudo apt-get install cuda
echo "export PATH=/usr/local/cuda/bin/:\$PATH; export LD_LIBRARY_PATH=/usr/local/cuda/lib64/:\$LD_LIBRARY_PATH; " >>~/.bashrc && source ~/.bashrc
```





---

## 3. cuDNN 설치

### 3.1 소스코드 설치 cuDNN v6.0 Library for Linux

* Download : [https://developer.nvidia.com/cudnn](https://developer.nvidia.com/cudnn) -&gt;  cuDNN 5.1 \(August 10, 2016\) for CUDA 8.0

```python
#copy the following files into the cuda toolkit directory.
sudo cp -P cuda/include/cudnn.h /usr/local/cuda/include
sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda/lib64/
sudo chmod a+r /usr/local/cuda/lib64/libcudnn*

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

## 3.2 삭제

해당 볼더 제거

---

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



---

# 에러 

libGL error: failed to load driver: swrast : ubuntu 16 + Nvidia에서 발생, ubuntu 18은 해결 
https://askubuntu.com/questions/834254/steam-libgl-error-no-matching-fbconfigs-or-visuals-found-libgl-error-failed-t
