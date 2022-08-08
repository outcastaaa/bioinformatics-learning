# Optional: MySQL

```shell script
bash $HOME/Scripts/dotfiles/mysql.sh

# Following the prompts, create mysql users and install DBD::mysql

```
#  $HOME/Scripts/dotfiles/mysql.sh 内容：
```
#!/usr/bin/env bash

mkdir -p $HOME/share/
#创建$HOME/share/目录

echo "==> Download mysql"
wget -N -P /tmp https://mirrors.ustc.edu.cn/mysql-ftp/Downloads/MySQL-5.1/mysql-5.1.73.tar.gz
#指定下载文件的存放目录，且除非远程文件比较新否则不再取回
#将mysql-5.1.73.tar.gz下载到/tmp目录下

echo "==> compile mysql"
cd $HOME/share/
rm -fr mysql-*
tar xvfz /tmp/mysql-5.1.73.tar.gz
cd mysql-*
#挂载到$HOME/share/，强制卸载mysql-*，检查卸载以前版本
#x : 从 tar 包中把文件提取出来；v : 显示详细信息；z : 表示 tar 包是被 gzip 压缩过的，所以解压时需要用 gunzip 解压；f xxx.tar.gz :  指定被处理的文件是 xxx.tar.gz
#将/tmp/mysql-5.1.73.tar.gz解压到$HOME/share/文件下，最后挂载到 $HOME/share/mysql-*


export MYSQL_USER=`whoami`
export MYSQL_DIR=$HOME/share/mysql


if [[ `uname` == 'Darwin' ]]; then
    export CC=gcc-5
    export CXX=gcc-5
else
    export CC=gcc-5
    export CXX=gcc-5
fi
#看系统是不是苹果Darwin系统，指定gcc环境变量的版本


CFLAGS="-O3 -fPIC" CXXFLAGS="-O3 -fPIC -felide-constructors -fno-exceptions -fno-rtti" \
#CFLAGS参数，O代表默认优化，GCC 对源代码进行基本优化。这些优化在大多数情况下都会使程序执行的更快。-O3高级优化
#-felide-constructors 如果看上去合理就省略构造子(仅C++)
#-fno-rtti 禁用运行时类型信息；-fno-exceptions 禁用异常机制，一般只有对程序运行效率及资源占用比较看重的场合才会使用
    ./configure \
    #CFLAGS="-O3 -fPIC" ./configure   //指定编译器参数
    --prefix=${MYSQL_DIR} \
    #--prefix指定安装位置，把所有文件（执行文件、库文件、配置文件、其他资源文件）存放在$HOME/share/mysql目录下
    --with-extra-charsets=complex \
    #表示支持大字符集
    --enable-thread-safe-client \
    #表示客户端API应该保持线程安全(但貌似Mysql 5.0有bug，即使开启此参数，有时也非线程安全)
    --enable-local-infile \
    #启用对LOAD DATA LOCAL INFILE语法的支持(默认不支持)
    --enable-shared \
    #--enable-shared：生成动态链接库，又称为共享库，
    #即编译时只对库进行简单的引用而不载入程序，在程序运行时才将动态库中的代码载入内存使用，因此使用动态库的程序在运行时需要其相关的动态库都存在。
    --enable-assembler \
    #允许使用汇编模式，性能优化
    --without-docs \
    --without-man \
    --without-debug \
    #编译为产品版，放弃debugging代码
    --with-plugins=myisam \
    --disable-dependency-tracking \
    --with-mysqld-user=${MYSQL_USER} \
    --localstatedir=${MYSQL_DIR}/data \
    #指定只能单机使用的可写数据的安装位置
    --sysconfdir=${MYSQL_DIR} \
    #指定在单个机器上使用的只读数据的安装位置
    --with-unix-socket-path=${MYSQL_DIR}/mysql.sock

make -j 4
#线程运行，即4个进程去竞争多核cpu
make install
#从Makefile中读取指令，安装已经编译好的程序到指定的位置



mkdir -p ${MYSQL_DIR}/data
#创建$HOME/share/mysql/data目录

if grep -q -i mysqlbin $HOME/.bashrc; then
#不区分大小写，看能否在$HOME/.bashrc匹配到mysqlbin
    echo "==> .bashrc already contains mysqlbin"
else
    echo "==> Update .bashrc"

    LB_PATH='export PATH="$HOME/share/mysql/bin:$PATH"'
    #在原有$PATH基础上，加上$HOME/share/mysql/bin的环境变量
    echo '# mysqlbin' >> $HOME/.bashrc
    echo ${LB_PATH} >> $HOME/.bashrc
    echo >> $HOME/.bashrc

    eval ${LB_PATH}
    #将变量设置写入$HOME/.bashrc
fi
#配置环境变量




cat <<EOF > ${MYSQL_DIR}/my.cnf

#配置文件，将下列内容写入$HOME/share/mysql/my.cnf



[mysqld]
user=${MYSQL_USER}       #=`whoami`
basedir=${MYSQL_DIR}       #=$HOME/share/mysql
# datadir=${MYSQL_DIR}/data    #=$HOME/share/data
port=3306
socket=${MYSQL_DIR}/mysql.sock     #=$HOME/share/mysql.sock 

loose-skip-innodb         #去掉“skip-innodb”的注释,表示跳过Innodb模式
skip-external-locking        #即“跳过外部锁定”, External-locking用于多进程条件下为MyISAM数据表进行锁定。

character-set-server=latin1      # 服务器安装时指定的默认字符集设定
default-storage-engine = MyISAM   #表示永久表(permanent tables)的默认存储引擎

key_buffer_size = 4096M
max_allowed_packet = 16M
table_open_cache = 512
sort_buffer_size = 16M
read_buffer_size = 16M
read_rnd_buffer_size = 32M
myisam_sort_buffer_size = 128M
thread_cache_size = 16
thread_concurrency = 32
query_cache_size = 128M
query_cache_limit = 8M

[mysqld_safe]
log-error=${MYSQL_DIR}/mysqld.log
pid-file=${MYSQL_DIR}/mysqld.pid
socket=${MYSQL_DIR}/mysql.sock

[client]
port=3306
user=${MYSQL_USER}
socket=${MYSQL_DIR}/mysql.sock

[mysqladmin]
user=root
port=3306
socket=${MYSQL_DIR}/mysql.sock

[mysql]
port=3306
socket=${MYSQL_DIR}/mysql.sock

[mysql_install_db]
user=${MYSQL_USER}
port=3306
basedir=${MYSQL_DIR}
datadir=${MYSQL_DIR}/data
socket=${MYSQL_DIR}/mysql.sock

[myisamchk]
key_buffer_size = 256M
sort_buffer_size = 256M
read_buffer = 16M
write_buffer = 16M

EOF

echo "==> Fill mysql system tables"
unset TMPDIR
#nnodb_tmpdir是在innodb online ddl中提到的一个参数；大致的意思是innodb在做online-ddl的时候会向临时目录写入“临时排序文件”，这里的临时目录默认就是由“tmpdir”这个参数的值
unset MYSQL_USER
unset MYSQL_DIR
#删除某些变量


mysql_install_db
#目的是生成新的MySQL授权表。它不覆盖已有的MySQL授权表，并且它不影响任何其它数据。初始化 InnoDB 表所需要的系统表空间和相关的数据结构


echo "==> Start mysql service"
cd $HOME/share/mysql
$HOME/share/mysql/bin/mysqld_safe &
#启动SQL
sleep 5
#休眠、暂停线程 for 5s


echo "==> Securing mysql service"
if [ "$(whoami)" == 'vagrant' ]; then
    cat <<EOF | mysql_secure_installation

Y
vagrant
vagrant
Y
Y
Y
Y

EOF
else
    mysql_secure_installation
fi

rm -fr $HOME/share/mysql-*
#使用brew命令安装完mysql后，根据提示我们可以知道当前root是没有密码的，我们可以通过执行mysql_secure_installation命令来进行安全设置。


# create mysql user
cat <<EOF



# copy & paste the following lines to command prompt; then type password of mysql root
复制并粘贴以下行到命令提示符;然后输入mysql root密码


source $HOME/.bashrc
mysql -uroot -p -e "GRANT ALL PRIVILEGES ON保证特权打开 *.* TO 'alignDB'@'%' IDENTIFIED BY 'alignDB'"
#使用命令mysql -uroot登陆, mysql -u root -p进行密码链接


# normal startup  #启动服务
~/share/mysql/bin/mysqld_safe &

# shutdown       #关闭服务
~/share/mysql/bin/mysqladmin shutdown -uroot -p

# DBD::mysql
cpanm --notest DBD::mysql
#用cpanm模块安装




# If the above command failed, use the following
cpanm --look DBD::mysql
mkdir -p /tmp/mysql-static && cp $HOME/share/mysql/lib/mysql/*.a /tmp/mysql-static
#创建/tmp/mysql-static目录, 创建成功后再复制  $HOME/share/mysql/lib/mysql/*.a /tmp/mysql-static内文件

perl Makefile.PL --testuser alignDB --testpassword alignDB --cflags="-I${HOME}/share/mysql/include/mysql -fPIC -DUNIV_LINUX -DUNIV_LINUX" --libs="-L/tmp/mysql-static -lmysqlclient -lz -lcrypt -lnsl -lm"
make test  #对编译结果进行测试
make install       
#从Makefile中读取指令,安装已经编译好的程序到指定的位置

EOF

```




