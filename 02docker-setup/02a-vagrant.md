
## Valgrant

> [Setting Up Vagrant Environment on Ubuntu Platform](https://github.com/saasbook/courseware/wiki/Setting-Up-Vagrant-Environment-on-Ubuntu-Platform), [VirtualBox와 Vagrant의 기본 사용법](https://junistory.blogspot.kr/2017/08/virtualbox-vagrant.html)

### 1. Install

`apt-get install vagrant`


### 2. 확인 
```
vagrant -v
vagrant status
```

### 3. 초기 설정 
```
mkdir -p ~/vagrant/vm_01; cd ~/vagrant/vm_01
vagrant init shadowrobot/ros-indigo-desktop-trusty64
```
또는 

```
vagrant init
vi Vagrantfile
- L15라인을 수정 `config.vm.box = "base"` -> `config.vm.box = "puphpet/centos65-x64"`

### 추천 설정
  config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
    vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
    vb.memory = "2048"
  end
###
```

> [검색하기](https://app.vagrantup.com/boxes/search)

### 5. 가상머신 실행 (Vagrant 실행)

vagrant up --provider virtualbox

### 6. 가상머신 접속 (로그인)

vagrant ssh
- Username : vagrant
- Password : vagrant