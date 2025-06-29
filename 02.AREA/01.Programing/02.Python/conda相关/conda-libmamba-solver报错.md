---
created: 2025-06-11 16:46
updated: 2025-06-15 22:45
---

运行conda出现以下错误：
```
Error while loading conda entry point: conda-libmamba-solver (/lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.33' not found (required by /home/guoyingwei/software/miniconda3/lib/python3.12/site-packages/libmambapy/../../../libstdc++.so.6))
```
解决方式：

暂时通过classic途径解析，并更新libstdcxx


```
CONDA_SOLVER=classic conda install -c conda-forge libstdcxx-ng=12.3.0 -y
```
安装完成后，conda即可正常使用。