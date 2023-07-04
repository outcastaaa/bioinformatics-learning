# bash code summary

- [循环](#循环)
- [批量替换后缀](#批量替换后缀)
- [批量下载](#批量下载)
- [传输文件](#传输文件)
- [查找文件](#查找文件)
- [统计](#统计)
- [表格内容处理](#表格内容处理)
- [对列处理](#对列处理)
- [对行处理](#对行处理)
- [排序](#排序)


# 循环
1. paralle
```bash
paralle -j 4 -k "
...{1}
" ::: $(ls *.gz)
```
2. while大循环
```bash
# 把每一个样本直接写入完整程序
cat id.list | while read id
do
  echo ""
done > a.sh
```
```bash
# 每个样本都变为一共小任务
cat 1.sh
# !/usr/bin/bash
.....{}.....

cat sample.list | while read id 
do 
  sed 's/{}/${id}/g' 1.sh > ${id}.sh
done

# 提交到超算
cat sample.list | while read id 
do 
  bsub -q mpi -n 48 -o ./ "
    bash ${id}.sh
"
done
```
3. for 循环
```bash
for i in ${ls *.gz};do
  ...{i}
done
```

4. 读取双端数据的循环
```bash
ls *_1.fq.gz >./1
ls *_2.fq.gz >./2
paste 1 1 2 | sed 's/_1.fastq.gz//' >config.raw

cat config.raw | while read id 
do
  echo ${id}
  arr = (${id})
  fq1 = ${arr[1]}
  fq2 = ${arr[2]}

  ...$fq1 ...$fq2
done
```

# 批量替换后缀
1. 替换删除
```bash
paralle -k -j 4 "
  ....{1}...
" ::: $(ls *.gz | perl -p -e 's/.fa.gz$//')
```
2. 批量添加后缀
```bash
cat  MOUSE.list | perl -ne ' \
chomp;
print "$_" . ".sra\n";
'
```
3. 删除后缀
```bash
ls *.bam | while read id 
do 
  ..${id%%.*}.bw
done
```

# 批量下载
```bash
aria2c -d ./ -Z 1.fq.gz 2.fq.gz
```


# 传输文件
1. rsync
```bash
# 网页下载
rsync -avP ftp.... ./本地文件
# 超算传输到本地
rsync -av /mnt/d/brain  wangq@202.119.37.251:/scratch/wangq/xrz/..
```
2. 



# 查找文件
1. where vs which 
which 命令查找可执行文件的位置，只在 PATH 环境变量中搜索，并返回第一个匹配项。
where 命令也查找可执行文件的位置，但它会搜索系统中的所有目录，并返回所有匹配项。  

2. find
```bash
find ./ -maxdepth 1 -type f -name "[0-9]*.list" 
```

# 统计
1. 存在多少种情况统计
```bash
tsv-summarize -g 1 --count
```
2. 对列进行加减计算
```bash
cat 1.txt | awk -F "\t" '{print $1, ($2-1),($3-1)}' > 1.txt
```



# 表格内容处理
1. 添加表头
```bash
# 在"methylation_result.txt"添加colnames:
# 1. 手动添加
chr           start            end     methyl%  methyled   unmethyled

# 2. bash添加
(echo -e "chr\tstart\tend\tmethyl%\tmethyled\tunmethyled " && cat ./NC_result.txt) > temp && mv temp NC_methylation_result.txt
```


# 对列处理
1. cut
```bash
# 提取第一列
cut -f 1 
# 提取某几列
cut -f 1,2,3 1.txt > 1.txt
```
2. awk
```bash
# 提取某几列
cat 1.lsit | awk  -F "\t" '{print $1, $2, $3}' > 1.txt
# 筛选、提取、计算
cat 1.list | awk -v OFS='\t' \
'{if($6 == "-"){print $1,$2+4,$3+4} else if($6 == "+"){print $1, $2-5, $3-5}}' >1.txt
# -v 自己定义变量

# 筛选第几列的数值大小
awk '{if ($5 >= 540) {print $0 }}' a.txt > a.txt
```
3. tsv-filter
```bash
tsv-filter --is-numeric 5 --ge 5:540 a.txt > a.txt
```
3. 列之间加减
```bash
# 统计peak长度
cat a.bed | perl -ne '
chomp;
my @info = split( /\t/, $_ );
my $result = ( $info[2] - $info[1] );
print "$_\t$result\n";
' | head
```


# 对行处理
1. 删除行
```bash
# 删除第一行
cat 1.txt | sed -d '1d'
# 删除NA行
sed -i "/^NA/d" 1.txt
```

# 排序
1. sort 
```bash
sort -k8,8nr 1.txt > 1.txt
# -k8,8nr 表示按照第 8 列进行排序
# -k8,8 表示使用第 8 列作为排序键，n 表示按照数字排序，r 表示以逆序排序（降序）

sort -k1,1 -k2,2n a.txt
# 先对第一列排，再对第二列
```

