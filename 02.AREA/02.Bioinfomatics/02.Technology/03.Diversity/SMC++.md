---
created: 2024-11-12 15:20
updated: 2025-06-10 09:55
---

## Installation

```
cd ~/software
wget https://github.com/popgenmethods/smcpp/releases/download/v1.15.2/smcpp-1.15.2-Linux-x86_64.sh
bash smcpp-1.15.2-Linux-x86_64.sh
```

```
smcpp will now be installed into this location:
/home/guoyingwei/smcpp

  - Press ENTER to confirm the location
  - Press CTRL-C to abort the installation
  - Or specify a different location below

[/home/guoyingwei/smcpp] >>> /home/guoyingwei/software/smcpp
PREFIX=/home/guoyingwei/software/smcpp
installing: python-3.6.8-h0371630_0 ...
Python 3.6.8 :: Anaconda, Inc.
installing: blas-1.0-mkl ...
installing: ca-certificates-2018.03.07-0 ...
installing: conda-env-2.6.0-1 ...
installing: intel-openmp-2019.1-144 ...
installing: libgcc-ng-8.2.0-hdf63c60_1 ...
installing: libgfortran-ng-7.3.0-hdf63c60_0 ...
installing: libstdcxx-ng-8.2.0-hdf63c60_1 ...
installing: bzip2-1.0.6-h14c3975_5 ...
installing: expat-2.2.6-he6710b0_0 ...
...
...
...
installing: smcpp-1.15.2-py36h6bb024c_0 ...
installing: conda-4.5.12-py36_0 ...

SMC++ has been installed to /home/guoyingwei/software/smcpp. To run it, type:

    $ source /home/guoyingwei/software/smcpp/bin/activate
    $ smc++

installation finished.
Do you wish the installer to initialize smcpp
in your /home/guoyingwei/.bashrc ? [yes|no]
[no] >>> yes

Initializing smcpp in /home/guoyingwei/.bashrc
A backup will be made to: /home/guoyingwei/.bashrc-smcpp.bak


For this change to become active, you have to open a new terminal.

Thank you for installing smcpp!
```

安装完成后，smc++自己创建了一个conda环境，`source /home/guoyingwei/software/smcpp/bin/activate`之后，就可以`smc++`运行软件了。
 
```
smc++ -h
```


## Analysis

### split

#### 雷琼
```shell
# 转换格式
for i in {1..29}; do nohup smc++ vcf2smc /home/guoyingwei/project/LeiQiong_cattle2/00.data/from_raw_vcf/leiqiong.filtered.snp.maf.vcf.gz data/GD.chr${i}.smc.gz ${i} GD:0201281,0201282,0201283,0201284,0201285,0201286,0201287,0201288,0201289,0201290 & done

for i in {1..29}; do nohup smc++ vcf2smc /home/guoyingwei/project/LeiQiong_cattle2/00.data/from_raw_vcf/leiqiong.filtered.snp.maf.vcf.gz data/HD.chr${i}.smc.gz ${i} HN:0203601,0203602,0203603,0203604,0203605,0203606,0203607,0203608,0203609 & done

smc++ estimate -o GD/ 1.2e-8 data/GD.chr*.smc.gz
smc++ estimate -o HN/ 1.2e-8 data/HN.chr*.smc.gz

for i in {1..29}; do nohup smc++ vcf2smc /home/guoyingwei/project/LeiQiong_cattle2/00.data/from_raw_vcf/leiqiong.filtered.snp.maf.vcf.gz data/GD_HN.chr${i}.smc.gz ${i} GD:0201281,0201282,0201283,0201284,0201285,0201286,0201287,0201288,0201289,0201290 HN:0203601,0203602,0203603,0203604,0203605,0203606,0203607,0203608,0203609,0203610 & done

for i in {1..29}; do nohup smc++ vcf2smc /home/guoyingwei/project/LeiQiong_cattle2/00.data/from_raw_vcf/leiqiong.filtered.snp.maf.vcf.gz data/HN_GD.chr${i}.smc.gz ${i} HN:0203601,0203602,0203603,0203604,0203605,0203606,0203607,0203608,0203609,0203610 GD:0201281,0201282,0201283,0201284,0201285,0201286,0201287,0201288,0201289,0201290 & done

smc++ split -o split/ GD/model.final.json HN/model.final.json data/*.smc.gz

smc++ plot split.pdf split/model.final.json
```

#### 湖羊


```shell
smc++ vcf2smc /stor9000/apps/users/NWSUAF/2015010726/Sheep/HuSheep_reseq/00.data2/06.concat/all.chr.snp.filtered.final.vcf.gz data/HUS.smc.gz 1 HUS:CNOA-HUS201,CNOA-HUS202

smc++ vcf2smc /stor9000/apps/users/NWSUAF/2015010726/Sheep/HuSheep_reseq/00.data2/06.concat/all.chr.snp.filtered.final.vcf.gz data/MONG.smc.gz 1 MONG:CNOA-STH01,CNOA-STH02

smc++ estimate -o HUS/ 2.5e-8 data/HUS.smc.gz

smc++ estimate -o MONG/ 2.5e-8 data/MONG.smc.gz

smc++ vcf2smc /stor9000/apps/users/NWSUAF/2015010726/Sheep/HuSheep_reseq/00.data2/06.concat/all.chr.snp.filtered.final.vcf.gz data/HUS_MONG.smc.gz 1 HUS:CNOA-HUS01,CNOA-HUS02 MONG:CNOA-STH01,CNOA-STH02

smc++ vcf2smc /stor9000/apps/users/NWSUAF/2015010726/Sheep/HuSheep_reseq/00.data2/06.concat/all.chr.snp.filtered.final.vcf.gz data/MONG_HUS.smc.gz 1  MONG:CNOA-STH01,CNOA-STH02 HUS:CNOA-HUS01,CNOA-HUS02

smc++ split -o split/ HUS/model.final.json MONG/model.final.json data/*.smc.gz

smc++ plot joint.pdf split/model.final.json
```

### estimate

```shell
# 对49个品种进行分析
# 首先创建目录
for i in  `cat ../../00.data/breed_lst`; do mkdir $i; done
# 生成每个品种的lst，用其中5个个体即可
for i in `cat ../../00.data/breed_lst`; do grep $i ../../00.data/sample_breed |cut -f 1 |head -5 | sed ':t;N;s/\n/,/;b t' > ${i}/${i}_sm_lst;done
# 每个品种，每个染色体的vcf2smc
# 太慢了，每个群体选一个品种作为代表即可
for i in `cat ../../00.data/breed_lst`; do for j in {1..29}; do smc++ vcf2smc /home/guoyingwei/project/Chinese_cattle_sequencing/00.data/vcf/auto.all_samples.filtered.filter_maf.snp.prune.vcf.gz ${i}/${i}.chr${j}.smc.gz ${j} ${i}:`cat ${i}/${i}_sm_lst`;done; done
# estimate
for i in Angus fuzhou hasake qinchuan minnan dianzhong xizang; do smc++ estimate -o ${i}/ 1.2e-8 ${i}/${i}.chr*.smc.gz ;done  2>&1 |tee estimate.log
# 


```