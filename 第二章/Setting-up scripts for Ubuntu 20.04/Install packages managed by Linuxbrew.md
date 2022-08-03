# Install packages managed by Linuxbrew

Packages include:

* Programming languages: Perl, Python, R, Java, Lua and Node.js
* Some generalized tools

```shell script
bash $HOME/Scripts/dotfiles/brew.sh
source $HOME/.bashrc
```

Attentions:

* `r` and `gnuplot` have a lot of dependencies. Just be patient.

* Sometimes there are no binary packages; compiling from source codes may take extra time.
# $HOME/Scripts/dotfiles/brew.sh中内容：
https://github.com/wang-q/dotfiles/blob/master/brew.sh
```
#!/bin/bash

export HOMEBREW_NO_AUTO_UPDATE=1
#export [-fnp][变量名称]=[变量设置值]export 可新增，修改或删除环境变量，供后续执行的程序使用。
#改变环境变量，禁用homebrew更新，直到使用brew install,brew upgrade, brew tap 命令


# export ALL_PROXY=socks5h://localhost:1080
#使用环境变量设置代理，设置代理，只在当前终端有效；写入配置文件(如: .bashrc)永久有效
#socks5h用于本地不能解析目标主机域名(比如google),由代理服务器解析目标主机域名
补充：
代理（英语：Proxy），也称网络代理，是一种特殊的网络服务，允许一个网络终端（一般为客户端）通过这个服务与另一个网络终端（一般为服务器）进行非直接的连接。一些网关、路由器等网络设备具备网络代理功能。一般认为代理服务有利于保障网络终端的隐私或安全，防止攻击。



# Clear caches 清除缓存和浏览记录
rm -f $(brew --cache)/*.incomplete
# rm -f 强制删除brew --cache的下载缓存
# brew --cache 这个命令会找到brew下载的缓存的地方



echo "==> gcc 完成"
brew install gcc@5
brew install gcc
brew install gpatch pkg-config

#下载gcc和pkg-config
#gcc是GNU编译器套件(GNU Compiler Collection)包括C、C++、Objective-C、Fortran、Java、Ada和Go语言的前端，也包括了这些语言的库，GCC的初衷是为GNU操作系统专门编写的一款编译器。
#pkg-config可用与列举出某个库的相关信息，比如此库的路径、相关头文件路径等，这在程序编译时将非常有用。例如，现在要编译一个依赖librtmp.so库的程序。pkg-config能找出头文件的路径，也能找出库存放在哪，而且还能知道依赖的其它库。
pkg-config常用参数：
–-list-all     列出所有已安装的共享库
-–cflags     列出指定共享库的预处理和编译flag。
-–libs     列出指定共享库的链接flag。



# 下载perl 完成
echo "==> Install Perl 5.34"
brew install perl


if grep -q -i PERL_534_PATH $HOME/.bashrc; then
#忽略大小写，看$HOME/.bashrc文件夹内能否匹配到PERL_534_PATH
    echo "==> .bashrc already contains PERL_534_PATH"
else
    echo "==> Updating .bashrc with PERL_534_PATH..."
    PERL_534_BREW=$(brew --prefix)/Cellar/$(brew list --versions perl | sed 's/ /\//' | head -n 1)
    #--prefix设置安装目录，
    #sed 's/ /\//'替换掉文本
    #head -n 1显示文件的前一行

    #brew --prefix：linux安装软件采用源码安装灵活自由，适用于不同的平台，维护也十分方便。指定安装到某个目录，执行成功后再编译、安装(make，make install);安装完成将自动生成目录,而且该软件任何的文档都被复制到这个目录。用了-prefix选项的另一个好处是卸载软件或移植软件。当某个安装的软件不再需要时，只须简单的删除该安装目录，就能够把软件卸载得干干净净;移植软件只需拷贝整个目录到另外一个机器即可(相同的操作系统)
    

    PERL_534_PATH="export PATH=\"$PERL_534_BREW/bin:\$PATH\""
    #设置环境变量，路径

    echo '# PERL_534_PATH' >> $HOME/.bashrc
    echo $PERL_534_PATH    >> $HOME/.bashrc
    echo >> $HOME/.bashrc

    # make the above environment variables available for the rest of this script让上面的环境变量对脚本的其余部分可用
    eval $PERL_534_PATH
    #和echo不同，eval会执行这个命令两次，可以显示出复杂嵌套的结果
fi

hash cpanm 2>/dev/null || {
    #hash缓存，大大提高命令的调用速率
    #CPANM是安装Perl模块最方便的方法，自动下载和安装依赖项包，使用cpan shell或下载源程序包来安装模块，会遇到很多依赖项
    #向dev/null丢入任何东西都返回成功值，而不会有真正东西写入

    curl -L https://cpanmin.us |
        perl - -v --mirror-only --mirror http://mirrors.ustc.edu.cn/CPAN/ App::cpanminus
        #指定镜像url，且只从镜像下载
}
#调用cpanm缓存，先用capnm安装Perl，如果不行再换网页安装
#安装成功结果：Successfully installed App-cpanminus-1.7046 完成




# Some building tools 完成
echo "==> Building tools"
brew install autoconf libtool automake # autogen
brew install cmake
brew install bison flex

#GNU 构建系统，主要组成部分有Autoconf、Automake和Libtool。
#Autoconf是一个用于生成shell脚本的工具，可以自动配置软件源代码以适应多种类似POSIX的系统解决了系统特使构建和运行时信息的难题，但在软件开发时还有更多的难题，GNU构建系统是为了更好的开发软件而开发的一套完整的公益事业。
#Automake为了兼容各个系统的make使用。从Makefile.am文件和Autoconf一起生成Makefile.in文件。
#Libtool生产动态的共享库是非常困难的事情，每个系统都有自己的编译工具、编译标志、etc.。Libtool会处理所有的共享库请求。需要共享库的时候会自Libtool会自动地被使用，无需知晓其语法规则。
#CMake是一个跨平台的安装(编译)工具,可以用简单的语句来描述所有平台的安装(编译过程)。他能够输出各种各样的makefile或者project文件,能测试编译器所支持的C++特性,类似UNIX下的automake。




# libs 完成  #需要导出的JAVA包
brew install gd gsl jemalloc boost # fftw
brew install libffi libgit2 libxml2 libgcrypt libxslt
brew install pcre libedit readline sqlite nasm yasm
brew install bzip2 gzip libarchive libzip xz
# brew link --force libffi

# python 有一点点小问题，也是找不到bottle 但是问题不大，已经安装好
brew install python@3.9

if grep -q -i PYTHON_39_PATH $HOME/.bashrc; then
#忽略大小写，看$HOME/.bashrc文件夹内能否匹配到PYTHON_39_PATH
    echo "==> .bashrc already contains PYTHON_39_PATH"
else
    echo "==> Updating .bashrc with PYTHON_39_PATH..."
    PYTHON_39_PATH="export PATH=\"$(brew --prefix)/opt/python@3.9/bin:$(brew --prefix)/opt/python@3.9/libexec/bin:\$PATH\""
    echo '# PYTHON_39_PATH' >> $HOME/.bashrc
    echo ${PYTHON_39_PATH} >> $HOME/.bashrc
    echo >> $HOME/.bashrc

    # make the above environment variables available for the rest of this script
    eval ${PYTHON_39_PATH}
fi

# https://github.com/Homebrew/homebrew-core/issues/43867
# Upgrading pip breaks pip
#pip3 install --upgrade pip setuptools wheel


# r
# brew install wang-q/tap/r@3.6.1
echo "==> Install R"
brew install r
cpanm --mirror-only --mirror http://mirrors.ustc.edu.cn/CPAN/ --notest Statistics::R
#指定镜像url，且只从镜像下载

#找不到某个依赖包版本，解决办法：单独下载 brew install 不行的依赖包
#台式机安装顺利




# java
echo "==> Install Java"
if [[ "$OSTYPE" == "darwin"* ]]; then
#如果操作系统类型是苹果系统
    brew install openjdk
else
    brew install openjdk
    #OpenJDK是GPL许可（GPL-licensed）的Java平台的开源化实现
    brew link openjdk --force
    #brew link xxx --force因为权限问题brew往往并不会像预想的那样把一切都安装完善。安装过程会报错，安装好的软件无法正常使用；需要人工干预修复这个错误。
fi
brew install ant maven
#Ant是一种基于Java的build（编译，打包）工具。

#根据操作系统类型不同选择不同的安装方式

  

# pin these
# brew pin perl    #pin用来锁定某个包
# brew pin python@3.9
brew pin r

# other programming languages
brew install lua node

# taps
brew tap wang-q/tap

# download tools
brew install aria2 curl wget

# gnu
brew install gnu-sed gnu-tar

# other tools
brew install screen stow htop parallel pigz
brew install tree pv
brew install jq pup datamash miller tsv-utils
brew install librsvg udunits
brew install proxychains-ng
brew install bat exa tealdeer # tiv
brew install hyperfine ripgrep tokei
brew install zellij bottom

# large packages
if [[ "$OSTYPE" == "darwin"* ]]; then
    brew install gpg2
fi

hash pandoc 2>/dev/null || {
    brew install pandoc
}
#相当于直接安装


hash gnuplot 2>/dev/null || {
    brew install gnuplot
}

hash dot 2>/dev/null || {
    brew install graphviz
}

hash convert 2>/dev/null || {
    brew install imagemagick
}

# weird dependancies by Cairo.pm 安装依赖项
# brew install linuxbrew/xorg/libpthread-stubs linuxbrew/xorg/renderproto linuxbrew/xorg/kbproto linuxbrew/xorg/xextproto

# gtk+3
# brew install gsettings-desktop-schemas gtk+3 adwaita-icon-theme gobject-introspection
```
