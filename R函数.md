- [R函数](#R函数)
    - [data.frame](#data.frame)
    - [Bash用法](#Bash用法)
    - [Cat用法](#Cat用法)
    - [文件/etc/apt/sources.list](#文件/etc/apt/sources.list)
    - [shell语句中各种符号](#shell语句中各种符号)
    - [条件判断语句if-then-fi](#条件判断语句if-then-fi)
    - [sed-i命令](#sed-i命令)
    - [export命令](#export命令)
    - [Mkdir命令](#Mkdir命令)
    - [For循环语句](#For循环语句)
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




# read.table
# write.table
# merge
# write.csv
# lapply
# gsub

