# [PyTorch installation](http://pytorch.org/)


```
pip install torch torchvision
pip3 install torch torchvision
```

```bash 
# conda (python2.7, CPU)
$ conda install pytorch torchvision -c soumith

# conda (python2.7, GPU, CUDA8.0)
$ conda install pytorch torchvision cuda80 -c soumith

# conda (python3.6, CPU)
$ conda install pytorch torchvision -c soumith

# conda (python3.6, GPU, CUDA8.0)
$ conda install pytorch torchvision cuda80 -c soumith

```


## Visdom for PyTorch Visualization
```bash
pip install visdom
python -m visdom.server
```
Open `http://localhost:8097`

> 출처 : [Visdom github](https://github.com/facebookresearch/visdom)

###### [Error] unsupported GNU version! gcc versions later than 5 are not supported!
```bash 
sudo apt-get install python-software-properties
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
sudo apt-get install gcc-4.9
sudo apt-get install g++-4.9

sudo rm /usr/bin/g++
sudo ln -s /usr/bin/g++-4.9 /usr/bin/g++

sudo rm /usr/bin/gcc
sudo ln -s /usr/bin/gcc-4.9 /usr/bin/gcc
```
