## 4. Mount

### 4.1 gcsfuse (Google Cloud)

installation
```bash

export GCSFUSE_REPO=gcsfuse-`lsb_release -c -s`
echo "deb http://packages.cloud.google.com/apt $GCSFUSE_REPO main" | sudo tee /etc/apt/sources.list.d/gcsfuse.list
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo apt-get update
sudo apt-get install gcsfuse
sudo usermod -a -G fuse $USER
```

> 중요 : Permission문제가 발생 할수 있으므로 sudo로 실행 하지 않기 

mount 
``` bash 
#curl https://sdk.cloud.google.com | bash  #gcloud: command not found

gcloud auth login

gcsfuse peta-fastness-4371 /home/hjlim99/mount/
#gcsfuse --foreground --debug_gcs --debug_http --debug_fuse --debug_invariants peta-fastness-4371 /home/hjlim99/mount/
``

Unmount
```bash
fusermount -u /home/hjlim99/mount
```

File transfer with status bar : `rsync --info=progress2 source dest`

> [ref](https://github.com/GoogleCloudPlatform/gcsfuse/blob/master/docs/mounting.md#basic-usage)
> [Automation](https://github.com/GoogleCloudPlatform/gcsfuse/blob/master/docs/mounting.md#mount8-and-fstab-compatibility)



### 4.4 AWS Mount 

#### A. 볼륨 마운트 
```
lsblk
file -s /dev/xvdb #최초 1회
mkfs -t ext4 /dev/xvdb #최초 1회
mkdir /mnt/aws
mount /dev/xvdb /mnt/aws
df -h
```

#### B. S3 마운트 

- 속도 초당 1M

- 사전 준비 : aws `IAM`서비스 - Users - adduser - Access type : Programmatic access - Attach existing policies directly : AmazonS3FullAccess - Access key ID:Secret access key 복사 

```
sudo apt-get install s3fs 
echo ACCESS_KEY:SECRET_KEY > ~/.passwd-s3fs
chmod 600 ~/.passwd-s3fs
mkdir /mnt/s3-drive
s3fs <bucketname> /mnt/s3-drive -ouse_cache=/tmp

# Check 
df -Th /mnt/s3-drive

# unmount
umount /mnt/s3-drive
```

### 4.3 sshfs

- 설치 및 접속 

```
apt-get install sshfs
sshfs {원격주소:원격폴더} {로컬 폴더}
sshfs root@211.112.123.123:/home/backup /mybackup -o allow_other #root 이외의 사용자도 접근할 수 있게 할지 선택.


!echo ellene | sshfs -o nonempty adioshun@128.46.80.28:/home/adioshun/docker_share /home/hjlim99/share -o workaround=rename -o password_stdin
```

- Docker에서 접속시 `docker run -it --cap-add SYS_ADMIN --cap-add MKNOD --device=/dev/fuse --security-opt apparmor:unconfined` 


### 4.4 windows-Ubuntu 

- [CIFS 유틸리티 이용](http://goproprada.tistory.com/198)



