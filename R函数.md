- [R函数](#R函数)
    - [data.frame](#data.frame)
    - [readcsv](#readcsv)
    - [factor](#factor)
    - [colnames-rownames](#colnames-rownames)
    - [readtable](#readtable)
    - [lapply](#lapply)
    - [gsub](#gsub)
    - [strsplit](#strsplit)
    - [substring](#substring)
    - [nchar](#nchar)
    - [文件目录的切换方式](#文件目录的切换方式)
    - [Curl的使用方法](#Curl的使用方法)
    - [Eval用法](#Eval用法)
    - [Chmod用法](#Chmod用法)
    - [linux中PATH=$PATH:$HOME/bin是什么意思](#linux中PATH=$PATH:$HOME/bin是什么意思)
    - [Cd用法](#Cd用法)
    - [Lsb-release](#Lsb-release)
    - [Proxy](#Proxy)
    - [Remove用法](#Remove用法)
    - [Head命令](#Head命令)
    - [Shell中2>/dev/null](#Shell中2>/dev/null)
    - [Hash命令](#Hash命令)
    - [使用cpanm安装模块](#使用cpanm安装模块)
    - [make命令](#make命令)
    - [wget命令](#wget命令)
    - [tar命令](#tar命令)
    - [wget命令](#wget命令)

# data.frame  
data.frame()函数用法  


1. 功能说明：  

data.frame()函数创建数据框，紧密耦合的变量集合，这些变量共享了矩阵和列表的许多属性，它们被大多数R的建模软件用作基本的数据结构。  

data Frame一般被翻译为数据框，感觉就像是R中的表，由行和列组成，与Matrix不同的是，每个列可以是不同的数据类型，而Matrix是必须相同的。  


链接：https://www.jianshu.com/p/cb2c2367b388  


2. 语 法：  
```R
Usage
data.frame(..., row.names = NULL, check.rows = FALSE,
           check.names = TRUE, fix.empty.names = TRUE,
           stringsAsFactors = default.stringsAsFactors())

default.stringsAsFactors()
```

* ... : 这些参数要么是表单值，要么是标签=值。组件名称是基于标签（如果存在）或离开的参数本身创建的。  
* row.names: NULL或单个整数或字符串，指定某列用作行名，或者一个字符或整型向量用作数据框的行名。  
* check.rows: 如果是TRUE，那么就检查行长度和名称是否一致性。  
* stringsAsFactors: 逻辑：字符向量是否应该转换为因子？‘factory-fresh’的默认值是TRUE，但是可以通过设置选项（stringsAsFactors=FALSE）来改变这一点。
* fix.empty.names:
逻辑指示是否参数“未命名”（在未被正式称为someName = arg）获得自动构造的名称。 如果应保留“ ”名称，即使check.names为false，也需要设置为FALSE。  

3. 举例  
```R
> L3 <- LETTERS[1:3]
> L3
[1] "A" "B" "C"
> fac <- sample(L3, 10, replace = TRUE)
> fac
 [1] "C" "A" "A" "A" "C" "B" "B" "A" "A" "A"
> (d <- data.frame(x = 1, y = 1:10, fac = fac))
   x  y fac
1  1  1   C
2  1  2   A
3  1  3   A
4  1  4   A
5  1  5   C
6  1  6   B
7  1  7   B
8  1  8   A
9  1  9   A
10 1 10   A
```

# read.csv

1. 说明：  

  `read.csv()`   R语言中的函数用于读取“comma separated value”文件。   它以 DataFrame 的形式导入数据。

2. 用法：
``` R
read.csv(file, header, sep, dec)
```
参数：  
```
file: 包含要导入到 R 中的数据的文件的路径。
header: 逻辑值。如果为 TRUE，则 read.csv() 假定您的文件具有标题行，因此第 1 行是每列的名称。如果不是这种情况，您可以添加参数 header = FALSE。
sep: 字段分隔符
dec: 文件中用于小数点的字符。
```
3. 示例   
```
1：从同一文件夹中读取文件

# R program to read a csv file
  
# Get content into a data frame 
data <- read.csv("CSVFileExample.csv",  
                  header = FALSE, sep = "\t") 
    
# Printing content of Text File 
print(data) 
输出：

   V1 V2 V3
1 100 AB ab
2 200 CD cd
3 300 EF ef
4 400 GH gh
5 500 IJ ij


示例 2：从不同目录读取文件


# Simple R program to read csv file 
x <- read.csv("D://Datas//myfile.csv") 
    
# print x 
print(x) 
输出：

  X  V1 V2 V3
1 1 100 a1 b1
2 2 200 a2 b2
3 3 300 a3 b3
```
# factor  

[参考1](https://blog.csdn.net/hsdcc217/article/details/78510087)      

[参考2](https://zhuanlan.zhihu.com/p/413042715#:~:text=R%E4%B8%AD%E7%9A%84%E5%9B%A0%E5%AD%90%E7%94%A8%E4%BA%8E%E5%AD%98%E5%82%A8%E4%B8%8D%E5%90%8C%E7%B1%BB%E5%88%AB%E7%9A%84%E6%95%B0%E6%8D%AE%EF%BC%8C%E5%8F%AF%E4%BB%A5%E7%94%A8%E6%9D%A5%E5%AF%B9%E6%95%B0%E6%8D%AE%E8%BF%9B%E8%A1%8C%E5%88%86%E7%BB%84%EF%BC%8C%E4%BE%8B%E5%A6%82%E4%BA%BA%E7%9A%84%E6%80%A7%E5%88%AB%E6%9C%89%E7%94%B7%E5%92%8C%E5%A5%B3%E4%B8%A4%E4%B8%AA%E7%B1%BB%E5%88%AB%EF%BC%8C%E6%A0%B9%E6%8D%AE%E5%B9%B4%E9%BE%84%E5%8F%AF%E4%BB%A5%E5%B0%86%E4%BA%BA%E5%88%86%E4%B8%BA%E6%9C%AA%E6%88%90%E5%B9%B4%E4%BA%BA%E5%92%8C%E6%88%90%E5%B9%B4%E4%BA%BA%EF%BC%8C%E8%80%83%E8%AF%95%E6%88%90%E7%BB%A9%E5%8F%AF%E4%BB%A5%E5%88%86%E4%B8%BA%E4%BC%98%EF%BC%8C%E8%89%AF%EF%BC%8C%E4%B8%AD%EF%BC%8C%E5%B7%AE%E3%80%82%20R%20%E8%AF%AD%E8%A8%80%E5%88%9B%E5%BB%BA%E5%9B%A0%E5%AD%90%E4%BD%BF%E7%94%A8,factor%20%28%29%20%E5%87%BD%E6%95%B0%EF%BC%8C%E5%90%91%E9%87%8F%E4%BD%9C%E4%B8%BA%E8%BE%93%E5%85%A5%E5%8F%82%E6%95%B0%E3%80%82)

1. 介绍  

在R语言中，因子（factor）表示的是一个编号或者一个等级，即，一个点。  

例如，人的个数可以是1，2，3，4……那么因子就包括，1，2，3，4…..;还有描述协变量水平时，会用到高、中、低，也是因子，因为这些都是一个点。   

与之区别的向量，是一个连续性的值，例如，数值中有1，1.1，1.2……可以作为数值来计算，而因子则不可以。简单通俗来讲：因子是一个点，向量是一个有方向的范围。  

在R中，如果把数字作为因子，那么在导入数据之后，需要将向量转换为因子（factor），而因子在整个计算过程中不再作为数值，而是一个”符号”而已。  

2. 用法：  
```R
factor(x = character(), levels, labels = levels, exclude = NA, 
ordered = is.ordered(x), nmax = NA)
```  
* x：向量。
* levels：指定各水平值, 不指定时由x的不同值来求得。
* labels：水平的标签, 不指定时用各水平值的对应字符串。
* exclude：排除的字符。
* ordered：逻辑值，用于指定水平是否有序。
* nmax：水平的上限数量。


3. 举例：
```
1. 
data <- c(1,2,2,3,1,2,3,3,1,2,3,3,1)  
> data
 [1] 1 2 2 3 1 2 3 3 1 2 3 3 1 

> fdata <- factor(data)
> fdata
 [1] 1 2 2 3 1 2 3 3 1 2 3 3 1
Levels: 1 2 3
>is.factor(sex)
[1]TRUE


> class(fdata)
[1] "factor"
> class(data)
[1] "numeric"

#factor()函数将原来的数值型的向量转化为了factor类型。
factor类型的向量中有Levels的概念。Levels就是factor中的所有元素的集合（没有重复）。
我们可以发现Levels就是factor中元素排除重复后且字符化的结果。因为Levels的元素都是character。

2. 
> levels(fdata)
[1] "1" "2" "3"

#我们可以在factor生成时，可以通过level直接指定其顺序，通过labels向量来指定levels的具体符号，继续上面的程序：

> rdata <- factor(data,labels=c("I","II","III"))
> rdata
 [1] I   II  II  III I   II  III III I   II  III III I  
Levels: I II III

> rdata <- factor(data,labels=c("e","ee","eee"))
> rdata
 [1] e   ee  ee  eee e   ee  eee eee e   ee  eee eee e  
Levels: e ee eee

3. 
#factors可以指定数据的顺序

> mons <- c("March","April","January","November","January", "September","October","September","November","August", "January","November","November","February","May","August", "July","December","August","August","September","November", "February","April")
> mons <- factor(mons)
> mons
 [1] March     April     January   November  January  
 [6] September October   September November  August   
[11] January   November  November  February  May      
[16] August    July      December  August    August   
[21] September November  February  April    
11 Levels: April August December February ... September

> table(mons)
mons
    April    August  December  February   January 
        2         4         1         2         3 
     July     March       May  November   October 
        1         1         1         5         1 
September 
        3 


#显然月份是有顺序的，我们可以为factor指定顺序
mons = factor(mons,levels=c("January","February","March","April","May","June","July","August","September","October","November","December"),ordered=TRUE)

> table(mons)
mons
  January  February     March     April       May 
        3         2         1         2         1 
     June      July    August September   October 
        0         1         4         3         1 
 November  December 
        5         1 
```
# colnames-rownames  

1. 介绍  
检索或设置一个类似矩阵的对象的行或列名称。  

2. 具体用法：  
```R
rownames(x, do.NULL = TRUE, prefix = "row")
rownames(x)
colnames(x, do.NULL = TRUE, prefix = "col")
colnames(x)
```

参数说明：  
* x : 一个类似于R的矩阵对象，至少有两个维度作为colnames。

* do.NULL : 合乎逻辑。如果为FALSE且名称为NULL，则创建名称。

* prefix : 用于创建的名称。

* value : dimnames（x）组件的有效值。对于矩阵或数组，这是NULL或长度不为零的字符向量，等于适当的维数。

3. 举例  
```R
> dataframe <- read.csv("merge.csv", header=TRUE, row.names = 1)
# 有row.names = 1参数

> head(dataframe)
                       SRR2190795.fastq.gz SRR2240182.fastq.gz
__alignment_not_unique              716726             1033461
__ambiguous                         551241              918531
__no_feature                        861781             1251102
__not_aligned                      1149668             2058240
__too_low_aQual                          0                   0
ENSRNOG00000000001                       2                   5
                       SRR2240183.fastq.gz SRR2240184.fastq.gz
__alignment_not_unique              824232              879059
__ambiguous                         655844              720217
__no_feature                        928688              962935
__not_aligned                      1053491              726882
__too_low_aQual                          0                   0
ENSRNOG00000000001                       0                   3
                       SRR2240185.fastq.gz SRR2240186.fastq.gz
__alignment_not_unique             1050839              546458
__ambiguous                         862109              435454
__no_feature                       1522026              788988
__not_aligned                      1076012              905083
__too_low_aQual                          0                   0
ENSRNOG00000000001                      17                   6
                       SRR2240187.fastq.gz SRR2240228.fastq.gz
__alignment_not_unique             1050149              903445
__ambiguous                         713154              628331
__no_feature                       1066604              972880
__not_aligned                      1444081             1270620
__too_low_aQual                          0                   0
ENSRNOG00000000001                       0                   1

> dataframe <- read.csv("merge.csv", header=TRUE)
# 没有row.names = 1参数，列表头被替换为数字
> head(dataframe)
                 gene_id SRR2190795.fastq.gz SRR2240182.fastq.gz
1 __alignment_not_unique              716726             1033461
2            __ambiguous              551241              918531
3           __no_feature              861781             1251102
4          __not_aligned             1149668             2058240
5        __too_low_aQual                   0                   0
6     ENSRNOG00000000001                   2                   5
  SRR2240183.fastq.gz SRR2240184.fastq.gz SRR2240185.fastq.gz
1              824232              879059             1050839
2              655844              720217              862109
3              928688              962935             1522026
4             1053491              726882             1076012
5                   0                   0                   0
6                   0                   3                  17
  SRR2240186.fastq.gz SRR2240187.fastq.gz SRR2240228.fastq.gz
1              546458             1050149              903445
2              435454              713154              628331
3              788988             1066604              972880
4              905083             1444081             1270620
5                   0                   0                   0
6                   6                   0                   1
```


# read.table

1. 介绍  
read.table()函数是R最基本函数之一，主要用来读取矩形表格数据。  
2. 用法  
```R
Usage
read.table(file, header = FALSE, sep = "", quote = "\"'",
           dec = ".", numerals = c("allow.loss", "warn.loss", "no.loss"),
           row.names, col.names, as.is = !stringsAsFactors,
           na.strings = "NA", colClasses = NA, nrows = -1,
           skip = 0, check.names = TRUE, fill = !blank.lines.skip,
           strip.white = FALSE, blank.lines.skip = TRUE,
           comment.char = "#",
           allowEscapes = FALSE, flush = FALSE,
           stringsAsFactors = default.stringsAsFactors(),
           fileEncoding = "", encoding = "unknown", text, skipNul = FALSE)

（1）file  
file是一个带分隔符的ASCII文本文件。  
（2）header  
一个表示文件是否在第一行包含了变量的逻辑型变量。
如果header设置为TRUE，则要求第一行要比数据列的数量少一列。
header=T表示将文件中第一行设为列名字
（3）sep  
分开数据的分隔符。默认sep=""。  
read.table()函数可以将1个或多个空格、tab制表符、换行符或回车符作为分隔符。  
（4）quote
用于对有特殊字符的字符串划定接线的字符串，默认值是TRUE(")或单引号。
（5）dec
decimal用于指明数据文件中小数的小数点。
（6）numerals
字符串类型。用于指定文件中的数字转换为双精度数据时丢失精度的情况下如何进行转换。
（7）row.names
row.names= 1表示第一列设为该行的行名。
保存行名的向量。可以使用此参数以向量的形式给出每行的实际行名。或者要读取的表中包含行名称的列序号或列名字符串。
在数据文件中有行头且首行的字段名比数据列少一个的情况下，数据文件中第1列将被视为行名称。除此情况外，在没有给定row.names参数时，读取的行名将会自动编号。
可以使用row.names = NULL强制行进行编号。
（8）col.names
指定列名的向量。缺省情况下是又"V"加上列序构成，即V1,V2,V3......

Tip:
rownames、colnames是base包中的行名、列名函数；
而row.names、col.names是read.table函数中的行名、参数
（9）as.is
该参数用于确定read.table()函数读取字符型数据时是否转换为因子型变量。当其取值为FALSE时，该函数将把字符型数据转换为因子型数据，取值为TRUE时，仍将其保留为字符型数据。其取值可以是逻辑值向量（必要时可以循环赋值），数值型向量或字符型向量，以控制哪些列不被转换为因子。
注意：可以通过设置参数 colClasses = "character"来阻止所有列转换为因子，包括数值型的列。
（10）na.strings
可选的用于表示缺失值的字符向量。
na.strings=c("-9","?")把-9和？值在读取数据时候转换成NA
（11）colClasses
用于指定列所属类的字符串向量。
（12）nrows
整型数。用于指定从文件中读取的最大行数。负数或其它无效值将会被忽略。
（13）skip
整型数。读取数据时忽略的行数。
（14）check.names
逻辑值。该参数值设置为TRUE时，数据框中的变量名将会被检查，以确保符在语法上是有效的变量名称。
（15）fill
逻辑值。在没有忽略空白行的情况下（即blank.lines.skip=FLASE），且fill设置为TRUE时，如果数据文件中某行的数据少于其他行，则自动添加空白域。
（16）strip.white
逻辑值，默认为FALSE。此参数只在指定了sep参数时有效。当此参数设置为TRUE时，数据文件中没有包围的字符串域的前边和后边的空格将会被去掉。
（17）blank.lines.skip
逻辑值，此参数值设置为TRUE时，数据文件中的空白行将被忽略。默认值为TRUE。
（18）comment.char
字符型。包含单个字符或空字符的向量。代表注释字符的开始字符。可以使用""关闭注释。
（19）allowEscapes
逻辑值。类似“\n”这种C风格的转义符。如果这种转义符并不是包含在字符串中，该函数可能解释为字段分隔符。
（20）flush
逻辑值。默认值为FALSE。当该参数值设置为TRUE时，则该函数读取完指定列数后将转到下一行。这允许用户在最后一个字段后面添加注释。
（21）stringsAsFactors
逻辑值，标记处字符向量是否需要转化为因子，默认是TRUE。

首先，明确String与Factor的区别。 
String是字符串，可用于记录琐细信息（比如发现UFO者的口头描述内容）。
Factor是因此，用于给一行记录做“分类标记”，比如人的性别factors可以设置为“男”、“女”，工作效率最高日期的factors可以是“Mon”、"Tue"，对于工作效率也可以有“high”、“low”等。
对于Factor类型属性，R语言可以自动统计数据的factor水平（level），比如，男，有多少，Mon有多少等。
stringsAsFactors = F意味着，“在读入数据时，遇到字符串之后，不将其转换为factors，仍然保留为字符串格式”。

（22）fileEncoding
字符串类型，指定文件的编码方式。如果指定了该参数，则文本数据按照指定的格式重新编码。
（23）encoding
假定输入字符串的编码方式。
（24）text
字符串类型。当未提供file参数时，则函数可以通过一个文本链接从text中读取数据。
（25）skipNul
逻辑值。是否忽略空值。默认为FALSE。


链接：https://www.jianshu.com/p/90e1d430c9ef
```
 writetable
 merge
 write.csv
# lapply
* 用法
```
lapply(X, FUN)

参数  描述
X           向量或对象
FUN     作用于x中的每个元素的函数
lapply()中的“l”代表list。lapply()和apply()之间的区别在于输出。lapply()的输出是一个列表。lapply()可以用于其他对象，比如数据帧和列表。
```
# gsub

![链接详情](https://blog.csdn.net/lztttao/article/details/82086346)  

1. 具体用法  
```
gsub(pattern, replacement, x, ignore.case = FALSE, perl = FALSE,
    fixed = FALSE, useBytes = FALSE)
```
其中pattern是要替换的字符，replacement是替换的字符，x是对应的string或string vector。ignore.case表示是否忽视大小写。

# strsplit

1. 用法
```
strsplit(x, split, fixed = F, perl = F, useBytes = F)	
```
```
* x-字符串格式向量，函数依次对向量的每个元素进行拆分。
* split-为拆分位置的字串向量，即在哪个字串处开始拆分；该参数默认是正则表达式匹配。
* fixed = T-表示是用普通文本匹配或者正则表达式的精确匹配。
* perl-其设置和perl的版本有关，表示可以使用perl语言里面的正则表达式。如果正则表达式过长，则可以考虑使用perl的正则来提高运算速度。
* useBytes-是否逐字节进行匹配，默认为FALSE，表示是按字符匹配而不是按字节进行匹配。

原文链接：https://blog.csdn.net/L_J_Kin/article/details/103870410
```

# substring

用于提取字符向量中的子串。

1. 用法： 
```R
substring(text, first, last)
参数：
text:字符向量
first:整数，要替换的第一个元素
last:整数，要替换的最后一个元素
```
2. 范例：

```R

范例1：
# R program to illustrate
# substring function
  
# Calling substring() function
substring("Geeks", 2, 3)
substring("Geeks", 1, 4)
substring("GFG", 1, 1)
substring("gfg", 3, 3)
输出：

[1] "ee"
[1] "Geek"
[1] "G"
[1] "g"


范例2：
# R program to illustrate
# substring function
  
# Initializing a string vector
x <- c("GFG", "gfg", "Geeks")
  
# Calling substring() function
substring(x, 2, 3)
substring(x, 1, 3)
substring(x, 2, 2)
输出：

[1] "FG" "fg" "ee"
[1] "GFG" "gfg" "Gee"
[1] "F" "f" "e"
```
[[]]这个符号主要用于列表(list)中的元素的引用，因为列表的元素可能有好几层，多一个[]就是调用更下一层


# nchar

nchar()R编程中的方法用于获取字符串的长度。

1. 用法： 
```
nchar(string)
返回：返回字符串的长度。
```

2. 范例1：

```
# R program to calculate length of string
  
# Given String
gfg <- "Geeks For Geeks"
  
# Using nchar() method
answer <- nchar(gfg)
  
print(answer)
输出：

[1] 15
```

# function
* 概括
```
function_name <- function(arg_1, arg_2, ...) {
  Function body 
}

function_name:       函数名字
arg_1, arg_2, ...：  参数
Function body：    函数主题，用于定义函数的作用
返回值 ： 函数的返回值是要评估/计算的函数体中的最后一个表达式
```

* 举例
```
new1.function<- function(a){
  for(i in 1:a){
  b = i^2
  print(b)
  }
    }
new1.function(3)
```
# paste
许多字符串使用 paste() 函数来组合。它可以将任意数量的参数组合在一起。

* 语法
粘贴（paste）函数的基本语法是：
`paste(..., sep = " ", collapse = NULL)`
以下是所使用的参数的说明：  

```
... - 表示要组合的任何数量的参数。
sep - 表示参数之间的分隔符。它是任选的。
collapse - 用于消除两个字符串之间的空间。但不是在一个字符串的两个词的空间。
```
* 示例
```
a <- "Hello"
b <- 'How'
c <- "are you? "

print(paste(a,b,c))

print(paste(a,b,c, sep = "-"))

print(paste(a,b,c, sep = "", collapse = ""))
当上面的代码执行时，它产生以下结果：

[1] "Hello How are you? "
[1] "Hello-How-are you? "
[1] "HelloHoware you? "
```

# dplyr  

dplyr是R中专门用于数据处理的包。更具体功能包括：  


```
select() 从数据中选择列
filter() 数据行的子集
group_by() 汇总数据
summarise() 汇总数据（计算汇总统计信息）
arrange() 排序数据
mutate() 创建新变量
```

* mutate()

```
使用时，通常你只需要指定3项内容：

您要修改的数据框的名称
您将创建的新变量的名称
您将分配给新变量的值
```
1. mutate()的第一个参数就是数据框，然后就是新变量名=旧变量的某种新式。就是说你可以轻松地以数据框中的原有变量生成新变量。
2. mutate()的第二个参数是“名称-值”对，就是说我们在创建变量时新变量需要一个名称，但是它也需要一个分配给该名称的值。因此，当使用mutate时，您需要提供名称和新值…即名称/值对。
 
* 举例：
```r
print(auto_specs)是一个数据框；

现在需要一个新变量叫做hp_to_weight，这个变量是原先horsepower / weight两个变量的比值:

auto_specs_new <- mutate(auto_specs, hp_to_weight = horsepower / weight)
print(auto_specs_new)

会在本来数据表最后一列加上hp_to_weight及其数值
```
