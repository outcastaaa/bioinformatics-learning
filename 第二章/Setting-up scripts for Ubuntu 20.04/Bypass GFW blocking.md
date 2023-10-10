
## 1. Bypass GFW blocking

* Query the IP address on [ipaddress](https://www.ipaddress.com/) for

    * `raw.githubusercontent.com`
    * `gist.githubusercontent.com`
    * `camo.githubusercontent.com`
    * `user-images.githubusercontent.com`

* Add them to `/etc/hosts` or `C:\Windows\System32\drivers\etc\hosts`

补充：  
IP地址：IP地址被用来给Internet上的电脑一个编号。IP协议中有一个非常重要的内容，那就是给因特网上的每台计算机和其它设备都规定了一个唯一的地址，叫做“IP地址”。由于有这种唯一的地址，才保证了用户在连网的计算机上操作时，能够高效而且方便地从千千万万台计算机中选出自己所需的对象来。
## Query the IP address  
1.  raw.githubusercontent.com  
查询结果：  
Domain——githubusercontent.com    
Domain Label——githubusercontent    
IP Addresses——4 × IPv4 and 4 × IPv6  
Web Server Location——2 locations in 🇺🇸 United States

![图片1](./pictures/%E5%9B%BE%E7%89%871.png)  

 

2. gist.githubusercontent.com  
查询结果：  
Domain——githubusercontent.com  
Domain Label——githubusercontent  
IP Addresses——4 × IPv4  
Web Server Location—— United States   

![图片](./pictures/%E5%9B%BE%E7%89%872.png)

3. camo.githubusercontent.com  
查询结果：  
Domain——githubusercontent.com  
Domain Label——githubusercontent  
IP Addresses——4 × IPv4  
Web Server Location	—— United States   
![图片](./pictures/%E5%9B%BE%E7%89%873.png) 

4. user-images.githubusercontent.com  
查询结果：  
Domain——githubusercontent.com  
Domain Label——githubusercontent  
IP Addresses——4 × IPv4 and 4 × IPv6  
Web Server Location	2 locations in —— United States     
![图片](./pictures/%E5%9B%BE%E7%89%874.png)

## Add them to `/etc/hosts` or `C:\Windows\System32\drivers\etc\hosts`  

1. Hosts作用：
* hosts文件的作用是将一些常用的网址域名与其对应的IP地址建立一个关联“数据库”，所以很多用户都会在hosts文件加ip地址。
* 在网络上访问网站，要首先通过DNS服务器把要访问的网络域名（XXXX.com）解析成XXX.XXX.XXX.XXX的IP地址后，计算机才能对这个网络域名作访问。要是对于每个域名请求我们都要等待域名服务器解析后返回IP信息，这样访问网络的效率就会降低，因为DNS做域名解析和返回IP都需要时间。  为了提高对经常访问的网络域名的解析效率，可以通过利用Hosts文件中建立域名和IP的映射关系来达到目的。根据Windows系统规定，在进行DNS请求以前，Windows系统会先检查自己的Hosts文件中是否有这个网络域名映射关系。如果有则，调用这个IP地址映射，如果没有，再向已知的DNS服务器提出域名解析。也就是说Hosts的请求级别比DNS高。
2. Hosts文件位置：  
在Windows 2000/XP系统中位于\%Systemroot%\System32\Drivers\Etc 文件夹中，其中，%Systemroot%指系统安装路径。  
例如，Windows XP 安装在C:\WINDOWS,那么Hosts文件就在C:\WINDOWS\system32\drivers\etc中。

3. 写入方法：  
① 打开hosts
```powershell
# Copyright (c) 1993-2009 Microsoft Corp.
#
# This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
#
# This file contains the mappings of IP addresses to host names. Each
# entry should be kept on an individual line. The IP address should
# be placed in the first column followed by the corresponding host name.
# The IP address and the host name should be separated by at least one
# space.这个文件包含IP地址到主机名的映射。每一个条目应该保持在单独一行。IP地址应该是放在第一列中，后面跟着相应的主机名。IP地址和主机名应该至少用一个分隔空间。
#
# Additionally, comments (such as these) may be inserted on individual
# lines or following the machine name denoted by a '#' symbol.
#
# For example:
#
#      102.54.94.97     rhino.acme.com          # source server
#       38.25.63.10     x.acme.com              # x client host

# localhost name resolution is handled within DNS itself.
#	127.0.0.1       localhost
#	::1             localhost
```
② 写入内容  
```powershell
185.199.108.133   raw.githubusercontent.com
185.199.109.133   gist.githubusercontent.com
185.199.110.133   camo.githubusercontent.com
185.199.111.133   user-images.githubusercontent.com
```
③ 问题及解决办法：  
* 如果没有书写权限，则copy桌面hosts，并将其粘贴到C:\Windows\System32\drivers\etc目录下，选择【继续】，并进行替换
* 怎么看IP地址有没有在hosts中书写成功？  
在cmd页面输入 ping+域名  
![tupian](./pictures/%E5%9B%BE%E7%89%875.png)
* 如果好几个IP地址同时对应一个域名怎么办？

如果我们在hosts文件中配置了 同一个域名，不同ip地址 的如下两个映射：
解析顺序是，从第一个IP开始获取，如果解析第一个IP失败，才会解析下一个IP。

理论上，一个域名是可以对应多个IP的，而在用户访问过程中，指向某一个具体IP，并不会同时访问多个IP。但不同用户在不同地点访问同一个域名，可能会访问到不同的IP地址，但表象仍旧是这个域名。

服务器将根据各地的访问IP，到达域名IP中路由跳数最小的那个IP地址作为访问的域名IP地址。这样能保证一个域名被访问时，能最大限度提供高速稳定的访问体验。同时，由于有多个备选IP，当其中一个出现问题时，可以实现故障自动切换，提高业务可用性，并提高资源利用率。

* 如果好几个域名同时对应一个IP地址怎么办？

反过来，一个IP地址可以解析绑定多个域名，没有限制。但是建议不要将同质化严重的网站绑定在同一个IP下，这样容易被搜索引擎判定作弊。

### 补充

* 域（Domain）是Windows网络中独立运行的单位，域之间相互访问则需要建立信任关系（即Trust Relation）。信任关系是连接在域与域之间的桥梁。当一个域与其他域建立了信任关系后，2个域之间不但可以按需要相互进行管理，还可以跨网分配文件和打印机等设备资源，使不同的域之间实现网络资源的共享与管理。 

* 域 domain：
.com　Commercial（商业组织）  
.cn   China (中国域名)  
.edu　Educational（教育机构）  
.gov　Government（政府部门）  
.mil　Military（军事部门）  
.org　Non-Profit organization（非盈利组织）  
.net　Network（主要网络支持中心）  
* IPv6 

IPv4地址正在耗尽，而IPv6通过更长的序列提供了更多的IP地址。IPv4向IPv6的迁移正在发生。

IPv4和IPv6是先后出现的两个IP协议版本。IPv4的地址就是一个32位的0/1序列，比如11000000 00000000 0000000 00000011。为了方便人类记录和阅读，我们通常将32位0/1分成4段8位序列，并用10进制来表示每一段(这样，一段的范围就是0到255)，段与段之间以.分隔。比如上面的地址可以表示成为192.0.0.3。IPv6地址是128位0/1序列，它也按照8位分割，以16进制来记录每一段(使用16进制而不是10进制，这能让写出来的IPv6地址短一些)，段与段之间以:分隔。

* 二进制如何软化为16进制  
![tu](./pictures/%E5%9B%BE%E7%89%876.png)



