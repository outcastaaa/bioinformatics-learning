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



#筛选出不合适的N，并且标注出每一序列的具体位置
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

# 提取 start-end 序列
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
## order          
`order extract some fa records by the given order       以一个给定的order提取一些FA记录`
* 具体用法
```
xuruizhi@DESKTOP-HI65AUV:~$ faops order

faops order - Extract multiple fa sequences by the given order.
              Consume much more memory for load all sequences in memory.
usage:
    faops order [options] <in.fa> <list.file> <out.fa>

options:
    -l INT     sequence line length [80]

in.fa  == stdin  means reading from stdin
out.fa == stdout means writing to stdout
```
* 举例
```
faops order in.fa \
      <(faops size in.fa | sort -n -r -k2,2 | cut -f 1) \
      out.fa
(faops size in.fa | sort -n -r -k2,2 | cut -f 1)
# 查看输入文件大小| 以-n ： number 按数字大小，-r 反向排序，从大到小进行排序，-k2以tab空格为分隔符，将第二个区域进行排序 即，以输入文件每个序列的大小从大到小排序| cut -f 指定显示哪个区域 即，该语句限制只显示每一个序列的 第一个区块 \ 输出到out.fa
```
① 包含所有命令,包含cut -f 1， 排序只显示第一个序列的序号
```
xuruizhi@DESKTOP-HI65AUV:~$ cat 1.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT

xuruizhi@DESKTOP-HI65AUV:~$ faops size 1.fa
1       27
2       15
3       33



xuruizhi@DESKTOP-HI65AUV:~$ faops order 1.fa \
>  >(faops size 1.fa | sort -n -r -k2,2 | cut -f 1) \
> 2.fa
3
1
2
```


② 删掉cut -f 语句，按序列长短排序，且全部显示
```
xuruizhi@DESKTOP-HI65AUV:~$ cat 1.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT



xuruizhi@DESKTOP-HI65AUV:~$ faops order 1.fa \
> <(faops size 1.fa | sort -n -r -k2,2) \
> 2.fa

xuruizhi@DESKTOP-HI65AUV:~$ cat 2.fa
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT
>(null)

>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>(null)

>2
ACTGGGGTCACTGGT
>(null)
```
③ 换成 cut -f 2， 只显示第二个序列
```
xuruizhi@DESKTOP-HI65AUV:~$ faops order 1.fa \
> <(faops size 1.fa | sort -n -r -k2,2 | cut -f 2) \
> 2.fa

xuruizhi@DESKTOP-HI65AUV:~$ cat 2.fa
>(null)

>(null)

>(null)
```
## replace
`replace        replace headers from a FA file      替换 header序列名`  

