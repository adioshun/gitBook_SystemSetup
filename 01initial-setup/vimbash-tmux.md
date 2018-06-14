## 1. bashrc

`cp /home/adioshun/.bashrc /root/.bashrc; source /root/.bashrc`

## 2. TMUX

> [홈페이지](https://github.com/tmux/tmux/wiki), [설치](http://code4rain.tistory.com/1169527180), [자동설치 스크립트](https://gist.github.com/bbelgodere/f77ee5e37ca661ad10ebe1f00020a8fd)

버젼확인 : tmux -V

### APT Install

```
apt-get install tmux
tmux new -s <원하는 이름>
```

### Source Install

```
cd ~
git clone https://github.com/tmux/tmux.git
cd tmux
sh autogen.sh
/configure && make

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
set-option -g mouse on 

bind -n WheelUpPane   select-pane -t= \; copy-mode -e \; send-keys -M
bind -n WheelDownPane select-pane -t= \;                 send-keys -M

# Set the default terminal mode to 256color mode
set -g default-terminal "screen-256color"

# enable activity alerts
setw -g monitor-activity on
set -g visual-activity on

# Center the window list
set -g status-justify centre
```

```
# Use mouse for 2.6+
set -g mouse on

setw -g mode-keys vi

# Use Alt-arrow keys without prefix key to switch panes
bind -n M-Left select-pane -L
bind -n M-Right select-pane -R
bind -n M-Up select-pane -U
bind -n M-Down select-pane -D

# Shift arrow to switch windows
bind -n S-Left  previous-window
bind -n S-Right next-window

# scrollback buffer size increase
set -g history-limit 100000

# change window order
bind-key -n C-S-Left swap-window -t -1
bind-key -n C-S-Right swap-window -t +1

# disable window name auto change
set-option -g allow-rename off

# bar color
set -g status-bg black
set -g status-fg white
```

```
# Use mouse for 1.8
setw -g mode-mouse on
set -g mouse-select-window on
set -g mouse-select-pane on
set -g mouse-resize-pane on
set -g mouse-utf on
```

## 4-windows Startup

script `start_tmux.sh`

```
echo 'start tmux'
#set session name
export SESSION=tmux
tmux -2 new-session -d -s $SESSION 
tmux split-window -v -t $SESSION  
tmux split-window -v -t $SESSION    
tmux split-window -h -t $SESSION  

tmux attach -t $SESSION
```

> [tmux shortcuts & cheatsheet](https://gist.github.com/MohamedAlaa/2961058)
>
> tmux 세션을 관리하는 경로가 /tmp,  /usr/lib/tmpfiles.d/tmp.conf 이쪽 경로일텐데, 조금 검색해보시고 /tmp/tmux-\* 경로를 삭제하지 않도록 예외처리
>
> Lost session : `killall -10 tmux`

* tmux확장 : [xpanes](https://github.com/greymd/tmux-xpanes), 여러 명령어 동시 수행, xpanes -c "ping {}" 192.168.0.{1..9}

## 3. VIM

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

> color schem dir : `/usr/share/vim/vimNN/colors`

## zsh변경

    sudo apt-get install zsh curl
    chsh -s `which zsh`
    sudo curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh

> zsh가 느리게 시작 할때 `sudo rm -rf /private/var/log/asl*.as`
>
> [oh-my-zsh](https://nolboo.kim/blog/2015/08/21/oh-my-zsh/)

[삭제 스크립트](https://github.com/robbyrussell/oh-my-zsh/blob/master/tools/uninstall.sh)


