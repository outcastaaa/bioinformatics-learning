# DESeq2差异表达分析
## 介绍  

DESeq2是一个为高维计量数据的归一化、可视化和差异表达分析而设计的一个R语言包。是目前差异表达分析方面最常用的R包。它通过经验贝叶斯方法(empirical Bayes techniques)来估计对数倍数变化(log2foldchange）和离差的先验值，并计算这些统计量的后验值。

## 安装DESeq2  

```R
if (!require("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

BiocManager::install("DESeq2")
```
## DEseq2包自带的rlog和vst函数  
1. 意义：  为了进行样本相关性分析，需要先把原始`read count`进行取对数转化进行校正，DEseq2包自带的rlog和vst函数（全名为variance stabilizing transformation），它们消除了方差对均值的依赖，尤其是低均值时的高log counts的变异。
2. 注：  
但是在DESeq2包中实际上已经有了归一化的方法，rlog和vst，在使用的需要根据样本量的多少来选择方法。样本量少于30的话，选择rlog，多于30的话，建议选择vst。
3. VST转换（方差稳定转换variance stabilizing transformation）  

* 目的:  to simplify considerations in graphical exploratory data analysis or to allow the application of simple regression-based or analysis of variance techniques.应用基于简单的基于回归或方差分析技术
* 概括：    
选择方差稳定变换的目的是找到一个简单的函数 ƒ 应用于数据集中的值 x，以创建新值 y = ƒ（x），使得值 y 的变异性与其平均值无关。在DESeq里就是，f是受平均值影响的raw count，找一个x，去避免y的方差收到一些极端值的影响。

4. rlog转换（规范化的log转换）  


函数rlog代表正则化log转换，通过拟合一个模型，将每个样本的一个项和一个根据数据估计的系数的先验分布进行拟合，将原始计数数据转换为log2尺度。这与DESeq和nbinomWaldTest所使用的对数倍数变化是相同的收缩（有时称为正则化或调节）。结果数据包含的元素定义如下：


```
\ [\ log_2（q_ {ij}）= \ beta_ {i0} + \ beta_ {ij} \]
```


其中\（q_ {ij} \）是与基因i和样本j的片段的预期真实浓度成比例的参数（参见下面的公式），\（\ beta_ {i0} \）是不经历收缩的截距，和\（\ beta_ {ij} \）是基于整个数据集上的色散平均趋势向零收缩的样本特定效应。这种趋势典型地表现为低计数的高分散性，因此这些基因表现出更高的收缩率。请注意，由于\（q_ {ij} \）表示大小因子\（s_j \）被分开之后的平均值\（\ mu_ {ij} \）的部分，因此很明显rlog转换具有固有的考虑到测序深度的差异。如果没有先验，这个设计矩阵将导致一个非独特的解决方案，然而，在非截距测试中增加一个先验的解决方案可以找到一个独特的解决方案。

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
# 同一个基因不同样本的count（一横行）加起来大于0

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
 #* row.names: NULL或单个整数或字符串，指定某列用作行名，或者一个字符或整型向量用作数据框的行名。
 ```

这两个信息可以理解为告诉差异分析函数是要分析哪些变量间的差异.

2. 开始进行差异表达分析  

```R
dds <- DESeqDataSetFromMatrix(countData = countData, colData = colData, design = ~ condition)
```