命令faops replace 能够实现对特定序列名的替换, 也可以仅对指定序列的提取、改名并输出。   
其中, replace.tsv 文件里名称的变换可以用制表符tab分隔开. 
* 具体用法
```
xuruizhi@DESKTOP-HI65AUV:~$ faops replace

faops replace - Replace headers from a FA file
usage:
    faops replace [options] <in.fa> <replace.tsv> <out.fa>

options:
    -s         only output sequences in the list, like `faops some` 
    #只输出在replace.tsv 文件中的序列
    -l INT     sequence line length [80]  

<replace.tsv> is a tab-separated file containing two fields
    original_name       replace_name

in.fa  == stdin  means reading from stdin
out.fa == stdout means writing to stdout
```
* 举例
```
xuruizhi@DESKTOP-HI65AUV:~$ cat 1.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT

xuruizhi@DESKTOP-HI65AUV:~$ cat >replace.tsv <<EOF
> 1   5
> 2   6
> EOF



xuruizhi@DESKTOP-HI65AUV:~$ faops replace -s 1.fa replace.tsv 2.fa

xuruizhi@DESKTOP-HI65AUV:~$ cat 2.fa
>5
CTTTTTGTTTACCAAGGCTTTTTTTTT
>6
ACTGGGGTCACTGGT



# -s 只显示replace.tsv中序列，删掉该命令则都显示
xuruizhi@DESKTOP-HI65AUV:~$ faops replace 1.fa replace.tsv 3.fa
xuruizhi@DESKTOP-HI65AUV:~$ cat 3.fa
>5
CTTTTTGTTTACCAAGGCTTTTTTTTT
>6
ACTGGGGTCACTGGT
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT
```
## filter        
` filter filter fa records    过滤 FA 记录`  
* 具体用法
```
xuruizhi@DESKTOP-HI65AUV:~$ faops filter

faops filter - Filter fa records
usage:
    faops filter [options] <in.fa> <out.fa>

options:
    -a INT     pass sequences at least this big ('a'-smallest)
    -z INT     pass sequences this size or smaller ('z'-biggest)
    -n INT     pass sequences with fewer than this number of N's
    -u         Unique, removes duplicated ids, keeping the first
    -U         Upper case, converts all sequences to upper cases
    -b         pretend to be a blocked fasta file
    -N         convert IUPAC ambiguous codes to 'N'
    -d         remove dashes '-'
    -s         simplify sequence names
    -l INT     sequence line length [80]

in.fa  == stdin  means reading from stdin
out.fa == stdout means writing to stdout

Not all faFilter options were implemented.
Names' wildcards are easily accomplished by "faops some".
```
* 举例  
①　Faops filter -a  INT 输入文件.fa  输出文件.fa   
 `筛掉短序列，留长序列，序列长度最小为INT`
```
xuruizhi@DESKTOP-HI65AUV:~$ cat 1.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT

xuruizhi@DESKTOP-HI65AUV:~$ faops size 1.fa
1       27
2       15
3       33



xuruizhi@DESKTOP-HI65AUV:~$ faops filter -a 25 1.fa 2.fa

xuruizhi@DESKTOP-HI65AUV:~$ cat 2.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT
```
②　Faops  filter -z  INT 输入文件.fa  输出文件.fa    
`筛掉长序列，留短序列，序列长度最大为INT`
```
xuruizhi@DESKTOP-HI65AUV:~$ faops filter -z 30 1.fa 2.fa

xuruizhi@DESKTOP-HI65AUV:~$ cat 2.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT
```
③　Faops  filter -n  INT 输入文件.fa  输出文件.fa  
`清除含有 超过INT个数的 N 的序列，留含N少的序列`
```
xuruizhi@DESKTOP-HI65AUV:~$ cat 3.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTTnnnnnnnnnn
>2
ACTGGGGTCACTGGTN
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGATNNNN



xuruizhi@DESKTOP-HI65AUV:~$ faops filter -n 3 3.fa 4.fa

xuruizhi@DESKTOP-HI65AUV:~$ cat 4.fa
>2
ACTGGGGTCACTGGTN
```
④　Faops  filter -u  输入文件.fa  输出文件.fa  
`Unique，删除重复id的序列，保留第一个`
```
xuruizhi@DESKTOP-HI65AUV:~$ cat 3.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTTnnn
>2
ACTGGGGTCACTGGTN
>3
CTTGGCCAGCGTGTTG
>2
CTTGGCCAGC



xuruizhi@DESKTOP-HI65AUV:~$ faops filter -u 3.fa 4.fa

xuruizhi@DESKTOP-HI65AUV:~$ cat 4.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTTnnn
>2
ACTGGGGTCACTGGTN
>3
CTTGGCCAGCGTGTTG
```
⑤　Faops  filter -U  输入文件.fa  输出文件.fa  
`大写，将所有序列转换为大写`
```
xuruizhi@DESKTOP-HI65AUV:~$ cat 3.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTTnnn
>2
ACTGGGGTCACTGGTN
>3
CTTGGCCAGCGTGTTG
>2
CTTGGCCAGC



xuruizhi@DESKTOP-HI65AUV:~$ faops filter -U 3.fa 4.fa

xuruizhi@DESKTOP-HI65AUV:~$ cat 4.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTTNNN
>2
ACTGGGGTCACTGGTN
>3
CTTGGCCAGCGTGTTG
>2
CTTGGCCAGC
```
⑥　Faops  filter -b  输入文件.fa  输出文件.fa  
`假装是一个被屏蔽的fasta文件`
```
还不太清楚怎么用
```

