# On Ubuntu 20.04 with python3 
```python
sudo docker run -it --privileged --network=host --name 'Ubuntu20' 1318b700e415 /bin/bash
```

```python
apt update
apt install -y sudo ssh vim dialog apt-utils 

#1. Install the CKAN package
sudo apt install -y libpq5 redis-server nginx supervisor libpython2.7 
sudo apt install -y python3.8-distutils  #python3 --version 
wget https://packaging.ckan.org/python-ckan_2.9-py3-focal_amd64.deb
sudo dpkg -i python-ckan_2.9-py3-focal_amd64.deb

#2. Install and configure PostgreSQL
sudo apt install -y postgresql postgresql-contrib

sudo service postgresql restart
cd /var/lib/postgresql/  #could not change directory to "/root": Permission denied 에러시
sudo -u postgres psql -l  #DB 출력
sudo -u postgres createuser -S -D -R -P ckan_default   #사용자 추가(=ckan_default)
sudo -u postgres createdb -O ckan_default ckan_default -E utf-8 #DB추가(=ckan_default)

#3. Install and configure Solr
apt install -y solr-tomcat


```






