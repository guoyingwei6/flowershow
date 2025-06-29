---
created: 2025-06-26 11:08
updated: 2025-06-26 16:16
---



```shell
bcftools plugin -lv

-- GTisec --
Count genotype intersections across all possible sample subsets in a vcf file.

-- GTsubset --
Output only sites where the requested samples all exclusively share a genotype (GT).

-- ad-bias --
Find positions with wildly varying ALT allele frequency (Fisher test on FMT/AD).

-- add-variantkey --
Add VariantKey INFO fields VKX and RSX.

-- af-dist --
AF and GT probability distribution stats.

-- allele-length --
Count the frequency of the length of REF, ALT and REF+ALT

-- check-ploidy --
Check if ploidy of samples is consistent for all sites

-- check-sparsity --
Print samples without genotypes in a region or chromosome

-- color-chrs --
Color shared chromosomal segments, requires phased GTs.

-- contrast --
Find novel alleles and genotypes in two groups of samples.

-- counts --
A minimal plugin which counts number of samples, SNPs,
INDELs, MNPs and total number of sites.

-- dosage --
Prints genotype dosage determined from tags requested by the user.

-- fill-AN-AC --
Fill INFO fields AN and AC. This plugin is DEPRECATED, use fill-tags instead.

-- fill-from-fasta --
Fill INFO or REF field based on values in a fasta file

-- fill-tags --
Set INFO tags AF, AC, AC_Hemi, AC_Hom, AC_Het, AN, ExcHet, HWE, MAF, NS; FORMAT/VAF and more.

-- fixploidy --
Fix ploidy.

-- fixref --
Fix reference strand orientation, e.g. from Illumina/TOP to fwd.

-- frameshifts --
Annotate frameshift indels.

-- guess-ploidy --
Determine sample sex by checking genotype likelihoods in haploid regions.

-- gvcfz --
Compress gVCF file by resizing gVCF blocks according to specified criteria.

-- impute-info --
Add imputation information metrics to the INFO field based on selected FORMAT tags.

-- indel-stats --
Calculate indel stats scanning over a range of thresholds simultaneously.

-- isecGT --
Compare two files and set non-identical genotypes to missing.

-- mendelian2 --
Count Mendelian consistent / inconsistent genotypes.

-- missing2ref --
Set missing genotypes ("./.") to ref or major allele ("0/0" or "0|0").

-- parental-origin --
Determine parental origin of a CNV region in a trio.

-- prune --
Annotate sites with or prune sites by linkage disequilibrium or number of sites within a window

-- remove-overlaps --
Remove, list or mark overlapping variants

-- scatter --
Scatter VCF by chunks or regions, creating multiple VCFs.

-- setGT --
Set genotypes: partially missing to missing, missing to ref/major allele, etc.

-- smpl-stats --
Calculate basic per-sample stats scanning over a range of thresholds simultaneously.

-- split --
Split VCF by sample, creating single- or multi-sample VCFs

-- split-vep --
Query structured annotations such as INFO/CSQ produced by VEP of INFO/BCSQ produced by bctools/csq.

-- tag2tag --
Convert between similar tags, such as GL,PL,GP or QR,QA,QS or localized alleles, eg PL,LPL.

-- trio-dnm2 --
Screen variants for possible de-novo mutations in trios.

-- trio-stats --
Calculate transmission rate and other stats in trio children.

-- trio-switch-rate --
Calculate phase switch rate in trio samples, children samples must have phased GTs.

-- variant-distance --
Annotate sites with distance to the nearest variant

-- variantkey-hex --
Generate VariantKey index files

-- vcf2table --
Convert VCF to tables in the terminal.
Author Pierre Lindenbaum PhD. Institut-du-Thorax. U1087. Nantes/France
Options:
   -h|--help (string)  help (this screen).
   -x|--hide (string)  comma separated list of features to hide:
                       HOM_REF or RR : genotypes with REF allele only
                       HET or AR : heterozygous genotypes
                       NO_CALL or MISSING : missing genotypes
                       CSQ or VEP : VEP table
                       SPLICEAI : SPLICEAI table
                       ANN or SNPEFF : SNPEFF table
                       LOF: SNPEFF LOF table
                       VC: general table
                       INFO: INFO table
                       GT: Genotype table
                       GTTYPES: Genotype count table
                       URL: hyperlink table

Example:
$ wget -O - 'http://ftp.1000genomes.ebi.ac.uk/vol1/ftp/data_collections/1000_genomes_project/release/20190312_biallelic_SNV_and_INDEL/ALL.chr22.shapeit2_integrated_snvindels_v2a_27022019.GRCh38.phased.vcf.gz' |\
	bcftools +vcf2table -i 'AC<10' -- --hide 'HOM_REF,INFO,NO_CALL'
(...)
<<< 22:10714247:C (n. 446)

# Variant
+--------+----------+
| KEY    | VALUE    |
+--------+----------+
| CHROM  | 22       |
| POS    | 10714247 |
| ID     | .        |
| REF    | C        |
| ALT    | G        |
| QUAL   | .        |
| FILTER | PASS     |
+--------+----------+

# GENOTYPE TYPES
+-----------+-------+----------+
| Type      | Count | %        |
+-----------+-------+----------+
| REF only  | 2545  | 99.8823  |
| HET       | 3     | 0.117739 |
+-----------+-------+----------+

# GENOTYPES
+---------+-------+-----+
| SAMPLE  | GTYPE | GT  |
+---------+-------+-----+
| HG03136 | HET   | 1|0 |
| HG03171 | HET   | 0|1 |
| HG03270 | HET   | 0|1 |
+---------+-------+-----+

>>> 22:10714247:C (n. 446)

-- vrfs --
Localised assessment of sequencing artefacts, estimate site noisiness (variant read frequency score)



```
