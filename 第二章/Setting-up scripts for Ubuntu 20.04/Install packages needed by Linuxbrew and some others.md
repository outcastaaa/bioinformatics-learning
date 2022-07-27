## Install packages needed by Linuxbrew and some others

```shell script
以下在终端的Ubuntu中运行

echo "==> When some packages went wrong, check http://mirrors.ustc.edu.cn/ubuntu/ for updating status."
bash -c "$(curl -fsSL https://raw.githubusercontent.com/wang-q/ubuntu/master/prepare/1-apt.sh)" 

# curl -fsSL:
-f (--fail) 表示在服务器错误时，阻止一个返回的表示错误原因的 html 页面；-s，--silent：安静模式。不显示进度表或错误信息，使curl静音，它仍然会输出您请求的数据，甚至可能输出到终端stdout，除非您对它进行重定向；--show-error：当与-s，--silent一起使用时，它会使curl在失败时显示错误消息；-L,--location:如果服务器报告请求的页面已移动到其他位置（用location:header和3xx 响应代码），此选项将使curl在新位置上重新执行请求。
```

## https://raw.githubusercontent.com/wang-q/ubuntu/master/prepare/1-apt中内容：
```shell script
#!/usr/bin/env bash
#在开头一行指定脚本的解释程序。脚本用env启动的原因，是因为脚本解释器在linux中可能被安装于不同的目录，env可以在系统的PATH目录中查找。同时，env还规定一些系统环境量。 而如果直接将解释器路径写死在脚本里，可能在某些系统就会存在找不到解释器的兼容性问题。  


echo "====> Install softwares via apt-get 通过apt-get安装软件<====" 

1. echo用于显示提示；
2. apt-get，Advanced Package Tool，是一条linux命令，适用于deb包管理式的操作系统，主要用于自动从互联网的软件仓库中搜索、安装、升级、卸载软件或操作系统。



echo "==> Disabling the release upgrader禁用发布升级程序"
sudo sed -i.bak 's/^Prompt=.*$/Prompt=never/' /etc/update-manager/release-upgrades

#对每行匹配到的  ^Prompt=.*$  替换为  Prompt=never,将''修改内容存放到/etc/update-manager/release-upgrades文件, 并且以/etc/update-manager/release-upgrades.bak文件将原文件备份

1. sudo 表示 “superuser do”。 它允许已验证的用户以其他用户的身份来运行命令。其他用户可以是普通用户或者超级用户。然而，大部分时候我们用它来以提升的权限来运行命令。
2. sed –i.bak ‘s/hello/A/’ file.txt
把修改内容保存到file.txt，同时会以file.txt.bak文件备份原来未修改文件内容，以确保原始文件内容安全性，防止错误操作而无法恢复原来内容。




echo "==> Switch to an adjacent mirror将镜像源转为临近镜像源"
# https://lug.ustc.edu.cn/wiki/mirrors/help/ubuntu 


cat <<EOF > list.tmp    
#编辑文档内容，把Ubuntu系统自带的源修改为国内的源，将下面内容写入list.tmp文件

1. cat<<EOF> 文件名
用来创建文件
在这之后输入任何东西 都是在 文件里的
输入完成之后EOF结尾 代表结束



deb https://mirrors.ustc.edu.cn/ubuntu/ focal main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal main restricted universe multiverse
#镜像源文件类型——仓库地址 ——发行版本，即所使用的Ubuntu版本 20.04：focal——软件包分类

1. deb行是相对于二进制软件包的，您可以使用进行安装apt。
2. deb-src相对于源代码包（由下载apt-get source $package），然后进行编译。
仅当您要自己编译某些软件包或检查源代码中是否有错误时，才需要源软件包。普通用户不需要包含此类存储库。



deb https://mirrors.ustc.edu.cn/ubuntu/ focal-security main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-security main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-updates main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-backports main restricted universe multiverse

1. Backport：是将一个软件的补丁应用到比此补丁所对应的版本更老的版本的行为。这是软件开发过程中维护步骤的一部分。最简单也可能是最常见的例子，就是针对某个软件的某个漏洞的补丁。某个软件的新版本发现了漏洞，通过修补源代码后可以修复；但此软件的旧版本因为源代码不同，而不能通过同样的修补来修复，这时就需要针对旧版本的软件来进行源代码修补了。


## Not recommended预发布软件源，不建议启用
# deb https://mirrors.ustc.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse

EOF


if [ ! -e /etc/apt/sources.list.bak ]; then
    sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
fi
sudo mv list.tmp /etc/apt/sources.list
#如果/etc/apt/sources.list.bak文件不存在,则将/etc/apt/sources.list备份到bak文件中，再将list.tmp文件软件源内容转移到/etc/apt/sources.list中

1. /etc/apt/sources.list是储存镜像源的文件
2. ！-e表示取非，如果filename存在exist，则为假。
3.Copy备份 备份source源文件，复制一份/etc/apt/sources.list 文件，以作备用,其中 sources.list.bak是备份文件名
4. mv /xxx/file/* /xxx/new/
上面的命令代表了将file文件夹下的所有文件都移动到new文件夹下。


#上面几步用于修改镜像源



# Virtual machines needn't this and I want life easier.
# https://help.ubuntu.com/lts/serverguide/apparmor.html

if [ "$(whoami)" == 'vagrant' ]; then
    echo "==> Disable AppArmor"
    sudo service apparmor stop
    sudo update-rc.d -f apparmor remove
    #从服务中删除（在系统启动时不再加载）
fi
#如果用户为vagrant，则禁用AppArmor

1.AppArmor是一个高效和易于使用的Linux系统安全应用程序。AppArmor对操作系统和应用程序所受到的威胁进行从内到外的保护，甚至是未被发现的0day漏洞和未知的应用程序漏洞所导致的攻击。AppArmor安全策略可以完全定义个别应用程序可以访问的系统资源与各自的特权。


echo "==> Disable whoopsie"
sudo sed -i 's/report_crashes=true/report_crashes=false/' /etc/default/whoopsie
sudo service whoopsie stop
#关闭whoopsie，要在Ubuntu服务器上禁用它，编辑/etc/default/whoopsie文件并将report_crashes=更改为false，然后运行sudo stop whoopsie

1.whoopsie是“Ubuntu错误报告”守护程序，默认安装在桌面/服务器安装中。
当某件事崩溃时，whoopsie会做两件事：
收集Apport生成的崩溃报告；将它们发送到Ubuntu /Canonical（具体到https://daisy.ubuntu.com）
2. 用sed -i 's/原字符串/新字符串/' ab.txt ,对每行匹配到的第一个字符串进行替换


echo "==> Install linuxbrew dependences安装linuxbrew依赖包"
sudo apt-get -y update
sudo apt-get -y upgrade
sudo apt-get -y install build-essential curl file git
sudo apt-get -y install libbz2-dev zlib1g-dev libzstd-dev
# sudo apt-get -y install libcurl4-openssl-dev libexpat-dev libncurses-dev
#apt-get -y install这个指令则是跳过系统提示,直接安装

1.apt-get update更新软件列表
访问服务器，更新可获取软件及其版本信息，但仅仅给出一个可更新的list
2.具体更新需要通过apt-get upgrade，apt-get upgrade可将软件进行更新
这个命令，会把本地已安装的软件，与刚下载的软件列表里对应软件进行对比，如果发现已安装的软件版本太低，就会提示你更新。如果你的软件都是最新版本，会提示：升级了0个软件包，新安装了0个软件包，要卸载0个软件包，有0个软件包未被升级。



echo "==> Install other software"
sudo apt-get -y install aptitude parallel vim screen xsltproc numactl

echo "==> Install develop libraries"
# sudo apt-get -y install libreadline-dev libedit-dev
sudo apt-get -y install libdb-dev libxml2-dev libssl-dev libncurses5-dev # libgd-dev
# sudo apt-get -y install gdal-bin gdal-data libgdal-dev # /usr/lib/libgdal.so: undefined reference to `TIFFReadDirectory@LIBTIFF_4.0'
# sudo apt-get -y install libgsl0ldbl libgsl0-dev

