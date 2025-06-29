---
created: 2024-06-04 20:58
updated: 2025-06-10 09:55
dg-publish: true
---
> 近交系数（inbreeding coefficient）是指根据近亲交配的世代数，将基因的纯化程度用百分数来表示即为近交系数，也指个体由于近交而造成异质基因减少时，同质基因或纯合子所占的百分比也叫近交系数，普遍以F或f来表示。


```shell
vcftools --gzvcf /workspace/home/cattle/guoyw/project/Chinese_cattle_sequencing/00.data/vcf/auto.all_samples.filtered.filter_maf.snp.prune.vcf.gz --keep /workspace/home/cattle/guoyw/project/Chinese_cattle_sequencing/02.selection/00.sample_info/breed_lst/${1} --maf 0.05 --het --out output/${1}
for i in `ls /workspace/home/cattle/guoyw/project/Chinese_cattle_sequencing/02.selection/00.sample_info/breed_lst/`; do dsub -R "cpu=2;mem=1024" -o log/$i.o -e log/$i.e --labels aarch64 bash het.sh $i ;done
```