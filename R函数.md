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

# read.table
# write.table
# merge
# write.csv
# lapply
# gsub

