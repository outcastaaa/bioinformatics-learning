# fastqc
## 一、介绍

1. 目的：  
FastQC旨在为来自高通量测序管道的原始序列数据提供`一种简单的质量控制检查方法`。它提供了一组模块化的分析，您可以使用它来快速了解您的数据是否存在任何问题，应该在进行进一步分析之前意识到这些问题。  
FastQC aims to provide a QC report which can spot problems which originate either in the sequencer or in the starting library material.


2. 主要功能
```
①　从BAM、SAM或FastQ文件(任何变体)导入数据
②　提供一个快速概览，告诉您哪些领域可能存在问题
③　总结图表和表格，快速评估您的数据
④　将结果导出到基于HTML的永久报告
⑤　离线操作，允许在不运行交互式应用程序的情况下自动生成报告
```
FastQC可以在两种模式中运行。  
它既可以作为`独立的交互式应用程序运行`，用于对少量FastQ文件进行即时分析;   
也可以以`非交互式模式运行`，在这种模式下，它适合集成到一个更大的分析管道中，用于对大量文件进行系统处理。

## 二、下载与安装
使用brew安装  
```
brew install fastqc
```
出现下列结果表示安装成功：
```
xuruizhi@DESKTOP-HI65AUV:~$ brew install fastqc
HOMEBREW_BREW_GIT_REMOTE set: using https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git for Homebrew/brew Git remote.
HOMEBREW_CORE_GIT_REMOTE set: using https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git for Homebrew/core Git remote.
Running `brew update --auto-update`...
Warning: fastqc 0.11.9_1 is already installed and up-to-date.
To reinstall 0.11.9_1, run:
  brew reinstall fastqc
```
## 三、一些基本操作


1. 打开文件   

要以交互方式打开一个或多个Sequence文件，只需运行程序并选择`File > open`。然后可以选择想要分析的文件。新打开的文件将立即出现在屏幕顶部的选项卡中。由于这些文件的大小，打开它们可能需要几分钟的时间。  
FastQC操作的是一个排队系统，`每次只打开一个文件，新文件将等待，直到现有文件被处理`。

2. 格式问题   

FastQC支持以下格式的文件:

* FastQ (all quality encoding variants)
* Casava FastQ files*
* Colorspace FastQ
* GZip compressed FastQ
* SAM
* BAM
* SAM/BAM Mapped only (normally used for colorspace data)  

Casava fastq  格式与常规  fastq  格式相同，不同之处在于:  

① Casava fastq 数据通常被拆分为单个样本的多个文件。在这种模式下，程序将合并样本组中的文件，并为每个样本提供一个单独的报告。  
② Casava fastq文件包含已被标记为要删除的、质量较差的序列。在该模式下，程序将从报告中删除这些标记的序列。    
③ 默认情况下，FastQC会尝试从输入文件的名称猜测文件格式。以. SAM或. BAM结尾的文件将被打开为SAM/BAM文件(使用所有序列、mapped和unmapped)，其他所有文件将被视为FastQ格式。  
如果您想覆盖此检测并手动确定文件格式，那么您可以使用文件选择器中`file chooser`的下拉文件过滤器`drop down file filter`来选择要加载的文件类型。您需要使用下拉选择器`drop down selector`使程序使用`Mapped BAM`或`Casava`文件模式，因为这些不会被自动选择。  


3. 评估结果  

FastQC中的分析是通过一系列的分析模块来完成的。主交互页面显示的左手边或HTML报告的顶部显示了所运行模块的总结，并快速评估模块的结果是`完全正常(绿色勾号)`、`轻微异常(橙色三角形)`或`非常异常(红色叉号)`。

尽管分析结果似乎给出了通过/失败的结果，但必须在您期望从库中获得什么进行评估。  


  **!  FastQC所关注的“正常”样本是随机和多样化的。有些实验可能会产生一些在某些方面有偏见的库。因此，您应该将总结计算视为指向您应该集中注意力的地方的指针，并理解为什么您的库可能看起来不是随机和多样化的。 !**



