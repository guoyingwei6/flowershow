---
created: 2024-12-19 22:51
updated: 2025-06-10 09:55
---

## Introduction

The admixture model estimated GBC by the weights of an underlying admixture distribution that governed the realization of genotypes for each animal [7]. Let there be M SNPs genotyped in T reference breeds. Assume that the allelic frequencies of these SNPs are known for these reference populations. Denote xjk as the frequency of allele A for the k-th SNP in the j-th reference population. Then, weighed allele frequency of the k-th SNP across the T reference populations was fk = ∑jT=1 wjxjk, where ωj was the weight for the j-reference population. Assuming Hardy–Weinberg equilibrium at each SNP, the probability of observing a genotype, denoted by gk, is the following:
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
> Admixture coefficients, **w = (w1 … wT)′**, can also be estimated using the Sequential Least Squares Programming (SLSQP) method under the constraints that **wk ≥ 0** and **∑k=1^T wk = 1**. Unlike methods requiring indirect parameterizations, SLSQP directly enforces these conditions during optimization, ensuring that w remains on the probability simplex. By maximizing the likelihood function, SLSQP iteratively refines the admixture coefficients until they fit the model well. In the admixture model, each coefficient thus lies between 0 and 1, and their sum is always one, reflecting the complete genetic contributions from the T known breeds for each individual.


### SLSQP方法介绍

SLSQP is an SQP method: it replaces the original problem with a sequence of Quadratic Problems (QP) whose objective are second-order approximations of the Lagrangian and whose constraints are the linearized original constraints.




### 为什么SLSQP方法比L-BFGS-B方法要好

1. **约束类型不同**：

您的优化问题要求祖先成分比例向量 w 的每个分量非负且总和为1。这属于具有等式约束（sum(w)=1）和不等式约束（w_j≥0）的优化问题。

• **L-BFGS-B**：

L-BFGS-B 是一种基于准牛顿法的优化方法，它支持简单的上下界约束（box constraints），即可以为每个参数指定下界和上界范围（如0≤w_j≤1），但不直接支持更复杂的等式约束（例如 sum(w)=1 这种线性等式约束）。如果需要强制 sum(w)=1，使用 L-BFGS-B 方法时只能采用参数化（如softmax）或自行转换参数的策略。这在实际数值优化时可能导致数值不稳定或收敛困难。

• **SLSQP**：

SLSQP（Sequential Least Squares Programming）方法是一个同时支持一般约束（包括等式和不等式线性约束和非线性约束）的优化算法。您可以直接将 sum(w) = 1 等式约束和 w_j≥0 不等式约束传给 SLSQP。SLSQP在内部使用了增广拉格朗日或序列二次规划的思路，可以更加顺畅地处理这些约束，从而在给定问题中更轻松地达到可行解并实现收敛。

2. **参数空间与约束处理**：

在含有 sum(w)=1 这类约束的问题中，若使用 L-BFGS-B，就必须在目标函数内部通过参数转换（如使用softmax）来间接满足约束。这会增加目标函数的复杂度和数值不稳定性，进而可能导致出现大量的 NaN、inf 或精度损失问题，优化器容易陷入困境。

而 SLSQP 可以直接接受线性等式约束，将这些约束显式传递给优化器，使得优化过程更透明和稳定，减少了对f的极端值和log(0)等数值问题的敏感性，也就降低了出现“精度损失”的可能性。

3. **精度与数值稳定性**：

当存在严格的等式和不等式约束时，SLSQP能更好地在可行域内搜索最优解，避免无谓地探索不可行区域。这减少了出现极端概率值（接近0或1）的情况，更有利于维持数值稳定性和精度，从而提高收敛的稳定性和成功率。

  

综上，在您的特定问题中，SLSQP方法比L-BFGS-B方法要好是因为：

• 您的问题存在需要严格满足的等式约束（sum(w)=1）和非负约束（w_j≥0）。SLSQP能够原生处理这些约束。

• SLSQP可以更直接地在可行域中进行搜索，而不必通过复杂的参数化或softmax函数来间接满足约束，这减少了数值不稳定与优化失败的可能。

