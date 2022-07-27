
# Scoop

## Install Scoop

* Install `Scoop`

```powershell
set-executionpolicy remotesigned -s currentuser  
#[更新执行策略][远程控制][下载到当前用户]，在 PowerShell 中打开远程权限
iex (new-object net.webclient).downloadstring('https://get.scoop.sh')#下载并安装 Scoop

#sudo:调用管理员权限，sudo 命令是 Linux 系统环境下，普通用户在普通用户状态下，完成超级用户所需要完成的任务时执行的命令

scoop install sudo 7zip
#7-Zip 是一款拥有极高压缩比的的开源压缩软件
scoop install aria2 dark innounp
# 安装 Aria2 来加速下载，Aria2是一款免费开源跨平台且不限速的多线程下载软件
#dark innounp 两个都在电脑上没下载成功
```


补充:    
1. 自定义 Scoop 安装目录
``` powershell
$env:SCOOP='D:\scoop'
[Environment]::SetEnvironmentVariable('SCOOP', $env:SCOOP, 'User')
```
2. 安装什么东西之前，可以用 scoop search <>先搜索一下，看看有没有
```powershell
scoop search <软件名称>
```
Scoop can utilize aria2 to use multi-connection downloads.

Close the powershell window and start a new one to refresh the environment variables.

## Install packages

```powershell
scoop bucket add extras
#添加官方维护的extras库（含大量GUI程序）

scoop install curl wget
#wget是一个从网络上自动下载文件的自由工具，支持通过 HTTP、HTTPS、FTP 三个最常见的 TCP/IP协议 下载，并可以使用 HTTP 代理。wget 可以在用户退出系统的之后在继续后台执行，直到下载任务完成。

scoop install gzip unzip grep
#Gzip是若干种文件压缩程序的简称，此处gzip和unzip都是压缩软件；
#grep （Globally search a Regular Expression and Print）是一种强大的文本搜索工具，它能使用特定模式匹配（包括正则表达式）搜索文本，并默认输出匹配行。

scoop install jq jid pandoc
#Pandoc是一个将一种标记语言文件转换为另一种标记语言文件的转换器。可以实现多种文档格式的转换，例如doc，latex，pdf，html等

scoop install bat ripgrep tokei hyperfine
#bat：批处理文件，在MS-DOS中，.bat文件是可执行文件，由一系列命令构成，其中可以包含对其他程序的调用；
#ripgrep是rust 编写的搜索工具；
#Tokei是一个按语言统计代码行数等统计信息的工具；

scoop install sqlite sqlitestudio

# scoop install proxychains
#proxychains新的版本已经称为proxychains-ng由rofl0r托管在GitHub中维护，一般使用proxychains用于加速更新和下载国外的一些开源组件，比如yum和pip
```

* List installed packages

```powershell
scoop list
#查看已经下载的安装包
```  



