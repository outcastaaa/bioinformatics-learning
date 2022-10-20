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
具体使用方法：  

```
xuruizhi@DESKTOP-HI65AUV:~$ fastqc --help

            FastQC - A high throughput sequence QC analysis tool

SYNOPSIS

        fastqc seqfile1 seqfile2 .. seqfileN

    fastqc [-o output dir] [--(no)extract] [-f fastq|bam|sam]
           [-c contaminant file] seqfile1 .. seqfileN

DESCRIPTION

    FastQC reads a set of sequence files and produces from each one a quality
    control report consisting of a number of different modules, each one of
    which will help to identify a different potential type of problem in your
    data.

    If no files to process are specified on the command line then the program
    will start as an interactive graphical application.  If files are provided
    on the command line then the program will run with no user interaction
    required.  In this mode it is suitable for inclusion into a standardised
    analysis pipeline.

    The options for the program as as follows:

    -h --help       Print this help file and exit

    -v --version    Print the version of the program and exit

    -o --outdir     Create all output files in the specified output directory.
                    Please note that this directory must exist as the program
                    will not create it.  If this option is not set then the
                    output file for each sequence file is created in the same
                    directory as the sequence file which was processed.

    --casava        Files come from raw casava output. Files in the same sample
                    group (differing only by the group number) will be analysed
                    as a set rather than individually. Sequences with the filter
                    flag set in the header will be excluded from the analysis.
                    Files must have the same names given to them by casava
                    (including being gzipped and ending with .gz) otherwise they
                    won't be grouped together correctly.

    --nano          Files come from nanopore sequences and are in fast5 format. In
                    this mode you can pass in directories to process and the program
                    will take in all fast5 files within those directories and produce
                    a single output file from the sequences found in all files.

    --nofilter      If running with --casava then don't remove read flagged by
                    casava as poor quality when performing the QC analysis.

    --extract       If set then the zipped output file will be uncompressed in
                    the same directory after it has been created.  By default
                    this option will be set if fastqc is run in non-interactive
                    mode.

    -j --java       Provides the full path to the java binary you want to use to
                    launch fastqc. If not supplied then java is assumed to be in
                    your path.

    --noextract     Do not uncompress the output file after creating it.  You
                    should set this option if you do not wish to uncompress
                    the output when running in non-interactive mode.

    --nogroup       Disable grouping of bases for reads >50bp. All reports will
                    show data for every base in the read.  WARNING: Using this
                    option will cause fastqc to crash and burn if you use it on
                    really long reads, and your plots may end up a ridiculous size.
                    You have been warned!

    --min_length    Sets an artificial lower limit on the length of the sequence
                    to be shown in the report.  As long as you set this to a value
                    greater or equal to your longest read length then this will be
                    the sequence length used to create your read groups.  This can
                    be useful for making directly comaparable statistics from
                    datasets with somewhat variable read lengths.

    -f --format     Bypasses the normal sequence file format detection and
                    forces the program to use the specified format.  Valid
                    formats are bam,sam,bam_mapped,sam_mapped and fastq

    -t --threads    Specifies the number of files which can be processed
                    simultaneously.  Each thread will be allocated 250MB of
                    memory so you shouldn't run more threads than your
                    available memory will cope with, and not more than
                    6 threads on a 32 bit machine

    -c              Specifies a non-default file which contains the list of
    --contaminants  contaminants to screen overrepresented sequences against.
                    The file must contain sets of named contaminants in the
                    form name[tab]sequence.  Lines prefixed with a hash will
                    be ignored.

    -a              Specifies a non-default file which contains the list of
    --adapters      adapter sequences which will be explicity searched against
                    the library. The file must contain sets of named adapters
                    in the form name[tab]sequence.  Lines prefixed with a hash
                    will be ignored.

    -l              Specifies a non-default file which contains a set of criteria
    --limits        which will be used to determine the warn/error limits for the
                    various modules.  This file can also be used to selectively
                    remove some modules from the output all together.  The format
                    needs to mirror the default limits.txt file found in the
                    Configuration folder.

   -k --kmers       Specifies the length of Kmer to look for in the Kmer content
                    module. Specified Kmer length must be between 2 and 10. Default
                    length is 7 if not specified.

   -q --quiet       Supress all progress messages on stdout and only report errors.

   -d --dir         Selects a directory to be used for temporary files written when
                    generating report images. Defaults to system temp directory if
                    not specified.
```
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
* Total sequence 总序列:所处理的序列总数的计数。报告了两个值，实际值actual和估计值estimated。现在这些都是一样的。
在未来，我们可能只分析序列的一个子集，并估计总数量，以加快分析速度，但由于我们发现有问题的序列并不是均匀分布在一个文件中，我们现在禁用了这个功能。
* Filtered sequences 过滤序列:如果以Casava模式运行，标记要过滤的序列将从所有分析中删除。这里将报告被删除的这些序列的数量。
上面的总序列计数将不包括这些过滤后的序列，而将包括用于其余分析的实际序列数。
* Sequence length 序列长度:表示集合中最短和最长序列的长度。
如果所有序列长度相同，则只报告一个值。
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

