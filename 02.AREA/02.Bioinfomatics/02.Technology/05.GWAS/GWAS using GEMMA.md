---
created: 2024-09-27 09:47
updated: 2025-06-10 09:55
dg-publish: true
tags:
  - garden/🌱Seedling
---

## GEMMA manual

![image.png](https://picbed.guoyingwei.top/2024/09/202409270949006.png)

## 我的脚本

数量性状并且有固定效应的话，直接用上面的MLM，混合线形模型就可以。
```
# 先生成一个关系矩阵
gemma -bfile ../all_samples317.filter.sort  -gk 2  -p tail_length.cut   -o all_samples317.filter.sort

  

gemma -bfile ../all_samples317.filter.sort -k output/all_samples317.filter.sort.sXX.txt -lmm 1 -p tail_length.cut -c covariates_for_GEMMA -o all_samples317

# -b 输入文件可以直接是plink的bed文件的前缀
# -k后面就是上一步的关系矩阵
# -lmm 选择模型
# -p是表型，好像可以把多个性状写到一个文件，然后有个参数控制指定第几个性状的GWAS，当然也可以所有性状一起做。
# -c是协变量或者固定效益，比如PCA的前三列，但是因为要有截距，所以第一列必须是1
# -o输出文件名
```


## 别人小麦数据的流程

```shell
##### For the wheatdata

# To calculate centered relatedness matrix:

./gemma -g wheatdata.geno.txt -p wheatdata.pheno.txt -gk 1 -o wheatdata

# The estimated relatedness matrix should look like this:

0.1934280 0.0192288 0.0180713 ...

0.0192288 0.2026770 0.1999510 ...

0.0180713 0.1999510 0.2034990 ...

.................................

# To perform Wald tests with centered relatedness matrix:

./gemma -g wheatdata.geno.txt -p wheatdata.pheno.txt -k ./output/wheatdata.cXX.txt -fa 1 -o wheatdata

# The result for top 10 SNPs should look like this:

     chr       rs ps n_miss       beta        se  l_remle       p_wald

74    -9 wPt.2185 -9      0  1.0840510 0.2602580 4.916977 3.568595e-05

718   -9 c.304701 -9      0 -1.0852800 0.2876071 5.358550 1.770458e-04

1137  -9 c.376463 -9      0  0.7702204 0.2117813 4.865404 2.998277e-04

577   -9 wPt.2448 -9      0  0.7607078 0.2131378 4.926681 3.870148e-04

423   -9 wPt.2087 -9      0  0.7294579 0.2129142 4.857752 6.544632e-04

596   -9 wPt.4533 -9      0  0.7355019 0.2180191 5.067510 7.900759e-04

885   -9 c.345107 -9      0  0.6629963 0.1992821 5.447568 9.320548e-04

1242  -9 c.380286 -9      0 -0.9898943 0.3038434 5.531822 1.186062e-03

866   -9 c.344809 -9      0 -0.4568435 0.1457309 5.434799 1.803903e-03

158   -9 wPt.3697 -9      0  0.4700972 0.1571677 5.394384 2.894810e-03

# The above results are included in the result folder.

##### For the mousedata

# To perform a linear regression

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt.gz -p mouse_hs1940.pheno.txt -a mouse_hs1940.anno.txt -lm 1 -o mouse_hs1940_lm

# To calculate centered relatedness matrix (will take ~ 1 min):

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p mouse_hs1940.pheno.txt -a mouse_hs1940.anno.txt -gk 1 -o mouse_hs1940

../gemma_v094/bin/gemma -bfile mouse_hs1940 -gk 1 -o mouse_hs1940_plink

# The estimated relatedness matrix should look like this:

0.3350590  -0.0227226  0.0103535 ...

-0.0227226  0.3035960 -0.0253762 ...

0.0103535  -0.0253762  0.3536100 ...

....................................

# To perform association tests with centered relatedness matrix (will take ~ 1 min):

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p mouse_hs1940.pheno.txt -n 1 -a mouse_hs1940.anno.txt -k ./output/mouse_hs1940.cXX.txt -lmm 4 -o mouse_hs1940_CD8

../gemma_v094/bin/gemma -bfile mouse_hs1940 -n 1 -k ./output/mouse_hs1940_plink.cXX.txt -lmm 4 -o mouse_hs1940_CD8_plink

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p mouse_hs1940.pheno.txt -n 1 -a mouse_hs1940.anno.txt -k ./output/mouse_hs1940.cXX.txt -lmm 4 -o mouse_hs1940_CD8_rs -snps mouse_hs1940.rs.txt

../gemma_v094/bin/gemma -bfile mouse_hs1940 -n 1 -k ./output/mouse_hs1940_plink.cXX.txt -lmm 4 -o mouse_hs1940_CD8_plink_rs -snps mouse_hs1940.rs.txt

# The result for top 10 SNPs should look like this (and a strong signal in chr17):

chr rs ps n_miss beta se l_remle p_wald

1 rs3683945 0 0 -7.788676e-02 6.193497e-02 4.317971e+00 2.087605e-01

1 rs3707673 0 0 -6.654296e-02 6.210229e-02 4.316122e+00 2.841257e-01

1 rs6269442 0 0 -5.344258e-02 5.377462e-02 4.323589e+00 3.204786e-01

1 rs6336442 0 0 -6.770167e-02 6.209261e-02 4.315691e+00 2.757527e-01

1 rs13475700 0 0 -5.659112e-02 7.175371e-02 4.340122e+00 4.304285e-01

1 rs3658242 0 0 -6.654296e-02 6.210229e-02 4.316122e+00 2.841257e-01

1 rs13475701 0 0 -4.551830e-02 6.708041e-02 4.330571e+00 4.975262e-01

1 rs6198069 0 0 -1.247314e-02 5.695145e-02 4.337159e+00 8.266713e-01

1 rs3659303 0 0 -4.210892e-02 6.185598e-02 4.320492e+00 4.961369e-01

tail:

19 rs13483698 0 0 4.447868e-02 4.307612e-02 4.308242e+00 4.308139e+00 3.019872e-01 3.020774e-01 3.028694e-01

19 UT_4_59.889299 0 0 1.674025e-02 8.341767e-02 4.330446e+00 4.333743e+00 8.409778e-01 8.409939e-01 8.410721e-01

19 UT_4_59.889271 0 0 1.674025e-02 8.341767e-02 4.330446e+00 4.333743e+00 8.409778e-01 8.409939e-01 8.410721e-01

19 rs3718998 0 0 1.674025e-02 8.341767e-02 4.330446e+00 4.333743e+00 8.409778e-01 8.409939e-01 8.410721e-01

19 rs13483699 0 0 5.287170e-02 4.312367e-02 4.307190e+00 4.307226e+00 2.203855e-01 2.203540e-01 2.210596e-01

19 rs3694467 0 0 1.504437e-02 8.338114e-02 4.330843e+00 4.334129e+00 8.568418e-01 8.568605e-01 8.569279e-01

19 rs6228270 0 0 1.689030e-02 8.340833e-02 4.330378e+00 4.333679e+00 8.395540e-01 8.395719e-01 8.396520e-01

19 mCV23482939 0 0 1.674025e-02 8.341767e-02 4.330446e+00 4.333743e+00 8.409778e-01 8.409939e-01 8.410721e-01

19 mCV23489377 0 0 6.946138e-02 4.528945e-02 4.308380e+00 4.308324e+00 1.253229e-01 1.251699e-01 1.257350e-01

19 rs6193060 0 0 6.840924e-02 4.626476e-02 4.319364e+00 4.323849e+00 1.394584e-01 1.391791e-01 1.395980e-01

# The log file also contains pve estimates (and its standard error):

## pve estimate in the null model = 0.608891

## se(pve) in the null model = 0.0327712

## To perform association tests with a multivariate linear mixed model, for two phenotypes CD8 (column 1) and MCH (column 6):

## Notice that the number of individuals in this analysis is different from that above, so the allele frequencies are different between the two analyses

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p mouse_hs1940.pheno.txt -n 1 6 -a mouse_hs1940.anno.txt -k ./output/mouse_hs1940.cXX.txt -lmm -o mouse_hs1940_CD8MCH_lmm

../gemma_v094/bin/gemma -bfile mouse_hs1940 -n 1 6 -k ./output/mouse_hs1940_plink.cXX.txt -lmm -o mouse_hs1940_CD8MCH_lmm_plink

# The result for top 5 SNPs should look like this:

chr rs ps n_miss allele1 allele0 af beta_1 beta_2 Vbeta_1_1 Vbeta_1_2 Vbeta_2_2 p_wald

1 rs3683945 3197400 0 A G 0.451 -9.611213e-02 8.165302e-02 3.966873e-03 -2.526118e-04 5.540032e-03 1.862363e-01

1 rs3707673 3407393 0 G A 0.451 -8.464470e-02 7.130876e-02 3.986286e-03 -2.593467e-04 5.571616e-03 2.757067e-01

1 rs6269442 3492195 0 A G 0.377 -7.146771e-02 5.179252e-02 3.157023e-03 -7.187157e-05 4.276041e-03 3.317712e-01

1 rs6336442 3580634 0 A G 0.451 -8.502513e-02 6.813728e-02 3.985054e-03 -2.577585e-04 5.568602e-03 2.835426e-01

1 rs13475700 4098402 0 A C 0.128 -6.727883e-02 1.685363e-01 5.597160e-03 -1.366799e-04 7.574216e-03 1.060482e-01

# The log file also contains Vg and Ve estimates and their standard errors

## REMLE estimate for Vg in the null model: 

1.39398

-0.226714 2.08168

## se(Vg): 

0.156661

0.136319 0.235858

## REMLE estimate for Ve in the null model: 

0.348882

0.0490525 0.414433

## se(Ve): 

0.0206226

0.0166233 0.0266869

# Since there are individuals with partially missing phenotypes, one can impute these missing values before association tests

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p mouse_hs1940.pheno.txt -n 1 6 -a mouse_hs1940.anno.txt -k ./output/mouse_hs1940.cXX.txt -predict -o mouse_hs1940_CD8MCH_prdt

../gemma_v094/bin/gemma -bfile mouse_hs1940 -n 1 6 -k ./output/mouse_hs1940_plink.cXX.txt -predict -o mouse_hs1940_CD8MCH_prdt_plink

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p ./output/mouse_hs1940_CD8MCH_prdt.prdt.txt -n 1 2 -a mouse_hs1940.anno.txt -k ./output/mouse_hs1940.cXX.txt -lmm -o mouse_hs1940_CD8MCH_prdt_lmm

# to fit bslmm in the training set (takes ~ 1 min):

# for quantitative trait

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p mouse_hs1940.pheno.txt -n 2 -a mouse_hs1940.anno.txt -bslmm 1 -o mouse_hs1940_CD8_bslmm -w 1000 -s 10000 -seed 1

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p mouse_hs1940.pheno.txt -n 2 -a mouse_hs1940.anno.txt -bslmm 1 -o mouse_hs1940_CD8_bslmm_r0 -w 1000 -s 10000 -seed 1 -rmin 0 -rmax 0 

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p mouse_hs1940.pheno.txt -n 2 -a mouse_hs1940.anno.txt -bslmm 1 -o mouse_hs1940_CD8_bslmm_r1 -w 1000 -s 10000 -seed 1 -rmin 1 -rmax 1 

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p mouse_hs1940.pheno.txt -n 2 -a mouse_hs1940.anno.txt -bslmm 2 -o mouse_hs1940_CD8_bslmm_rr

../gemma_v094/bin/gemma -bfile mouse_hs1940 -n 2 -bslmm 1 -o mouse_hs1940_CD8_bslmm_plink -w 1000 -s 10000 -seed 1

../gemma_v094/bin/gemma -bfile mouse_hs1940 -n 2 -bslmm 1 -o mouse_hs1940_CD8_bslmm_plink_r0 -w 1000 -s 10000 -seed 1 -rmin 0 -rmax 0 

../gemma_v094/bin/gemma -bfile mouse_hs1940 -n 2 -bslmm 1 -o mouse_hs1940_CD8_bslmm_plink_r1 -w 1000 -s 10000 -seed 1 -rmin 1 -rmax 1 

../gemma_v094/bin/gemma -bfile mouse_hs1940 -n 2 -bslmm 2 -o mouse_hs1940_CD8_bslmm_plink_rr

# generate relatedness matrix based on training data

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p mouse_hs1940.pheno.txt -n 2 -a mouse_hs1940.anno.txt -gk 1 -o mouse_hs1940_CD8_train

../gemma_v094/bin/gemma -bfile mouse_hs1940 -n 2 -gk 1 -o mouse_hs1940_CD8_train_plink

# obtain predicted value for missing phenotypes in the training set

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p mouse_hs1940.pheno.txt -n 2 -epm ./output/mouse_hs1940_CD8_bslmm.param.txt -predict -o mouse_hs1940_CD8_prdt -emu ./output/mouse_hs1940_CD8_bslmm.log.txt

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p mouse_hs1940.pheno.txt -n 2 -epm ./output/mouse_hs1940_CD8_bslmm_r0.param.txt -predict -o mouse_hs1940_CD8_r0_prdt

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p mouse_hs1940.pheno.txt -n 2 -epm ./output/mouse_hs1940_CD8_bslmm_r1.param.txt -predict -o mouse_hs1940_CD8_r1_prdt

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p mouse_hs1940.pheno.txt -n 2 -epm ./output/mouse_hs1940_CD8_bslmm_rr.param.txt -predict -o mouse_hs1940_CD8_rr_prdt

../gemma_v094/bin/gemma -bfile mouse_hs1940 -n 2 -epm ./output/mouse_hs1940_CD8_bslmm_plink.param.txt -predict -o mouse_hs1940_CD8_plink_prdt

../gemma_v094/bin/gemma -bfile mouse_hs1940 -n 2 -epm ./output/mouse_hs1940_CD8_bslmm_plink_r0.param.txt -predict -o mouse_hs1940_CD8_plink_r0_prdt

../gemma_v094/bin/gemma -bfile mouse_hs1940 -n 2 -epm ./output/mouse_hs1940_CD8_bslmm_plink_r1.param.txt -predict -o mouse_hs1940_CD8_plink_r1_prdt

../gemma_v094/bin/gemma -bfile mouse_hs1940 -n 2 -epm ./output/mouse_hs1940_CD8_bslmm_plink_rr.param.txt -predict -o mouse_hs1940_CD8_plink_rr_prdt

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p mouse_hs1940.pheno.txt -n 2 -epm ./output/mouse_hs1940_CD8_bslmm.param.txt -ebv ./output/mouse_hs1940_CD8_bslmm.bv.txt -k ./output/mouse_hs1940_CD8_train.cXX.txt -predict -o mouse_hs1940_CD8_prdt_k

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p mouse_hs1940.pheno.txt -n 2 -epm ./output/mouse_hs1940_CD8_bslmm_r0.param.txt -ebv ./output/mouse_hs1940_CD8_bslmm_r0.bv.txt -k ./output/mouse_hs1940_CD8_train.cXX.txt -predict -o mouse_hs1940_CD8_r0_prdt_k

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p mouse_hs1940.pheno.txt -n 2 -epm ./output/mouse_hs1940_CD8_bslmm_r1.param.txt -ebv ./output/mouse_hs1940_CD8_bslmm_r1.bv.txt -k ./output/mouse_hs1940_CD8_train.cXX.txt -predict -o mouse_hs1940_CD8_r1_prdt_k

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p mouse_hs1940.pheno.txt -n 2 -epm ./output/mouse_hs1940_CD8_bslmm_rr.param.txt -ebv ./output/mouse_hs1940_CD8_bslmm_rr.bv.txt -k ./output/mouse_hs1940_CD8_train.cXX.txt -predict -o mouse_hs1940_CD8_rr_prdt_k

../gemma_v094/bin/gemma -bfile mouse_hs1940 -n 2 -epm ./output/mouse_hs1940_CD8_bslmm_plink.param.txt -ebv ./output/mouse_hs1940_CD8_bslmm_plink.bv.txt -k ./output/mouse_hs1940_CD8_train_plink.cXX.txt -predict -o mouse_hs1940_CD8_plink_prdt_k

../gemma_v094/bin/gemma -bfile mouse_hs1940 -n 2 -epm ./output/mouse_hs1940_CD8_bslmm_plink_r0.param.txt -ebv ./output/mouse_hs1940_CD8_bslmm_plink_r0.bv.txt -k ./output/mouse_hs1940_CD8_train_plink.cXX.txt -predict -o mouse_hs1940_CD8_plink_r0_prdt_k

../gemma_v094/bin/gemma -bfile mouse_hs1940 -n 2 -epm ./output/mouse_hs1940_CD8_bslmm_plink_r1.param.txt -ebv ./output/mouse_hs1940_CD8_bslmm_plink_r1.bv.txt -k ./output/mouse_hs1940_CD8_train_plink.cXX.txt -predict -o mouse_hs1940_CD8_plink_r1_prdt_k

../gemma_v094/bin/gemma -bfile mouse_hs1940 -n 2 -epm ./output/mouse_hs1940_CD8_bslmm_plink_rr.param.txt -ebv ./output/mouse_hs1940_CD8_bslmm_plink_rr.bv.txt -k ./output/mouse_hs1940_CD8_train_plink.cXX.txt -predict -o mouse_hs1940_CD8_plink_rr_prdt_k

# for case control data

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p mouse_hs1940_CD8.pheno.txt -n 4 -a mouse_hs1940.anno.txt -bslmm 3 -o mouse_hs1940_CD8_bslmm_cc -w 1000 -s 10000 -seed 1 -smax 100

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p mouse_hs1940_CD8.pheno.txt -n 4 -a mouse_hs1940.anno.txt -bslmm 3 -o mouse_hs1940_CD8_bslmm_cc_r0 -w 1000 -s 10000 -seed 1 -rmin 0 -rmax 0 

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p mouse_hs1940_CD8.pheno.txt -n 4 -a mouse_hs1940.anno.txt -bslmm 3 -o mouse_hs1940_CD8_bslmm_cc_r1 -w 1000 -s 10000 -seed 1 -rmin 1 -rmax 1 -smax 100

../gemma_v094/bin/gemma -bfile mouse_hs1940 -n 4 -bslmm 3 -o mouse_hs1940_CD8_bslmm_cc_plink -w 1000 -s 10000 -seed 1 -smax 100

../gemma_v094/bin/gemma -bfile mouse_hs1940 -n 4 -bslmm 3 -o mouse_hs1940_CD8_bslmm_cc_plink_r0 -w 1000 -s 10000 -seed 1 -rmin 0 -rmax 0 -smax 100 

../gemma_v094/bin/gemma -bfile mouse_hs1940 -n 4 -bslmm 3 -o mouse_hs1940_CD8_bslmm_cc_plink_r1 -w 1000 -s 10000 -seed 1 -rmin 1 -rmax 1 -smax 100

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p mouse_hs1940_CD8.pheno.txt -n 4 -epm ./output/mouse_hs1940_CD8_bslmm_cc.param.txt -ebv ./output/mouse_hs1940_CD8_bslmm_cc.bv.txt -k ./output/mouse_hs1940_CD8_train.cXX.txt -emu ./output/mouse_hs1940_CD8_bslmm_cc.log.txt -predict 2 -o mouse_hs1940_CD8_cc_prdt_k

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p mouse_hs1940_CD8.pheno.txt -n 4 -epm ./output/mouse_hs1940_CD8_bslmm_cc_r0.param.txt -ebv ./output/mouse_hs1940_CD8_bslmm_cc_r0.bv.txt -k ./output/mouse_hs1940_CD8_train.cXX.txt -emu ./output/mouse_hs1940_CD8_bslmm_cc_r0.log.txt -predict 2 -o mouse_hs1940_CD8_cc_r0_prdt_k

../gemma_v094/bin/gemma -g mouse_hs1940.geno.txt -p mouse_hs1940_CD8.pheno.txt -n 4 -epm ./output/mouse_hs1940_CD8_bslmm_cc_r1.param.txt -ebv ./output/mouse_hs1940_CD8_bslmm_cc_r1.bv.txt -k ./output/mouse_hs1940_CD8_train.cXX.txt -emu ./output/mouse_hs1940_CD8_bslmm_cc_r1.log.txt -predict 2 -o mouse_hs1940_CD8_cc_r1_prdt_k

../gemma_v094/bin/gemma -bfile mouse_hs1940 -n 4 -epm ./output/mouse_hs1940_CD8_bslmm_cc_plink.param.txt -ebv ./output/mouse_hs1940_CD8_bslmm_cc_plink.bv.txt -k ./output/mouse_hs1940_CD8_train.cXX.txt -emu ./output/mouse_hs1940_CD8_bslmm_cc_plink.log.txt -predict 2 -o mouse_hs1940_CD8_cc_plink_prdt_k

../gemma_v094/bin/gemma -bfile mouse_hs1940 -n 4 -epm ./output/mouse_hs1940_CD8_bslmm_cc_plink_r0.param.txt -ebv ./output/mouse_hs1940_CD8_bslmm_cc_plink_r0.bv.txt -k ./output/mouse_hs1940_CD8_train.cXX.txt -emu ./output/mouse_hs1940_CD8_bslmm_cc_plink_r0.log.txt -predict 2 -o mouse_hs1940_CD8_cc_plink_r0_prdt_k

../gemma_v094/bin/gemma -bfile mouse_hs1940 -n 4 -epm ./output/mouse_hs1940_CD8_bslmm_cc_plink_r1.param.txt -ebv ./output/mouse_hs1940_CD8_bslmm_cc_plink_r1.bv.txt -k ./output/mouse_hs1940_CD8_train.cXX.txt -emu ./output/mouse_hs1940_CD8_bslmm_cc_plink_r1.log.txt -predict 2 -o mouse_hs1940_CD8_cc_plink_r1_prdt_k

# compare the predicted value with true value in the test set, correlation will be around 0.65

y_prdt=read.table("~/Documents/Code/example/output/mouse_hs1940_CD8_prdt.prdt.txt")

y_prdt_r0=read.table("~/Documents/Code/example/output/mouse_hs1940_CD8_r0_prdt.prdt.txt")

y_prdt_r1=read.table("~/Documents/Code/example/output/mouse_hs1940_CD8_r1_prdt.prdt.txt")

y_prdt_rr=read.table("~/Documents/Code/example/output/mouse_hs1940_CD8_rr_prdt.prdt.txt")

y_prdt_p=read.table("~/Documents/Code/example/output/mouse_hs1940_CD8_plink_prdt.prdt.txt")

y_prdt_r0_p=read.table("~/Documents/Code/example/output/mouse_hs1940_CD8_plink_r0_prdt.prdt.txt")

y_prdt_r1_p=read.table("~/Documents/Code/example/output/mouse_hs1940_CD8_plink_r1_prdt.prdt.txt")

y_prdt_rr_p=read.table("~/Documents/Code/example/output/mouse_hs1940_CD8_plink_rr_prdt.prdt.txt")

y_prdt_k=read.table("~/Documents/Code/example/output/mouse_hs1940_CD8_prdt_k.prdt.txt")

y_prdt_r0_k=read.table("~/Documents/Code/example/output/mouse_hs1940_CD8_r0_prdt_k.prdt.txt")

y_prdt_r1_k=read.table("~/Documents/Code/example/output/mouse_hs1940_CD8_r1_prdt_k.prdt.txt")

y_prdt_rr_k=read.table("~/Documents/Code/example/output/mouse_hs1940_CD8_rr_prdt_k.prdt.txt")

y_prdt_p_k=read.table("~/Documents/Code/example/output/mouse_hs1940_CD8_plink_prdt_k.prdt.txt")

y_prdt_r0_p_k=read.table("~/Documents/Code/example/output/mouse_hs1940_CD8_plink_r0_prdt_k.prdt.txt")

y_prdt_r1_p_k=read.table("~/Documents/Code/example/output/mouse_hs1940_CD8_plink_r1_prdt_k.prdt.txt")

y_prdt_rr_p_k=read.table("~/Documents/Code/example/output/mouse_hs1940_CD8_plink_rr_prdt_k.prdt.txt")

y_true=read.table("~/Documents/Code/example/mouse_hs1940_CD8.pheno.txt")

s=!is.na(y_true$V3)

cor(y_true$V3[s],y_prdt$V1[s])

cor(y_true$V3[s],y_prdt_r0$V1[s])

cor(y_true$V3[s],y_prdt_r1$V1[s])

cor(y_true$V3[s],y_prdt_rr$V1[s])

cor(y_true$V3[s],y_prdt_p$V1[s])

cor(y_true$V3[s],y_prdt_r0_p$V1[s])

cor(y_true$V3[s],y_prdt_r1_p$V1[s])

cor(y_true$V3[s],y_prdt_rr_p$V1[s])

cor(y_true$V3[s],y_prdt_k$V1[s])

cor(y_true$V3[s],y_prdt_r0_k$V1[s])

cor(y_true$V3[s],y_prdt_r1_k$V1[s])

cor(y_true$V3[s],y_prdt_rr_k$V1[s])

cor(y_true$V3[s],y_prdt_p_k$V1[s])

cor(y_true$V3[s],y_prdt_r0_p_k$V1[s])

cor(y_true$V3[s],y_prdt_r1_p_k$V1[s])

cor(y_true$V3[s],y_prdt_rr_p_k$V1[s])

y_prdt_cc_k=read.table("~/Documents/Code/example/output/mouse_hs1940_CD8_cc_prdt_k.prdt.txt")

y_prdt_cc_r0_k=read.table("~/Documents/Code/example/output/mouse_hs1940_CD8_cc_r0_prdt_k.prdt.txt")

y_prdt_cc_r1_k=read.table("~/Documents/Code/example/output/mouse_hs1940_CD8_cc_r1_prdt_k.prdt.txt")

y_prdt_cc_p_k=read.table("~/Documents/Code/example/output/mouse_hs1940_CD8_cc_plink_prdt_k.prdt.txt")

y_prdt_cc_r0_p_k=read.table("~/Documents/Code/example/output/mouse_hs1940_CD8_cc_plink_r0_prdt_k.prdt.txt")

y_prdt_cc_r1_p_k=read.table("~/Documents/Code/example/output/mouse_hs1940_CD8_cc_plink_r1_prdt_k.prdt.txt")

y_true=read.table("~/Documents/Code/example/mouse_hs1940_CD8.pheno.txt")

s=!is.na(y_true$V5)

mean((y_true$V5[s]-y_prdt_cc_k$V1[s])^2)

mean((y_true$V5[s]-y_prdt_cc_r0_k$V1[s])^2)

mean((y_true$V5[s]-y_prdt_cc_r1_k$V1[s])^2)

mean((y_true$V5[s]-y_prdt_cc_p_k$V1[s])^2)

mean((y_true$V5[s]-y_prdt_cc_r0_p_k$V1[s])^2)

mean((y_true$V5[s]-y_prdt_cc_r1_p_k$V1[s])^2)

mean((y_true$V5[s]-pnorm(y_prdt_cc_k$V1[s]))^2)

mean((y_true$V5[s]-pnorm(y_prdt_cc_r0_k$V1[s]))^2)

mean((y_true$V5[s]-pnorm(y_prdt_cc_r1_k$V1[s]))^2)

mean((y_true$V5[s]-pnorm(y_prdt_cc_p_k$V1[s]))^2)

mean((y_true$V5[s]-pnorm(y_prdt_cc_r0_p_k$V1[s]))^2)

mean((y_true$V5[s]-pnorm(y_prdt_cc_r1_p_k$V1[s]))^2)

# Below we will show how to estimate variance components using either individual-level data or summary statistics. Note that the mouse data is not a good data for demonstrating the variance component estimation methods because of (1) high relatedness among individuals a\

nd (2) relatively large snp effect sizes. In particular, the HE estimates will be noisy and the HE estimates from using summary statistics will be upward biased due to large snp effect sizes (which makes marginal z-score approximation less accurate). Some of the followi\

ng analyses will work much better in a human data.

# To estimate variance components using relatedness matrices

../gemma_v094/bin/gemma -p mouse_hs1940.pheno.txt -k ./output/mouse_hs1940.cXX.txt -vc 1 -o mouse_hs1940_he

../gemma_v094/bin/gemma -p mouse_hs1940.pheno.txt -k ./output/mouse_hs1940.cXX.txt -vc 2 -o mouse_hs1940_reml

# The reml pve estimate is: pve estimates =   0.608803; se(pve) =   0.0323024. Both are consistent with -lmm analysis option.

# The HE pve estimate is: pve estimates =   1.21001; se(pve) =   0.791357. The pve estimate is above 1 and with a very large standard error. This is consistent with our expectation (in the MQS paper) that the HE estimate from very related individuals is not accurate.

# To estimate variance components using summary statistics, we use the lm output file to provide marginal z-scores, and use all individuals or randomly select 500 individuals to provide individual-level genotypes.

../gemma_v094/bin/gemma -beta ./output/mouse_hs1940_lm.assoc.txt -g mouse_hs1940.geno.txt.gz -p mouse_hs1940.pheno.txt -vc 1 -o mouse_hs1940_he_summary

../gemma_v094/bin/gemma -beta ./output/mouse_hs1940_lm.assoc.txt -g mouse_hs1940.geno.txt.gz -p mouse_hs1940.pheno.txt -sample 500 -seed 1 -vc 1 -o mouse_hs1940_he_summary_500

# The pve estimate from using all individuals is: pve estimates =   1.34681; se(pve) =   0.0800072. Using 500 individuals will yield similar results. Because of high individual relatedness, using a random subset of individuals in the mouse data will generally be less ac\

curate than that in a human data.

# Note that unlike humans, the SNP effect sizes in the mouse data is often large, thus using z-scores will over-estimate pve (i.e. > 1.21001). In addition, the standard error (0.0800072) is computed with the CI2 method via z-score permuation and is expected to be not ac\

curate in related individuals. To compute the exact standard error with the CI1, you can use

../gemma_v094/bin/gemma -beta ./output/mouse_hs1940_lm.assoc.txt -g mouse_hs1940.geno.txt.gz -p mouse_hs1940.pheno.txt -ref output/mouse_hs1940_he_summary -pve 1.34681 -ci 1 -o mouse_hs1940_he_ci

# Now you will see that the standard error gets back to 0.811461, which is very close to 0.791357.

```