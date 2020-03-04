# Proxy 설정 

## 1. 글로벌 

```
#bashrc
http_proxy="http://168.219.61.xxx:8080"
https_proxy="http://168.219.61.xxx:8080"
no_proxy="127.0.0.1, localhost, 168.219.61.*"
```


## 2. apt
```
#sudo vi /etc/apt/apt.conf.d/90proxy  #90숫자는 큰 의미 없음, 중복 되지 않는 숫자로 지정 
Acquire {
    HTTP::proxy "http:// 168.219.61.xxx:8080/";
    HTTPS::proxy "http:// 168.219.61.xxx:8080/";
}
```

## 3. pip 
```
$ pip install --proxy http://proxy.252:port 패키지명 
$ python -m pip install --trusted-host pypi.org --trusted-host files.pythonhosted.org [패키지명]

# vi ~/.pip/pip.conf 
[global]
trusted-host = pypi.org files.pythonhosted.org

```



---

## etc. Cert Installation

```
$ sudo mkdir /usr/share/ca-certificates/samsung
$ sudo cp samsung.crt /usr/share/ca-certificates/samsung/
$ sudo cp samsung.crt /etc/ssl/certs/
$ sudo dpkg-reconfigure ca-certificates
$ sudo update-ca-certificates
```

