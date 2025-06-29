---
created: 2024-10-30 22:36
updated: 2025-06-10 09:55
---

```shell
for i in `cat ../01.pi/breed_lst`; do /home/guoyingwei/software/PopLDdecay/bin/PopLDdecay -InVCF /home/guoyingwei/project/LeiQiong_cattle2/00.data/leiqiong.filtered.vcf.gz -SubPop /home/guoyingwei/project/LeiQiong_cattle2/01.diversity/01.pi/$i -MaxDist 1000000 -OutStat $i.stat.gz ;done
perl ~/software/PopLDdecay/bin/Plot_MultiPop.pl -inList lst -output leiqiong
```