图表的背景将 y 轴分为good quality calls (green), calls of reasonable quality (orange), and calls of poor quality (red)。 
大多数平台上的calls质量会随着运行的进行而下降，因此通常会看到base calls在读取结束时落入橙色区域。
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
  如果库的质量下降到低水平，那么最常见的补救措施是执行质量修整quality trimming ，其中根据平均质量截断读取。 
  对于发生此类下降的大多数库，您通常会同时遇到 adapter read-through问题，因此通常采用a combined adapter and quality trimming step。

  通常情况下，在序列的起始和结束部分可能出现质量较差的情况，对于最初测序的部分数据，测序仪直接使用默认参数进行base calling, 这部分碱基的质量一般， 然后会利用这部分数据去调整base calling的参数设置，以符合真实的数据.
  在之后的测序中，用调整后的参数进行base caling, 此时碱基的质量会更好.
  所以会观察到，在开头部分存在碱基质量上升的趋势；随着测序反应的进行，酶活性等因素降低，会导致测序质量变差，所以在结尾部分会观察到碱基质量降低的趋势。
```

[原文链接](https://blog.csdn.net/weixin_43569478/article/details/108079243)




b. 运行早期的短暂质量损失:   a short loss of quality earlier in the run  
发现办法：可通过per-tile quality plot 来发现此类错误   

解决办法：  不建议trimming；后续mapping或assembly期间屏蔽碱基mask bases
```
  另一种可能性是，由于运行早期的短暂质量损失a short loss of quality earlier in the run而触发警告/错误，然后恢复以产生稍后的高质量序列。 
  如果运行出现暂时性问题（例如，气泡通过流通池flowcell），则可能会发生这种情况。
   可以通过查看per-tile quality plot 来查看此类错误。 在这些情况下，不建议trimming，因为它会删除以后的良好序列，但需要考虑在后续mapping或assembly期间屏蔽碱基mask bases。