4. 保存报告  


除了提供一个交互式的报告，FastQC还可以选择创建一个`HTML版本的报告`，作为一个更永久的记录。该HTML报告也可以通过在非交互模式下运行FastQC直接生成。  


要创建一个报告，只需从主菜单中选择`File > Save report`。默认情况下，将使用fastq文件的名称创建一个报告，并将_fastqc.html追加到末尾。当选择菜单选项时，将为任何激活的文件选项卡创建报告。  
 

所保存的HTML文件是一个自包含的文档，所有图形都嵌入其中，因此您可以分发这个文件。与HTML文件并列的是一个zip文件(与HTML文件同名，但在末尾添加了.zip)。该文件包含报告中的图形作为独立文件，但也包含易于解析的数据文件，以便对QC报告所依据的原始数据进行更详细和自动化的评估。


## 四、每个模块  




①　Basic Statistics模块生成一些简单的组合统计信息  

```
* Filename 文件名:被分析的文件的原始文件名
* File type 文件类型:表示文件是否包含实际的基本调用base calls，还是必须转换为base calls的colorspace 数据
* Encoding:表示在此文件中的quality values 对应的ASCII编码是哪个。
* Total sequence 总序列:所处理的序列总数的计数。报告了两个值，实际值actual和估计值estimated。现在这些都是一样的。在未来，我们可能只分析序列的一个子集，并估计总数量，以加快分析速度，但由于我们发现有问题的序列并不是均匀分布在一个文件中，我们现在禁用了这个功能。
* Filtered sequences 过滤序列:如果以Casava模式运行，标记要过滤的序列将从所有分析中删除。这里将报告被删除的这些序列的数量。上面的总序列计数将不包括这些过滤后的序列，而将包括用于其余分析的实际序列数。
* Sequence length 序列长度:表示集合中最短和最长序列的长度。如果所有序列长度相同，则只报告一个值。
* %GC:所有序列中所有碱基的总 %GC
```
-----------------------------------

②　 Per base sequence quality每个碱基序列质量  


1). 意义：显示了 FastQ 文件中每个位置的所有碱基的 quality values范围的概览。  

2). 图片  
![tu](../pictures/%E5%9B%BE%E7%89%871.png)  

为每个位置绘制一个` BoxWhisker `类型图。   
`x轴代表碱基在每一条read上的位置，y值Q = -10*log10（error P），代表同一时间读取了每个plot上结合的每一个read的第n个碱基，每一个y值实际上是该flow cell上某一时刻结合的所有碱基的quality scores`

该图的元素如下：   
```
中心红线是中位数；
黄色box代表inter-quartile四分位数范围（25-75%） ；
上下whiskers代表 10% 和 90% 点 ；
蓝线代表 平均质量 ；
图表上的 y 轴显示quality scores。 分数越高，base call越好。 

图表的背景将 y 轴分为good quality calls (green), calls of reasonable quality (orange), and calls of poor quality (red)。 大多数平台上的calls质量会随着运行的进行而下降，因此通常会看到base calls在读取结束时落入橙色区域。
```
`随着程序进行，质量下降`  

有许多不同的方法可以在 FastQ 文件中编码quality scores。  FastQC 尝试自动确定使用了哪种编码方法，但在一些非常有限的数据集中，它可能会错误地猜测这一点（具有讽刺意味的是，只有当您的数据普遍非常好时！）。 图的标题将描述 FastQC 认为您的文件使用的编码。
如果您的输入是未记录quality scores的 BAM/SAM 文件，则不会显示此模块的结果.   

3). Warning   

如果任何碱基的下四分位数小于 10，或任何碱基的中位数小于 25，将发出警告。 

4). Failure   

如果任何碱基的下四分位数小于 5 ，或任何碱基的中位数小于 20，此模块将显示失败。  

5). Warning 原因  