# Gtk stuff, Need by alignDB
# install them in a fresh machine to avoid problems

echo "==> Install gtk3"
#GTK+（GIMP Toolkit)是一套源码以LGPL许可协议分发、跨平台的图形工具包。最初是为GIMP写的，已成为一个功能强大、设计灵活的一个通用图形库，是GNU/Linux下开发图形界面的应用程序的主流开发工具之一。

#sudo apt-get -y install libcairo2-dev libglib2.0-0 libglib2.0-dev libgtk-3-dev libgirepository1.0-dev
#sudo apt-get -y install gir1.2-glib-2.0 gir1.2-gtk-3.0 gir1.2-webkit-3.0

echo "==> Install gtk3 related tools"
# sudo apt-get -y install xvfb glade

echo "==> Install graphics tools三种画图工具"
sudo apt-get -y install gnuplot graphviz imagemagick

#echo "==> Install nautilus plugins"
#sudo apt-get -y install nautilus-open-terminal nautilus-actions

# Mysql will be installed separately.
# Remove system provided mysql package to avoid confusing linuxbrew.


echo "==> Remove system provided mysql"
#MySQL是一种关联数据库管理系统，使用完整的管理系统统一管理，易于查询。在 WEB 应用方面 MySQL 是最好的 RDBMS(Relational Database Management System：关系数据库管理系统)应用软件之一。


# sudo apt-get -y purge mysql-common

echo "==> Restore original sources.list"
if [ -e /etc/apt/sources.list.bak ]; then
    sudo rm /etc/apt/sources.list 
     #删除该文件 将/etc/apt/sources.list.bak 内容写入/etc/apt/sources.list
    sudo mv /etc/apt/sources.list.bak /etc/apt/sources.list
fi

echo "====> Basic software installation complete! <===="

```