• 因此，SLSQP在这个有约束的祖先成分估计问题中表现更为稳定和高效。

### 不管是SLSQP还是BFGS方法，他们和 admixture model的关系是什么？

无论是 SLSQP 还是 BFGS，这些优化方法本身并不属于“admixture model”框架的一部分，而是为实现该模型目标所采用的数学求解工具。在 admixture model 中，我们的目标是估计每个个体在多个参考群体中的祖先成分比例（admixture coefficients），这些比例要满足非负且总和为 1 的约束条件。为此，需要最大化（或最小化）与这些参数相关的目标函数——通常是基于观测基因型数据和已知群体等位基因频率所定义的似然函数。

• **admixture model**：提供问题的生物学与统计学框架。它假设个体的基因型数据来源于若干已知参考群体的混合，并根据等位基因频率与Hardy-Weinberg平衡假设构建似然函数。优化的目标是找到一组祖先成分参数，使得该模型最大程度地解释观察到的基因型数据。

• **SLSQP或BFGS等优化方法**：则是在这个框架下，为了求解该参数估计问题而引入的数学工具。它们是通用的数值优化算法，用于在给定的约束条件和目标函数下寻找最优参数值。

	• BFGS 是一种准牛顿法，不直接处理复杂约束，但可通过间接参数化来满足要求。
	• SLSQP 是一种可直接处理线性与非线性约束的优化方法，可让参数始终在可行域内搜索解。

简单来说，**admixture model** 为优化问题提供了目标函数和约束条件的生物统计描述（即祖先成分比例及其限制），而 **SLSQP或BFGS** 则是实现求解这一优化问题的数值方法和工具。


## File format
同线性模型

## Code 支持多个祖先群体

