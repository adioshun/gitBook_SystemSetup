## 1. TMUX && BYOBU

sudo apt-get install byobu

> [홈페이지](https://github.com/tmux/tmux/wiki), [설치](http://code4rain.tistory.com/1169527180), [자동설치 스크립트](https://gist.github.com/bbelgodere/f77ee5e37ca661ad10ebe1f00020a8fd)

버젼확인 : tmux -V

### APT Install

```
apt-get install tmux
tmux new -s <원하는 이름>
```

* 창 나누기 Ctrl + b, " \(새로\) Ctrl + b, % \(가로\) 
* Ctrl + b, 방향키 : 화면 이동
* 창 이동 Ctrl + b, o\(알파벳\)
* zoom ctrl+ b, z

출처: [http://kaos119.tistory.com/113](http://kaos119.tistory.com/113) \[기다리면 먹는거다\]

* 미리 창 분활 해놓기 : [Tmuxinator](https://github.com/tmuxinator/tmuxinator)

출처: [http://code4rain.tistory.com/1169527180](http://code4rain.tistory.com/1169527180) \[codeRain's Life Blog\]

## Mouse Support

touch /root/.tmux.conf

```

## set mouse for tmux 2.1 as shipped with Ubuntu 14.04
wget https://gist.githubusercontent.com/adioshun/5334a2fdbec7b79c6ed62e5158901b13/raw/f231167155b09bcfabe09939de1219010535f39a/.tmux.conf_2.1

# Use mouse for 2.6+
wget https://gist.githubusercontent.com/adioshun/5334a2fdbec7b79c6ed62e5158901b13/raw/f231167155b09bcfabe09939de1219010535f39a/.tmux.conf_2.6+



# Use mouse for 1.8
wget https://gist.githubusercontent.com/adioshun/5334a2fdbec7b79c6ed62e5158901b13/raw/f231167155b09bcfabe09939de1219010535f39a/.tmux.conf_1.8

## start_tmux.sh

wget https://gist.githubusercontent.com/adioshun/5334a2fdbec7b79c6ed62e5158901b13/raw/f231167155b09bcfabe09939de1219010535f39a/start_tmux.sh

```

> [tmux shortcuts & cheatsheet](https://gist.github.com/MohamedAlaa/2961058)
>
> tmux 세션을 관리하는 경로가 /tmp,  /usr/lib/tmpfiles.d/tmp.conf 이쪽 경로일텐데, 조금 검색해보시고 /tmp/tmux-\* 경로를 삭제하지 않도록 예외처리
>
> Lost session : `killall -10 tmux`

* tmux확장 : [xpanes](https://github.com/greymd/tmux-xpanes), 여러 명령어 동시 수행, xpanes -c "ping {}" 192.168.0.{1..9}

## 3. VIM

```

    # ~/.vimrc
    set hlsearch " 검색어 하이라이팅
    set nu " 줄번호
    set autoindent " 자동 들여쓰기
    set scrolloff=2
    set wildmode=longest,list
    set ts=4 "tag select
    set sts=4 "st select
    set sw=1 " 스크롤바 너비
    set autowrite " 다른 파일로 넘어갈 때 자동 저장
    set autoread " 작업 중인 파일 외부에서 변경됬을 경우 자동으로 불러옴
    set cindent " C언어 자동 들여쓰기
    set bs=eol,start,indent
    set history=256
    set laststatus=2 " 상태바 표시 항상
    "set paste " 붙여넣기 계단현상 없애기
    set shiftwidth=4 " 자동 들여쓰기 너비 설정
    set showmatch " 일치하는 괄호 하이라이팅
    set smartcase " 검색시 대소문자 구별
    set smarttab
    set smartindent
    set softtabstop=4
    set tabstop=4
    set ruler " 현재 커서 위치 표시
    set incsearch
    set statusline=\ %<%l:%v\ [%P]%=%a\ %h%m%r\ %F\

    " 마지막으로 수정된 곳에 커서를 위치함
    au BufReadPost *
    \ if line("'\"") > 0 && line("'\"") <= line("$") |
    \ exe "norm g`\"" |
    \ endif

    " 파일 인코딩을 한국어로
    if $LANG[0]=='k' && $LANG[1]=='o'
    set fileencoding=korea
    endif

    " 구문 강조 사용
    if has("syntax")
     syntax on
    endif

    " 컬러 스킴 사용
    colorscheme jellybeans
```

> color schem dir : `/usr/share/vim/vimNN/colors`

vimrc : [이번에는 우분투 호환 vimrc](http://chanacademy.tistory.com/38), [좋은 vimrc를 찾았다](http://chanacademy.tistory.com/37)



## zsh변경

    sudo apt-get install zsh curl
    chsh -s `which zsh`
    sudo curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh

> zsh가 느리게 시작 할때 `sudo rm -rf /private/var/log/asl*.as`
>
> [oh-my-zsh](https://nolboo.kim/blog/2015/08/21/oh-my-zsh/)

[삭제 스크립트](https://github.com/robbyrussell/oh-my-zsh/blob/master/tools/uninstall.sh)



