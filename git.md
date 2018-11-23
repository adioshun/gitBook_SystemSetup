## github 키 등록

```
    1  ssh-keygen -t rsa -b 4096 -C "hunjung.lim@samsung.com"
    2  eval $(ssh-agent -s)
    3  ssh-add ~/.ssh/id_rsa
    4  clip < ~/.ssh/id_rsa.pub  ## 복사된 내용을 킷허브 설정 페이지에서 키 추가
```

## git 폴더 연동

```
#로컬 폴더로 이동 
$ git init
$ git remote add origin {https://github....}

# 소스 가져 오기 
$ git pull origin master #remote의 소스 가져 오기 

# 소스 업로드 하기 
$ git add *
$ git commit -m ""
$ git push --set-upstream origin master
```

## [git 자동 완성 트릭](https://github.com/progit/progit/blob/master/ko/02-git-basics/01-chapter2.markdown#자동완성)

- [Git 에서 비밀번호 물어보지 않게 하기](https://www.devkwon.com/posts/132)
    - git config --global credential.helper cache  (15분)
    - git config --global credential.helper 'cache --timeout=864000' (10일)
    - git config ~~--global~~ credential.helper 'cache --timeout=864000' (10일, 현재 repo만 적)


