---
created: 2025-06-26 11:05
updated: 2025-06-26 16:16
dg-publish: true
---

总是提示缺少库



```
conda install -c bioconda -c conda-forge \
  bcftools=1.22 \
  htslib=1.22 \
  libcurl=8.14.0 \
  zlib=1.3.1 -y
```

好像可以指定各种库的版本进行安装
