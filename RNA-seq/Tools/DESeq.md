# DESeq2差异表达分析
## 介绍  

DESeq2是一个为高维计量数据的归一化、可视化和差异表达分析而设计的一个R语言包。是目前差异表达分析方面最常用的R包。它通过经验贝叶斯方法(empirical Bayes techniques)来估计对数倍数变化(log2foldchange）和离差的先验值，并计算这些统计量的后验值。

## 安装DESeq2  

```R
if (!require("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

BiocManager::install("DESeq2")
```
## 使用流程  

### 1. 读入和处理数据  

DESeq2要求输入的数据必须是没有标准化的raw count，第i行第j列代表着在样本j分配到featrue(i)
的reads数或fragemnts数（feature可以是基因、转录本、ChIP-seq等NGS测的对应区段bin、定量质谱中的某肽段序列）  

链接：https://www.jianshu.com/p/88511070e2dd  


* 加载包  
```R
library(DESeq2)   
library(pheatmap)  # 用于作热图的包
library(ggplot2)   # 用于作图的包
```
* 对数据形式有要求，需要：  

|      gene   |  sample1  |  sample2  |
| ----------- | --------- | --------- |
| Gene名称1    | count   | count    |
| Gene名称2    | count    | count    |


整理数据：  

① 去除低表达的基因   
```R
countdata <- countdata[rowSums(countdata) > 0,]

# 或者通过加和平均值来判断
countData <- countData[rowMeans(countData)>1,]
```

② 去除附带多余的信息，以及会出现一些无效的行  

```R
# 去除前面5行
countdata <- dataframe[-(1:5),]

#将ID的版本号去除[看个人情况是否去除]
# 得到行的名
row_names <- row.names(countdata)
# 开始替换
name_replace <- gsub("\\.\\w+","", row.names(countdata))
row.names(countdata) <- name_replace

# 查看数据
head(countdata)  
```
### 2.差异表达分析

1.  一些需要提前构建的信息  

* 分组：我们分析的数据有4个对照组和4个实验组。分组信息构建为condition向量。  

SRR228——PBScontrol_2；SRR187——PBScontrol_1；SRR185——lowdoseDEN_1；SRR186——lowdoseDEN_2；
SRR184——lowdoseDEN_AM063_2；SRR183——lowdoseDEN_AM063_1；SRR182——lowdoseDEN_AM095_2；SRR795——lowdoseDEN_AM095_1。

```R
 condition <- factor(c(rep("control",4),rep("treatment",4)))
 ```  


样本信息colData，每一行对应一个样本，行名与countData(即，统计出来的每个样本对应多少基因reads的count文件中横表头)的样本顺序一一对应，列为各种分组信息。  

```R
 colData <- data.frame(row.names=colnames(countData), condition)
 ```

这两个信息可以理解为告诉差异分析函数是要分析哪些变量间的差异.

2. 开始进行差异表达分析  

```R
dds <- DESeqDataSetFromMatrix(countData = countData, colData = colData, design = ~ condition)
```