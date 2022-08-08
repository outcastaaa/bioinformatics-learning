# faops使用说明书
- [介绍](#介绍)
- [下载](#下载)
- [Commands](#Commands)
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
# Commands
## summary
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
## help  
```
xuruizhi@DESKTOP-HI65AUV:~$ faops help

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
## count and size 
`count    -base statistics in FA file(s)    -计算base数据`  
`size     -count total bases in FA file(s)        -计算总base数`
* 先用cat把序列信息写入文件test.fa
```
xuruizhi@DESKTOP-HI65AUV:~$ cat >test.fa <<EOF
> >1
> ACCCCAGGACTCCCGAGGGTGGCCTGAGGGGGTTCCGACTCCCTGTTCGTCGGGCATGCC
> CTGTGGCCTGGCACCCAGCCCTGAGCAGGGTGGCCTCTGCTGCTTGCTACCTCCACAGCC
> CTTTTCAACTCTTGCCCTTTTGTCTGCAGCCCGCCACCTCTTCTGAAGGGCCTCTGCGTG
> GCTTTTACCCCCTTTTTGTTTACCAAGGCTTTTTTTTTTTTTAATTCCTTTAAGTCACTT
> GGCAGAATGGGCTGGGTGCAGAGAGTGTTCTTGCATGACGGAGCTGAGAGGGAGCCACTC
> TCCAAAGGCTCTGTATTCCTGCCACTCAGGCTGTGTCTCCGGGGGAAAGGAGGGGGTCCA
> TTACCCCGCTGAGGAGTTCAGATGTTCATGGTGGGGCTGGAGCTGCTGACACATGCAGCT
> GAGCTGTCAGGCCCCCACATGCCCTTCTTTGAGAAGATGCTTGGGGGGCAAGGGCCTCAG
> TCCTGCTTGGCTGCTCCTGTTTGTAGAGGTTTCCTTGCTGGGGAGGTAGGGAGGATGAGT
> TATTCCCGCTTCACTCTCTGGTCTTGTCTGTCCAAAAAGGAATCCATCTTCTCGCCCACT
> GCCTGCTTCCTCACCTGCCA
> >2
> GGTCATTCTCGTATAGGTAGATGACCTGCACGTTGACCTTGGTCTTGAGGTCCTGGGGGA
> TGCCGGCGTTGTTGATCTGGTTGTTCTGCAGGTAGAGGGTGGTGGCATCATCAGGGATAT
> CTGCGGGGATGGATGTGAGTCCCCGGTCGTTGCAGTAGATGAAGCCGTTGTCGCAGCGGC
> ACACCGAGGGGCAGGTGGTGCTGTCGATGACCTCCGTCAGGAAGGCGATGAGCCCGTAGC
> AGAGGAACAGCCAGTCCCGCAGGTCCATGGTGGCCGTGGTCATCACAACGGTGGCCGTGA
> CAGTGGCAGTGGGCGTGGTGGTGGCAGTGGCGGTGGGGTGTGCCACCACCATGGTGGATG
> GCTGGGGGCGTCCGGCCCCACCTGGCCTGGAGCCTGAATACCTCCTTCGTCGCTGCCGCC
> CAAGACCCTCTGGGTTTGACTGGGGTCACTGGTCCGGAGGCCCCTGCCAAAGTCACGAAG
> CCGCGACTCAGGACACCGTGAGCTCAGATGGCGCTCACATTCCCTGCCCCGTGGTCCTAC
> CAGGCCTTGCACCACTAGGGACGCAGACCTCAGCCCTGGGGTGACTGGGCAGCACTGGGG
> CTCCCCAGGGCACAGAGAGGGGATCCGGTGCCTCGGTATCCTGGTCTCCTGGCGTTCAGA
> TGAGGCTGTGGGCCACAATCAGCAGCCTCCTTGGCACCCTAGGCTGCAGGCGCAGCTGGC
> TTATCCCTGCAGAGTGAGGGTCCTGTGTGCAAGGGGTACAGGGACACCCCTCTGG
> >3
> TCCATCTCGCTGGTAATGTCCTTGATGGCCATGCCCCGGACCTTCTCAGGGCCCTGGCAC
> ATGAGGCCCCGCACGTTGACCACGGCCGCCCGTGCCTTCACCCAGTCCCGCAGCCACATG
> AGGTTGCAGCCACAAAACCAAGGGTTGTTCCTGAGCAGCAGCTGGGCCAGGTTCCCCAGG
> TCGTCGAACAGGCCGCGGGGCAGCGTGGTCAGGTTGTTGTTGGACAGGTCCAGCCGCTCC
> AGCTCACGCATCTTGGCCAGCGTGTTGTAGGGGATGTGGCTGATGGCATTGTCCTGCAGG
> TAGAGCTTCTGCAGGTGGGCGCTGGGCAGGTTGAGGGGTGGCGCGGCCAGCGAATTGCGC
> ACCAGCGAGAGCTCTGTGAGGTTCTGTAGGCGGCTGAAGGTGTCGTCGGCGATGCGCTGG
> TTGGCCAGCAGGTTACCGTCCAGCACCAGGCGCCGCAGGCTGTTGAGGCCCTTGAAGGCA
> TGCAGCGGGATGGTGGAGATGCGGTTGTCATCCAGCCGCAGCTCCTCCAGCGTGTGCGGC
> AGCCCCGAGGGGATGCTGCTCAGGTGGTTCCGGCTCAGGAAGAGCAGCTTGAGCTGTTTG
> CTGTCGGCGAAGGCGTCCTCCTCAATGCTGACGGTGGACACGGAGTTGTCATCCAGGTGC
> AGCTTCTCCAGCAGCGGGATGCGGGCCAGCGAGTCCCTGGCAATGGTGCGCACATTGTTG
> TCCTGCAGGTGCAGCTCCCGGAGGGAGCGGGGCAGGTTGATGGGGAACTCATCCA
> EOF
```
* count -base statistics in FA file(s)  -计算base数据
```
xuruizhi@DESKTOP-HI65AUV:~$ faops count test.fa
#seq    len     A       C       G       T       N
1       620     95      179     174     172     0
2       775     127     218     275     155     0
3       775     125     220     276     154     0
total   2170    347     617     725     481     0
```
* size           -count total bases in FA file(s)     -计算总base数
```
xuruizhi@DESKTOP-HI65AUV:~$ faops size test.fa
1       620
2       775
3       775
xuruizhi@DESKTOP-HI65AUV:~$ faops size test.fa | head -n 2 #只显示前两行
1       620
2       775
```
## masked         
`masked         -masked (or gaps) regions in FA file(s)  -筛选出N（AGCT中any）`  
命令`faops masked `可以快速找到一个或多个序列文件中冗余碱基位置, 其中，冗杂基因包括重复序列, N 碱基, 下划线等. 使用参数-g后, 仅定位 N 碱基和下划线位置.
此外，值得注意的是, 重复碱基在序列文件中一般以小写字母区分.
*  具体用法  
```
xuruizhi@DESKTOP-HI65AUV:~$ faops masked

faops masked - Masked (or gaps) regions in fasta files
usage:
    faops masked [options] <in.fa> [more_files.fa]

options:
    -g         only record regions of N/n  
    #筛选出不合适的N，并且标注出每一序列的具体位置

in.fa  == stdin  means reading from stdin
```
* 举例
```
xuruizhi@DESKTOP-HI65AUV:~$ cat >1.fa <<EOF
> >1
> ACCCCAGGACTCCCGAGGGTGGCCTGAGGGGGTTCCGACTCCCTGTTCGTCGGGCATGCC
> CTGTGGCCTGGCACCCAGCCCTGAGCAGGGTGGCCTCTGCTGCTTGCTACCTCCACAGCC
> CTTTTCAACTCTTGCCCTTTTGTCTGCAGCCCGCCACCTCTTCTGAAGGGCCTCTGCGTGNNN
> >2
> GGTCATTCTCGTATAGGTAGATGACCTGCACGTTGACCTTGGTCTTGAGGTCCTGGGGGA
> TGCCGGCGTTGTTGATCTGGTTGTTCTGCAGGTAGAGGGTGGTGGCATCATCAGGGATAT
> CTGCGGGGATGGATGTGAGTCCCCGGTCGTTGCAGTAGATGAAGCCGTTGTCGCAGCGGC
> nnn
> EOF

xuruizhi@DESKTOP-HI65AUV:~$ faops masked -g 1.fa
1:181-183
2:181-183
```
## frag
`frag           -extract sub-sequences from a FA file   -提取子序列`
* 具体用法  
faops frag  -l （每行多少个字母）  输入文件.fa（test.fa） 开始序列数   终止序列数   输出文件.fa （2.fa）  

  `注：如果有多个序列，则只会提出第一个序列的 start-end 序列`
```
xuruizhi@DESKTOP-HI65AUV:~$ faops frag

faops frag - Extract a piece of DNA from a FA file.
usage:
    faops frag [options] <in.fa> <start> <end> <out.fa>

options:
    -l INT     sequence line length [80]

in.fa  == stdin  means reading from stdin
out.fa == stdout means writing to stdout
```
* 举例  
```
xuruizhi@DESKTOP-HI65AUV:~$ faops frag -l 3 test.fa 55 88 1.fa
More than one sequence in test.fa, just using first

xuruizhi@DESKTOP-HI65AUV:~$ cat 1.fa
>1:55-88
CAT
GCC
CTG
TGG
CCT
GGC
ACC
CAG
CCC
TGA
GCA
G
```
## rc
`rc             -reverse complement a FA file       -反向互补 FA 文件`
* 具体用法
```
xuruizhi@DESKTOP-HI65AUV:~$ faops rc

faops rc - Reverse complement a FA file.
usage:
    faops rc [options] <in.fa> <out.fa>

options:
    -n         keep name identical (don't prepend RC_)
    -r         just Reverse, prepends R_
    -c         just Complement, prepends C_
    -f STR     only RC sequences in this list.file
    -l INT     sequence line length [80]

in.fa  == stdin  means reading from stdin
out.fa == stdout means writing to stdout
```
① Faops  rc   输入文件.fa    输出文件.fa   
注：转化为反向、互补序列，且序列名称改变
```
xuruizhi@DESKTOP-HI65AUV:~$ cat >1.fa <<EOF
> >1
> CTTTTTGTTTACCAAGGCTTTTTTTTT
> >2
> ACTGGGGTCACTGGT
> EOF

xuruizhi@DESKTOP-HI65AUV:~$ cat 1.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT

xuruizhi@DESKTOP-HI65AUV:~$ faops rc 1.fa 2.fa

xuruizhi@DESKTOP-HI65AUV:~$ cat 2.fa
>RC_1
AAAAAAAAAGCCTTGGTAAACAAAAAG
>RC_2
ACCAGTGACCCCAGT
```

② Faops  rc -n    输入文件.fa    输出文件.fa  
注：转化为反向、互补序列，且序列名称不变为RC__
```

xuruizhi@DESKTOP-HI65AUV:~$ cat 1.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT

xuruizhi@DESKTOP-HI65AUV:~$ faops rc -n 1.fa 2.fa

xuruizhi@DESKTOP-HI65AUV:~$ cat 2.fa
>1
AAAAAAAAAGCCTTGGTAAACAAAAAG
>2
ACCAGTGACCCCAGT
```
③ Faops  rc -r    输入文件.fa     输出文件.fa  
注：仅转化为反向序列reverse，不互补，且序列名称改变
```
xuruizhi@DESKTOP-HI65AUV:~$ cat 1.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT

xuruizhi@DESKTOP-HI65AUV:~$ faops rc -r 1.fa 2.fa

xuruizhi@DESKTOP-HI65AUV:~$ cat 2.fa
>R_1
TTTTTTTTTCGGAACCATTTGTTTTTC
>R_2
TGGTCACTGGGGTCA
```
④ Faops rc -c    输入文件.fa    输出文件.fa  
注：转化为互补序列complement，不反向，且序列名称改变
```
xuruizhi@DESKTOP-HI65AUV:~$ cat 1.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT

xuruizhi@DESKTOP-HI65AUV:~$ faops rc -c 1.fa 2.fa

xuruizhi@DESKTOP-HI65AUV:~$ cat 2.fa
>C_1
GAAAAACAAATGGTTCCGAAAAAAAAA
>C_2
TGACCCCAGTGACCA
```
⑤　Faops  rc -f  list.txt  输入文件.fa  输出文件.fa  
注：list.txt文件中提到的序列都转换为 反向互补序列，没有提到的还是原序列
```
xuruizhi@DESKTOP-HI65AUV:~$ cat 1.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT

xuruizhi@DESKTOP-HI65AUV:~$ cat >list.txt <<EOF
> 1
> EOF

xuruizhi@DESKTOP-HI65AUV:~$ faops rc -f list.txt 1.fa 2.fa

xuruizhi@DESKTOP-HI65AUV:~$ cat 2.fa
>RC_1
AAAAAAAAAGCCTTGGTAAACAAAAAG
>2
ACTGGGGTCACTGGT
```

⑥ 　faops rc -l INT  输入文件.fa    输出文件.fa  
注：规定每行的字母数，既反向又互补，其实是faops rc
```
xuruizhi@DESKTOP-HI65AUV:~$ cat 1.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT

xuruizhi@DESKTOP-HI65AUV:~$ faops rc -l 8 1.fa 2.fa

xuruizhi@DESKTOP-HI65AUV:~$ cat 2.fa
>RC_1
AAAAAAAA
AGCCTTGG
TAAACAAA
AAG
>RC_2
ACCAGTGA
CCCCAGT
```

## one and some
`one            extract one fa record        提取一个FA记录   `

`some           extract some fa records       提取一些FA记录`
* 具体用法
```
xuruizhi@DESKTOP-HI65AUV:~$ faops one

faops one - Extract one fa sequence
usage:
    faops some [options] <in.fa> <name> <out.fa>

options:
    -l INT     sequence line length [80]

in.fa  == stdin  means reading from stdin
out.fa == stdout means writing to stdout
```
```
xuruizhi@DESKTOP-HI65AUV:~$ faops some

faops some - Extract multiple fa sequences
usage:
    faops some [options] <in.fa> <list.file> <out.fa>

options:
    -i         Invert, output sequences not in the list
    -l INT     sequence line length [80]

in.fa  == stdin  means reading from stdin
out.fa == stdout means writing to stdout
```
* 举例  
① faops one  输入文件.fa  序列名称   输出文件.fa 
```
xuruizhi@DESKTOP-HI65AUV:~$ cat 1.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT

xuruizhi@DESKTOP-HI65AUV:~$ faops one 1.fa 2 2.fa

xuruizhi@DESKTOP-HI65AUV:~$ cat 2.fa
>2
ACTGGGGTCACTGGT
```
② faops some 输入文件.fa  list.txt   输出文件.fa 
```
xuruizhi@DESKTOP-HI65AUV:~$ cat 1.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT

xuruizhi@DESKTOP-HI65AUV:~$ cat list.txt
1

xuruizhi@DESKTOP-HI65AUV:~$ faops some 1.fa list.txt 2.fa

xuruizhi@DESKTOP-HI65AUV:~$ cat 2.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
```
③ Faops some -i 3 1.fa list.txt 2.fa  
注：从 输入文件 1.fa 中，根据 list.txt 里的序列名称 提取特定序列到 输出文件2.fa 中，-i 用来提取 `名称不在列表中`的序列
（如list.txt 含有 1 序列，则只提取2，3序列）
```
xuruizhi@DESKTOP-HI65AUV:~$ cat 1.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT

xuruizhi@DESKTOP-HI65AUV:~$ cat list.txt
1

xuruizhi@DESKTOP-HI65AUV:~$ faops some -i 1.fa list.txt 2.fa

xuruizhi@DESKTOP-HI65AUV:~$ cat 2.fa
>2
ACTGGGGTCACTGGT
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT
```

 # 补充
 1. 核苷酸和氨基酸缩写表  

 ![tu](./pictures/%E5%9B%BE%E7%89%872.png)
