# On Ubuntu 20.04 with python3 
```python
sudo docker run -d --privileged --network=host --name 'u20' ubuntu20-systemd /sbin/init
#System has not been booted with systemd as init system (PID 1). Can't operate.
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
vi /etc/tomcat9/server.xml
`<Connector port="8080" protocol="HTTP/1.1"` -> <Connector port="8983" protocol="HTTP/1.1" `

sudo mv /etc/solr/conf/schema.xml /etc/solr/conf/schema.xml.bak
sudo ln -s /usr/lib/ckan/default/src/ckan/ckan/config/solr/schema.xml /etc/solr/conf/schema.xml

sudo service tomcat9 restart #http://localhost:8983/solr/

vi /etc/ckan/default/ckan.ini
solr_url=http://127.0.0.1:8983/solr  #L94
ckan.site_url = http://www.bigdata-car.kr #L62

sudo ckan db init #

```

- [Ubuntu, PostgreSQL 설치하기](https://noweaver.github.io/posts/Journal/2020/05/13-Ubuntu-PostgreSQL-Installation/)
- [CKAN Source 설치 가이드 ( Ubuntu 18.04 )](https://cntechsystems.tistory.com/25) : 씨앤텍시스템즈 기술블로그




