
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
**通过  “电脑win - 系统 - 关于”  查看**  
20H1表示Win10 2020年上半年系统，本电脑系统为21H2，即21年下半年系统  
Build 19041.84 or later√ 操作系统内部版本 


English or Chinese Simplified√

64-bit√

Windows 10√

## Install, active and update Windows
* Enable Virtualization in BIOS or VM √  
1. 进入BIOS方法：联想yoga——关机按 insert键可以进入BIOS界面；  
**大部分电脑会在开机界面上显示进入BIOS界面的方法**  
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
主机名:           DESKTOP-HI65AUV
OS 名称:          Microsoft Windows 10 专业教育版
* 操作系统（英语：Operating System，简称OS）是管理和控制计算机硬件与软件资源的计算机程序，是直接运行在“裸机”上的最基本的系统软件，任何其他软件都必须在操作系统的支持下才能运行。
OS 版本:          10.0.19044 暂缺 Build 19044
OS 制造商:        Microsoft Corporation
OS 配置:          独立工作站
OS 构建类型:      Multiprocessor Free
注册的所有人:     XRZ
注册的组织:       暂缺
产品 ID:          00378-40000-00001-AA107
初始安装日期:     2022/7/15, 19:31:13
系统启动时间:     2022/7/25, 9:53:37
系统制造商:       Gigabyte Technology Co., Ltd.
系统型号:         To be filled by O.E.M.
系统类型:         x64-based PC
处理器:           安装了 1 个处理器。
                  [01]: Intel64 Family 6 Model 94 Stepping 3 GenuineIntel ~4001 Mhz
BIOS 版本:        American Megatrends Inc. F7, 2016/3/1
Windows 目录:     C:\WINDOWS
系统目录:         C:\WINDOWS\system32
启动设备:         \Device\HarddiskVolume2
系统区域设置:     zh-cn;中文(中国)
输入法区域设置:   zh-cn;中文(中国)
时区:             (UTC+08:00) 北京，重庆，香港特别行政区，乌鲁木齐
物理内存总量:     32,584 MB
可用的物理内存:   874 MB
虚拟内存: 最大值: 39,530 MB
虚拟内存: 可用:   5,416 MB
虚拟内存: 使用中: 34,114 MB
页面文件位置:     C:\pagefile.sys
域:               WORKGROUP
登录服务器:       \\DESKTOP-HI65AUV
修补程序:         安装了 13 个修补程序。
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
网卡:             安装了 3 个 NIC。
                  [01]: Intel(R) Ethernet Connection (2) I219-V
                      连接名:      以太网
                      启用 DHCP:   是
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
Hyper-V 要求:     已检测到虚拟机监控程序。将不显示 Hyper-V 所需的功能。
```


## Enable some optional features of Windows 10

* Mount Windows ISO to D: (or others)

* Open PowerShell as an Administrator

```powershell
# .Net 2.5 and 3
DISM /Online /Enable-Feature /FeatureName:NetFx3 /All /LimitAccess /Source:D:\sources\sxs

# Online
# DISM /Online /Enable-Feature /FeatureName:NetFx3 /All

# SMB 1
Enable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol -All

# Telnet
DISM /Online /Enable-Feature /FeatureName:TelnetClient

```

## WSL 2

* Follow instructions of [this page](https://docs.microsoft.com/en-us/windows/wsl/wsl2-install)

* Open PowerShell as an Administrator

```powershell
# HyperV
Enable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform -NoRestart
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All -NoRestart

# WSL
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux -NoRestart

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
}
Add-AppxPackage .\Ubuntu.appx

```

Launch the distro from the Start menu, wait for a minute or two for the installation to complete,
and set up a new Linux user account.

The following command verifies the status of WSL:

```powershell
wsl -l -v

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
if (!(Test-Path Microsoft.WindowsTerminal.msixbundle -PathType Leaf))
{
    Invoke-WebRequest `
        'https://github.com/microsoft/winget-cli/releases/download/v1.2.10271/Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.msixbundle' `
        -OutFile 'Microsoft.DesktopAppInstaller.msixbundle'
}
Add-AppxPackage -path .\Microsoft.DesktopAppInstaller.msixbundle

winget --version

winget install -e --id Microsoft.WindowsTerminal

winget install -e --id Microsoft.PowerShell

winget install -e --id Git.Git

```

Open `Windows Terminal`

* Set `Settings` -> `Startup` -> `Default profile` to `PowerShell`, not `Windows PowerShell`.

* Set `Default terminal application` to `Windows Terminal`.

* Hide unneeded `Profiles`.

## Optional: Adjusting Windows

Works with Windows 10 or 11.

```powershell
mkdir -p ~/Scripts
cd ~/Scripts
git clone --recursive https://github.com/wang-q/windows

cd ~/Scripts/windows/setup
powershell.exe -NoProfile -ExecutionPolicy Bypass `
    -File "Win10-Initial-Setup-Script/Win10.ps1" `
    -include "Win10-Initial-Setup-Script/Win10.psm1" `
    -preset "Default.preset"

```