⑦　Faops  filter -N  输入文件.fa  输出文件.fa  
`将IUPAC模糊二义码转换为“N”` 不太清楚咋用    
IUPAC：  
![tu](./pictures/%E5%9B%BE%E7%89%873.png)
![tu](./pictures/%E5%9B%BE%E7%89%874.png)  

⑧　Faops  filter -d  输入文件.fa  输出文件.fa    
`删除破折号“-”`  
⑨　Faops  filter -s  输入文件.fa  输出文件.fa    
`简化序列名称`  
```
xuruizhi@DESKTOP-HI65AUV:~$ cat 3.fa
>human spins
CTTTTTGTTTACCAAGGCTTTTTTTTTnnn
>mouse
CTTGGCCAGCGTGTTG



xuruizhi@DESKTOP-HI65AUV:~$ faops filter -s 3.fa 4.fa

xuruizhi@DESKTOP-HI65AUV:~$ cat 4.fa
>human
CTTTTTGTTTACCAAGGCTTTTTTTTTnnn
>mouse
CTTGGCCAGCGTGTTG
```
## split-name    
`split-name splitting by sequence names       按序列名称拆分`  
`命令faops split-name 可以按序列名称对序列文件进行切割. 此功能会输出一个包含所有序列的文件夹`
* 具体用法  
```
xuruizhi@DESKTOP-HI65AUV:~$ faops split-name

faops split-name - Split an fa file into several files
                   Using sequence names as file names
usage:
    faops split-name [options] <in.fa> <outdir>

options:
    -l INT     sequence line length [80]

in.fa  == stdin  means reading from stdin
```
* 举例  
```
xuruizhi@DESKTOP-HI65AUV:~$ cat 1.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT




# 每个序列被单独拆分为n个文件（n.fa），保存在out.dir里
xuruizhi@DESKTOP-HI65AUV:~$ faops split-name 1.fa out.dir

xuruizhi@DESKTOP-HI65AUV:~$ ls
1.fa   2.fa  4.fa     bin           download.sh  list.txt  replace.tsv  software
1.txt  3.fa  Scripts  brew-install  faops        out.dir   share        test.fa

xuruizhi@DESKTOP-HI65AUV:~$ cd out.dir

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ ls
1.fa  2.fa  3.fa

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 1.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 2.fa
>2
ACTGGGGTCACTGGT

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 3.fa
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT
```
##  split-about    
`split-sbout  splitting to chunks about specified size    分割成指定大小的块并放在不同文件夹内      输出文件.dir`  
* 具体用法
```
xuruizhi@DESKTOP-HI65AUV:~$ faops split-about

faops split-about - Split an fa file into several files
                    of about approx_size bytes each by record
usage:
    faops split-about [options] <in.fa> <approx_size> <outdir>
# approx_size 指的是碱基片段的长度

options:
    -e         sequences in one file should be EVEN
    -m INT     max parts
    -l INT     sequence line length [80]
```

* 举例  

