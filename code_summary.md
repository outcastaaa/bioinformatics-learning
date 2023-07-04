# bash code summary

- [循环](#循环)
- [批量替换后缀](#批量替换后缀)
- [批量下载](#批量下载)
- [传输文件](#传输文件)
- [提取列](#提取列)
- [查找文件](#查找文件)
- [统计](#统计)
- [表格内容处理](#表格内容处理)
- [对行处理](#对行处理)


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

# 批量替换后缀
```bash
paralle -k -j 4 "
  ....{1}...
" ::: $(ls *.gz | perl -p -e 's/.fa.gz$//')
```

# 批量下载
```bash
aria2c -d ./ -Z 1.fq.gz 2.fq.gz
```


# 传输文件
1. rasync
```bash
# 网页下载
rsync -avP ftp.... ./本地文件
```
2. 


# 提取列
1. cut
```bash
cut -f 1 # 提取第一列
```
2. awk
```bash
cat 1.lsit | awk  -F "\t" '{print $1, $2, $3}' > 1.txt
```

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

# 对行处理
1. 删除行
```bash
# 删除第一行
cat 1.txt | sed -d '1d'
# 删除NA行
sed -i "/^NA/d" 1.txt
```