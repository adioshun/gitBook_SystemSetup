# VMware Setup

## 1. time 및 zone 변경
```bash
date 
sudo rm /etc/localtime
sudo ln -s /usr/share/zoneinfo/US/East-Indiana /etc/localtime 
```
## 2. Automatic shutdown 
```bahs
sudo vi /etc/crontab 
30 17   * * *   root    echo "I'm about to restart" |wall
00 18   * * *   root    shutdown -h now 
/etc/init.d/cron restart
```
## 3.SSH접속

- apt-get update;apt-get install net-tools openssh-server vim
- vi /etc/ssh/sshd_config변경: 
    - port number 22 -> 2222
    - PermitRootLogin yes 
    - PasswordAuthentication yes
- service ssh start
- port check : `netstat -anp | grep "LISTEN "`

접속법:`ssh -p 2222 root@localhost`


> 출처: http://chanhy63.tistory.com/11 [Notepad]





## 9. 작업종료시알림
```
python train.py; echo "Done" | mail -s "fishished" xxxx@gmail.com
python train.py;sudo shoutdown now
```

---

네이버 클라우드 윈도우 서버 & 원격데스크톱 https://www.ncloud.com/guideCenter/guide/3