①　approx_size  指的是碱基片段的长度  # 不确定是不是对的，未总结出规律
```
xuruizhi@DESKTOP-HI65AUV:~$ cat 1.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT

xuruizhi@DESKTOP-HI65AUV:~$ faops size 1.fa
1       27
2       15
3       33




# approx_size 为20
xuruizhi@DESKTOP-HI65AUV:~$ faops split-about 1.fa 20 out.dir

xuruizhi@DESKTOP-HI65AUV:~$ cd out.dir

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ ls
000.fa  001.fa

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 000.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 001.fa
>2
ACTGGGGTCACTGGT
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT



# 改变approx_size 为10
xuruizhi@DESKTOP-HI65AUV:~$ faops split-about 1.fa 10 out.dir

xuruizhi@DESKTOP-HI65AUV:~$ cd out.dir

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ ls
000.fa  001.fa  002.fa

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 000.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 001.fa
>2
ACTGGGGTCACTGGT
xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 002.fa
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT

```
② -e 一个文件中的序列是偶数个   

比如：一共3个序列1，2，3，就会分割呈两个文件，每个文件中含两个序列（不够两个就一个）
```
xuruizhi@DESKTOP-HI65AUV:~$ cat 1.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT

xuruizhi@DESKTOP-HI65AUV:~$ faops size 1.fa
1       27
2       15
3       33



# approx_size为20，没有-e
xuruizhi@DESKTOP-HI65AUV:~$ faops split-about 1.fa 20 out.dir

xuruizhi@DESKTOP-HI65AUV:~$ cd out.dir

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ ls
000.fa  001.fa

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 000.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 001.fa
>2
ACTGGGGTCACTGGT
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT



# approx_size为20，加上-e
xuruizhi@DESKTOP-HI65AUV:~$ faops split-about -e  1.fa 20 out.dir

xuruizhi@DESKTOP-HI65AUV:~$ cd out.dir

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ ls
000.fa  001.fa

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 000.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT
xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 001.fa
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT




# approx_size 为10
xuruizhi@DESKTOP-HI65AUV:~$ faops split-about 1.fa 10 out.dir

xuruizhi@DESKTOP-HI65AUV:~$ cd out.dir

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ ls
000.fa  001.fa  002.fa

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 000.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 001.fa
>2
ACTGGGGTCACTGGT
xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 002.fa
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT



#approx_size为10， 加上-e
xuruizhi@DESKTOP-HI65AUV:~$ faops split-about -e 1.fa 10 out.dir

xuruizhi@DESKTOP-HI65AUV:~$ cd out.dir

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ ls
000.fa  001.fa

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 000.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT
xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 001.fa
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT

```

③　-m INT 确定最大分割byte
```
xuruizhi@DESKTOP-HI65AUV:~$ cat 1.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT




#没有-m 参数
xuruizhi@DESKTOP-HI65AUV:~$  faops split-about 1.fa 20 out.fa

xuruizhi@DESKTOP-HI65AUV:~$ cd out.fa

xuruizhi@DESKTOP-HI65AUV:~/out.fa$ ls
000.fa  001.fa  002.fa

xuruizhi@DESKTOP-HI65AUV:~/out.fa$ cat 000.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTTACTGGGGTCA
xuruizhi@DESKTOP-HI65AUV:~/out.fa$ cat 001.fa
>2
ACTGGGGTCACTGGTACTGGGGTCACTGGT
xuruizhi@DESKTOP-HI65AUV:~/out.fa$ cat 002.fa
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT




# -m 1 参数， 只分割出来序列1

xuruizhi@DESKTOP-HI65AUV:~$ faops split-about -m 1 1.fa 20 out.dir

xuruizhi@DESKTOP-HI65AUV:~$ cd out.dir

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ ls
000.fa
xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 000.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT



# -m 2 参数，将三个序列分割到两个文件

xuruizhi@DESKTOP-HI65AUV:~$ faops split-about -m 2 1.fa 20 out.dir

xuruizhi@DESKTOP-HI65AUV:~$ cd out.dir

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ ls
000.fa  001.fa

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 000.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 001.fa
>2
ACTGGGGTCACTGGT
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT



# -m 3 参数，将三个序列分割到两个文件     # 不是很明白为什么

xuruizhi@DESKTOP-HI65AUV:~$  faops split-about -m 3 1.fa 20 out.dir

xuruizhi@DESKTOP-HI65AUV:~$ cd out.dir

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ ls
000.fa  001.fa

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 000.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 001.fa
>2
ACTGGGGTCACTGGT
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT



# 同时有-e -m， 从第一个文件开始尽量保证是偶数

xuruizhi@DESKTOP-HI65AUV:~$ faops split-about -e -m 3 1.fa 20 out.dir

xuruizhi@DESKTOP-HI65AUV:~$ cd out.dir

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ ls
000.fa  001.fa

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 000.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT
xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 001.fa
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT
```



