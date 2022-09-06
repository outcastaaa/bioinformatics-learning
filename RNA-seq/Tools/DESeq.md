# DESeq2差异表达分析

[该文章](https://www.jianshu.com/p/2689e9a1d10c)写的非常细致全面  



# 介绍  

DESeq2是一个为高维计量数据的归一化、可视化和差异表达分析而设计的一个R语言包。是目前差异表达分析方面最常用的R包。它通过经验贝叶斯方法(empirical Bayes techniques)来估计对数倍数变化(log2foldchange）和离差的先验值，并计算这些统计量的后验值。

# 安装DESeq2  

```R
if (!require("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

BiocManager::install("DESeq2")
```


# DEseq2包自带的rlog和vst函数  

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



1.  构建DESeq2对象  

（1）从SummarizedExperiment对象构建DESeqDataSet对象  

```
dds <- DESeqDataSet( se, design = ~ cell + dex) 
```

* se为RangedSummarizedExperiment对象，行信息rowRanges(se)为基因区间，列信息colData(se)为样本信息（四种细胞系，每一个cell lines都有对照组与dex处理组），实验数据assays(se)为原始的read counts。   本流程中为（两组对照，三组实验各有两个重复）。  

* dds为DESeqDataSet对象，dds与se的区别是se的assay slot被DESeq2的counts accessor function代替。   

* dds包含设计公式design formula，design(dds)。实验设计为cell+dex，希望检测对于不同细胞，地塞米松处理的效果。本流程实验设计为 treatment，希望检测对于小鼠肝脏组织的不同处理的效果。代码应改为  
```
dds <- DESeqDataSet( se, design = ~ treatment) 
```  

（2）从表达矩阵countData和样品信息colData构建DESeqDataSet对象    
```
dds <- DESeqDataSetFromMatrix( countData = countdata, colData = coldata, design = ~ Group) 
```
* 表达矩阵countData，行为基因，列为样本，表达量必须是非负整数。  
也就是经过数据处理的raw count，每个样本的每个基因都对应了一个count。  


* 样本信息colData，每一行对应一个样本，`行名与countData的样本顺序一一对应`(必须完全一致，否则会报错)，列为各种分组信息。  

```
例如:

> coldata
                     state   condition treatment
SRR2190795 Liver cirrhosis DEN + AM095 treatment
SRR2240182 Liver cirrhosis DEN + AM095 treatment
SRR2240183 Liver cirrhosis DEN + AM063 treatment
SRR2240184 Liver cirrhosis DEN + AM063 treatment
SRR2240185 Liver cirrhosis         DEN treatment
SRR2240186 Liver cirrhosis         DEN treatment
SRR2240187 Healthy control         PBS   control
SRR2240228 Healthy control         PBS   control
```

* 设计公式通常格式为`~ batch批次影响 + conditions处理条件影响`，batch和conditions都是colData的一列，是因子型变量。为了方便后续计算，`最为关注的分组信息放在最后一位`。  

如果记录了样本的批次信息，或者其它需要抹除的信息可以定义在design参数中，在下游回归分析中会根据design formula来估计batch effect的影响，并在下游分析时减去这个影响。这是处理batch effect的推荐方式。  

在模型中考虑batch effect并没有在数据矩阵中移除bacth effect，如果下游处理时确实有需要，可以使用limma包的removeBatchEffect来处理。  


* 默认情况下，R会根据字母表顺序排列因子型变量，排在最前面的因子作为对照。设置对照：  

colData$Group <- relevel(colData$Group, ref=“WTF”)  

或 colData$Group <- factor(colData$Group, levels = c(“WTF”, ”WTM”, ”MF”, ”MMF”))  

* 多个因子——designs with multiple variables, e.g., ~ group + condition,  

and   

因子之间有相互作用——designs with interactions (answering: is the condition effect different across genotypes?) , e.g., ~ genotype + treatment + genotype:treatment.   

默认情况下，此包中的函数将使用公式中的最后一个变量来构建结果表和绘图。  


* design(dds) <- value. value, 用于估计色散和拟合负二项式 GLM 的公式。


2. [选择性过滤低丰度数据]  

构建dds之前： 
```
countData <- count[apply(count, 1, sum) > 1 , ] 

```

构建dds之后：
```
dds <- dds[rowSums(counts(dds)) > 1, ] 
```
* 在独立筛选（independent filtering）中，DESeq2可以去掉在所有样品中平均表达量CPM不大于min.CPM的基因，以减少假阴性。  

* EdgeR是保留在2个或更多样品中表达量大于min.CPM的基因。  

*  可以尝试不同的cutoff，以获得最佳效果   

[选择性指定factor levels]    

R 将基于字母顺序默认参考水平，但实际通常是根据对照组作为参考水平。因此有必要时要设置  
```
dds$condition <- factor(dds$condition, levels=c('untreated', 'treated'))
# 也可以直接指定
dds$condition <- relevel(dds$condition, ref='untreated')
# 如果有时对dds取子集时，导致某些水平不含数据，那么这个水平就可以丢弃
dds$condition <- droplevels(dds$condition)
```

3. 主成分分析[选择性采取该步骤，只用于看样本相关性]

PCA的两种数据转化方法  

**为什么要转换？** 

为了确保所有基因有大致相同的贡献。     

对于RNA-seq raw counts，方差随均值增长。如，假设值 x 是来自不同泊松分布的实现：即每个分布μ具有不同的平均值。然后，由于对于泊松分布，方差与均值相同，因此方差随均值而变化。    

如果直接用size-factor-normalized read counts：counts(dds, normalized=T) 进行主成分分析，结果通常只取决于少数几个表达最高的基因，因为它们显示了样本之间最大的绝对差异。  


为了避免这种情况，一个策略是采用the logarithm of the normalized count values plus a small pseudocount：log2(counts(dds2, normalized=T) +1)。就是加一个常数再取对数。但是这样，有很低counts的基因将倾向于主导结果。  


作为一种解决方案，DESeq2为counts数据提供了stabilize the variance across the mean的转换。其中之一是regularized-logarithm transformation or rlog2。  

对于counts较高的基因，rlog转换可以得到与普通log2转换相似的结果。然而，对于counts较低的基因，所有样本的值都缩小到基因的平均值。  

用于绘制PCA图或聚类的数据可以有多种：counts、CPM、log2(counts+1)、log2(CPM+1)、vst、rlog等。  


（1）方差稳定变换，The variance stabilizing transformation   

```
vsd <- vst(object=dds,blind=T) 
```
* 样本信息的列名names(colData(vsd))多了1列sizeFactor，colData(vsd)$sizeFactor  

* 基因信息的列名names(rowData(vsd))多了4列    

* vst函数快速估计离散趋势并应用方差稳定变换。该函数从拟合的离散-均值关系中计算方差稳定变换(VST)，然后变换count data（除以标准化因子），得到一个近似为同方差的值矩阵（沿均值范围具有恒定的方差）。许多常见的多维数据探索性分析方法，例如聚类或PCA，对于同方差的数据表现良好。  

**数据集小于30个样品可以用rlog，数据集大于30个样品用vst，因为rlog速度慢。**  


（2）正则化对数变换，The regularized-logarithm transformation  

```
rld <- rlog(object=dds,blind=F)
```

* 样本信息colData names多了1列sizeFactor，和vsd的sizeFactor相同  

* 基因信息rowData names多了7列  

* rlog函数将count data转换为log2尺度，以最小化有small counts的行的样本间差异，并使library size标准化。`rlog在size factors变化很大的情况下更稳健`。  


（3）用法  


blind，转换时是否忽视实验设计。blind=T，不考虑实验设计，用于样品质量保证（sample quality assurance，QA）。blind=F，考虑实验设计，用于downstream analysis。  

 (4) intgroup：分组  
 PCA详情见[该网址](https://www.jianshu.com/p/b7e55bacbede)    


interesting groups: a character vector of names in colData(x) to use for grouping。也就是在构建dds的时候的分组，实际上这个分组最后影响的是PCA图中的颜色，但是并不影响PCA图中各个样本的位置。换句话说，PCA降维的是时候并不会考虑这个分组。  


 (5) ntop：用于主成分分析的时候基因数  

官方解释：number of top genes to use for principal components, selected by highest row variance。也就是说PCA分析的时候，是根据基因表达的counts值来的。如果给出了ntop，那么意思就是只选出表达量高的这几个基因去计算。没想通作者为什么设置这个，我觉得应该是先筛选出差异的基因，然后再设置这个数会比较好，这样就是看筛选出的基因是不是符合预期。反正感觉ntop目前来说用的不是很多。  

 (6) returnData：返回PC1和PC2的dataframe  

官方解释：should the function only return the data.frame of PC1 and PC2 with intgroup covariates for custom plotting (default is FALSE)。注意只返回PC1和PC2，其他的成分不返回，而且还返回分组情况和样本名。是这种格式：
```
              PC1         PC2 group condition  name
X11_T  23.7381223  -3.9537594    II        II X11_T
X77_T  -1.7273395  29.0222667    II        II X77_T
X14_T -14.2359870   1.8616294    II        II X14_T
X76_T  -0.2251326  27.9834964    II        II X76_T
```
这个参数的作用就是当你的PCA图需要加样本名的时候，也就是你希望在PCA图上知道哪个点是哪个样本，你就需要导出这个, 设置returnData=T。

* 上述过程全部代码：   
```
#构建dds
dds <- DESeqDataSetFromMatrix(countData=indata, colData = state, design= ~ condition )
#归一化，因为样本量超过了30，因此用vst
vsd <- vst(dds, blind = FALSE)
#返回样本名和分组
pcaData <- plotPCA(vsd, intgroup=c("condition"),returnData = T)
#这里按照condition排序了，原因见下。
pcaData <- pcaData[order(pcaData$condition,decreasing=F),]
#知道每一个组有多少样本
table(pcaData$condition)
# II III  IV 
# 20  11  10 
#根据上面的结果，设置每一个组的数量，方便加颜色；这一步是把每一个样本根据分组情况画出来，效果见图1
plot(pcaData[,1:2],pch = 19,col= c(rep("red",20),rep("green",11),rep("blue",10)),
     cex=1)
#加上样本名字，效果见图2
text(pcaData[,1],pcaData[,2],row.names(pcaData),cex=1, font = 1)
#加上图例，效果见图3
legend(-70,43,inset = .02,pt.cex= 1.5,title = "Grade",legend = c("II", "III","IV"), 
       col = c( "red","green","blue"),pch = 19, cex=0.75,bty="n")
```


4. DESeq2的标准化方法







































* 分组：我们分析的数据有2个对照组和6个实验组。分组信息构建为condition向量。  

SRR228——PBScontrol_2；  SRR187——PBScontrol_1；  SRR185——lowdoseDEN_1；  SRR186——lowdoseDEN_2；  
SRR184——lowdoseDEN_AM063_2；SRR183——lowdoseDEN_AM063_1；  SRR182——lowdoseDEN_AM095_2；SRR795——lowdoseDEN_AM095_1。  

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