```


c.  库具有不同长度的reads  

解决办法：
提前通过 序列长度分布`模块 sequence length distribution module ` 结果来检查有多少序列会触发错误

如果您的库具有不同长度的读取，那么您会发现此模块会触发警告或错误，因为给定碱基范围的覆盖率非常低。 
在提交任何操作之前，通过查看序列长度分布模块 sequence length distribution module 结果来检查有多少序列会触发错误。  

-----------------------------------    

③　Per sequence quality scores每个序列质量得分   

1).  意义：Per sequence quality scores 报告允许查看序列子集是否具有普遍的低quality values。通常情况下，序列子集的质量普遍较差，通常是因为它们成像不佳（在视野边缘等），但是这些应该只占总序列的一小部分。  

2).  图片：  
![tu](../pictures/%E5%9B%BE%E7%89%872.png)  

每一个Base都有一个`quality score`。该图以read划分，每一个`read`都有一个`mean quality score`，x值为`mean quality score`，y值为`该质量值下的reads数目`，对应到图中绘制出一条单峰曲线  

如果一次运行中的的大部分序列总体质量较低，则这可能表明存在某种系统问题 - 可能只是运行的哪一个环节出问题了（例如流通池的一端）。
    如果您的输入是未记录quality scores的 BAM/SAM 文件，则不会显示此模块的结果。  
3).  Warning  
如果最常观察到的平均quality（即峰值）低于 27，则会发出警告 - 这相当于 0.2% 的错误率。  
4). Failure  
如果最常观察到的平均quality（即峰值）低于 20 - 这相当于 1% 的错误率，此模块将显示失败。  
5). Warning 原因  
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

如果` A 和 T `或 `G 和 C `之间的差异在任何位置大于 10%，此模块会发出警告。  

4). Failure  

如果 `A 和 T` 或 `G 和 C `之间的差异在任何位置大于 20%，则此模块将失败。  

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

-----------------------------------
⑤　Per sequence GC content  

1). 意义：计算文件中每个序列的整个长度的 GC 含量，并将其与 GC 含量的建模正态分布进行比较。  

2). 图像：  
![tu](../pictures/%E5%9B%BE%E7%89%874.png)  

该图`以read划分`，每一个read都有一个平均 GC含量百分比，x值为`平均 GC含量百分比`，y值为该`百分比下的reads数目`，对应到图中绘制出一条曲线  

  在正常的随机文库中，会期望看到大致正态分布的 GC 含量，中心峰对应于基础基因组的整体 GC 含量。 由于我们不知道基因组的 GC 含量，因此模态 GC 含量是根据观察到的数据计算的，并用于构建参考分布。  


3). Warning  

如果与正态分布的偏差总和（即加起来超过的面积）超过 15% 的reads，则会发出警告。  

4). Failure  

如果与正态分布的偏差总和（即加起来超过的面积）超过 30% 的reads，则会发出警告。  

5). Warning原因

异常形状的分布可能因为`库受污染了`，或某些`其他类型的有偏向性的subset`。  
 偏移的正态分布表示一些与base位置无关的系统偏差。 如果存在导致正态分布偏移的系统偏差，不会显示错误，因为不知道这个基因组的 GC 含量是多少。  

 Warning通常表明库有问题。 平滑分布上的尖峰（在单峰上又有个尖峰）通常是特定污染物（例如adapter二聚体）的结果，这很可能被overrepresented sequences 模块显示。 较宽的峰可能代表不同物种的污染。   

 -----------------------------------

 ⑥　Per Base N Content  

1). 意义：如果测序仪不能有足够的信心进行base call，那么它通常会用 N 而不是传统的base call来替换。该模块会绘制出每个位置上call  N 的碱基的百分比。  
2). 图像:  
![tu](../pictures/%E5%9B%BE%E7%89%875.png)  
以每一个时刻读取到的flew cell上所有碱基为一个单位，x表示`碱基在read上的位置`，y表示`N在读取到的所有base中的百分比`。  

看到序列中出现非常低比例的 N 并不罕见，尤其是在序列末尾附近。 然而，如果这个比例上升到几个百分点以上，则表明分析pipeline无法很好地解释数据以进行有效的base calls。  

3). Warning  

如果任何位置显示 N 含量 >5%，此模块会发出警告。  

4). Failure  

如果任何位置显示 N 含量 >20%，此模块会发出警告。  
5). Warning原因  

包含大量 Ns 的最常见原因是`质量普遍下降`，因此该模块的结果应与各种质量模块的结果一起评估。 应该检查特定 bin 的覆盖范围，因为最后一个 bin 可能包含非常少的序列，并且在这种情况下可能会过早触发错误。  

另一种情况是`在库早期的少数位置上发生高比例的 N`，但库通常质量很好。 当您在库中的序列组成`非常有偏差bias`时，可能会发生这种偏差。当查看`per-base sequence content结果`时，这种类型的问题将很明显。

-----------------------------------

⑦ Sequence Length Distribution序列长度分布  

1). 意义:一些高通量测序仪会生成长度一致的序列片段，但其他测序仪可能包含长度变化很大的reads。 即使在统一长度的库中，一些lane也会修剪序列以从末端移除质量差的base calls。该模块生成一个图表，显示所分析文件中片段大小的分布。  
理论情况下所有序列的长度都和机器读长一致。  


2).  图像    
![yu](../pictures/%E5%9B%BE%E7%89%876.png)  

以一个read为单位，每一个read都有其长度。x轴代表`每一个read的长度`，y轴代表`该长度read的数目`，连成顺滑曲线。  
在许多情况下，这将生成一个简单的图表，仅显示一个大小的峰值，但对于可变长度的 FastQ 文件，这将显示每个不同大小的序列片段的相对数量。 

3). Warning  

如果所有序列的长度不同，此模块将发出警告。  

4). Failure  

如果任何序列的长度为零，此模块将引发错误。  

5). Warning原因  

对于某些测序平台，具有不同的读取长度是完全正常的，因此可以忽略此处的警告。  


-----------------------------------
 
⑧ Duplicate Sequences重复序列  

1). 意义：在一个多样化的库中，大多数序列只会出现一次。 低水平的重复可能表明目标序列的覆盖率非常高，但高水平的重复更有可能表明某种富集偏差（例如 PCR 过度扩增）。  

2). 图像  
![tu](../pictures/%E5%9B%BE%E7%89%877.png)  
 
该模块计算文库中每个序列的重复度，并创建一个图表，显示具有不同重复度的序列的相对数量。
x轴是`序列重复水平`，y是`序列百分比` 。  

为了减少此模块的内存需求，仅分析首先出现在`每个文件的前 100,000 个序列`，但这应该足够了。 每个序列都被跟踪到文件的末尾，以给出总体重复级别的代表性计数。 为了减少最终图中的信息量，任何`超过 10 个重复的序列`都被放入`分组bins`中，以便清楚地了解总体重复水平，而不必显示每个单独的重复值。  

由于重复检测需要在整个序列长度上进行精确的序列匹配，因此出于分析的目的，`长度超过 75bp 的任何读取都将被截断为 50bp`。 即便如此，较长的读取更有可能包含测序错误，这将人为地增加观察到的多样性，并且往往会低估高度重复的序列。`**呈现的重复率将偏低** ` 

该图显示了由每个不同重复级别bins中的序列组成的文库的比例。  
 有两条线：  
 * 蓝线采用完整的序列集并显示其重复级别是如何分布的。 
 * 红线中序列被去重，显示的比例是 来自原始数据中不同重复级别的  去重集的百分比。  
 一条线段每个x轴上点所对应的y值百分比加起来=100%。  

在一个适当多样化的库中，在红线和蓝线中大多数序列应该`落在图的最左侧`。   
general的富集水平，表明文库中广泛的oversequencing将趋向于使线条变平，降低低端并通常提高其他类别。 更specific 的子集富集或低复杂性污染物会在图的右侧产生尖峰。 这些高重复峰最常出现在蓝线中，因为它们在原始库中占很大比例，但通常会在红线中消失，因为它们在去重集中所占比例很小。 如果蓝线中持续存在峰值，则表明存在大量不同的高度重复序列，这可能表明存在污染集或非常严重的技术重复。  

3). Warning  

如果某重复序列占总数的 20% 以上，此模块将发出警告。  

4). Failure 

如果某重复序列占总数的50% 以上，此模块将发出警告。  

5). Warning原因  

a. 该模块的基本假设是一个多样化的未丰富库。 任何与此假设的偏差都会自然产生重复，并可能导致该模块出现警告或错误。  

b. 一般来说，文库中有两种潜在的重复类型，一种是`由 PCR 人工制品（引物污染）产生的技术重复`，另一种是`自然碰撞的生物重复`，随机选择了完全相同序列的不同拷贝。 从sequence level 来看，没有办法区分这两种类型，并且在这里都将报告为重复。  

c. 此模块中的警告或错误只是声明您已用尽至少部分文库的多样性并正在重新测序相同的序列。 在一个所谓的多样化文库中，这表明多样性已经部分或完全耗尽，因此您正在浪费测序能力。 但是，在某些库类型中，您`会倾向于对库的部分进行过度测序，因此会产生重复`。  

d. 在 RNA-Seq 文库中，来自不同转录本的序列将在起始种群中以完全不同的水平存在。 因此，`为了能够观察低表达的转录本，通常对高表达的转录本进行过度测序，这可能会产生大量重复`。 这将导致此测试中的总体重复率很高，并且通常会在较高的重复bins中产生峰值。 这种重复来自物理上连接的区域，并且对特定基因组区域中重复分布的检查可以区分出：`过度测序`和`一般技术重复`，但这些区别在原始 fastq 文件中是不可能的。 在高度丰富的 ChIP-Seq 文库中可能会出现类似的情况，尽管那里的重复不太明显。 最后，如果您有一个`序列起始点受到限制的文库（例如，围绕限制性位点构建的文库，或未片段化的小 RNA 文库）`，那么受限制的起始位点将产生巨大的重复水平，这不是个问题，也不用进行deduplication处理。 在这些类型的图书馆中，您应该考虑使用诸如`random barcoding`之类的系统来区分技术和生物学重复  

-----------------------------------

⑨  Overrepresented sequences过表达的序列  

1). 意义  

一个正常的高通量文库将包含一组多样性的序列，不是单个序列构成整个序列的一小部分。 发现单个序列在集合中的代表性过高要么意味着它`具有高度的生物学意义`，要么表明`文库被污染（多为引物二聚体）`，或者`不像预期的那样多样化`。   

2). 图像    

![tu](../pictures/%E5%9B%BE%E7%89%878.png)    

 
该模块列出了`占总数超过 0.1% 的所有序列`。为了节省内存，仅将出现在`前 100,000 个序列`中的序列跟踪到文件末尾。 因此，该模块可能会遗漏一个被过度表达但由于某种原因没有出现在文件开头的序列。  

对于每个过度表达的序列，程序将`在常见污染物的数据库中寻找匹配，并报告它找到的最佳匹配`。 命中的长度必须`至少为 20 bp，并且不匹配不超过 1 个`。   
**找到命中并不一定意味着这是污染的来源，但可能会为您指明正确的方向**. 许多adapter序列彼此非常相似，因此您可能会得到一个命中报告，这在技术上不正确，但与实际匹配的序列非常相似。  

由于重复检测需要在整个序列长度上进行精确的序列匹配，因此出于分析目的，任何长度超过 75bp 的读取都将被截断为 50bp。 即便如此，较长的读取更有可能包含测序错误，这将人为地增加观察到的多样性，并且往往会低估高度重复的序列。`*呈现的重复率将偏低*`  



3). Warning  

如果发现任何序列占总数的 0.1% 以上，此模块将发出警告。  

4). Failure  

如果发现任何序列占总数的 1% 以上，此模块将发出错误。  

5). Warning 原因   

当用于分析`小 RNA 文库`时，通常会触发该模块，其中序列不会受到随机片段化，并且相同的序列可能自然地存在于文库的很大一部分中。  
 
-----------------------------------

⑩ Adapter content adapter的含量  

1).  意义  

Kmer 含量模块将对库中的所有 Kmer 进行通用分析，以找到那些在reads长度中没有覆盖的内容。 这可以在库中找到bias的不同来源，其中包括在序列末端建立的read-through adapter sequences。
任何overrepresented的序列（例如adapter二聚体）的存在都会导致 Kmer 图被这些序列包含的 Kmer 所支配，并且并不总是很容易看出其中是否存在其他偏差。  

一类明显的序列是`adapter序列`。 了解库是否包含大量adapter，以便能够评估是否需要修剪adapter。 尽管 Kmer 分析理论上可以发现这种污染，但并不总是很清楚。 因此，此模块会`针对一组单独定义的 Kmer 进行特定搜索，并提供包含这些 Kmer 的库的总比例的视图`。 将始终为adapter配置文件中存在的所有序列生成结果跟踪，可以查看库的即使很低的adapter含量。  

2). 图像  
![tu](../pictures/%E5%9B%BE%E7%89%879.png)    

`该图显示了在每个位置看到每个adapter序列的文库比例的累积百分比计数`。 一旦在读取中看到一个序列，它就被视为一直存在到读取结束，因此看到的百分比只会随着读取长度的增加而增加。  

3). Warning  

如果发现任何序列占reads的5% 以上，此模块将发出警告。  

4).  Failure  

如果发现任何序列占reads的 10% 以上，此模块将发出错误。  

5). Warning 原因  

任何插入大小的合理比例小于读取长度的库都将触发此模块。 这并不表示存在这样的问题 - 只是在进行任何下游分析之前需要对序列进行adapter修剪。
如果在当时fastqc分析的时候-a选项没有内容，则默认使用图例中的四种通用adapter序列进行统计。如果有adapter序列没有去除干净的情况，在后续分析的时候需要先`使用cutadapt软件进行去接头`，也可以用 `trimmomatic来去除接头`  

----------------------------------

11. Kmer Content   Kmer的含量  

1). 意义  

对overrepresented sequences分析将发现完全重复的序列的增加，但对其他问题不起作用：
* 如果序列很长且序列质量较差，那么随机测序错误将显着减少完全重复序列的计数。
* 如果有一个部分序列出现在序列中的多个位置，那么 per base content plot 或the duplicate sequence分析都不会看到这一点。  

Kmer 模块假设: 在多样化库中, 任何小序列片段不应有位置bias。 某些 Kmer 总体上富集或耗尽可能有生物学原因，但这些偏差应该平等地影响序列中的所有位置。 因此，该模块测量库中每个位置的每个 7-mer 的数量，然后使用二项式检验 binomial test来寻找与所有位置的均匀覆盖率的有显着偏差的kmer。 报告任何具有位置偏差富集的 Kmer。   

如果某k个bp的短序列在reads中大量出现，其频率高于统计期望的话，fastqc将其记为over-represented k-mer。默认的`k = 5`，可以用`-k --kmers选项`来调节，范围是2-10。`出现频率总体上3倍于期望或是在某位置上5倍于期望的k-mer被认为是over-represented`。fastqc除了列出所有over-represented k-mers，还会把`前6个的per base distribution画出来`。

2). 图像  

下图是` 6 个最有bias的 Kmer`，以显示它们的分布。

![tu](../pictures/%E5%9B%BE%E7%89%8710.png)    


为了让这个模块在合理的时间内运行，只分析整个库的 2%，并将结果外推到库的其余部分。 长于 500bp 的序列将被截断为 500bp 用于分析。  

3). Warning  

如果任何 k-mer 不平衡且二项式 p 值 <0.01，此模块将发出警告。  

4). Failure  

如果任何 k-mer 不平衡且二项式 p 值 <10^-5，此模块将失败。  
当有出现频率总体上3倍于期望或是在某位置上5倍于期望的k-mer时，报”WARN“；当有出现频率在某位置上10倍于期望的k-mer时报"FAIL"。  
  
5). Warning原因  

任何单独的overrepresented sequences，即使没有以足够高的阈值来触发the overrepresented sequences module，也会导致来自这些序列的 Kmer 在该模块中高度富集。 这些通常会在序列中的单个点上显示为富集的尖锐尖峰，而不是渐进或广泛的富集。  

由于可能的随机引物的采样不完整，源自随机引物的文库几乎总是在文库开始时显示 Kmer 偏差。  

----------------------------------

12.　Per tile sequence quality   每tile的测序质量  

1). 意义:
如果使用的是保留其原始序列标识符的 Illumina 文库，该图只会出现在分析结果中。 其中编码的是每次read来自的flow cell tile。 该图表允许您查看所有碱基中每个图块的quality scores，以查看是否存在因为一部分flow cell造成的质量损失。    

![tu](../pictures/%E5%9B%BE%E7%89%8711.png)  

2). 图像：  

![tu](../pictures/%E5%9B%BE%E7%89%8712.png)    


该图显示了与每个tile的平均质量的偏差。 颜色是从冷到热的等级，冷色是在运行中质量达到或高于该base平均水平的位置，而较热的颜色表明一个tile的质量比该base的其他tile更差。 x轴从左到右，代表`每一个tile读取的read位置`，y轴是`tile的Index编号，表示每一个tile的位置`。好的结果应该一直是蓝色的。  

3). Warning  

如果任何图块显示的平均 Phred 分数比所有tile中该碱基的平均值小 2 多，此模块将发出警告。  

4). Failure  

如果任何图块显示的平均 Phred 分数比所有tile中该碱基的平均值小 5 多，此模块将发出警告。  

5). Warning原因  

在该图上看到警告或错误的原因可能是暂时性问题，例如`通过流通池的气泡`，也可能是更持久的问题，例如`流通池上的污迹或流通池通道内的碎片`。  

虽然此模块中的警告可以由个别特定事件触发，但我们还观察到，当流通池通常过载时，归因于tile的 phred 分数的更大变化也可能出现。 在这种情况下，事件会出现在整个流通池中，而不是局限于特定区域或循环范围。 我们通常会忽略仅在 1 或 2 个周期内轻微影响少量tiles的错误，但会修正更大的影响，即分数偏差较大或持续几个周期。  


## 五、用法，步骤  

![tu](../pictures/%E5%9B%BE%E7%89%8713.png)    
```
fastqc -help

            FastQC - A high throughput sequence QC analysis tool

