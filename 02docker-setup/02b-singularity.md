# singularity 

##  install
```bash
VERSION=2.2.1
wget https://github.com/singularityware/singularity/releases/download/$VERSION/singularity-$VERSION.tar.gz
tar xvf singularity-$VERSION.tar.gz
cd singularity-$VERSION
./configure --prefix=/usr/local
make
sudo make install
```
Check release : [github](https://github.com/singularityware/singularity/releases)

## create img from Docker(docker hub)
```bash
sudo singularity create --size 1000 analysis.img
sudo singularity import analysis.img docker://ubuntu:latest
singularity shell analysis.img
```
## Run 
```bash
singularity exec analysis.img cat /etc/lsb-release #/bin/bash
```

## convert Docker img to singularity img 
```
docker run -v /var/run/docker.sock:/var/run/docker.sock -v \root:/output --privileged -t --rm filo/docker2singularity adioshun/deeplearning:Full
```
- \root : 저장위치
- adioshun/deeplearning:Full : 변환할이미지



