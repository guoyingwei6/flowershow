---
created: 2024-08-31 16:23
updated: 2025-06-26 16:16
dg-publish: true
---


>**--recode** creates a new text fileset, after applying sample/variant filters and other operations. By default, the fileset includes a [.ped](https://www.cog-genomics.org/plink/1.9/formats#ped) and a [.map](https://www.cog-genomics.org/plink/1.9/formats#map) file, readable with [--file](https://www.cog-genomics.org/plink/1.9/input#ped). Note that the .ped+.map format is NOT a native file format for PLINK 1.9 — any operation on it requires inefficient conversion to .bed+.bim+.fam — and should almost never be used in new scripts. VCF is usually a better choice when you must convert to text.
>The '**AD**' modifier causes an [additive (0/1/2) + dominant (het = 1, otherwise 0) component file](https://www.cog-genomics.org/plink/1.9/formats#raw), suitable for loading from R, to be generated. '**A**' is the same, except without the dominance component.  

 - By default, A1 alleles are counted; this can be customized with **--recode-allele**. --recode-allele's input file should have variant IDs in the first column and allele IDs in the second.
- By default, the header line for .raw files only names the counted alleles. To include the alternate allele codes as well, add the '**include-alt**' modifier.
- Haploid additive components are 0/2-valued instead of 0/1-valued, to maintain a consistent scale on the X chromosome.


--recode A 转成012的raw格式的时候，默认是A1 alleles are counted，即最小等位基因用来计数。

如果想变成以reference allele计数，需要使用`--recode-allele`自定义需要计数的allele