a. 长时间运行质量下降：  
* chemistry；伴随着 adapter read-through问题  
* 解决办法：trimming; a combined adapter and quality trimming step  

详细解释：  
```
  此模块中出现警告和失败的最常见原因是长时间运行期间质量普遍下降。 
  一般来说，测序化学会随着读取长度的增加而降低，对于长时间运行，您可能会发现运行的总体质量下降到触发警告或错误的水平。
  如果库的质量下降到低水平，那么最常见的补救措施是执行质量修整quality trimming ，其中根据平均质量截断读取。 对于发生此类下降的大多数库，您通常会同时遇到 adapter read-through问题，因此通常采用a combined adapter and quality trimming step。

  通常情况下，在序列的起始和结束部分可能出现质量较差的情况，对于最初测序的部分数据，测序仪直接使用默认参数进行base calling, 这部分碱基的质量一般， 然后会利用这部分数据去调整base calling的参数设置，以符合真实的数据，在之后的测序中，用调整后的参数进行base caling, 此时碱基的质量会更好，所以会观察到，在开头部分存在碱基质量上升的趋势；随着测序反应的进行，酶活性等因素降低，会导致测序质量变差，所以在结尾部分会观察到碱基质量降低的趋势。
```

[原文链接](https://blog.csdn.net/weixin_43569478/article/details/108079243)




b. 运行早期的短暂质量损失:   a short loss of quality earlier in the run  
发现办法：可通过per-tile quality plot 来发现此类错误   

解决办法：  不建议trimming；后续mapping或assembly期间屏蔽碱基mask bases
```
  另一种可能性是，由于运行早期的短暂质量损失a short loss of quality earlier in the run而触发警告/错误，然后恢复以产生稍后的高质量序列。 
  如果运行出现暂时性问题（例如，气泡通过流通池flowcell），则可能会发生这种情况。 您通常可以通过查看per-tile quality plot 来查看此类错误。 在这些情况下，不建议trimming，因为它会删除以后的良好序列，但需要考虑在后续mapping或assembly期间屏蔽碱基mask bases。
```


c.  库具有不同长度的reads  

解决办法：
提前通过 序列长度分布模块 sequence length distribution module  结果来检查有多少序列会触发错误
```
如果您的库具有不同长度的读取，那么您会发现此模块会触发警告或错误，因为给定碱基范围的覆盖率非常低。 在提交任何操作之前，通过查看序列长度分布模块 sequence length distribution module 结果来检查有多少序列会触发错误。
```
-----------------------------------    

③　Per sequence quality scores每个序列质量得分   

1).  意义：Per sequence quality scores 报告允许查看序列子集是否具有普遍的低quality values。通常情况下，序列子集的质量普遍较差，通常是因为它们成像不佳（在视野边缘等），但是这些应该只占总序列的一小部分。  

2).  图片：  
![tu](../pictures/%E5%9B%BE%E7%89%872.png)  

每一个Base都有一个`quality score`。该图以read划分，每一个`read`都有一个`mean quality score`，x值为`mean quality score`，y值为`该质量值下的reads数目`，对应到图中绘制出一条单峰曲线  

如果一次运行中的的大部分序列总体质量较低，则这可能表明存在某种系统问题 - 可能只是运行的哪一个环节出问题了（例如流通池的一端）。
    如果您的输入是未记录quality scores的 BAM/SAM 文件，则不会显示此模块的结果。  
3) Warning  
如果最常观察到的平均quality（即峰值）低于 27，则会发出警告 - 这相当于 0.2% 的错误率。  
4)Failure  
如果最常观察到的平均quality（即峰值）低于 20 - 这相当于 1% 的错误率，此模块将显示失败。  
5)Warning 原因  
该模块通常相当强，此处的错误通常表明运行中质量普遍下降。 对于长期运行，这可以通过quality trimming来缓解。 如果看到双峰或复杂分布，则应根据per-tile qualities对结果进行评估，因为这可能表明序列子集质量损失的原因。  