④ 一个文件下多个序列， 直接合并到一起
```
xuruizhi@DESKTOP-HI65AUV:~$ cat 1.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
ACTGGGGTCA
>2
ACTGGGGTCACTGGT
ACTGGGGTCACTGGT
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT

xuruizhi@DESKTOP-HI65AUV:~$ faops size 1.fa
1       37
2       30
3       33



xuruizhi@DESKTOP-HI65AUV:~$ faops split-about 1.fa 30 out.dir

xuruizhi@DESKTOP-HI65AUV:~$ cd out.dir

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ ls
000.fa  001.fa  002.fa

xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 000.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTTACTGGGGTCA
xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 001.fa
>2
ACTGGGGTCACTGGTACTGGGGTCACTGGT
xuruizhi@DESKTOP-HI65AUV:~/out.dir$ cat 002.fa
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT
```

## n50            
` n50   compute N50 and other statistics        计算 N50 和其他统计数据`  
* 具体用法
```
xuruizhi@DESKTOP-HI65AUV:~$ faops n50

faops n50 - compute N50 and other statistics.
usage:
    faops n50 [options] <in.fa> [more_files.fa]

options:
    -H         do not display header
    -N INT     compute Nx statistic [50]
    -S         compute sum of size of all entries
    -A         compute average length of all entries
    -E         compute the E-size (from GAGE)
    -C         count entries
    -g INT     size of genome, instead of total size in files

in.fa  == stdin  means reading from stdin
```

①　-H 只显示最后统计结果的数字，但是不显示每个数字对应的条目  
②　-N 50或者 -N 90    
`每一段含名称的序列都是一个contig`    

③　-S 计算所有条目的大小总和  
④　-A 计算所有条目的平均长度= -S/序列个数  
⑤　-E 计算E-size(来自GAGE)  
⑥　-C 计算条目数    
⑦    -g INT  基因组的大小，而不是文件中的总大小  
 * 举例
 ```
 xuruizhi@DESKTOP-HI65AUV:~$ cat 1.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT



#  没有-H参数
xuruizhi@DESKTOP-HI65AUV:~$ faops n50 -N90 -S -A -E -C -g 2 1.fa
N90     33
S       75       #计算所有条目的大小总和 
A       25.00    #计算所有条目的平均长度= -S/序列个数
E       27.24    #计算E-size(来自GAGE)
C       3        #计算条目数  


#  有-H参数
xuruizhi@DESKTOP-HI65AUV:~$ faops n50 -N90 -S -A -E -C -g 2 -H 1.fa
33
75
25.00
27.24
3
```


## dazz  
`dazz,     rename records for dazz_db        重命名 dazz_db 的记录`  
`命令faops dazz 可以对序列信息命名进行新的标准化, 为下游组装分析做准备.——改名卡`
* 具体用法
```
xuruizhi@DESKTOP-HI65AUV:~$ faops dazz

faops dazz - Rename records for dazz_db
usage:
    faops dazz [options] <in.fa> <out.fa>

options:
    -p STR     prefix of names [read]
    -s INT     start index [1]
    -a         don't drop duplicated ids
    -l INT     sequence line length [80]

in.fa  == stdin  means reading from stdin
out.fa == stdout means writing to stdout

Sequences with duplicated ids will be dropped, keeping the first one.
This command don't write a replace.tsv file, `anchr dazzname` does.
```
①　-p STR 名称前缀[read]  
②　-s INT 开始指数[1]  
③　-a 不要删除重复的id  
具有重复id的序列将被删除，保留第一个。  
这个命令不会书写  replace.tsv file，但是 anchr dazzname 可以。 