```python
import pandas as pd
import numpy as np
from scipy.optimize import minimize

def read_allele_freqs(freq_file):
    """
    读取等位基因频率文件，'CHR:POS'为索引，后面列是各群体的等位基因频率
    """
    allele_freqs = pd.read_table(freq_file, sep='\s+', header=0, index_col='CHR:POS')
    allele_freqs_matrix = allele_freqs.values
    return allele_freqs, allele_freqs_matrix

def read_genotypes(gt_file):
    """
    读取个体基因型文件，'CHR:POS'为索引，其余为个体列
    """
    genotypes = pd.read_table(gt_file, sep='\s+', header=0)
    genotypes.set_index('CHR:POS', inplace=True)
    return genotypes

def negative_log_likelihood(w, allele_freqs_matrix, genotypes):
    """
    计算平均负对数似然
    w: 祖先成分比例向量，长度为T
    allele_freqs_matrix: (num_snps, T)
    genotypes: (num_snps,)
    """
    # 确保w是numpy数组
    w = np.array(w)
    
    # 计算加权等位基因频率 f_k = sum_j w_j * x_jk
    f = allele_freqs_matrix.dot(w)  # (num_snps,)
    
    # 增加数值稳定性，避免f=0或f=1
    epsilon = 1e-8
    f = np.clip(f, epsilon, 1 - epsilon)
    
    # 基因型
    g = genotypes  # (num_snps,) 的基因型0,1,2
    
    # 计算对数概率
    # g=0: lnP = 2 * ln(1 - f)
    # g=1: lnP = ln(2) + ln(f) + ln(1 - f)
    # g=2: lnP = 2 * ln(f)
    
    lnP = np.zeros_like(g, dtype=float)
    mask0 = (g == 0)
    mask1 = (g == 1)
    mask2 = (g == 2)
    
    lnP[mask0] = 2 * np.log(1 - f[mask0])
    lnP[mask1] = np.log(2) + np.log(f[mask1]) + np.log(1 - f[mask1])
    lnP[mask2] = 2 * np.log(f[mask2])
    
    # 检查是否有NaN或inf
    if np.isnan(lnP).any() or np.isinf(lnP).any():
        print(f"Encountered NaN or inf: w={w}")
        return np.inf
    
    # 计算平均负对数似然
    ll = np.sum(lnP)
    avg_nll = -ll / len(g)
    return avg_nll

def estimate_admixture(allele_freqs_matrix, genotypes, T):
    """
    估计祖先成分比例向量w
    使用SLSQP方法，设置约束：w_j >= 0，sum(w_j) = 1
    """
    # 初始值为均匀分布
    init_w = np.full(T, 1.0 / T)
    
    # 约束条件
    constraints = ({
        'type': 'eq',
        'fun': lambda w: np.sum(w) - 1
    })
    
    # 边界条件
    bounds = [(0, 1) for _ in range(T)]
    
    # 优化
    res = minimize(negative_log_likelihood, init_w,
                   args=(allele_freqs_matrix, genotypes),
                   method='SLSQP',
                   bounds=bounds,
                   constraints=constraints,
                   options={'disp': False, 'maxiter': 1000})
    
    if res.success:
        return res.x
    else:
        print(f"Optimization failed: {res.message}")
        return None

def main():
    # 文件路径（请根据实际情况修改）
    freq_file = 'RAD.merge_MP.frq.fmt'  # 等位基因频率文件
    gt_file = 'FinalSNP.filteredmaf01.rename_chr.ex_dup.phased.keepUN.raw.trans.fmt' # 基因型文件
    
    # 读取等位基因频率数据
    allele_freqs, allele_freqs_matrix = read_allele_freqs(freq_file)
    T = allele_freqs_matrix.shape[1]  # 群体数目
    
    # 读取基因型数据
    genotypes = read_genotypes(gt_file)
    
    # 对齐SNP
    common_snps = allele_freqs.index.intersection(genotypes.index)
    filtered_allele_freqs = allele_freqs.loc[common_snps]
    filtered_genotypes = genotypes.loc[common_snps]
    
    # 数据清洗：检查缺失值
    if filtered_allele_freqs.isnull().values.any():
        print("等位基因频率数据中存在缺失值。已删除含缺失值的SNP。")
        filtered_allele_freqs.dropna(inplace=True)
        filtered_genotypes = filtered_genotypes.loc[filtered_allele_freqs.index]
    
    if filtered_genotypes.isnull().values.any():
        print("基因型数据中存在缺失值。已删除含缺失值的SNP。")
        filtered_genotypes.dropna(inplace=True)
        filtered_allele_freqs = filtered_allele_freqs.loc[filtered_genotypes.index]
    
    # 确保等位基因频率在[0,1]范围内
    if ((filtered_allele_freqs < 0).any().any() or (filtered_allele_freqs > 1).any().any()):
        print("等位基因频率数据中存在不在[0,1]范围内的值。已裁剪到[0,1]范围。")
        filtered_allele_freqs = filtered_allele_freqs.clip(lower=0, upper=1)
    
    
    # 获取样本列表
    samples = filtered_genotypes.columns
    
    contributions_dict = {}
    
    # 对每个样本估计其祖先成分
    for sample_name in samples:
        g = filtered_genotypes[sample_name].values
        # 检查基因型是否为0,1,2
        if not np.all(np.isin(g, [0,1,2])):
            print(f"Sample {sample_name} contains invalid genotypes. Skipping.")
            contributions_dict[sample_name] = [None] * T
            continue
        w = estimate_admixture(filtered_allele_freqs.values, g, T)
        contributions_dict[sample_name] = w
    
    # 准备将结果保存为 CSV
    # 列名为 "SM" 加上参考群体名称
    col_names = ["SM"] + list(allele_freqs.columns)
    
    results_list = []
    for sample_name, w in contributions_dict.items():
        if w is not None:
            # w是长度为T的数组，每个元素是对应群体的祖先比例
            results_list.append([sample_name] + list(w))
        else:
            # 优化失败用None表示
            none_list = [None]*T
            results_list.append([sample_name] + none_list)
    
    df_results = pd.DataFrame(results_list, columns=col_names)
    output_csv = "admixture_results.csv"
    df_results.to_csv(output_csv, index=False)
    
    print(f"Results saved to {output_csv}")

if __name__ == "__main__":
    main()
```