Log in to the Microsoft Store and get updates from there.

## Optional: winget-pkgs

```powershell
# programming
# winget install -s winget -e --id AdoptOpenJDK.OpenJDK
winget install -s winget -e --id Oracle.JavaRuntimeEnvironment
winget install -s winget -e --id Oracle.JDK.18
# winget install -s winget -e --id Microsoft.dotnet
winget install -s winget -e --id StrawberryPerl.StrawberryPerl
# winget install -e --id Python.Python
winget install -s winget -e --id RProject.R
# winget install -s winget -e --id RProject.Rtools
# winget install -s winget-e --id OpenJS.NodeJS.LTS
winget install -s winget -e --id RStudio.RStudio.OpenSource
winget install -s winget -e --id Kitware.CMake

# development
winget install -s winget -e --id GitHub.GitHubDesktop
winget install -s winget -e --id WinSCP.WinSCP
winget install -s winget -e --id Microsoft.VisualStudioCode
winget install -s winget -e --id ScooterSoftware.BeyondCompare4
winget install -s winget -e --id JetBrains.Toolbox
winget install -s winget -e --id Clement.bottom
# winget install -e --id WinFsp.WinFsp
# winget install -e --id SSHFS-Win.SSHFS-Win
# \\sshfs\REMUSER@HOST[\PATH]

# winget install -e --id Docker.DockerDesktop

# winget install -e --id VMware.WorkstationPlayer

winget install -s winget -e --id Canonical.Multipass

# utils
winget install -s winget -e --id voidtools.Everything
winget install -s winget -e --id Bandisoft.Bandizip
winget install -s msstore Rufus # need v3.18 or higher
winget install -s winget -e --id QL-Win.QuickLook
winget install -s winget -e --id AntibodySoftware.WizTree
winget install -s winget -e --id HandBrake.HandBrake
# winget install -s winget -e --id Microsoft.PowerToys
winget install -s winget -e --id qBittorrent.qBittorrent
winget install -s winget -e --id IrfanSkiljan.IrfanView

# apps
winget install -s winget -e --id Mozilla.Firefox
winget install -s winget -e --id Tencent.WeChat
winget install -s winget -e --id Tencent.TencentMeeting
winget install -s winget -e --id Tencent.QQ
winget install -s winget -e --id NetEase.CloudMusic
winget install -s winget -e --id Youdao.YoudaoDict
winget install -s winget -e --id Baidu.BaiduNetdisk
winget install -s winget -e --id stax76.mpvdotnet
winget install -s winget -e --id Zotero.Zotero

# winget install -e --id Adobe.AdobeAcrobatReaderDC
# winget install -e --id Alibaba.DingTalk

```

## Optional: Windows 7 games

<https://winaero.com/download.php?view.1836>

## Optional: Packages Managements

* [`scoop.md`](setup/scoop.md)
* [`msys2.md`](setup/msys2.md)

## Optional: Rust and C/C++

* [`rust.md`](setup/rust.md)

## Optional: sysinternals

* Add `$HOME/bin` to Path
* Open PowerShell as an Administrator

```powershell
mkdir $HOME/bin

# Add to Path
[Environment]::SetEnvironmentVariable(
    "Path",
    [Environment]::GetEnvironmentVariable("Path", [EnvironmentVariableTarget]::Machine) + ";$HOME\bin",
    [EnvironmentVariableTarget]::Machine)

```

* Download and extract

```powershell
scoop install unzip

$array = "DU", "ProcessExplorer", "ProcessMonitor", "RAMMap"

foreach ($app in $array) {
    aria2c.exe -c "https://download.sysinternals.com/files/$app.zip"
}

foreach ($app in $array) {
    unzip "$app.zip" -d $HOME/bin -x Eula.txt
}

rm $HOME/bin/*.chm
rm $HOME/bin/*64.exe
rm $HOME/bin/*64a.exe

```

## Optional: QuickLook Plugins

<https://github.com/QL-Win/QuickLook/wiki/Available-Plugins>

```powershell
# epub
$url = (
curl.exe -fsSL https://api.github.com/repos/QL-Win/QuickLook.Plugin.EpubViewer/releases/latest |
    jq -r '.assets[0].browser_download_url'
)
curl.exe -LO $url

# office
$url = (
curl.exe -fsSL https://api.github.com/repos/QL-Win/QuickLook.Plugin.OfficeViewer/releases/latest |
    jq -r '.assets[0].browser_download_url'
)
curl.exe -LO $url

# folder
$url = (
curl.exe -fsSL https://api.github.com/repos/adyanth/QuickLook.Plugin.FolderViewer/releases/latest |
    jq -r '.assets[0].browser_download_url'
)
curl.exe -LO $url

```

Select the `qlplugin` file and press `Spacebar` to install the plugin.

## Directory Organization

* [`packer/`](packer/): Scripts for building a Windows 10 box for Parallels.

* [`setup/`](setup/): Scripts for setting-up Windows.