SYNOPSIS

        fastqc seqfile1 seqfile2 .. seqfileN

    fastqc [-o output dir] [--(no)extract] [-f fastq|bam|sam]
           [-c contaminant file] seqfile1 .. seqfileN

DESCRIPTION

    FastQC reads a set of sequence files and produces from each one a quality
    control report consisting of a number of different modules, each one of
    which will help to identify a different potential type of problem in your
    data.

    If no files to process are specified on the command line then the program
    will start as an interactive graphical application.  If files are provided
    on the command line then the program will run with no user interaction
    required.  In this mode it is suitable for inclusion into a standardised
    analysis pipeline.

    The options for the program as as follows:

    -h --help       Print this help file and exit

    -v --version    Print the version of the program and exit

    -o --outdir     Create all output files in the specified output directory.
                    Please note that this directory must exist as the program
                    will not create it.  If this option is not set then the
                    output file for each sequence file is created in the same
                    directory as the sequence file which was processed.

    --casava        Files come from raw casava output. Files in the same sample
                    group (differing only by the group number) will be analysed
                    as a set rather than individually. Sequences with the filter
                    flag set in the header will be excluded from the analysis.
                    Files must have the same names given to them by casava
                    (including being gzipped and ending with .gz) otherwise they
                    won't be grouped together correctly.

    --nano          Files come from nanopore sequences and are in fast5 format. In
                    this mode you can pass in directories to process and the program
                    will take in all fast5 files within those directories and produce
                    a single output file from the sequences found in all files.

    --nofilter      If running with --casava then don't remove read flagged by
                    casava as poor quality when performing the QC analysis.

    --extract       If set then the zipped output file will be uncompressed in
                    the same directory after it has been created.  By default
                    this option will be set if fastqc is run in non-interactive
                    mode.

    -j --java       Provides the full path to the java binary you want to use to
                    launch fastqc. If not supplied then java is assumed to be in
                    your path.

    --noextract     Do not uncompress the output file after creating it.  You
                    should set this option if you do not wish to uncompress
                    the output when running in non-interactive mode.

    --nogroup       Disable grouping of bases for reads >50bp. All reports will
                    show data for every base in the read.  WARNING: Using this
                    option will cause fastqc to crash and burn if you use it on
                    really long reads, and your plots may end up a ridiculous size.
                    You have been warned!

    --min_length    Sets an artificial lower limit on the length of the sequence
                    to be shown in the report.  As long as you set this to a value
                    greater or equal to your longest read length then this will be
                    the sequence length used to create your read groups.  This can
                    be useful for making directly comaparable statistics from
                    datasets with somewhat variable read lengths.

    -f --format     Bypasses the normal sequence file format detection and
                    forces the program to use the specified format.  Valid
                    formats are bam,sam,bam_mapped,sam_mapped and fastq

    -t --threads    Specifies the number of files which can be processed
                    simultaneously.  Each thread will be allocated 250MB of
                    memory so you shouldn't run more threads than your
                    available memory will cope with, and not more than
                    6 threads on a 32 bit machine

    -c              Specifies a non-default file which contains the list of
    --contaminants  contaminants to screen overrepresented sequences against.
                    The file must contain sets of named contaminants in the
                    form name[tab]sequence.  Lines prefixed with a hash will
                    be ignored.

    -a              Specifies a non-default file which contains the list of
    --adapters      adapter sequences which will be explicity searched against
                    the library. The file must contain sets of named adapters
                    in the form name[tab]sequence.  Lines prefixed with a hash
                    will be ignored.

    -l              Specifies a non-default file which contains a set of criteria
    --limits        which will be used to determine the warn/error limits for the
                    various modules.  This file can also be used to selectively
                    remove some modules from the output all together.  The format
                    needs to mirror the default limits.txt file found in the
                    Configuration folder.

   -k --kmers       Specifies the length of Kmer to look for in the Kmer content
                    module. Specified Kmer length must be between 2 and 10. Default
                    length is 7 if not specified.

   -q --quiet       Supress all progress messages on stdout and only report errors.

   -d --dir         Selects a directory to be used for temporary files written when
                    generating report images. Defaults to system temp directory if
                    not specified.