* 举例  
 
```
xuruizhi@DESKTOP-HI65AUV:~$ faops dazz -p read -s 0 -a 1.fa 2.fa
# 序列名称前缀为 `read`, 从`0`开始，且不删除重复id

xuruizhi@DESKTOP-HI65AUV:~$ cat 2.fa
>read/0/0_27
CTTTTTGTTTACCAAGGCTTTTTTTTT
>read/1/0_15
ACTGGGGTCACTGGT
>read/2/0_33
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT

#>read/2/0_33  —— -p前缀为 read/-s 从几开始编号/ 0_碱基个数
```
##  interleave    
`interleave,   interleave two PE files        交错两个 PE 文件`  
命令faops interleave 可以将双端测序的两个文件交错合并. 可以只输入一个文件, 
此时另一端会以N 为序列内容。
写入到标准输出，不支持从标准输入读取。
* 具体用法
```
xuruizhi@DESKTOP-HI65AUV:~$ faops interleave

faops interleave - Interleave two PE files
                   One file is also OK, output a single `N`.
                   With -q, the quality value set to `!` (33)
usage:
    faops interleave [options] <R1.fa> [R2.fa]

options:
    -q         write FQ. The inputs must be FQs
    -p STR     prefix of names [read]
    -s INT     start index [0]

Write to stdout and don't support reading from stdin.
```
交叉放置两个PE文件, 一个文件也OK，输出一个' N '。  
使用-q，质量值设置为' !”(33)  
①　-q   写入FQ。输入必须是fq  
②　-p STR 名称前缀[read]  
③　-s INT 开始指数[0]  
* 举例  
```
xuruizhi@DESKTOP-HI65AUV:~$ cat 1.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT


xuruizhi@DESKTOP-HI65AUV:~$ cat 2.fa
>4
GGCAGAATGGGCTGGGTGCAGAGAGTGTTCTTGCATGACGGAGCTGAGAGGGAGCCACTC
TCCAAAGGCTCTGTATTCCTGCCACTCAGGCTGTGTCTCCGGGGGAAAGGAGGGGGTCCA
>5
GCGTGGTGGTGGCAGTGGCGGTGGGGTGTGCCACCACCATGG
>6
CAATCAGCAGCCTC



# 没有-q 参数
xuruizhi@DESKTOP-HI65AUV:~$ faops interleave -p read -s 0 1.fa 2.fa
>read0/1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>read0/2
GGCAGAATGGGCTGGGTGCAGAGAGTGTTCTTGCATGACGGAGCTGAGAGGGAGCCACTCTCCAAAGGCTCTGTATTCCTGCCACTCAGGCTGTGTCTCCGGGGGAAAGGAGGGGGTCCA
>read1/1
ACTGGGGTCACTGGT
>read1/2
GCGTGGTGGTGGCAGTGGCGGTGGGGTGTGCCACCACCATGG
>read2/1
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT
>read2/2
CAATCAGCAGCCTC



#有-q 参数
xuruizhi@DESKTOP-HI65AUV:~$ faops interleave -p read -s 0 -q 1.fa 2.fa
@read0/1
CTTTTTGTTTACCAAGGCTTTTTTTTT
+
(null)
@read0/2
GGCAGAATGGGCTGGGTGCAGAGAGTGTTCTTGCATGACGGAGCTGAGAGGGAGCCACTCTCCAAAGGCTCTGTATTCCTGCCACTCAGGCTGTGTCTCCGGGGGAAAGGAGGGGGTCCA
+
(null)
@read1/1
ACTGGGGTCACTGGT
+
(null)
@read1/2
GCGTGGTGGTGGCAGTGGCGGTGGGGTGTGCCACCACCATGG
+
(null)
@read2/1
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT
+
(null)
@read2/2
CAATCAGCAGCCTC
+
(null)
```
Read 0/1 ——①；Read 0/2 ——1；Read 1/1 ——②；Read 1/2 ——2；
Read 2/1 ——③；Read 2/2 ——3