# Optional: dotfiles

```shell script
bash $HOME/Scripts/dotfiles/install.sh
source $HOME/.bashrc

git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
vim +PluginInstall +qall

```

Edit `.gitconfig` to your own manually.
   




#  $HOME/Scripts/dotfiles/install.sh内容：
```
#bash $HOME/Scripts/dotfiles/install.sh内容：

#!/usr/bin/env bash

# check gnu stow is installed
hash stow 2>/dev/null || {
    echo >&2 "GNU stow is required but it's not installed.";
    echo >&2 "Install with homebrew: brew install stow";
    exit 1;
}
#如果系统中已经有了stow，直接hash调取，出错写入/dev/null文件；不管成功与否，都进行下一步
# >&2：将进度信息写入stderr，这样脚本即可以方便地和其他应用协作（通过管道或者重定向到文件作为其他应用的输入），同时也可以在屏幕上看到进度。
#exit（1）：非正常运行导致退出程序



# colors in term
# http://stackoverflow.com/questions/5947742/how-to-change-the-output-color-of-echo-in-linux
GREEN=
RED=
NC=
if tty -s < /dev/fd/1 2> /dev/null; then
  GREEN='\033[0;32m'
  RED='\033[0;31m'
  NC='\033[0m' # No Color
fi
#2>/dev/null意思就是把错误输出到“黑洞”
#tty(teletypewriter)指令查询目前使用的终端机的文件名称，-s或--silent或--quiet 不显示任何信息，只回传状态代码。
#/dev/fd/1是指标准输出
#设置字体颜色


# echo -e 处理特殊字符
log_warn () {
  echo -e "==> ${RED}$@${NC} <=="
}
#出现warn，用红色显示

log_info () {
  echo -e "==> ${GREEN}$@${NC}"
}
#出现info提示，用绿色显示

log_debug () {
  echo -e "==> $@"
}
#出现bug



# enter BASE_DIR
BASE_DIR=$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )
cd "${BASE_DIR}" || exit
#将目录挂载到$HOME/Scripts/dotfiles/下，后退出


# stow configurations
mkdir -p ~/.config
#创建~/.config目录


log_warn "Restow dotfiles"
#出现警示提示“Restow dotfiles”
DIRS=( stow-git stow-latexmk stow-perltidy stow-screen stow-wget stow-vim stow-proxychains )


for d in ${DIRS[@]}; do
    log_info "${d}"
    for f in $(find ${d} -maxdepth 1 | cut -sd / -f 2- | grep .); do
        homef="${HOME}/${f}"
        if [ -h "${homef}" ] ; then
            log_debug "Symlink exists: [${homef}]"
        elif [ -f "${homef}" ] ; then
            log_debug "Delete file: [${homef}]"
            rm "${homef}"
        elif [ -d "${homef}" ] ; then
            log_debug "Delete dir: [${homef}]"
            rm -fr "${homef}"
        fi
    done
	stow -t "${HOME}" "${d}" -v 2
done
#d是DIRS=( stow-git stow-latexmk stow-perltidy stow-screen stow-wget stow-vim stow-proxychains )中的变量，执行下列命令
#出现信息提示“该变量名称”
#find ${d} -maxdepth 1指定遍历搜索的最大深度。以当前目录 . 作为起始深度1，即只列出当前目录下的所有普通文件
#cut -sd：-s不打印没有包含分界符的行;-d使用指定分界符代替制表符作为区域分界




# don't ruin Ubuntu
log_info "stwo-bash"
#出现信息提示“stwo-bash”
stow -t "${HOME}" stow-bash -v 2
```

# 补充内容:
1.   configure  
![tupian](./pictures/%E5%9B%BE%E7%89%8720.png)
2. tty命令  
![tu](./pictures/%E5%9B%BE%E7%89%8721.png)
