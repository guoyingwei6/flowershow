---
title: 【GWAS】利用EMMAX软件进行GWAS
author: Yingwei Guo
tags:
  - garden/🌳Evergreen
created: 2023-09-10 01:08
updated: 2025-06-10 09:55
dg-publish: true
---





Step1 ：


其实所有软件的使用方法基本上都是三部分：软件名字    输入文件   参数    输出文件   ，软件名字不用管，只需要把后面三个搞清楚就可以

1.` /home/cngbdb011/Softwares/miniconda3/bin/plink --file SNP50_Breedv4 --recode --chr-set 26 --keep need_samples --out need_samples`

这一步是把我们需要的样本从大的集合中提取出来，核心参数就是`--keep`，输入文件就是下载的那个大集合，名字就是`SNP50_breedV`  ，输出文件就是我们需要的样本集合，这个可以自定义

 `--file` 选取ped和map文件的前缀

 `--recode` 控制输出格式的参数，recode的话输出文件就是ped和map两个文件，

 `--chr-set` plink是针对人开发设计的，所以它默认文件里面染色体数只能1-23，如果是其他物种的话，你要给他染色体数，不然会报错

 `--keep` 后面跟需要的样本的FID和IID，两列，和ped文件中前两列的格式相同。

 `--out`  就是输出文件的名字，可以自定义，这里我们就叫`need_samples`。这个运行完会看到`need_samples`开头的`ped` `map` 两个文件

2.`/home/cngbdb011/Softwares/miniconda3/bin/plink --file need_samples --output-missing-genotype 0 --recode12 --transpose --out need_samples --allow-extra-chr --chr-set 26`

 这一步给我们的文件转置一下，emmax需要转置后的plink文件格式（本来是ped和map，转置之后就变成tped和tfam了），所以我们就转化一下格式，核心参数就是`--transpose`
 
 `--output-missing-genotype`是说把里面基因型缺失的用0表示，

 `--recode12` 把AGCT这种基因型用12表示，和参考基因组一样的用1，不一样的用2，我不知道是不是一定要变成数字形式的，我看他ppt上转了就跟着转了，你可以试一下不转数字行不行，就是直接去掉这个参数。

 `--transpose`和`--recode`一样都是控制输出文件格式的，recode的话输出文件就是ped和map两个文件，而--transpose就是输出tped和tfam格式的文件，transpose  动词，名词，调换 转置的意思

 `--allow-extra-chr` 里面有XY染色体的话他会报错，加上这个参数的话他就把性染色体变成27 28号了
 这步生成的输出文件是tped和tfam结尾的两个文件

3.`/home/cngbdb011/Softwares/miniconda3/bin/plink --file need_samples --pca --chr-set 26 --maf 0.05 --out need_samples.pca`

 我们GWAS的时候要把群体的分层作为固定效应中的协方差，群体分层就用PCA，也就是主成分分析，他是一种降维手段，把高维的数据解析成效应较高的前几个主要维度

 `--pca`就是这步的主要参数，给这个参数他会自动去做PCA，

 `--maf 0.05` MAF是指minor allele frequency，即最小等位基因频率，MAF过小的话，说明这个SNP在群体中的频率很低，很多个体就没有这个变异，我们先把他过滤掉。
 这步运行完了会生成一个`pca.eigenvec` 和 `pca.eigenval`这两个结尾的文件


4.`awk '{print $1,$2,1,$3,$4,$5}' OFS="\t" need_samples.pca.eigenvec > covariate`

上一步的结果还不是emmax需要的文件格式，它需要家系名字，样本名，1（固定格式）和前三个主成分，所以我们这里用awk简单处理一下，把需要的几列取出来。

5.`/home/cngbdb011/Softwares/emmax/emmax-kin -v -s -d 10 need_samples`

除了群体分层的固定效应，还有亲缘关系所导致的随机效应，所以要计算一下亲缘关系矩阵

 `-v`是指verbose模式，他会把处理过程的详细信息打印到命令行，当然不加这个参数结果也一样。

 `-s`就是计算亲缘关系矩阵的参数

 `-d` precision of the kinship values，亲缘关系的精度，默认是10，我们就用10就好

 最后给输入文件，他会自动生成.aIBS.kinf结尾的输出文件，那个就是亲缘关系矩阵的文件
 这里我们虽然换了一下新软件，但是使用方法还是差不多，大同小异的，也是给输入，参数，输出，只不过输出文件是他默认生成的，我们不需要给（其实也可以给），输入文件放到了最后

6.`/home/cngbdb011/Softwares/emmax/emmax -v -d 10 -t need_samples -p trait -k need_samples.aIBS.kinf -c covariate -o gwas.trait`

前期都是文件准备工作，这一步就是关联分析，

 `-t`参数后面跟 我们转置好的tped和tfam文件的前缀

 `-p`给他表型文件，这个文件是自己做的，基本格式就是 FID IID trait 三列

 比如我们想做高原适应性，可以这样
```
TIB_familyid1 TIB_individulid1 1
TIB_familyid2 TIB_individulid2 1
non_TIB_familyid1 non_TIB_individulid1 0
non_TIB_familyid2 non_TIB_individulid2 0
```

 就是把高原的表型定位1，非高原的表型定位0，，FID和IID我随便写的，你看原文件里面是什么就写什么

 `-k`后面跟上一步算亲缘关系的文件，

 `-c` 后面是协方差那个文件 
 
 `-o` 加输出文件名字

7.`cut -d" " -f1-4 need_samples.tped |paste - gwas.trait.ps | awk '{print $1,$4,$8}' OFS="\t" | sed  '1i\CHR\tBP\tP-VALUE' |awk '$2!=0' > gwas.assoc.txt`

这时候已经有结果了，但是画图的话需要转换一下格式，先用cut把tped文件前四列cut出来，因为里面有样本名，然后和gwas.trait.ps合并在一起，cat是上下合并，paste是左右合并， -是指管道前面的输出，就是上一个命令的输出文件，但是我们没有保存成文件，而是变成了数据流，通过 | 这个管道，他可以直接被下个命令利用和处理。
 然后同样把处理好的管道给下一个命令，awk取出来1 4 8 这三列，然后在第一行加一个表头， sed '1i 什么什么'  就是在第一行进行插入，这里我们插入了CHR\tBP\tP-VALUE  ，，\t是指的tab键，类似于在word里面按tab，
 这时候你会发现前面几个文件他们的位置（就是第二列）都是0，，是因为芯片上的某些点没有在染色体上，这些我们就直接不要就好了，所以awk '$2!=0' 意思是只要第二列不等于0 的  把它重定向到一个文件中gwas.assoc.txt
 ppt中他是分开写了，我合并到一起了，你可以cut -d" " -f1-4 need_samples.tped |less 这样暂时查看一下输出内容，每一步你都可以跟一个less  ，，cut -d" " -f1-4 need_samples.tped |paste - gwas.trait.ps |less  ，，一步一步看下你就知道了。

 8.打开R studio
```
library(qqman)
data <- read.table("C:/Users/Administrator/Desktop/gwas.assoc.txt",header = T) # 读取数去存到data变量里面
colorset<-c("blue4", "orange3")  # 创建一个颜色集合
manhattan(data, CHR = "CHR", BP = "bp", SNP = "SNP", p = "P-VALUE", col = colorset)   # 画曼哈顿图
```

可视化部分参考[曼哈顿图可视化](曼哈顿图可视化.md)