-----------------------------------


④　 Per base sequence content每种碱基的含量  
1). 意义：Per Base Sequence Content绘制出文件中每个碱基位置的比例，其中四个正常 DNA 碱基中的每一个都被调用  
2). 图像：  
![tu](../pictures/%E5%9B%BE%E7%89%873.png)  

图中，以`某一个时刻flow cell上读取的所有碱基为一个单位`，x轴代表`每个碱基在read上的位置`，y值为`各种碱基所占百分比`。  
理论上来说，四种碱基应该每个出现的概率为25%，但是因为具有物种差异性或者其他原因，可能会出现bias，但是差别不大，应当都平行于X轴   

在随机库中，`期望一个 sequence run 的不同碱基比例之间几乎没有差异，因此该图中的线应该彼此平行`。 每个碱基的相对数量应该反映基因组中这些碱基的总量，但无论如何它们不应该彼此之间存在巨大的不平衡。  

某些类型的文库通常在读取开始时，会产生有偏差的序列组成。 `使用hexamers 构建的文库（包括几乎所有的 RNA-Seq 文库）`和`使用转座酶片段化的文库`在读取开始位置都有内在偏差。 这种偏差与绝对序列无关，而是在读数的 5' 末端提供了许多不同 K-mer 的富集。 虽然这是一个真正的技术偏差，但它不可以通过trimming来纠正的，并且在大多数情况下不会对下游分析产生不利影响。  


3). Warning  

如果 A 和 T 或 G 和 C 之间的差异在任何位置大于 10%，此模块会发出警告。  

4). Failure  

如果 A 和 T 或 G 和 C 之间的差异在任何位置大于 20%，则此模块将失败。  

5). Warning原因  

a.overrepresented sequence：  
如果有任何证据表明样本中存在`过度表达的序列`，例如 adapter dimers 引物二聚体或 rRNA，那么这些序列可能会bias整体的组成。当部分位置碱基的比例出现bias时，即四条线在某些位置纷乱交织，往往提示我们有overrepresented sequence的污染。

b.有偏差的片段化：  

任何基于随机六聚体hexamers连接或通过标记生成的文库 理论上都应具有良好的序列多样性，但经验表明，这些文库在`每次运行的前 12bp 左右总是存在选择偏差`。 这是由于随机adapter的选择有偏差，但并不代表任何单独的有偏差的序列。 几乎所有的 RNA-Seq 文库都会因为这种偏差而无法通过该模块，但这不是可以通过处理解决的问题，而且它似乎`不会对测量产生不利影响`。  
一般测序的时候，刚开始测序仪状态不稳定，很可能出现严重分离的情况。像这种情况，即使测序的得分很高，也需要cut开始部分的序列信息，一般像这种情况，会`cut前面5-10bp`。  


c. 有bias的组合文库：  

一些文库在其序列组成方面天生就有bias。  
 * 最明显的例子是`用亚硫酸氢钠处理的文库`，然后将大部分C转化为T，这意味着碱基组成几乎没有C，因此会引发错误，尽管这是完全正常的；  
 * 比如`植物基因组，本来AT就比GC要高`。  
 * `使用hexamers 构建的文库（包括几乎所有的 RNA-Seq 文库）`和`使用转座酶片段化的文库`在读取开始位置都有内在偏差。  
 当所有位置的碱基比例一致的表现出bias时，即四条线平行但分开，往往代表文库有bias (建库过程或本身特点)，或者是测序中的系统误差。

d. 经过aggressivley adapter修剪的文库:  

对于那种类型的文库如果您正在分析一个经过aggressivley adapter修剪的文库，那么自然会在读取结束时引入组成偏差，因为恰好匹配短片段adaptor的序列被删除，只留下不匹配的序列匹配。 因此，经过aggressivley adapter修剪的文库末尾的组成突然偏差可能是不准确的。  




 
