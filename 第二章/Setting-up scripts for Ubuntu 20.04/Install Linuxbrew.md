# Install Linuxbrew
使用清华的[镜像](https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/).

#与该文章类似[文章链接](https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/)

```shell script
echo "==> Tuna mirrors of Homebrew/Linuxbrew"  #使用清华镜像，在终端输入以下几行命令设置环境变量
export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git"
export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git"
export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles"

git clone --depth=1 https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/install.git brew-install
/bin/bash brew-install/install.sh
#从清华镜像下载脚本并安装 Homebrew / Linuxbrew

 git clone如果我们克隆仓库这个仓库，会把所有的历史协作记录都clone下来，这样整个文件会非常大，其实对于我们直接使用仓库，而不是参与仓库工作的人来说，只要把最近的一次commit给clone下来就好了。这就好比一个产品有很多个版本，我们只要clone最近的一个版本来使用就行了。


rm -rf brew-install

#安装成功后需将 brew 程序的相关路径加入到环境变量中：
#以下针对 Linux 系统上的 Linuxbrew：


#　test –d File     文件存在并且是目录
test -d ~/.linuxbrew && PATH="$HOME/.linuxbrew/bin:$HOME/.linuxbrew/sbin:$PATH"
#~/.linuxbrew 目录存在，且将路径写为。。。后面这些
#表示在保留原来的$PATH环境变量的基础上，再增加$HOME/.linuxbrew/sbin和$HOME/.linuxbrew/bin这个路径作为新的$PATH环境变量

test -d /home/linuxbrew/.linuxbrew && PATH="/home/linuxbrew/.linuxbrew/bin:/home/linuxbrew/.linuxbrew/sbin:$PATH"


if grep -q -i Homebrew $HOME/.bashrc; then
#忽略大小写，看$HOME/.bashrc文件夹内能否匹配到Homebrew

    echo "==> .bashrc already contains Homebrew"
    #如果已经包含，则输出 该文件夹已经包含homebrew

else
    echo "==> Update .bashrc"

# 在$HOME/.bashrc中配置PATH环境变量
    echo >> $HOME/.bashrc
    echo '# Homebrew' >> $HOME/.bashrc
    #将 homebrew写入该文件夹

    echo "export PATH='$(brew --prefix)/bin:$(brew --prefix)/sbin'":'"$PATH"' >> $HOME/.bashrc
    #将指定路径$(brew --prefix)/bin:$(brew --prefix)/sbin写入默认环境变量$PATH中，再把写好的环境变量写入$HOME/.bashrc文件夹 

    echo "export MANPATH='$(brew --prefix)/share/man'":'"$MANPATH"' >> $HOME/.bashrc
    #MANPATH man手册的默认路径

    echo "export INFOPATH='$(brew --prefix)/share/info'":'"$INFOPATH"' >> $HOME/.bashrc
    #INPUTRC 默认键盘映象

    echo "export HOMEBREW_NO_ANALYTICS=1" >> $HOME/.bashrc
    echo 'export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git"' >> $HOME/.bashrc
    echo 'export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git"' >> $HOME/.bashrc
    echo 'export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles"' >> $HOME/.bashrc
    echo >> $HOME/.bashrc
    #设置环境变量，用echo将path写入文件夹
fi

source $HOME/.bashrc
#source：使当前shell读入路径为filepath的shell文件并依次执行文件中的所有语句，通常用于重新执行刚修改的初始化文件，使之立即生效，而不必注销并重新登录

#表示安装成功：
#xuruizhi@LAPTOP-VVGKELBI:~$ brew --version
#Homebrew 3.4.11
#Homebrew/homebrew-core (git revision 04c73a4f47a; last commit 2022-05-15)

```