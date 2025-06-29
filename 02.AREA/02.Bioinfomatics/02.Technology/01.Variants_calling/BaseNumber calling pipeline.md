---
created: 2024-06-04 20:58
updated: 2025-06-10 09:54
dg-publish: true
tags:
  - garden/🌱Seedling
---


```shell
#!/bin/bash
# cat 01.sla_slc.sh

############# config #############
reference=/data/project/cattle/reference_UCD2.0/GCF_002263795.3_ARS-UCD2.0_genomic.rename.fna
#reference=/data/project/cattle/reference_UCD1.3/GCF_002263795.2_ARS-UCD1.3_genomic.rename.fna
#reference=/data/saile/fastq/Duck_data/ref/GCF_015476345.1_ZJU1.0_genomic.fna
output=/data1/project/cattle/output
bam_split=/data1/project/cattle/bam_split
sm=${1}
fq1=${2}
fq2=${3}
##################################


bts=`date +%s`
/data/saile/cmd/slat \
    -R "@RG\tID:${sm}\tSM:${sm}\tLB:${sm}\tPL:ILLUMINA" \
    -B ${bam_split} \
    --filter-enable \
    --r1-adapter AGATCGGAAGAGCACACGTCTGAACTCCAGTCA \
    --r2-adapter AGATCGGAAGAGCGTCGTGTAGGGAAAGAGTGT \
    --cut-head \
    --cut-tail \
    -i ${output}/${sm}.fq.metrics \
    --sl-log-path /data/project/cattle/log/sla/${sm}_sla \
    ${reference} ${fq1} ${fq2}
ets=`date +%s`
slats=`expr $ets - $bts`


bts=`date +%s`
/data/saile/cmd/slc \
    -P ${bam_split} \
    -R ${reference} \
    -o ${output}/${sm}.gvcf.gz \
    -e GVCF  \
    --vcf-index \
    --with-metrics \
    --metrics-file ${output}/${sm}.dedup.bam.metrics \
    --with-flagstat \
    --flagstat-file ${output}/${sm}.flagstat \
    --sl-log-path /data/project/cattle/log/slc/${sm}_slc
ets=`date +%s`
slcts=`expr $ets - $bts`

echo "${sm},${slats},${slcts}" >> running_time/running_time.csv

```

cmd：

```shell
# 02.cat slmgvcf.sh
bts=`date +%s`
/data/saile/cmd/slmgvcf_gpu_t \
-l /data/project/cattle/log/slmgvcf/${1} \
-o ${1}.merged.gvcf.gz \
# -g ${1}.merged.vcf.gz \  # 直接输出vcf
-r /data/project/cattle/reference_UCD2.0/GCF_002263795.3_ARS-UCD2.0_genomic.rename.fna \
$(ls /data1/project/cattle/output/*.gvcf.gz)
ets=`date +%s`
slcts=`expr $ets - $bts`
echo "${1}:${slcts}" >> ${1}_slmgvcf.log
```

cmd：

```shell
# cat slmgvcf.calling.sh
/data/saile/cmd/slmgvcf_gpu_t \
-l ./slmgvcf_calling.log \
-r /data/project/cattle/reference_UCD2.0/GCF_002263795.3_ARS-UCD2.0_genomic.rename.fna \
-b 40 \  #把大于40Mb的染色体单独split出来一个文件，小于40Mb的放一个文件，实测Y只有10Mb，也被单独分出来了
-g all_samples.vcf.gz \
$(cat merged_gvcfs_lst)
```



```shell
# slf.sh
for i in {1..34}; do /data/saile/cmd/slf -r /data/project/cattle/reference_UCD2.0/GCF_002263795.3_ARS-UCD2.0_genomic.rename.fna -f/data2/project/cattle/split_vcfs/split.${i}.all_samples.vcf.gz -n 'lowquality' -p -e 'QD<2.0 or QUAL < 30.0 or SOR > 3.0 or FS > 60.0 or MQ < 40.0 or MQRankSum < -12.5 or ReadPosRankSum < -8.0' split.${i}.all_samples.filtered.vcf.gz; done

for i in {1..34}; do boftools view -v snps -i 'F_MISSING <  0.1 & MAF > 0.001' -m2 -M2 split.${i}.all_samples.filtered.vcf.gz -Oz -o split.${i}.all_samples.filtered.filtered.vcf.gz; done

```

