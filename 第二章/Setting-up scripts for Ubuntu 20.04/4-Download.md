 # download
Fill `$HOME/bin`, `$HOME/share` and `$HOME/Scripts`.

```shell script
curl -LO https://raw.githubusercontent.com/wang-q/dotfiles/master/download.sh  #下载文件

bash download.sh   #bash处理以sh结尾的文件

source $HOME/.bashrc
#source：使当前shell读入路径为filepath的shell文件并依次执行文件中的所有语句，通常用于重新执行刚修改的初始化文件，使之立即生效，而不必注销并重新登录
#.bashrc 是进入.bashrc文件夹，就是用户目录下的名字是.bashrc的目录。
```
##  https://raw.githubusercontent.com/wang-q/dotfiles/master/download.sh内容：
```

#!/usr/bin/env bash

echo "====> Download Genomics related tools <===="
#下载基因组学相关工具

mkdir -p $HOME/bin
mkdir -p $HOME/.local/bin
mkdir -p $HOME/share
mkdir -p $HOME/Scripts
#在HOME目录下创建新的4个文件夹，且不可重名

# make sure $HOME/bin in your $PATH
if grep -q -i homebin $HOME/.bashrc; then
    # 忽略大小写，看$HOME文件夹内能否匹配到Homebin
    echo "==> .bashrc already contains homebin"
else
    echo "==> Update .bashrc"

    HOME_PATH='export PATH="$HOME/bin:$HOME/.local/bin:$PATH"'
    #在$PATH环境变量的基础上，新写入$HOME/bin和$HOME/.local/bin作为新的环境变量
    echo '# Homebin' >> $HOME/.bashrc
    echo $HOME_PATH >> $HOME/.bashrc
    echo >> $HOME/.bashrc

    eval $HOME_PATH
    #将这个文件内所有的复杂程序再执行一遍，类似于echo，但是可执行内部嵌套的更复杂的命令
fi
#若$HOME/.bashrc 文件夹含有Homebin，则显示“已经存在”；如果不包含，则升级homebin，将homebin和$HOME_PATH写入$HOME/.bashrc


# Clone or pull other repos   #复制或者调用其他回购协议
for OP in dotfiles withncbi; do
    #OP是withncbi和dotfiles中的一个，一个个筛选

    if [[ ! -d "$HOME/Scripts/$OP/.git" ]]; then
    #如果"$HOME/Scripts/$OP/.git"不是目录，即没有这个目录，则

        if [[ ! -d "$HOME/Scripts/$OP" ]]; then
        #如果"$HOME/Scripts/$OP"不是目录，即没有这个目录，则直接克隆OP这个仓库为这个目录

            echo "==> Clone $OP"
            git clone https://github.com/wang-q/${OP}.git "$HOME/Scripts/$OP"
            #将网页中内容直接拷贝到文件中

        else

            echo "==> $OP exists"
            #若"$HOME/Scripts/$OP"为目录，则显示已经存在
        fi
    else 
    #如果"$HOME/Scripts/$OP/.git"是目录，已经存在该目录，则

        echo "==> Pull $OP  调用dotfiles  withncbi"
        pushd "$HOME/Scripts/$OP" > /dev/null
        #使用 pushd 和 popd 命令来执行存储目录路径并删除它的操作
        #pushd+文件目录， 向目录栈中添加相应的文件目录，同时当前工作目录调整到相应的目录下，将命令的输出信息输入到 /dev/null中并且不显示信息
        
        git pull
        #git pull的作用是从一个仓库或者本地的分支拉取并且整合代码。
        popd > /dev/null
        #popd 命令不仅会将当前目录切换到 上一个父目录，它还会从目录堆栈中删除这个子目录，即OP目录
    fi
done

#目的：如果含有withncbi或者dotfiles，则进入循环，
#如果没有$HOME/Scripts/dotfiles/.git目录也没有$HOME/Scripts/dotfiles目录，就把https://github.com/wang-q/dotfiles.git内容克隆到$HOME/Scripts/dotfiles文件夹中，
#如果已经有了$HOME/Scripts/dotfiles/.git目录，就把$HOME/Scripts/dotfiles内容转移到dev/dull，即把$HOME/Scripts/dotfiles内容全部丢弃，
#回到原来$HOME/Scripts/dotfiles上一级父目录$HOME/Scripts
#直到dotfiles、ncbi都筛查完，退出循环



# alignDB
# chmod +x $HOME/Scripts/alignDB/alignDB.pl 
给该文件$HOME/Scripts/alignDB/alignDB.pl的执行权限

# ln -fs $HOME/Scripts/alignDB/alignDB.pl $HOME/bin/alignDB.pl  
b 指向a, 即$HOME/bin/alignDB.pl指向$HOME/Scripts/alignDB/alignDB.pl,即两个目录下存放着相同的文件，不是复制是统一个文件的两个路径


echo "==> Jim Kent bin"
cd $HOME/bin/ 
#在$HOME/bin/目录使用


RELEASE=$( ( lsb_release -ds || cat /etc/*release || uname -om ) 2>/dev/null | head -n1 )
#linux中"\"在是一个转义字符，“｜”是一个特殊字符，有“或”的功能 ||上一个命令执行失败后再执行下一个命令

#/etc/*release是系统安装时默认的发行版本信息，通常安装好系统后文件内容不会发生变化。
#用来显示LSB当前版本的描述信息 或 系统安装时默认的发行版本信息 或 打印操作系统和机器硬件CPU名


if [[ $(uname) == 'Darwin' ]]; then
#Darwin是由苹果电脑于2000年所释出的一个开放原始码操作系统。Darwin 是MacOSX操作环境的操作系统成份。
#苹果电脑于2000年把Darwin 释出给开放原始码社群。
#现在的Darwin皆可以在苹果电脑的PowerPC 架构和X86  架构下执行，而后者的架构只有有限的驱动程序支援。

    curl -L https://github.com/wang-q/ubuntu/releases/download/20190906/jkbin-egaz-darwin-2011.tar.gz
    #curl-L 跟随网站的跳转 直接下载这个安装包

else

    if echo ${RELEASE} | grep CentOS > /dev/null ; then
    #将命令的输出信息输入到 /dev/null中并且不显示信息
    #CentOS是免费的、开源的、可以重新分发的开源操作系统 ，CentOS(Community Enterprise Operating System，
    #中文意思是社区企业操作系统，是Linux发行版之一。CentOS Linux发行版是一个稳定的，可预测的，可管理的和可复现的平台

        curl -L https://github.com/wang-q/ubuntu/releases/download/20190906/jkbin-egaz-centos-7-2011.tar.gz
    else
        curl -L https://github.com/wang-q/ubuntu/releases/download/20190906/jkbin-egaz-ubuntu-1404-2011.tar.gz
    fi
fi \
    > jkbin.tar.gz

#如果uname == 'Darwin'，即系统是苹果系统则下载darwin-2011.tar.gz；如果不，则下载其他系统版本。
#不同系统下载不同版本的Jim Kent bin



echo "==> untar from jkbin.tar.gz"
tar xvfz jkbin.tar.gz x86_64/axtChain
tar xvfz jkbin.tar.gz x86_64/axtSort
tar xvfz jkbin.tar.gz x86_64/axtToMaf
tar xvfz jkbin.tar.gz x86_64/chainAntiRepeat
tar xvfz jkbin.tar.gz x86_64/chainMergeSort
tar xvfz jkbin.tar.gz x86_64/chainNet
tar xvfz jkbin.tar.gz x86_64/chainPreNet
tar xvfz jkbin.tar.gz x86_64/chainSplit
tar xvfz jkbin.tar.gz x86_64/chainStitchId
tar xvfz jkbin.tar.gz x86_64/faToTwoBit
tar xvfz jkbin.tar.gz x86_64/lavToPsl
tar xvfz jkbin.tar.gz x86_64/netChainSubset
tar xvfz jkbin.tar.gz x86_64/netFilter
tar xvfz jkbin.tar.gz x86_64/netSplit
tar xvfz jkbin.tar.gz x86_64/netSyntenic
tar xvfz jkbin.tar.gz x86_64/netToAxt

mv $HOME/bin/x86_64/* $HOME/bin/
#将目录/$HOME/bin/x86_64中的所有文件移到当前目录($HOME/bin/)中
rm jkbin.tar.gz
#上面两块：先下载jkbin再解压，再删除压缩包


if [[ $(uname) == 'Darwin' ]]; then
    curl -L http://hgdownload.soe.ucsc.edu/admin/exe/macOSX.x86_64/faToTwoBit
else
    curl -L http://hgdownload.soe.ucsc.edu/admin/exe/linux.x86_64/faToTwoBit
fi \
    > faToTwoBit

mv faToTwoBit $HOME/bin/
#下载Linux版本的faToTwoBit
#将faToTwoBit改名为$HOME/bin/

chmod +x $HOME/bin/faToTwoBit
#给 $HOME/bin/faToTwoBit文件夹的执行权限
```