# region      
`region,   extract regions from a FA file      从 FA 文件中提取区域`  
命令faops region 可以在序列文件中摘取一个或多个指定的序列片段, 本功能可以使用参数标注摘取的正反方向. region.txt 中某个序列的多个位置用逗号分隔,多个序列之间用回车换行分隔.
此外, 指的注意的是, 此命令不能进行空序列片段的摘取, 会输出其他序列的片段.
* 具体用法
```
xuruizhi@DESKTOP-HI65AUV:~$ faops region

faops region - Extract regions from a FA file
usage:
    faops region [options] <in.fa> <region.txt> <out.fa>

options:
    -s         add strand '(+)' to headers
    -l INT     sequence line length [80]

<region.txt> is a text file containing one field
    seq_name:begin-end[,begin-end]

in.fa  == stdin  means reading from stdin
out.fa == stdout means writing to stdout
```
* 举例  
　-s    add strand '(+)' to headers, 在序列名称添加'(+)'
```
xuruizhi@DESKTOP-HI65AUV:~$ cat 1.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT




# 一个序列中多个位置需要用`,`分割，要不然会直接覆盖
xuruizhi@DESKTOP-HI65AUV:~$ cat region.txt
1:3-11
1:6-22
3:3-15

xuruizhi@DESKTOP-HI65AUV:~$ faops region -s 1.fa region.txt 2.fa

xuruizhi@DESKTOP-HI65AUV:~$ cat 2.fa
>1(+):6-22
TGTTTACCAAGGCTTTT
>3(+):3-15
TGGCCAGCGTGTT


# 用`,`分割  
xuruizhi@DESKTOP-HI65AUV:~$ cat region.txt
1:3-11,6-22
3:3-15

xuruizhi@DESKTOP-HI65AUV:~$ faops region -s 1.fa region.txt 2.fa

xuruizhi@DESKTOP-HI65AUV:~$ cat 2.fa
>1(+):3-11
TTTTGTTTA
>1(+):6-22
TGTTTACCAAGGCTTTT
>3(+):3-15
TGGCCAGCGTGTT
```





# Examples
 1. 取反向互补序列,   Reverse complement   
