---
created: 2024-09-24 10:07
updated: 2025-06-10 09:54
dg-publish: true
tags:
  - garden/🌳Evergreen
---
[03.Admixture](03.Admixture.md)

## 介绍

Admixture官方教程对有监督方法的介绍：

>Estimating P and Q from the SNP matrix G, without any additional information, can be viewed as an unsupervised learning problem. However it is not uncommon that some or all of the individuals in our data sample will have known ancestries, allowing us to set some rows in the matrix Q to known constants. This allows more accurate estimation of the ancestries of the remaining individuals, and of the ancestral allele frequencies. Viewing these reference individuals as training samples, the problem is transformed into a supervised learning problem.   
>
>Supervised learning mode is enabled with the flag `--supervised` and requires an additional file with a `.pop` suffix, specifying the ancestries of the reference individuals. It is assumed that all reference samples have 100% ancestry from some ancestral population. Each line of the pop file corresponds to individual listed on the same line number in the `fam` or `ped` file. If the individual is a population reference, the pop file line should be a string (beginning with an alphanumeric character) designating the population. If the individual is of unknown ancestry, use "-" (or a blank line, or any non-alphanumeric character) to indicate that the ancestry should be estimated.

## Usage

```shell
admixture -j8 --cv --supervised angus_limousin.bed 2
```

加上`--supervised` 参数就可以有监督分析，但要提供一个额外的`.pop`文件，和fam文件的样本顺序保持一致，想要作为reference的样本写上群体名字，要作为分析的样本写-或者非字母数字的符号。只需要一列就可以，第二列可有可无。

还是需要指定K的数值，然后pop文件中的群体数目，要等于K。也就是说，有几个祖先群体就要提供几个reference pop。

```
Angus   0203093
Angus   0203094
Angus   0203096
Angus   0203097
Angus   0203098
Angus   0203099
Angus   0203100
-       0203053
-       0203055
-       0203058
-       0203095
-       0203001
-       0203002
Limousin        0203003
Limousin        0203004
Limousin        0203005
Limousin        0203006
Limousin        0203007
Limousin        0203008
Limousin        0203009
Limousin        0203010
Limousin        0203011
```

