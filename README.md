Oh My Zsh 是一款社区驱动的命令行工具，正如它的主页上说的，Oh My Zsh 是一种生活方式。它基于 zsh 命令行，提供了主题配置，插件机制，已经内置的便捷操作。给我们一种全新的方式使用命令行。

oh-my-zsh安装
# 先安装 zsh
yum -y install zsh
# 设置 zsh 为默认 shell
chsh -s /bin/zsh

# 安装 oh-my-zsh
## curl 方式
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
## wget 方式
sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"

# ~/.oh-my-zsh 目录
lib         # 提供核心功能的脚本库
tools       # 提供安装、升级等功能的工具
plugins     # 自带插件的存放位置
templates   # 自带模板的存放位置
themes      # 自带主题的存放位置
custom      # 个性化配置目录，自安装的插件和主题可放这里

# zsh-syntax-highlighting 插件
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.oh-my-zsh/plugins/zsh-syntax-highlighting

# 如何更新 oh-my-zsh
upgrade_oh_my_zsh
BashCopy
oh-my-zsh配置
修改 ~/.zshrc 文件，下面是我自己的 zshrc 配置，可供参考：

# 主题配置
ZSH_THEME="ys"

# 插件配置
plugins=(git z wd extract colored-man-pages zsh-syntax-highlighting)
# git                       最常用插件，git 相关
# z                         按照使用频率排序曾经进过的目录，进行模糊匹配
# wd                        通过设置 tag，快速切换目录
# extract                   'x'命令，支持自动识别压缩格式并将其解压
# colored-man-pages         'man'帮助文档页面开启高亮显示
# zsh-syntax-highlighting   oh-my-zsh 命令行语法高亮插件

# 历史记录
HIST_STAMPS="yyyy-mm-dd"        # 时间戳 "mm/dd/yyyy"|"dd.mm.yyyy"|"yyyy-mm-dd"
export HISTSIZE=100000          # 保存 100000 条记录
export HISTCONTROL=ignoredups   # 不记录重复的命令

# 其他配置
# CASE_SENSITIVE="true"         # 命令补全区分大小写，默认不区分
HYPHEN_INSENSITIVE="true"       # 命令补全不区分'_'、`-`，建议启用
DISABLE_AUTO_UPDATE="true"      # 不进行自动更新检查（每两个星期）
# export UPDATE_ZSH_DAYS=13     # 修改自动更新检查的周期，单位为天
# DISABLE_LS_COLORS="true"      # 禁用 ls 命令显示颜色
# DISABLE_AUTO_TITLE="true"     # 禁用 oh-my-zsh 自动设置终端标题
# ENABLE_CORRECTION="true"      # 启用命令自动更正，不好用
# COMPLETION_WAITING_DOTS="true"# 在等待命令补全时显示红点

# 加载 oh-my-zsh.sh
export ZSH=~/.oh-my-zsh
source $ZSH/oh-my-zsh.sh

# function path，可使用 autoload 加载
fpath=(~/.zsh_func $fpath)

# 修复键位冲突，如果没有此情况请注释掉！
# key bindings
bindkey "\e[1~" beginning-of-line
bindkey "\e[4~" end-of-line
bindkey "\e[5~" beginning-of-history
bindkey "\e[6~" end-of-history
# for rxvt
bindkey "\e[8~" end-of-line
bindkey "\e[7~" beginning-of-line
# for non RH/Debian xterm, can't hurt for RH/DEbian xterm
bindkey "\eOH" beginning-of-line
bindkey "\eOF" end-of-line
# for freebsd console
bindkey "\e[H" beginning-of-line
bindkey "\e[F" end-of-line
# completion in the middle of a line
bindkey '^i' expand-or-complete-prefix
# Fix numeric keypad
# 0 . Enter
bindkey -s "^[Op" "0"
bindkey -s "^[On" "."
bindkey -s "^[OM" "^M"
# 1 2 3
bindkey -s "^[Oq" "1"
bindkey -s "^[Or" "2"
bindkey -s "^[Os" "3"
# 4 5 6
bindkey -s "^[Ot" "4"
bindkey -s "^[Ou" "5"
bindkey -s "^[Ov" "6"
# 7 8 9
bindkey -s "^[Ow" "7"
bindkey -s "^[Ox" "8"
bindkey -s "^[Oy" "9"
# + - * /
bindkey -s "^[Ol" "+"
bindkey -s "^[Om" "-"
bindkey -s "^[Oj" "*"
bindkey -s "^[Oo" "/"

# xterm-256color
if [ $TERM = 'xterm' ]; then
    export TERM=xterm-256color
fi

# 默认编辑器 vim
export EDITOR=vim
export SVNEDITOR=vim
alias vi='vim'

# autoload
alias autoload='autoload -Uz'
alias al='autoload'
# src
alias src='source'

# zshedit | zshload
alias zshedit='vim ~/.zshrc'
alias zshload='source ~/.zshrc'
# zshfuncedit
function zshfuncedit() {
    vim ~/.zsh_func/$1
}

# trash-cli
alias rm='trash-put'
alias rm.list='trash-list'
alias rm.restore='trash-restore'
alias rm.delete='trash-rm'
alias rm.empty='trash-empty'

# echo
alias echo='echo -e'

# curl
alias curl='curl -s'

# grep
alias grep='grep -P --color'
alias egrep='egrep --color'

# diff
alias diff='diff -u --color'

# gcc/g++
alias gcc='gcc -std=c11 -Wall -Wextra'
alias g++='g++ -std=c++14 -Wall -Wextra'
# gdb/cgdb
alias gdb='gdb -q'
alias cgdb='cgdb -q'
# javac
alias javac='javac -Xlint:all'

# ps
alias ps='ps -eo user,pid,ppid,lwp,nlwp,tty,%cpu,%mem,stat,cmd'
# free
alias free='free -h'
# drop_cache
alias drop_cache='sync && sleep 3 && echo 3 > /proc/sys/vm/drop_caches'

# iptables
alias ipts_show='iptables --line-numbers -nvL -t'
alias ipts_clear='iptables -F -t raw && iptables -F -t mangle && iptables -F -t nat && iptables -F -t filter'

# date_show
alias date_show='date "+%A %F %T %Z"'
# auto_service
alias auto_service='systemctl list-unit-files -t service | grep enabled'
# catconf
alias catconf='egrep -v "^\s*$|^\s*#"'
# catfunc
alias catfunc='typeset -f'
# catdns
alias catdns='cat /etc/resolv.conf | egrep -v "^\s*$|^\s*#"'
# vimdns
alias vimdns='vim /etc/resolv.conf'

# getip
function getip() {
    local ip_info=$(curl -sL ip.chinaz.com/getip.aspx | sed -r "s/^\{ip:'(.*)',address:'(.*)'\}$/\1|\2/g")
    local my_ip=$(echo ${ip_info} | awk -F'|' '{print $1}')
    local my_loc=$(echo ${ip_info} | awk -F'|' '{print $2}')
    echo -e "\e[37mIP:\e[0m ${my_ip}\t\e[37m位置:\e[0m ${my_loc}"
}

# # http/https 代理
# proxy=http://192.168.0.103:1080
# export http_proxy=$proxy
# export https_proxy=$proxy
# export no_proxy="localhost, 127.0.0.1, ::1, ip.cn, chinaz.com"
# # unset proxy
# alias unset_proxy='unset http_proxy https_proxy ftp_proxy no_proxy'
