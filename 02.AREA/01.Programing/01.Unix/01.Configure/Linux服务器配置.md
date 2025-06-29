---
tags:
  - garden/🌳Evergreen
created: 2024-06-04 20:58
updated: 2025-06-27 20:38
dg-publish: true
---
## 账户配置文件
### 配置～/.bashrc

```shell
#edit by gyw

# .bashrc

# Source global definitions
if [ -f /etc/bashrc ]; then
    . /etc/bashrc
fi

# User specific aliases and functions
export PS1="\[\e[31;1m\]Aliyun \[\e[35;1m\]\h \[\e[34;1m\]\u \[\e[36;1m\]\d \t \[\e[32;1m\]\$PWD/ (\$(ls -A|wc -l))\[\e[0m\] \n\[\e[0m\]\[\e[33;1m\]\$\[\e[0m\]"
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8

# alias set by gyw
alias ch="chmod +x"
alias les="less -S"             # 不换行显示
alias kes="less -S"
alias la='ls -Al'               # 显示隐藏文件
alias ls='ls -hF --color'   # 为识别的文件类型添加颜色
alias lk='ls -lSr'              # 按尺寸排序
alias lr='ls -lR'               # 递归ls
alias lt='ls -ltr'              # 按日期排序
alias du='du -kh'    # Makes a more readable output.
alias df='df -kTh'
alias h="history | less"
alias wl="wc -l"
alias vb="vi ~/.bashrc"
alias sb="source ~/.bashrc"
alias vv="vi ~/.vimrc"
alias untar="tar -zxf"
alias l='ls -lthr --color=auto'
alias lll='ls -althr'
alias lss='ls -s --color'
alias ..="cd .."
alias ...="cd ../.."
alias ~="cd ~"
alias rm="rm -I"
alias ct="cleartrash"
alias CAT="cat -A"
alias mv="mv -i"
alias cp="cp -i"
alias cdl='function __cdl(){ if [ $# == 1 ]; then cd $1; ls -al; unset -f __cdl; fi }; __cdl'
alias cdls='function __cdls(){ if [ $# == 1 ]; then cd $1; ls; unset -f __cdls; fi }; __cdls'
alias cdlss='function __cdlss(){ if [ $# == 1 ]; then cd $1; ls -s; unset -f __cdlss; fi }; __cdlss'
alias mkcd='function __mkcd(){ if [ $# == 1 ]; then mkdir $1; cd $1; unset -f __mkcd; elif [ $# == 2 ]; then mkdir $1 $2; cd $2; unset -f __mkcd; fi }; __mkcd'
alias sc="screen"


# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('/home/guoyingwei/software/miniconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/home/guoyingwei/software/miniconda3/etc/profile.d/conda.sh" ]; then
        . "/home/guoyingwei/software/miniconda3/etc/profile.d/conda.sh"
    else
        export PATH="/home/guoyingwei/software/miniconda3/bin:$PATH"
    fi
fi
unset __conda_setup
# <<< conda initialize <<<

```

### 配置~/.vimrc

```shell
set encoding=utf8
set hlsearch    "高亮度反白
set nu          "显示行号
set bg=dark     "不同底色色调
syntax on       "语法检验


inoremap ( ()<LEFT>
inoremap { {}<LEFT>
inoremap [ []<LEFT>
set showmatch     "显示匹配的括号
set smartindent
set autoindent
""set mouse=a       "允许光标点击
set tabstop=4      "(注：ts是tabstop的缩写，设TAB宽4个空格)
set expandtab      "设置tab用空格表示
set shiftwidth=4   "缩进长度
set noswapfile     "不备份文件
set softtabstop=4  "退格键长度
set pastetoggle=<F9> "切换自动缩进
map <F10> :set paste<CR>
map <F11> :set nopaste<CR>
```

### 配置～/.tmux.conf

```shell
# -----------------------------------------------------------------------------
# Tmux 基本配置 - 要求 Tmux >= 2.3
# 如果不想使用插件，只需要将此节的内容写入 ~/.tmux.conf 即可
# -----------------------------------------------------------------------------

# C-b 和 VIM 冲突，修改 Prefix 组合键为 Control-Z，按键距离近
#set -g prefix C-z

set -g base-index         1     # 窗口编号从 1 开始计数
set -g display-panes-time 10000 # PREFIX-Q 显示编号的驻留时长，单位 ms
set -g mouse              on    # 开启鼠标
set -g pane-base-index    1     # 窗格编号从 1 开始计数
set -g renumber-windows   on    # 关掉某个窗口后，编号重排

setw -g allow-rename      off   # 禁止活动进程修改窗口名
setw -g automatic-rename  off   # 禁止自动命名新窗口
setw -g mode-keys         vi    # 进入复制模式的时候使用 vi 键位（默认是 EMACS）

# ------------------------------------------------------------------------
```
## 安装miniconda

### 安装
```shell
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh
```

### 初始化
```shell
~/miniconda3/bin/conda init bash
# 撤销初始化
# conda init --reverse bash
# 默认不激活base环境
# conda config --set auto_activate_base false
```

### 配置清华源

```shell
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels bioconda
conda config --set show_channel_urls yes
```

## GitHub同步配置文件

### 配置GitHub账号

```shell
git config --global user.name "guoyingwei_aliyun"
git config --global user.email "guoyingwei6@outlook.com"
ssh-keygen -t ed25519 -C "guoyingwei6@outlook.com"



# 复制显示的内容
cat ~/.ssh/id_ed25519.pub
# 把公钥添加到GitHub > Setting > SSH and GPG keys
```

### 远程和本地配置文件同步
```shell
git clone git@github.com:guoyingwei6/dotfiles.git
cd dotfiles
/usr/bin/cp -f ~/.bashrc .bashrc
/usr/bin/cp -f ~/.vimrc .vimrc
git add ./
git commit -m "syn dotfiles"
git push
```

### 利用cron每天自动备份配置文件

```shell
vi syn.sh
# syn.sh写入下列命令:
/usr/bin/cp -f ~/.bashrc /home/guoyingwei/dotfiles/.bashrc
/usr/bin/cp -f ~/.vimrc /home/guoyingwei/dotfiles/.vimrc
git -C /home/guoyingwei/dotfiles/ add /home/guoyingwei/dotfiles/
git -C /home/guoyingwei/dotfiles/ commit -m "syn dotfiles"
git -C /home/guoyingwei/dotfiles/ push # 非git目录下使用git需要-C参数

vi .gitignore
# 在dotfiles仓库目录下创建.gitignore文件
syn.sh    # 忽略syn.sh

# 调用crontab
crontab -e
* 1 * * * bash /home/guoyingwei/dotfiles/syn.sh  # 每天凌晨1点自动同步一次
```


只能往GitHub备份，不能同步。如果其它服务器上的配置文件发生改动并上传，则本地必需手动pull一次。