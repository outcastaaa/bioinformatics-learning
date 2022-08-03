# Bioinformatics Apps
```
bash $HOME/Scripts/dotfiles/genomics.sh

bash $HOME/Scripts/dotfiles/perl/ensembl.sh

# Optional: huge apps
# bash $HOME/Scripts/dotfiles/others.sh
```








## bash $HOME/Scripts/dotfiles/genomics.sh内容：
```
#!/usr/bin/env bash

echo "====> Building Genomics related tools构建基因组学相关工具 <===="


echo "==> Anchr先不同安装"
# TODO: superreads can't find `jellyfish/circular_buffer.hpp`
#   `include/jellyfish-2.2.4/`

#curl -fsSL https://raw.githubusercontent.com/wang-q/App-Anchr/master/share/install_dep.sh | bash
#通过网页下载bash文件并执行


echo "==> Install bioinformatics softwares安装生信软件"
brew tap brewsci/bio
brew tap brewsci/science
#在核心仓库源找不到软件，通过brew tap在第三方仓库安装软件

#Taps(third-party-repositories)
brew tap可以为brew的软件的跟踪,更新,安装添加更多的tap formulae，如果你在核心仓库没有找到你需要的软件,那么你就需要安装第三方的仓库去安装你需要的软件，tap命令的仓库源默认来至于Github，但是这个命令也不限制于这一个地方


brew install clustal-w mafft muscle trimal
#·CLUSTALW是一种渐进的多序列比对方法，先将多个序列两两比对构建距离矩阵，反应序列之间两两关系。然后根据距离矩阵计算产生系统进化指导树，对关系密切的序列进行加权；然后从最紧密的两条序列开始，逐步引入临近的序列并不断重新构建比对，直到所有序列都被加入为止。
·Muscle、mafft：序列比对软件
·trimal：比对序列修剪软件


brew install lastz diamond paml fasttree iqtree # raxml
#·LASTZ是一个简易替换为BLASTZ。 LASTZ被设计来预处理一个或一组序列中，然后对齐多个查询序列给它。
·DIAMOND是一种高通量比对程序，可将DNA测序reads文件与蛋白质参考序列文件（如NCBI-nr）进行比较。它以C ++实现，旨在多核服务器上快速运行。
·PAML是一款利用最大似然法对DNA或蛋白质序列进行系统发育分析的软件包。构建系统发育树工具
·FastTree和RAxML均是用最大似然法构建系统发育树的工具，前者速度快，后者准确度高。 FastTtree采用的是SH检验来判断每个节点的可信度。该值的范围在0~1之间，与一般用的bootstrap值高度相关。
·IQtree利用最大似然法构建系统发生树的软件



brew install raxml --without-open-mpi
#RaxML是最大似然法（ML法）建树的经典软件，可以建有根树，支持服务器上多线程运行。而且RaxML是目前为数不多支持ML法多基因联合建树的软件，可以说是一款功能非常完备的软件。


brew install --force-bottle newick-utils
#--force-bottle: 使用编译好的文件；选用最新版本，即使这个版本不是最常用的 
#Newick Utilities是一套用于处理系统发育树的unix shell工具，其功能包括重置根（re-root）、提取子树、修剪、合并枝以及可视化。

brew install bowtie bowtie2 bwa samtools
#·Bowtie是一个超级快速的，较为节省内存的短序列拼接至模板基因组的工具。它在拼接35碱基长度的序列时，可以达到每小时2.5亿次的拼接速度。Bowtie并不是一个简单的拼接工具，它不同于Blast等。它适合的工作是将小序列比对至大基因组上去。它最长能读取1024个碱基的片段。换言之，bowtie非常适合下一代测序技术。
·BWA，即Burrows-Wheeler-Alignment Tool。BWA 是一种能够将差异度较小的序列比对到一个较大的参考基因组上的软件包。
·samtools是一个用于操作sam和bam文件（通常是短序列比对工具如bwa,bowtie2,hisat2,tophat2等等产生的，具体格式可以在消息框输入“SAM”查看）的工具合集，包含有许多命令。



brew install stringtie hisat2 # tophat cufflinks
brew install seqtk minimap2 minigraph # gfatools
#seqtk基于C语言编写的软件，运行速度极快，极大的提高工作效率。seqtk日常序列的处理包括：fq转换为fa，格式化序列，截取序列，随机抽取序列等。


brew install genometools # igvtools
#GenomeTools基因组分析系统是一个免费的生物信息学工具集合（在基因组信息学领域），组合成一个名为gt二进制文件。 它基于一个名为libgenometools的C库，该库包含用于高效，便捷地实现序列和注释处理软件的各种类。


brew install canu fastqc picard-tools # kat
#canu是一个用JAVA语言写的三代数据组装工具。canu源于celera Assembler，现在celera Assembler不在更新。canu专门用于三代这种错误率较高的测序的结果进行组装。
Picard是一组命令行工具，用于处理高通量排序（HTS）数据和格式，例如SAM / BAM / CRAM和VCF。这些文件格式在Hts-specs存储库中定义。


brew install --build-from-source snp-sites # macOS bottles broken
#从源代码构建

brew install bcftools
#可以用于bcf个vcf的格式转换。还可以对vcf文件进行过滤。主要包括根据样品、基因型、突变类型、是否分型等等条件

brew install edirect
#NCBI作为一个巨大的bioinformatics数据库，除了提供B/S界面的查询外，还提供了许多工具查询和下载DB中的数据。其中最强大的一种Entrez Direct（Edirect），这是NCBI官方提供的UNIX平台DB数据检索工具

brew install sratoolkit
#sra toolkit是ncbi上将 .sra文件转换为 .fstaq.gz文件的工具

#上述文件已经完成安装


# less used

brew install augustus prodigal
#Augusust是一款预测真核生物基因结构的软件
原核的动态编程基因查找算法，prodigal主要应用于细菌和古生菌的基因预测，不能用于真核生物，如果要对meta样品做基因预测，prodigal还专门提供了meta的版本。 除此之外，prodigal还支持在线提交序列的方式来预测基因预测。也非常的易于使用。而且相对与glimmer基因预测工具，prodigal更加好用，只需一步即可，而且，软件可以直接输出基因的核酸序列并翻译出的相应的氨基酸序列，这对很多初学者来说是非常方便的。


brew install megahit spades sga
#MEGAHIT是超快的宏基因组序列组装工具，尤其适合组装超大规模数据。
SPAdes支持配对末端读取，配对和非配对读取。SPAdes可以同时作为几个配对端和配对库的输入。请注意，SPAdes最初是为小型基因组设计的。它在细菌（单细胞MDA和标准分离株），真菌和其他小基因组上进行了测试。SPAdes不适用于较大的基因组（例如哺乳动物大小的基因组）


brew install quast --HEAD
#quast通过计算各种指标来评估基因组组装


brew install ntcard
brew install gatk freebayes
#The Genome Analysis Toolkit，用于二代重测序数据分析的一款软件，里面包含了很多有用的工具，主要注重与变异的查找，基因分型且对于数据质量保证高度重视。
#FreeBayes是一种贝叶斯遗传变异检测器，设计用于发现小的多态性，特别是SNP（单核苷酸多态性），插入缺失（插入和缺失），MNP（多核苷酸多态性）和复杂事件（复合插入和替代事件） 短读序列比对的长度。


#上述文件已经完成安装


# brew unlink proj
brew install --force-bottle blast

echo "==> Custom tap"
brew tap wang-q/tap
brew install faops multiz sparsemem intspan
# brew install jrunlist jrange

echo "==> circos"
brew install wang-q/tap/circos@0.69.9
#报错  Error: circos@0.69.9: SHA256 mismatch

echo "==> RepeatMasker"
brew install hmmer easel

brew install wang-q/tap/rmblast@2.10.0
# brew link rmblast@2.10.0 --overwrite

brew install wang-q/tap/trf@4

brew install wang-q/tap/repeatmasker@4.1.1
#从wang-q/tap库中下载，以上均下载完成


# Config repeatmasker
pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple h5py
#指定国内镜像源下载安装，清华：https://pypi.tuna.tsinghua.edu.cn/simple


cd $(brew --prefix)/Cellar/repeatmasker@4.1.1/4.1.1/libexec
#brew --prefix指定安装到该目录，cd挂载到该目录
perl configure \
#将上面安装的各类软件目录配置到该目录下
    -hmmer_dir=$(brew --prefix)/bin \
    -rmblast_dir=$(brew --prefix)/Cellar/rmblast@2.10.0/2.10.0/bin \
    -libdir=$(brew --prefix)/Cellar/repeatmasker@4.1.1/4.1.1/libexec/Libraries \
    -trf_prgm=$(brew --prefix)/bin/trf \
    -default_search_engine=rmblast
#默认搜索引擎为rmblast
cd -
#cd -返回上一次的工作目录


#echo "==> Config repeatmasker"
#wget -N -P /tmp https://github.com/egateam/egavm/releases/download/20170907/repeatmaskerlibraries-20140131.tar.gz
#指定下载文件的存放目录为tmp

#pushd $(brew --prefix)/opt/repeatmasker/libexec
#将目录栈中添加相应/opt/repeatmasker/libexec目录，同时当前工作目录调整到相应目录下

#rm -fr lib/perl5/x86_64-linux-thread-multi/
#rm Libraries/RepeatMasker.lib*
#rm Libraries/DfamConsensus.embl
#rm -rf作用是无提示地强制递归删除文件。


#tar zxvf /tmp/repeatmaskerlibraries-20140131.tar.gz
#解压缩命令，将一个tar或者tar.gz结尾的文件解压到指定的目录下

#sed -i".bak" 's/\/usr\/bin\/perl/env/' configure.input
#./configure < configure.input
#popd
#将当前目录切换到上一个父目录，同时从目录栈堆中删除这个子目录

```






