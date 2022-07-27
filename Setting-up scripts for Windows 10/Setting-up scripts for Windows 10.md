
# Setting-up scripts for Windows 10
- [Setting-up scripts for Windows 10](#setting-up-scripts-for-windows-10)
    - [Get ISO](#get-iso)
    - [Install, active and update Windows](#install-active-and-update-windows)
    - [Enable some optional features of Windows 10](#enable-some-optional-features-of-windows-10)
    - [WSL 2](#wsl-2)
    - [Ubuntu 20.04](#ubuntu-2004)
    - [Install `winget` and `Windows Terminal`](#install-winget-and-windows-terminal)
    - [Optional: Adjusting Windows](#optional-adjusting-windows)
    - [Optional: winget-pkgs](#optional-winget-pkgs)
    - [Optional: Windows 7 games](#optional-windows-7-games)
    - [Optional: Packages Managements](#optional-packages-managements)
    - [Optional: Rust and C/C++](#optional-rust-and-cc)
    - [Directory Organization](#directory-organization)

## Get ISO

Some features of Windows 10 20H1/2004 are needed here.  
`通过  “电脑win - 系统 - 关于”  查看`  
`20H1表示Win10 2020年上半年系统，本电脑系统为21H2，即21年下半年系统`  
Build 19041.84 or later√ 操作系统内部版本 


English or Chinese Simplified√

64-bit√

Windows 10√

## Install, active and update Windows
* Enable Virtualization in BIOS or VM √  
1. 进入BIOS方法：联想yoga——关机按 insert键可以进入BIOS界面；  
`大部分电脑会在开机界面上显示进入BIOS界面的方法`  
2. BIOS（basic input output system 即基本输入输出系统）设置程序是被固化到电脑主板上的ROM芯片（只读存储芯片）中的一组程序，其主要功能是为电脑提供最底层地、最直接地硬件设置和控制。

* Active Windows 10 via KMS, <http://kms.nju.edu.cn/>  
南京大学内部资料，可下载各类硬件、软件等

* Update Windows and then check system info  
由win+R键进入cmd界面查询以下信息

```powershell
# simple
winver
```
→结果：
![1-winver](./pictures/1-winver.png)
```powershell
# details
systeminfo
```
→结果：
```powershell
1.主机名:           DESKTOP-HI65AUV
2.OS 名称:          Microsoft Windows 10 专业教育版
* 操作系统（英语：Operating System，简称OS）是管理和控制计算机硬件与软件资源的计算机程序，是直接运行在“裸机”上的最基本的系统软件，任何其他软件都必须在操作系统的支持下才能运行。
3.OS 版本:          10.0.19044 暂缺 Build 19044
* 暂缺：该版本不带补丁包Service Pack，也可能不是官方正版。一般在一个软件的开发过程中，一开始有很多因素是没有考虑到的，但是随着时间的推移，软件所存在的问题会慢慢的被发现。这时候，为了对软件本身存在的存在的问题进行修复，软件开发者会发布相应的补丁,也就是修复本身存在的洞（缺陷）
4.OS 制造商:        Microsoft Corporation
5.OS 配置:          独立工作站
* 独立工作站：工作站是一种高端的通用微型计算机。它是为了单用户使用并提供比个人计算机更强大的性能，尤其是在图形处理能力，任务并行方面的能力。通常配有高分辨率的大屏、多屏显示器及容量很大的内存储器和外部存储器，并且具有极强的信息和高性能的图形、图像处理功能的计算机。就是，PC会连接更厉害的大电脑，来实现更高级的功能。
6.OS 构建类型:      Multiprocessor Free
7.注册的所有人:     XRZ
8.注册的组织:       暂缺
9.产品 ID:          00378-40000-00001-AA107
10.初始安装日期:     2022/7/15, 19:31:13
11.系统启动时间:     2022/7/25, 9:53:37
12.系统制造商:       Gigabyte Technology Co., Ltd.
13.系统型号:         To be filled by O.E.M.
14.系统类型:         x64-based PC
15.处理器:           安装了 1 个处理器。
                  [01]: Intel64 Family 6 Model 94 Stepping 3 GenuineIntel ~4001 Mhz
16.BIOS 版本:        American Megatrends Inc. F7, 2016/3/1
17.Windows 目录:     C:\WINDOWS
18.系统目录:         C:\WINDOWS\system32
19.启动设备:         \Device\HarddiskVolume2
20.系统区域设置:     zh-cn;中文(中国)
21.输入法区域设置:   zh-cn;中文(中国)
22.时区:             (UTC+08:00) 北京，重庆，香港特别行政区，乌鲁木齐
23.物理内存总量:     32,584 MB
* 物理内存是指由于安装内存条而获得的临时储存空间。主要作用是在计算机运行时为操作系统和各种程序提供临时储存
24.可用的物理内存:   874 MB
25.虚拟内存: 最大值: 39,530 MB
* 虚拟内存是计算机系统内存管理的一种技术，即匀出一部分硬盘空间来充当内存使用。当内存耗尽时，电脑就会自动调用硬盘来充当内存，以缓解内存的紧张。
26.虚拟内存: 可用:   5,416 MB
27.虚拟内存: 使用中: 34,114 MB
28.页面文件位置:     C:\pagefile.sys
29.域:               WORKGROUP
* 工作组：工作组（Work Group）就是将不同的电脑按功能分别列入不同的组中，以方便管理。你可以随便加入同一网络上的任何工作组，也可以随时离开一个工作组。“工作组”就像一个自由加入和退出的俱乐部一样。它本身的作用仅仅是提供一个“房间”，以方便网上计算机共享资源的浏览。
30.登录服务器:       \\DESKTOP-HI65AUV
31.修补程序:         安装了 13 个修补程序。
                  [01]: KB5013887
                  [02]: KB4562830
                  [03]: KB5003791
                  [04]: KB5015807
                  [05]: KB5006753
                  [06]: KB5007273
                  [07]: KB5009636
                  [08]: KB5011352
                  [09]: KB5011651
                  [10]: KB5012677
                  [11]: KB5014032
                  [12]: KB5014671
                  [13]: KB5005699
32.网卡:             安装了 3 个 NIC。
* 台式机一般都采用内置网卡来连接网络。网卡也叫“网络适配器”，英文全称为“Network Interface Card”，简称“NIC”，它是连接计算机与网络的硬件设备。
  网卡的功能主要有两个:一是将电脑的数据封装为帧，并通过网线(对无线网络来说就是电磁波)将数据发送到网络上去;二是接收网络上其它设备传过来的帧，并将帧重新组合成数据，发送到所在的电脑中。

                  [01]: Intel(R) Ethernet Connection (2) I219-V
                      连接名:      以太网
                      启用 DHCP:   是

* DHCP，Dynamic Host Configuration Protocol，动态主机配置协议，是一个应用于局域网的网络协议，该协议允许服务器向客户端动态分配IP地址和配置信息。

                      DHCP 服务器: 192.168.1.1
                      IP 地址
                        [01]: 192.168.1.8
                        [02]: fe80::d8b5:58ed:158b:62be
                  [02]: Hyper-V Virtual Ethernet Adapter
                      连接名:      vEthernet (Default Switch)
                      启用 DHCP:   否
                      IP 地址
                        [01]: 172.23.176.1
                        [02]: fe80::144e:dd81:5df7:4eda
                  [03]: Hyper-V Virtual Ethernet Adapter
                      连接名:      vEthernet (WSL)
                      启用 DHCP:   否
                      IP 地址
                        [01]: 172.24.32.1
                        [02]: fe80::d441:d262:ee88:9a51
33.Hyper-V 要求:     已检测到虚拟机监控程序。将不显示 Hyper-V 所需的功能。
```


## Enable some optional features of Windows 10

* Mount Windows ISO to D: (or others)
将ISO 挂在了C盘

* Open PowerShell as an Administrator
`Win+X按钮，选择管理员`

```powershell
打开各项功能

# .Net 2.5 and 3
DISM /Online /Enable-Feature /FeatureName:NetFx3 /All /LimitAccess /Source:D:\sources\sxs

# Online
# DISM /Online /Enable-Feature /FeatureName:NetFx3 /All

# SMB 1
Enable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol -All

# Telnet
DISM /Online /Enable-Feature /FeatureName:TelnetClient
```
启动方法(以SMB1为例)：  
1.代码Enable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol -All  
2.用 win+R键，输入control.exe，打开控制面板，在单击/点击“ 程序和功能”图标。单击/点击左侧的“ 打开或关闭Windows功能”链接，勾选需要功能。

## WSL 2

Windows Subsystem for Linux (WSL)

* Follow instructions of [this page](https://docs.microsoft.com/en-us/windows/wsl/wsl2-install)  
mainly:
```powershell
wsl --install
```
* Open PowerShell as an Administrator

```powershell
设置方法同上

# HyperV
Enable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform -NoRestart
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All -NoRestart

# WSL
Enable-WindowsOptionalFeature -Online 
-FeatureName Microsoft-Windows-Subsystem-Linux -NoRestart
```

Update the [WSL 2](https://docs.microsoft.com/en-us/windows/wsl/wsl2-kernel) Linux kernel if
necessarily. 

Restart then set WSL 2 as default.

```powershell
wsl --set-default-version 2
```

## Ubuntu 20.04

Search `bash` in Microsoft Store or use the following command lines.

```powershell
if (!(Test-Path Ubuntu.appx -PathType Leaf))
{
    Invoke-WebRequest -Uri https://aka.ms/wslubuntu2004 -OutFile Ubuntu.appx -UseBasicParsing  
    # 通过网页下载
}
Add-AppxPackage .\Ubuntu.appx

```

Launch the distro from the Start menu, wait for a minute or two for the installation to complete,
and set up a new Linux user account.

The following command verifies the status of WSL:

```powershell
wsl -l -v

```
→结果：
```
 C:\WINDOWS\system32> wsl -l -v
  NAME      STATE           VERSION
* Ubuntu    Running         2
```
### Symlinks

* WSL: reduce the space occupied by virtual disks

```shell
cd

rm -fr Script data

ln -s /mnt/c/Users/wangq/Scripts/ ~/Scripts

ln -s /mnt/d/data/ ~/data

```

* Windows: second disk
    * Open `cmd.exe` as an Administrator

```cmd
cd c:\Users\wangq\

mklink /D c:\Users\wangq\data d:\data

```

## Install `winget` and `Windows Terminal`

```powershell
if (!(Test-Path Microsoft.WindowsTerminal.msixbundle -PathType Leaf))  #此命令检查变量中存储的路径是否指向文件。
{
    Invoke-WebRequest `
    #获取 http web请求访问内容虽然没有curl那么主流，但一样可以成为http访问的一个选择。
        'https://github.com/microsoft/winget-cli/releases/download/v1.2.10271/Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.msixbundle' `
        -OutFile 'Microsoft.DesktopAppInstaller.msixbundle'
}
Add-AppxPackage -path .\Microsoft.DesktopAppInstaller.msixbundle

winget --version

winget install -e --id Microsoft.WindowsTerminal

winget install -e --id Microsoft.PowerShell

winget install -e --id Git.Git
#以上利用winget下载，windows推出的命令行安装工具——winget,全称windows package manager client
```

Open `Windows Terminal`

* Set `Settings` -> `Startup` -> `Default profile` to `PowerShell`, not `Windows PowerShell`. √

* Set `Default terminal application` to `Windows Terminal`. 暂时找不到

* Hide unneeded `Profiles`.  暂时找不到

## Optional: Adjusting Windows

Works with Windows 10 or 11.

```powershell
# 创建~/Scripts文件目录，并且挂载到该目录下
mkdir -p ~/Scripts
cd ~/Scripts
# 将网页中内容clone下来，用于循环克隆git子项目 
git clone --recursive https://github.com/wang-q/windows

cd ~/Scripts/windows/setup
powershell.exe -NoProfile -ExecutionPolicy Bypass `
    -File "Win10-Initial-Setup-Script/Win10.ps1" `
    -include "Win10-Initial-Setup-Script/Win10.psm1" `
    -preset "Default.preset"

```

Log in to the Microsoft Store and get updates from there.

## Optional: winget-pkgs



## Optional: Windows 7 games

<https://winaero.com/download.php?view.1836>

## Optional: Packages Managements

* [`scoop.md`](setup/scoop.md)
* [`msys2.md`](setup/msys2.md)

## Optional: Rust and C/C++

* [`rust.md`](setup/rust.md)

## Optional: sysinternals



## Optional: QuickLook Plugins


## Directory Organization

