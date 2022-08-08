# faops使用说明书
- [介绍](#介绍)
    - [Echo](#Echo)
    - [Bash用法](#Bash用法)
    - [Cat用法](#Cat用法)
    - [文件/etc/apt/sources.list](#文件/etc/apt/sources.list)
    - [shell语句中各种符号](#shell语句中各种符号)
    - [条件判断语句if-then-fi](#条件判断语句if-then-fi)
    - [sed-i命令](#sed-i命令)
# 介绍
1. faops operates fasta files  
在生物信息学中，FASTA格式是一种用于`记录核酸序列`或`肽序列`的文本格式，其中的核酸或氨基酸均以单个字母编码呈现。该格式同时还允许在序列之前定义名称和编写注释。这一格式最初由FASTA软件包定义，但现今已是生物信息学领域的一项标准。  
`FASTA格式`是BLAST组织数据的基本格式，无论是数据库还是查询序列，还是各种生物的基因组序列数据也都是用FASTA格式进行存储的，大多数情况都使用FASTA格式。另外，FASTA简明的格式降低了序列操纵和分析的难度，令序列可被文本处理工具和诸如Python、Ruby和Perl等脚本M语言处理。（来自维基百科）


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