## bash $HOME/Scripts/dotfiles/perl/ensembl.sh中内容：

```
#!/usr/bin/env bash

export ENSEMBL_VERSION='105'
#设置ensemble的版本为105

cd /tmp
#一个绝对路径，更改为/tmp目录


if [ ! -e ensembl-${ENSEMBL_VERSION}.tar.gz ]; then
#如果文件ensembl-${ENSEMBL_VERSION}.tar.gz 不存在，则

    echo "==> Get ensembl tarballs"
    wget https://github.com/Ensembl/ensembl/archive/release/${ENSEMBL_VERSION}.tar.gz           -O ensembl-${ENSEMBL_VERSION}.tar.gz
    wget https://github.com/Ensembl/ensembl-compara/archive/release/${ENSEMBL_VERSION}.tar.gz   -O ensembl-compara-${ENSEMBL_VERSION}.tar.gz
    wget https://github.com/Ensembl/ensembl-variation/archive/release/${ENSEMBL_VERSION}.tar.gz -O ensembl-variation-${ENSEMBL_VERSION}.tar.gz
#wget命令是Linux系统用于从Web下载文件的命令行工具，支持 HTTP、HTTPS及FTP协议下载文件
#wget -o 下载指定的URL地址并以指定的文件名保存（o后面跟的文件名称）
fi
#${ENSEMBL_VERSION}=105
#ensemble文件名称分别保存为：/tmp/ensembl-105.tar.gz;/tmp/ensembl-compara-105.tar.gz;/tmp/ensembl-variation-105.tar.gz



SITE_PERL=$(perl -e '($first) = grep {/site_perl/} grep {!/darwin/i and !/x86_64/} @INC; print $first')

echo "==> ensembl"
tar xfz /tmp/ensembl-${ENSEMBL_VERSION}.tar.gz \
    ensembl-release-${ENSEMBL_VERSION}/modules/Bio/EnsEMBL
cp -R ensembl-release-${ENSEMBL_VERSION}/modules/Bio ${SITE_PERL}


#\说明上一行语句还没有结束，可以拐行接着写
#tar 解压 解包+指定压缩文件+解压缩
#将下载的tar.gz文件解压缩到相应目录  将xfz /tmp/ensembl-105.tar.gz解压到ensembl-release-105/modules/Bio/EnsEMBL目录
#cp -r复制文件，如果是目录，不能直接复制，要加上 -r参数，将ensembl-release-105/modules/Bio目录下所有都行复制到${SITE_PERL}下


echo "==> ensembl-compara"
tar xfz /tmp/ensembl-compara-${ENSEMBL_VERSION}.tar.gz \
    ensembl-compara-release-${ENSEMBL_VERSION}/modules/Bio/EnsEMBL
cp -R ensembl-compara-release-${ENSEMBL_VERSION}/modules/Bio ${SITE_PERL}
#将/tmp/ensembl-compara-105.tar.gz解压到ensembl-compara-release-105/modules/Bio/EnsEMBL目录
#将 ensembl-compara-release-105/modules/Bio目录下所有内容都复制到${SITE_PERL}下

echo "==> ensembl-variation"
tar xfz /tmp/ensembl-variation-${ENSEMBL_VERSION}.tar.gz \
    ensembl-variation-release-${ENSEMBL_VERSION}/modules/Bio/EnsEMBL
cp -R ensembl-variation-release-${ENSEMBL_VERSION}/modules/Bio ${SITE_PERL}
#同上

perl -MBio::EnsEMBL::ApiVersion -e 'print qq{If you see this, means ensembl installation successful.\n}'

unset ENSEMBL_VERSION
#unset命令用于删除变量或函数

```








