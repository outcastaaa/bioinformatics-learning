# faops使用说明书
- [介绍](#介绍)
- [下载](#下载)
- [语句用法](#语句用法)
    - [总结](#总结)
    - [Bash用法](#Bash用法)
    - [Cat用法](#Cat用法)
    - [文件/etc/apt/sources.list](#文件/etc/apt/sources.list)
    - [shell语句中各种符号](#shell语句中各种符号)
    - [条件判断语句if-then-fi](#条件判断语句if-then-fi)
    - [sed-i命令](#sed-i命令)
- [补充](#补充)
# 介绍
1. fasta files  
在生物信息学中，FASTA格式是一种用于`记录核酸序列`或`肽序列`的文本格式，其中的核酸或氨基酸均以单个字母编码呈现。该格式同时还允许在序列之前定义名称和编写注释。这一格式最初由FASTA软件包定义，但现今已是生物信息学领域的一项标准。  
`FASTA格式`是BLAST组织数据的基本格式，无论是数据库还是查询序列，还是各种生物的基因组序列数据也都是用FASTA格式进行存储的，大多数情况都使用FASTA格式。另外，FASTA简明的格式降低了序列操纵和分析的难度，令序列可被文本处理工具和诸如Python、Ruby和Perl等脚本M语言处理。（来自维基百科）  
![tu](./pictures/%E5%9B%BE%E7%89%871.png)


2. faops 是用于操作 fasta 格式序列的简便工具  
该工具可以看作是 UCSC Jim Kent 实用程序中的 `faCount、faSize、faFrag、faRc、faSomeRecords、faFilter 和 faSplit `的组合。



3. 与 Kent 的 fa* 实用程序相比，faops 是：  
* much smaller (kilo vs mega bytes)  小得多的
* easy to compile (only one external dependency)    易于编译（只有一个外部依赖项）
* well tested   经过良好测试
* contains only one executable file  仅包含一个可执行文件
* can operate gzipped (bgzipped) files  可以操作gzip压缩（bgzipped）文件
* and can be run under all major OSes (including Windows).  可以在所有主要操作系统（包括Windows）下运行  


 faops 也受到 seqtk 和 ufasta 的启发/影响(seqtk基于C语言编写的软件，运行速度极快，极大的提高工作效率。seqtk日常序列的处理包括：fq转换为fa，格式化序列，截取序列，随机抽取序列等)

# 下载
* Installing with Homebrew or Linuxbrew
```
git clone https://github.com/wang-q/faops
cd faops
make
brew install wang-q/tap/faops
```
# 语句用法
## 总结
```
$ ./faops

Usage:     faops <command> [options] <arguments>
Version:   0.8.21

Commands:
    help           print this message
    count          count base statistics in FA file(s)
    size           count total bases in FA file(s)
    masked         masked (or gaps) regions in FA file(s)
    frag           extract sub-sequences from a FA file
    rc             reverse complement a FA file
    one            extract one fa record
    some           extract some fa records
    order          extract some fa records by the given order
    replace        replace headers from a FA file
    filter         filter fa records
    split-name     splitting by sequence names
    split-about    splitting to chunks about specified size
    n50            compute N50 and other statistics
    dazz           rename records for dazz_db
    interleave     interleave two PE files
    region         extract regions from a FA file

Options:
    There're no global options.
    Type "faops command-name" for detailed options of each command.
    Options *MUST* be placed just after command.
```



















 # 补充
 1. 核苷酸和氨基酸缩写表  

 ![tu](./pictures/%E5%9B%BE%E7%89%872.png)