```
 faops rc  test/ufasta.fa   out.fa          # 在名称前添加 RC_ 反向互补
 faops rc -n  test/ufasta.fa    out.fa      # 保留原有名称
```
```
xuruizhi@DESKTOP-HI65AUV:~$ cat 1.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT

# 在名称前添加 RC_ 反向互补
xuruizhi@DESKTOP-HI65AUV:~$ faops rc 1.fa 2.fa
xuruizhi@DESKTOP-HI65AUV:~$ cat 2.fa
>RC_1
AAAAAAAAAGCCTTGGTAAACAAAAAG
>RC_2
ACCAGTGACCCCAGT
>RC_3
ATCAGCCACATCCCCTACAACACGCTGGCCAAG


# 保留原有名称
xuruizhi@DESKTOP-HI65AUV:~$ faops rc -n 1.fa 2.fa
xuruizhi@DESKTOP-HI65AUV:~$ cat 2.fa
>1
AAAAAAAAAGCCTTGGTAAACAAAAAG
>2
ACCAGTGACCCCAGT
>3
ATCAGCCACATCCCCTACAACACGCTGGCCAAG
```
2.  提取 list.file 中所包含序列名称 对应的整个序列  
```
faops sometest/ufasta.fa       list.file       out.fa  #提取一些FA记录
```
3. 提取 list.file 中名称的序列，每行一个名称，但从标准输入到标准输出  
```
  Cat   test/ufasta.fa   | faops   some     stdin      list.file     stdout
```
4. 按序列编号大小排序  
``` 
faops order      test/ufasta.fa \        # 以一个给定的order提取一些FA记录
      <(  cat   test/ufasta.fa | grep '>' | sed 's/>//' | sort) \
         out.fa

# Faops order  输入文件.fa \ < (输入文件.fa  | 在文本中查找 '>' 字符 | 将 '>' 字符替换为空，即删除>字符 | 排序  →即，按照序列标号的大小排序) \ 输出文件.fa
```
```
xuruizhi@DESKTOP-HI65AUV:~$ cat 3.fa
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT


xuruizhi@DESKTOP-HI65AUV:~$  faops order 3.fa \
> <(cat 3.fa | grep '>' |  sed 's/>//' | sort) \    # 注意|前后有空格
> 4.fa
xuruizhi@DESKTOP-HI65AUV:~$ cat 4.fa
>1
CTTTTTGTTTACCAAGGCTTTTTTTTT
>2
ACTGGGGTCACTGGT
>3
CTTGGCCAGCGTGTTGTAGGGGATGTGGCTGAT
```
5. 按每个序列的base长度进行排序Sort by lengths
```
faops order           test/ufasta.fa \         
      <(faops size test/ufasta.fa | sort -n -r -k2,2 | cut -f 1) \         
   out2.fa

#计算总base数；按数字大小进行反向排序（从大到小）；按照分割的第二区域进行排序；只排序每个序列的第一段（如果没有该语句，还会显示其他段）
```
6. 整理fasta文件到每行80个字符的序列Tidy fasta file to 80 characters of sequence per line   
```
faops filter   -l 80   test/ufasta.fa   out.fa
```
7. 所有内容写在一行上All content written on one line  
```      
  faops filter  -l 0      test/ufasta.fa       out.fa
```
8. 将 fastq格式 转换为 fasta格式    Convert fastq to fasta 
```     
  Faops  filter   -l 0   in.fq   out.fa
```
9. 计算N50，不显示序列名称    Compute N50, clean result
```
   Faops  n50   -H   test/ufasta.fa
```
10. 用估计的基因组大小计算 N90、contig的总和和平均值
```
 Faops   n50   -N 90   -S   -A   -g 10000   test/ufasta.fa
```



 # 补充
 1. 核苷酸和氨基酸缩写表  

 ![tu](./pictures/%E5%9B%BE%E7%89%872.png)  

2. N50  
* 概念  
①  contigs是reads的拼接结果；scaffold是contigs的组装结果，而且总scaffold长度长于总contigs。    
②  Reads拼接后会获得一些不同长度的Contigs.将所有的Contig长度相加,能获得一个Contig总长度.然后将所有的Contigs按照从长到短进行排序,如获得Contig 1,Contig 2,contig 3...………Contig 25.将Contig按照这个顺序依次相加,当相加的长度达到Contig总长度的一半时,最后一个加上的Contig长度即为Contig N50.  
举例：Contig 1+Contig 2+ Contig 3 +Contig 4=Contig总长度*1/2时,Contig 4的长度即为Contig N50.  
`ContigN50可以作为基因组拼接的结果好坏的一个判断标准.`


![tu](./pictures/%E5%9B%BE%E7%89%875.png)  

* N50如何影响基因预测    

  成功注释基因组的第一步就是看组装有没有达到要求，除了一些统计指标来表述组装的完整性和连续性之外，最重要的就是N50.尽管没有绝对的标准，但是对于基因预测而言，n50达到基因的平均长度是一个合理的目标，原因十分简单：基因中约有50%有望包括在单个scaffold或者contig中。对于n90，就是基因中约有90%有望包括在单个scaffold或者contig中。这样会得到完整的基因序列。这是很有意义的。  
  理论上，N50 越小，说明拼接效果越好