```  

* 对于单端数据，基本用法如下:  
```
fastqc -o out_dir -t 10 input.fq
```
* 对于双端数据，基本用法如下:  
```
fastqc -o out_dir -t 10 R1.fq R2.fq
```
需要注意的是，输出目录必须手动新建。


## 六、如何根据fastqc给出的结果判断高通量测序结果的好坏：  

* 好的illumina data ，[见网页](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/good_sequence_short_fastqc.html)

* 差的illumina data ，[见网页](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/bad_sequence_fastqc.html)

1. Per base sequence quality每个碱基序列质量  
![hao](../pictures/%E5%9B%BE%E7%89%8714.png)  
![huai](../pictures/%E5%9B%BE%E7%89%8715.png)  
Quality score 集中在最高值，且都很稳定，没有非常明显的倒峰；做出来的趋势线顺滑且尽量平行于x轴   
所以上面的不好的测序结果，需要把后面的24bp以后的序列切除，从而保证后续分析的正确性
2. Per tile sequence quality   每tile的测序质量  
![hao](../pictures/%E5%9B%BE%E7%89%8716.png)    
![huai](../pictures/%E5%9B%BE%E7%89%8717.png)     
Seq quality 尽量蓝屏，没有彩色条带
如果某些tile出现暖色，可以在后续分析中把该tile测序的结果全部都去除  
3. Per sequence quality scores每个序列质量得分  
![hao](../pictures/%E5%9B%BE%E7%89%8718.png)    
![huai](../pictures/%E5%9B%BE%E7%89%8719.png)     
好的序列质量得分图，在x轴最后分数最高段，有且只有一个尖锐峰  
第二个图其实也行  
4. Per base seq content每种碱基的含量   
![hao](../pictures/%E5%9B%BE%E7%89%8720.png)    
![huai](../pictures/%E5%9B%BE%E7%89%8721.png)   
每种碱基含量应该很稳定，呈现平滑平行于x轴的直线，几乎没有交叉  
5. Per sequence GC content每个测序的GC含量  
![hao](../pictures/%E5%9B%BE%E7%89%8722.png)    
![huai](../pictures/%E5%9B%BE%E7%89%8723.png)   
GC比例几乎与理论值相同，曲线重合度高，没有突出峰尖尖  
6. Per base N content 每个碱基中N含量  
![hao](../pictures/%E5%9B%BE%E7%89%8724.png)      
![huai](../pictures/%E5%9B%BE%E7%89%8725.png)     
几乎没有N出现，出现一点也行    

7. Sequence Length Distribution序列长度分布  
![tu](../pictures/%E5%9B%BE%E7%89%8726.png)  
呈现等腰三角形  

8. Sequence Duplication Levels重复序列的水平  
![hao](../pictures/%E5%9B%BE%E7%89%8727.png)      
![huai](../pictures/%E5%9B%BE%E7%89%8728.png)  

9. Overrepresented sequences过表达的序列  
![hao](../pictures/%E5%9B%BE%E7%89%8729.png)      
![huai](../pictures/%E5%9B%BE%E7%89%8730.png)

10. Adapter content adapter的含量  
![hao](../pictures/%E5%9B%BE%E7%89%8731.png)      
![huai](../pictures/%E5%9B%BE%E7%89%8732.png)  

 几乎没有adaptor含量，出现一点也行
* 此图衡量的是序列中两端adapter的情况
8 如果在当时fastqc分析的时候-a选项没有内容，则默认使用图例中的四种通用adapter序列进行统计
* 本例中adapter都已经去除，如果有adapter序列没有去除干净的情况，在后续分析的时候需要先使用cutadapt软件进行去接头，也可以用 trimmomatic来去除接头
