[HISAT2官网](https://daehwankimlab.github.io/hisat2/)  
[参考文章1](https://www.jianshu.com/p/ce3f4afb9b60)   
[参考文章2](https://zhuanlan.zhihu.com/p/451939113)  


# 使用方法  
```
$ hisat2 -h

HISAT2 version 2.2.1 by Daehwan Kim (infphilo@gmail.com, www.ccb.jhu.edu/people/infphilo)

Usage:
  hisat2 [options]* -x <ht2-idx> {-1 <m1> -2 <m2> | -U <r>} [-S <sam>]

  <ht2-idx>  Index filename prefix (minus trailing .X.ht2).
  <m1>       Files with #1 mates, paired with files in <m2>.
             Could be gzip'ed (extension: .gz) or bzip2'ed (extension: .bz2).
  <m2>       Files with #2 mates, paired with files in <m1>.
             Could be gzip'ed (extension: .gz) or bzip2'ed (extension: .bz2).
  <r>        Files with unpaired reads.
             Could be gzip'ed (extension: .gz) or bzip2'ed (extension: .bz2).
  <sam>      File for SAM output (default: stdout)

  <m1>, <m2>, <r> can be comma-separated lists (no whitespace) and can be
  specified many times.  E.g. '-U file1.fq,file2.fq -U file3.fq'.


Options (defaults in parentheses):

 Input:
  -q                 query input files are FASTQ .fq/.fastq (default)
  --qseq             query input files are in Illumina's qseq format
  -f                 query input files are (multi-)FASTA .fa/.mfa
  -r                 query input files are raw one-sequence-per-line
  -c                 <m1>, <m2>, <r> are sequences themselves, not files
  -s/--skip <int>    skip the first <int> reads/pairs in the input (none)
  -u/--upto <int>    stop after first <int> reads/pairs (no limit)
  -5/--trim5 <int>   trim <int> bases from 5'/left end of reads (0)
  -3/--trim3 <int>   trim <int> bases from 3'/right end of reads (0)
  --phred33          qualities are Phred+33 (default)
  --phred64          qualities are Phred+64
  --int-quals        qualities encoded as space-delimited integers

 Presets:                 Same as:
   --fast                 --no-repeat-index
   --sensitive            --bowtie2-dp 1 -k 30 --score-min L,0,-0.5
   --very-sensitive       --bowtie2-dp 2 -k 50 --score-min L,0,-1

 Alignment:
  --bowtie2-dp <int> use Bowtie2's dynamic programming alignment algorithm (0) - 0: no dynamic programming, 1: conditional dynamic programming, and 2: unconditional dynamic programming (slowest)
  --n-ceil <func>    func for max # non-A/C/G/Ts permitted in aln (L,0,0.15)
  --ignore-quals     treat all quality values as 30 on Phred scale (off)
  --nofw             do not align forward (original) version of read (off)
  --norc             do not align reverse-complement version of read (off)
  --no-repeat-index  do not use repeat index

 Spliced Alignment:
  --pen-cansplice <int>              penalty for a canonical splice site (0)
  --pen-noncansplice <int>           penalty for a non-canonical splice site (12)
  --pen-canintronlen <func>          penalty for long introns (G,-8,1) with canonical splice sites
  --pen-noncanintronlen <func>       penalty for long introns (G,-8,1) with noncanonical splice sites
  --min-intronlen <int>              minimum intron length (20)
  --max-intronlen <int>              maximum intron length (500000)
  --known-splicesite-infile <path>   provide a list of known splice sites
  --novel-splicesite-outfile <path>  report a list of splice sites
  --novel-splicesite-infile <path>   provide a list of novel splice sites
  --no-temp-splicesite               disable the use of splice sites found
  --no-spliced-alignment             disable spliced alignment
  --rna-strandness <string>          specify strand-specific information (unstranded)
  --tmo                              reports only those alignments within known transcriptome
  --dta                              reports alignments tailored for transcript assemblers
  --dta-cufflinks                    reports alignments tailored specifically for cufflinks
  --avoid-pseudogene                 tries to avoid aligning reads to pseudogenes (experimental option)
  --no-templatelen-adjustment        disables template length adjustment for RNA-seq reads

 Scoring:
  --mp <int>,<int>   max and min penalties for mismatch; lower qual = lower penalty <6,2>
  --sp <int>,<int>   max and min penalties for soft-clipping; lower qual = lower penalty <2,1>
  --no-softclip      no soft-clipping
  --np <int>         penalty for non-A/C/G/Ts in read/ref (1)
  --rdg <int>,<int>  read gap open, extend penalties (5,3)
  --rfg <int>,<int>  reference gap open, extend penalties (5,3)
  --score-min <func> min acceptable alignment score w/r/t read length
                     (L,0.0,-0.2)

 Reporting:
  -k <int>           It searches for at most <int> distinct, primary alignments for each read. Primary alignments mean
                     alignments whose alignment score is equal to or higher than any other alignments. The search terminates
                     when it cannot find more distinct valid alignments, or when it finds <int>, whichever happens first.
                     The alignment score for a paired-end alignment equals the sum of the alignment scores of
                     the individual mates. Each reported read or pair alignment beyond the first has the SAM ‘secondary’ bit
                     (which equals 256) set in its FLAGS field. For reads that have more than <int> distinct,
                     valid alignments, hisat2 does not guarantee that the <int> alignments reported are the best possible
                     in terms of alignment score. Default: 5 (linear index) or 10 (graph index).
                     Note: HISAT2 is not designed with large values for -k in mind, and when aligning reads to long,
                     repetitive genomes, large -k could make alignment much slower.
  --max-seeds <int>  HISAT2, like other aligners, uses seed-and-extend approaches. HISAT2 tries to extend seeds to
                     full-length alignments. In HISAT2, --max-seeds is used to control the maximum number of seeds that
                     will be extended. For DNA-read alignment (--no-spliced-alignment), HISAT2 extends up to these many seeds
                     and skips the rest of the seeds. For RNA-read alignment, HISAT2 skips extending seeds and reports
                     no alignments if the number of seeds is larger than the number specified with the option,
                     to be compatible with previous versions of HISAT2. Large values for --max-seeds may improve alignment
                     sensitivity, but HISAT2 is not designed with large values for --max-seeds in mind, and when aligning
                     reads to long, repetitive genomes, large --max-seeds could make alignment much slower.
                     The default value is the maximum of 5 and the value that comes with -k times 2.
  -a/--all           HISAT2 reports all alignments it can find. Using the option is equivalent to using both --max-seeds

                     and -k with the maximum value that a 64-bit signed integer can represent (9,223,372,036,854,775,807).
  --repeat           report alignments to repeat sequences directly

 Paired-end:
  -I/--minins <int>  minimum fragment length (0), only valid with --no-spliced-alignment
  -X/--maxins <int>  maximum fragment length (500), only valid with --no-spliced-alignment
  --fr/--rf/--ff     -1, -2 mates align fw/rev, rev/fw, fw/fw (--fr)
  --no-mixed         suppress unpaired alignments for paired reads
  --no-discordant    suppress discordant alignments for paired reads

 Output:
  -t/--time          print wall-clock time taken by search phases
  --un <path>           write unpaired reads that didn't align to <path>
  --al <path>           write unpaired reads that aligned at least once to <path>
  --un-conc <path>      write pairs that didn't align concordantly to <path>
  --al-conc <path>      write pairs that aligned concordantly at least once to <path>
  (Note: for --un, --al, --un-conc, or --al-conc, add '-gz' to the option name, e.g.
  --un-gz <path>, to gzip compress output, or add '-bz2' to bzip2 compress output.)
  --summary-file <path> print alignment summary to this file.
  --new-summary         print alignment summary in a new style, which is more machine-friendly.
  --quiet               print nothing to stderr except serious errors
  --met-file <path>     send metrics to file at <path> (off)
  --met-stderr          send metrics to stderr (off)
  --met <int>           report internal counters & metrics every <int> secs (1)
  --no-head             suppress header lines, i.e. lines starting with @
  --no-sq               suppress @SQ header lines
  --rg-id <text>        set read group id, reflected in @RG line and RG:Z: opt field
  --rg <text>           add <text> ("lab:value") to @RG line of SAM header.
                        Note: @RG line only printed when --rg-id is set.
  --omit-sec-seq        put '*' in SEQ and QUAL fields for secondary alignments.

 Performance:
  -o/--offrate <int> override offrate of index; must be >= index's offrate
  -p/--threads <int> number of alignment threads to launch (1)
  --reorder          force SAM output order to match order of input reads
  --mm               use memory-mapped I/O for index; many 'hisat2's can share

 Other:
  --qc-filter        filter out reads that are bad according to QSEQ filter
  --seed <int>       seed for random number generator (0)
  --non-deterministic seed rand. gen. arbitrarily instead of using read attributes
  --remove-chrname   remove 'chr' from reference names in alignment
  --add-chrname      add 'chr' to reference names in alignment
  --version          print version information and quit
  -h/--help          print this usage message
  ```
# 采用的比对策略  

1. RNA-seq产生的reads可能跨长度比较大的内含子，哺乳动物中甚至最长能达到1MB，同时外显子比较短，read也比较短，会有很多read（模拟数据中大概34%）跨两个外显子的情况  

2. 为了更好的比对，将跨外显子的reads分成了三类：  
1）长锚定read，至少有16bp在两个外显子的每一个上   
2）中间锚定read，有8-15bp在一个外显子上     
3）短锚定read，只有1-7bp在一个外显子上    


3. 所以总的reads可以被划分为五类：  
1）不跨外显子的read 2）长锚定read 3）中间锚定read 4）短锚定read 5）跨两个外显子以上的read  
![P6](../pictures/P6.webp)  

4. 在模拟的数据中，有25%左右的read是长锚定read，`这种read在大多数情况下可以被唯一的定位到人的基因组上`  ;  
5%为中间锚定read，对于这类，很多依赖于全局索引的算法就`很难执行`下去（需要比对很多次）;  
而hisat，可以`先将read中的长片段实现唯一比对，之后再使用局部索引对剩下的小片段进行比对（局部索引可以实现快速检索）`


5. 4.2% 为短锚定read，因为这些序列特别短，因此只能通过在hisat比对其它read时发现的剪切位点或者用户自己提供的剪切位点来`辅助比对 `

6. 最后还有3%的是跨多个外显子的read，比对策略在hisat的online method中有介绍，文章中没有详解  

7. 比对过程中，中间锚定read、短锚定read、跨多个外显子read的比对占总比对时长的30%-60%，而且比对错误率很高

# 比对命令  
```
建索引：

hisat2-build [options]* <reference_in> <ht2_base>
#<reference_in> ：fasta文件 list，如果为list，使用逗号分开
#<ht2_base> ：索引文件的前缀名，如设为xxx，则生成的索引文件为xxx.1.ht2,xxx.2.ht2，默认的前缀名为NAME
#option:详见说明书
检查索引：hisat2-inspect [options]* <ht2_base>
#输出结果为一个fasta文件，主要用于检查已经构建好的索引所用的构建信息，感觉没啥用
比对：hisat2 [options]* -x <hisat2-idx> {-1 <m1> -2 <m2> | -U <r> | --sra-acc <SRA accession number>} [-S <hit>]
#参数说明：
#-p ：线程数目
#--dta  ：注意！！！在下游使用stringtie组装的时候一定要在hisat中设置这个参数！！！
#-x <hisat2-idx> ：参考基因组索引的basename，即前缀名
#{}：其中的内容意思为hisat2可以接受单端测序，双端测序，或者直接提交SRA ID号
#-1 <m1> ：双端测序的read1 list ，若为list，使用逗号隔开，名字与2要匹配，如-1 flyA_1.fq,flyB_1.fq
#-2 <m2> ：双端测序的read2 list ，若为list，使用逗号隔开，名字与1要匹配，如-2 flyA_2.fq,flyB_2.fq
#-U <r>：单端测序list，若为list，使用逗号隔开，-U lane1.fq,lane2.fq,lane3.fq,lane4.fq
#--sra-acc <SRA accession number> : SRAID list，若为list，使用逗号隔开，--sra-acc SRR353653,SRR353654
#-S <hit> ：SAM写入的文件名，默认写入到标准输出中
#options:这里只列出可调节的类别，至于参数调整，详见说明书
#Input options
#Alignment options
#Scoring options
#Spliced alignment options（重要）
#Reporting options
#Paired-end options（重要）
#Output options（重要）
#SAM options
#Performance options
#Other options
report格式
若为单端测序
20000 reads; of these:
  20000 (100.00%) were unpaired; of these:
    1247 (6.24%) aligned 0 times
    18739 (93.69%) aligned exactly 1 time
    14 (0.07%) aligned >1 times
93.77% overall alignment rate
若为双端测序
10000 reads; of these:
  10000 (100.00%) were paired; of these:
    650 (6.50%) aligned concordantly 0 times
    8823 (88.23%) aligned concordantly exactly 1 time
    527 (5.27%) aligned concordantly >1 times
    ----
    650 pairs aligned concordantly 0 times; of these:
      34 (5.23%) aligned discordantly 1 time
    ----
    616 pairs aligned 0 times concordantly or discordantly; of these:
      1232 mates make up the pairs; of these:
        660 (53.57%) aligned 0 times
        571 (46.35%) aligned exactly 1 time
        1 (0.08%) aligned >1 times
96.70% overall alignment rate
```