## 查看运行结果的方法：
```
xuruizhi@LAPTOP-VVGKELBI:~$ ls
#查看home文件夹下目录
Scripts  bin  download.sh  share


xuruizhi@LAPTOP-VVGKELBI:~$ cd Scripts 
#change 目录到Scripts

xuruizhi@LAPTOP-VVGKELBI:~/Scripts$ ls
dotfiles  withncbi

xuruizhi@LAPTOP-VVGKELBI:~/Scripts$ cd dotfiles  
#change目录到dotfiles

xuruizhi@LAPTOP-VVGKELBI:~/Scripts/dotfiles$ ls
#查看dotfiles文件夹下目录
README.md    conda        images      mpv        packages  standalone     stow-proxychains  zotero
asdf.sh      download.sh  install.sh  mysql.sh   perl      stow-bash      stow-screen       生信学习流程.md
bin          genomics.sh  jksrc.sh    mysql8.sh  python    stow-git       stow-vim
brew.sh      hackintosh   macos.sh    nodejs     r         stow-latexmk   stow-wget
brewcask.sh  hammerspoon  misc        others.sh  rust      stow-perltidy  tex


xuruizhi@LAPTOP-VVGKELBI:~/Scripts/dotfiles$ cd ../../ 
#change目录到原大目录

xuruizhi@LAPTOP-VVGKELBI:~$ bash download.sh
====> Download Genomics related tools <====
==> .bashrc already contains homebin
==> Pull dotfiles
Already up to date.
==> Pull withncbi
Already up to date.
==> Jim Kent bin
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:--  0:02:00 --:--:--     0
curl: (52) Empty reply from server
==> untar from jkbin.tar.gz

gzip: stdin: unexpected end of file
tar: Child returned status 1
tar: Error is not recoverable: exiting now
mv: cannot stat '/home/xuruizhi/bin/x86_64/*': No such file or directory
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 5437k  100 5437k    0     0  1402k      0  0:00:03  0:00:03 --:--:-- 1402k
mv: 'faToTwoBit' and '/home/xuruizhi/bin/faToTwoBit' are the same file


xuruizhi@LAPTOP-VVGKELBI:~$ ls
Scripts  bin  download.sh  share

xuruizhi@LAPTOP-VVGKELBI:~$ cd bin
xuruizhi@LAPTOP-VVGKELBI:~/bin$ ls
#查看bin文件夹下目录 下载
axtChain  axtToMaf         chainMergeSort  chainPreNet  chainStitchId  lavToPsl        netFilter  netSyntenic  x86_64
axtSort   chainAntiRepeat  chainNet        chainSplit   faToTwoBit     netChainSubset  netSplit   netToAxt
xuruizhi@LAPTOP-VVGKELBI:~/bin$ cd ../../
xuruizhi@LAPTOP-VVGKELBI:/home$
#下载出错时就多试几遍，有时候可能因为网络原因连接不上网页，可以通过查看各级文件夹里子目录看是否已经下载
```
## 成功下载结果：
```
1. ==> .bashrc already contains homebin
==> Pull dotfiles
2. Already up to date.
==> Pull withncbi
Already up to date.
3.  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
100 1203k  100 1203k    0     0   430k      0  0:00:02  0:00:02 --:--:-- 1070k
4. xuruizhi@LAPTOP-VVGKELBI:~/bin$ ls
axtChain  axtToMaf         chainMergeSort  chainPreNet  chainStitchId  lavToPsl        netFilter  netSyntenic  x86_64
axtSort   chainAntiRepeat  chainNet        chainSplit   faToTwoBit     netChainSubset  netSplit   netToAxt
5. chmod +x $HOME/bin/faToTwoBit  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 5437k  100 5437k    0     0   235k      0  0:00:23  0:00:23 --:--:--  110k
xuruizhi@LAPTOP-VVGKELBI:~/bin$ mv faToTwoBit $HOME/bin/
mv: 'faToTwoBit' and '/home/xuruizhi/bin/faToTwoBit' are the same file
```
