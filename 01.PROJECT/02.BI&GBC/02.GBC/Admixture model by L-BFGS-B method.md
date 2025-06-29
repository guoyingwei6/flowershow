---
created: 2024-12-19 21:48
updated: 2025-06-10 09:55
dg-publish: true
tags:
  - garden/🌱Seedling
---
## Description
> The admixture model estimated GBC by the weights of an underlying admixture distribution that governed the realization of genotypes for each animal [7]. Let there be M SNPs genotyped in T reference breeds. Assume that the allelic frequencies of these SNPs are known for these reference populations. Denote xjk as the frequency of allele A for the k-th SNP in the j-th reference population. Then, weighed allele frequency of the k-th SNP across the T reference populations was fk = ∑jT=1 wjxjk, where ωj was the weight for the j-reference population. Assuming Hardy–Weinberg equilibrium at each SNP, the probability of observing a genotype, denoted by gk, is the following:
> 
> $$P_r(g_k | f_k) = \begin{cases} 
> (1 - f_k)^2 & \text{if } g_k = 0 \\
> 2f_k(1 - f_k) & \text{if } g_k = 1 \\
> f_k^2 & \text{if } g_k = 2
> \end{cases}$$
> 
> Assuming mutual independence between the M SNPs, the log-likelihood (denoted by l) computed for M SNPs genotyped on this animal is defined as follows:
> 
> $$l = \sum_{k=1}^M \ln(P_r(g_k | f_k)) = \left[ \sum_{k=1}^M g_k \ln(f_k) + (2 - g_k) \ln(1 - f_k) \right] + C$$
> 
> where
> 
> $$C = \sum_{k=1}^M \ln \left( \frac{2}{g_k} \right)$$
> 
> .
> 
> Admixture coefficients, w = (w1 . . . wT)′, were obtained using the Broyden-Fletcher-Goldfarb-Shanno (BFGS) method under the restrictions that wk ≥ 0 and ∑kT=1 wT = 0 [90]. BFGS is a quasi-Newton second derivative line search family method, one of the most powerful methods for solving nonlinear optimization problems. By optimizing the likelihood function of BFGS, it iteratively removed the nonzero mixing coefficient, which did not significantly improve the model fitting, and thus obtained a concise set of individual mixture coefficients. In the admixture model, the value of each admixture coefficient was between 0 and 1. The sum of admixture coefficients computed for each animal is always one under the assumption of 100% genetic contributions by the T known breeds to each animal. (Yin et al., 2023, p. 17)

## Code 两个祖先群体

```python
import pandas as pd
import numpy as np
from scipy.optimize import minimize

def read_allele_freqs(freq_file):
    # 读取等位基因频率文件，第一列为'CHR:POS'，其他列为各群体等位基因频率
    # index_col='CHR:POS' 将SNP标识设为行索引
    allele_freqs = pd.read_table(freq_file, sep='\s+', header=0, index_col='CHR:POS')
    # allele_freqs_matrix: shape = (num_snps, T)
    allele_freqs_matrix = allele_freqs.values
    return allele_freqs, allele_freqs_matrix

def read_genotypes(gt_file):
    # 读取个体基因型文件，一列为'CHR:POS'，后面为各个体列
    genotypes = pd.read_table(gt_file, sep='\s+', header=0)
    genotypes.set_index('CHR:POS', inplace=True)
    # genotypes_matrix: shape = (num_snps, num_samples)
    # columns为样本名，rows为SNP
    return genotypes

def negative_log_likelihood(w_scalar, allele_freqs_matrix, genotypes):
    w1 = w_scalar[0]
    if w1 < 0 or w1 > 1:
        return np.inf
    w2 = 1 - w1
    w = np.array([w1, w2])
    
    f = allele_freqs_matrix.dot(w)
    epsilon = 1e-8  # 增大epsilon
    f = np.clip(f, epsilon, 1 - epsilon)

    g = genotypes
    mask0 = (g == 0)
    mask1 = (g == 1)
    mask2 = (g == 2)

    lnP = np.zeros_like(g, dtype=float)
    lnP[mask0] = 2 * np.log(1 - f[mask0])
    lnP[mask1] = np.log(2) + np.log(f[mask1]) + np.log(1 - f[mask1])
    lnP[mask2] = 2 * np.log(f[mask2])

    # 检查lnP是否有NaN
    if np.isnan(lnP).any():
        return np.inf  # 避免NaN导致优化崩溃

    ll = np.sum(lnP)
    return -ll

def estimate_admixture(allele_freqs_matrix, genotypes):
    # 假设T=2，使用单参数w1表示w=[w1, 1-w1]
    bounds = [(0,1)]
    res = minimize(negative_log_likelihood, [0.5],
                   args=(allele_freqs_matrix, genotypes),
                   method='L-BFGS-B', bounds=bounds, options={'disp': False})
    if res.success:
        w1 = res.x[0]
        w2 = 1 - w1
        return np.array([w1, w2])
    else:
        return None

def main():
    # 文件路径
    freq_file = 'RAD.merge_MP.frq.fmt'  # 等位基因频率文件
    gt_file = 'FinalSNP.filteredmaf01.rename_chr.ex_dup.phased.keepUN.raw.trans.fmt' # 基因型文件
    
    
    # 读取等位基因频率数据
    allele_freqs, allele_freqs_matrix = read_allele_freqs(freq_file)
    T = allele_freqs_matrix.shape[1]  # 群体数目，这里假设为2，请确保数据中为2列

    # 读取基因型数据
    genotypes = read_genotypes(gt_file)
    
    # 对齐SNP
    common_snps = allele_freqs.index.intersection(genotypes.index)
    filtered_allele_freqs = allele_freqs.loc[common_snps]
    filtered_genotypes = genotypes.loc[common_snps]
    
    # 获取样本列表
    samples = filtered_genotypes.columns
    
    contributions_dict = {}

    # 对每个样本估计其祖先成分
    for sample_name in samples:
        g = filtered_genotypes[sample_name].values
        w = estimate_admixture(filtered_allele_freqs.values, g)
        contributions_dict[sample_name] = w

    # 准备将结果保存为 CSV
    # 创建一个列表，用于存储最终结果的数据行
    results_list = []

    for sample_name, w in contributions_dict.items():
        if w is not None:
            # w 是一个包含两个祖先成分比例的数组，如 w = [w_M, w_P]
            w_m = w[0]
            w_p = w[1]
            results_list.append([sample_name, w_m, w_p])
        else:
            # 如果优化失败，可以选择跳过或给出默认值
            # 这里假设失败的结果不写入文件，也可以写入 NaN
            results_list.append([sample_name, None, None])
            pass

    # 将结果转为 DataFrame
    df_results = pd.DataFrame(results_list, columns=["SM", "M", "P"])

    # 保存为 CSV 文件
    output_csv = "admixture_results.csv"
    df_results.to_csv(output_csv, index=False)

    print(f"Results saved to {output_csv}")

if __name__ == "__main__":
    main()
```



## File format

同上