## bash $HOME/Scripts/dotfiles/others.sh内容：
```
#!/usr/bin/env bash

mkdir -p $HOME/share/
#创建$HOME/share/目录

echo "==> TrimGalore"
curl -LO https://raw.githubusercontent.com/FelixKrueger/TrimGalore/master/trim_galore
#跟随网站跳转写入到文件，下载文件


mv trim_galore $HOME/bin
#将trim_galore内容转移到$HOME/bin中


chmod +x $HOME/bin/trim_galore
#赋予用户文件的执行权限

echo "==> circos-tools"
cd $HOME/share/
#将目录挂载到$HOME/share/下
curl -LO http://circos.ca/distribution/circos-tools-0.22.tgz
#在$HOME/share/目录下，下载circos-tools-0.22压缩包


rm -fr circos-tools
#强制删除circos-tools
tar xvfz circos-tools-0.22.tgz
#tar 解包+显示详细信息+指定压缩文件+解压缩
mv circos-tools-0.22 circos-tools
#将解压后的circos-tools-0.22内容转移到circos-tools
rm circos-tools-0.22.tgz
#解压后删除原来的压缩包


echo "==> trinity 2.6.6"
brew install salmon
#安装salmon


#brew install trinity
wget -N -P /tmp https://github.com/trinityrnaseq/trinityrnaseq/archive/Trinity-v2.6.6.tar.gz
#指定下载文件的存放目录，且除非远程文件比较新否则不再取回
#将Trinity-v2.6.6.tar.gz下载到/tmp目录下


cd $HOME/share/
tar xvfz /tmp/Trinity-v2.6.6.tar.gz
#将目录挂载到$HOME/share/下，将Trinity-v2.6.6.tar.gz解压缩到该目录下，即$HOME/share/trinityrnaseq-Trinity-v2.6.6


cd $HOME/share/trinityrnaseq-Trinity-v2.6.6
make
make plugins
#挂载到$HOME/share/trinityrnaseq-Trinity-v2.6.6目录下


sed -i".bak" 's/::Bin/::RealBin/' $HOME/share/trinityrnaseq-Trinity-v2.6.6/Trinity
#将匹配到的每一行的第一个::Bin更改为::RealBin字符，
#把修改内容保存在$HOME/share/trinityrnaseq-Trinity-v2.6.6/Trinity文件中，同时以$HOME/share/trinityrnaseq-Trinity-v2.6.6/Trinity/back文件备份

ln -fs $HOME/share/trinityrnaseq-Trinity-v2.6.6/Trinity $HOME/bin/Trinity
#$HOME/bin/Trinity档案的内容是指向另一个档案$HOME/share/trinityrnaseq-Trinity-v2.6.6/Trinity的位置
#在两个目录$HOME/share/trinityrnaseq-Trinity-v2.6.6和$HOME/bin都含有trinity文件，而不会占用重复空间
#-f : 链结时先将与 dist 同档名的档案删除


echo "==> interproscan"
mkdir -p $HOME/software/interproscan
#创建$HOME/software/interproscan目录
cd $HOME/software/interproscan
#挂载到$HOME/software/interproscan目录


proxychains4 wget -N ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/5.48-83.0/interproscan-5.48-83.0-64-bit.tar.gz
wget ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/5.48-83.0/interproscan-5.48-83.0-64-bit.tar.gz.md5
#在所要运行的命令行之前加上proxychains4就可以通过代理进行网络访问了，相当于加快访问速度
#在网页上下载interproscan-5.48-83.0-64-bit.tar.gz和interproscan-5.48-83.0-64-bit.tar.gz.md5到$HOME/software/interproscan目录


md5sum -c interproscan-5.48-83.0-64-bit.tar.gz.md5
#-c选项来对文件md5进行校验。校验时，根据已生成的md5来进行校验。生成当前文件的md5，并和之前已经生成的md5进行对比，如果一致，则返回OK，否则返回错误信息


# proxychains4 wget -N ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/data/panther-data-12.0.tar.gz
# wget ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/data/panther-data-12.0.tar.gz.md5

# md5sum -c panther-data-12.0.tar.gz.md5

上述三行代码同上
#在网页上下载data/panther-data-12.0.tar.gz和panther-data-12.0.tar.gz.md5到$HOME/software/interproscan目录



# wget -N -P /tmp ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/lookup_service/lookup_service_5.32-71.0.tar.gz
# wget -N -P /tmp ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/lookup_service/lookup_service_5.32-71.0.tar.gz.md5

# md5sum -c lookup_service_5.32-71.0.tar.gz.md5
上述三行代码同上
#在网页上下载lookup_service_5.32-71.0.tar.gz和lookup_service_5.32-71.0.tar.gz.md5到$HOME/software/interproscan目录



cd $HOME/share/
#挂载到$HOME/share/

rm -fr interproscan
#强制删除$HOME/software/interproscan目录及其内容

pigz -dc /$HOME/software/interproscan/interproscan-5.48-83.0-64-bit.tar.gz | pv | tar -pxv -f -
#解压tar.gz压缩包，得到output.tar文件  | x : 从 tar 包中把文件提取出来；v : 显示详细信息；f xxx.tar.gz :  指定被处理的文件是 xxx.tar.gz
mv interproscan-5.48-83.0 interproscan
#将interproscan-5.48-83.0转移到interproscan文件下


# pigz -dc /$HOME/software/interproscan/panther-data-12.0.tar.gz | pv | tar -pxv -f -
# mv panther interproscan/data
#解压panther-data-12.0.tar.gz文件，将panther转移到interproscan/data



# turn off local pre-calculated match lookup service
sed -i '/precalculated.match.lookup.service.url/s/^/#/g' interproscan/interproscan.properties


ln -fs $HOME/share/interproscan/interproscan.sh $HOME/bin/interproscan.sh
#$HOME/bin/interproscan.sh内容同$HOME/share/interproscan/interproscan.sh一致


# Manually download SignalP, TMHMM
# Need to accept licences
# https://services.healthtech.dtu.dk/software.php
tar -xvz -f /$HOME/software/interproscan/signalp-4.1g.Linux.tar.gz
#x : 从 tar 包中把文件提取出来；v : 显示详细信息；z : 表示 tar 包是被 gzip 压缩过的，所以解压时需要用 gunzip 解压；f xxx.tar.gz :  指定被处理的文件是 xxx.tar.gz



mkdir -p interproscan/bin/signalp/4.1/
cp -R signalp-4.1/* interproscan/bin/signalp/4.1/
#-r：若给出的源文件是一个目录文件，此时将复制该目录下所有的子目录和文件。
sed -i '/\$ENV{SIGNALP} =/c\'"\$ENV{SIGNALP} = 'bin/signalp/4.1';" interproscan/bin/signalp/4.1/signalp

#上述步骤,把signalp-4.1g解压,创建interproscan/bin/signalp/4.1/,并且将signalp-4.1/内容转移到interproscan/bin/signalp/4.1/下载
#将\$ENV{SIGNALP} =/c修改为\$ENV{SIGNALP} = 'bin/signalp/4.1



tar -xvz -f /$HOME/software/interproscan/tmhmm-2.0c.Linux.tar.gz
mkdir -p interproscan/bin/tmhmm/2.0c/
cp tmhmm-2.0c/bin/decodeanhmm.Linux_x86_64 interproscan/bin/tmhmm/2.0c/decodeanhmm
mkdir -p interproscan/data/tmhmm/2.0c/
cp tmhmm-2.0c/lib/TMHMM2.0.model interproscan/data/tmhmm/2.0c/TMHMM2.0c.model


cd $HOME/share/interproscan
python3 initial_setup.py


# PPanGGOLiN
# Python 3.7
# We need the testing dataset in the source repo
pip3 install tqdm
pip3 install tables
pip3 install networkx
pip3 install dataclasses
pip3 install scipy
pip3 install plotly
pip3 install gmpy2
pip3 install pandas
pip3 install colorlover
pip3 install numpy
pip3 install bokeh

brew install prodigal
brew install brewsci/bio/aragorn
brew install brewsci/bio/infernal
brew install mmseqs2
brew install mafft


# pip3 install git+https://github.com/labgem/netsyn.git


cd $HOME/share/
rm -fr PPanGGOLiN
#挂载到$HOME/share/，强制删除PPanGGOLiN


git clone https://github.com/labgem/PPanGGOLiN.git


cd PPanGGOLiN
python3 setup.py install


cd testingDataset
ppanggolin workflow --fasta organisms.fasta.list


ppanggolin workflow --anno organisms.gbff.list
```