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
#
1. echo用于显示提示；
2. apt-get，Advanced Package Tool，是一条linux命令，适用于deb包管理式的操作系统，主要用于自动从互联网的软件仓库中搜索、安装、升级、卸载软件或操作系统。
#


echo "==> Disabling the release upgrader禁用发布升级程序"
sudo sed -i.bak 's/^Prompt=.*$/Prompt=never/' /etc/update-manager/release-upgrades
#
对每行匹配到的  ^Prompt=.*$  替换为  Prompt=never,将''修改内容存放到/etc/update-manager/release-upgrades文件, 并且以/etc/update-manager/release-upgrades.bak文件将原文件备份

#sudo 表示 “superuser do”。 它允许已验证的用户以其他用户的身份来运行命令。其他用户可以是普通用户或者超级用户。然而，大部分时候我们用它来以提升的权限来运行命令。




echo "==> Switch to an adjacent mirror"
#将镜像源转为临近镜像源
# https://lug.ustc.edu.cn/wiki/mirrors/help/ubuntu 


cat <<EOF > list.tmp    
#将下面内容写入list.tmp文件

deb https://mirrors.ustc.edu.cn/ubuntu/ focal main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal main restricted universe multiverse
#镜像源文件类型——仓库地址 ——发行版本——软件包分类

deb https://mirrors.ustc.edu.cn/ubuntu/ focal-security main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-security main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-updates main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-backports main restricted universe multiverse

## Not recommended
# deb https://mirrors.ustc.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse

EOF


if [ ! -e /etc/apt/sources.list.bak ]; then
    sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
fi
sudo mv list.tmp /etc/apt/sources.list
#如果/etc/apt/sources.list.bak文件不存在,则将/etc/apt/sources.list备份到bak文件中，再将list.tmp文件转移到/etc/apt/sources.list中
#/etc/apt/sources.list是储存镜像源的文件
#上面几步用于修改镜像源



# Virtual machines needn't this and I want life easier.
# https://help.ubuntu.com/lts/serverguide/apparmor.html
if [ "$(whoami)" == 'vagrant' ]; then
    echo "==> Disable AppArmor"
    sudo service apparmor stop
    sudo update-rc.d -f apparmor remove
fi
#如果用户为vagrant，则禁用AppArmor


echo "==> Disable whoopsie"
sudo sed -i 's/report_crashes=true/report_crashes=false/' /etc/default/whoopsie
sudo service whoopsie stop
#用sed -i 's/原字符串/新字符串/' ab.txt ,对每行匹配到的第一个字符串进行替换,关闭whoopsie

echo "==> Install linuxbrew dependences"
sudo apt-get -y update
sudo apt-get -y upgrade
sudo apt-get -y install build-essential curl file git
sudo apt-get -y install libbz2-dev zlib1g-dev libzstd-dev
# sudo apt-get -y install libcurl4-openssl-dev libexpat-dev libncurses-dev
#apt-get -y install这个指令则是跳过系统提示,直接安装

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
#sudo apt-get -y install libcairo2-dev libglib2.0-0 libglib2.0-dev libgtk-3-dev libgirepository1.0-dev
#sudo apt-get -y install gir1.2-glib-2.0 gir1.2-gtk-3.0 gir1.2-webkit-3.0

echo "==> Install gtk3 related tools"
# sudo apt-get -y install xvfb glade

echo "==> Install graphics tools"
sudo apt-get -y install gnuplot graphviz imagemagick

#echo "==> Install nautilus plugins"
#sudo apt-get -y install nautilus-open-terminal nautilus-actions

# Mysql will be installed separately.
# Remove system provided mysql package to avoid confusing linuxbrew.
echo "==> Remove system provided mysql"
# sudo apt-get -y purge mysql-common

echo "==> Restore original sources.list"
if [ -e /etc/apt/sources.list.bak ]; then
    sudo rm /etc/apt/sources.list  #删除该文件 将/etc/apt/sources.list.bak 内容写入/etc/apt/sources.list
    sudo mv /etc/apt/sources.list.bak /etc/apt/sources.list
fi

echo "====> Basic software installation complete! <===="

```