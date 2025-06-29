---
created: 2024-08-14 01:38
updated: 2025-06-23 09:24
dg-publish: true
link: https://guoyingwei6.notion.site/GBC-bbd2f88a4f144164b71dc273abf1ffd3
---
讲解见[利用线性模型估计GBC](利用线性模型估计GBC.md)



## Description

We estimated the GBC using the following linear model based on the genotype data:

$$y=Fb+e$$

Where  is the genotype vector  of all  SNPs of the individual to be estimated, and the SNP genotypes are represented by 0 (AA), 1 (AB), and 2 (BB) respectively.  is the allele frequency matrix of , where  is the number of breeds in the reference population. The regression coefficient  ( ) vector is the GBC of each breed to the individual to be estimated. Then, we solved the linear model using ordinary least squares (OLS) regression, where . We normalized the regression coefficients according to a previous study (He et al., 2018). After the adjustment, all negative regression coefficients were replaced by zeroes and minor contributions were filtered out based on a confidence threshold. Finally, the GBC of a breed was estimated from the ratio of that breed’s regression coefficient over the sum of regression coefficients across all breeds.


## File format
等位基因频率文件格式：
```
CHR:POS	M	P
1:208308	0.181818	0
1:208309	0	0.152174
1:224028	0.232143	0.0416667
1:224038	0.259259	0
1:230229	0.0555556	0.479167
1:234481	0.240741	0
1:234612	0.44	0
1:238247	0.240741	0.0416667
1:238321	0.0357143	0.291667
1:266494	0.339286	0
```

待测个体基因型文件格式：

```
CHR:POS	SM01	SM02
1:208308	1	0
1:208309	0	2
1:224028	1	0
1:224038	2	0
1:230229	0	0
1:234481	1	0
1:234612	0	0
1:238247	0	1
1:238321	2	1
1:266494	2	0
```






## Code
```R
# 读取基因型和等位基因频率数据
genotypes <- scan("gt")  # 假设基因型文件是纯文本格式head
head(genotypes)

# 读取等位基因频率文件，带有列名
allele_freqs <- as.matrix(read.table("x", header=FALSE))  
head(allele_freqs)

# 检查数据维度
if (length(genotypes) != nrow(allele_freqs)) {
  stop("基因型数据和等位基因频率数据的维度不匹配!")
}

# 构建线性模型，无截距
# -1 不是指数值，而是告诉R要用无截距模型。用来指定模型中不包含截距项
# 因为多了截距之后，会有多个解
model <- lm(genotypes ~ allele_freqs - 1)

# 查看模型摘要
summary(model)


# 提取系数（b向量）
coefficients <- coef(model)

# 将负系数转换为0
coefficients[coefficients < 0] <- 0

# 计算每个品种的遗传贡献比例
contributions <- coefficients / sum(coefficients)

# 输出结果
contributions

```

还存在问题：
- 这只是一个个体的求解，怎么把多个品种批量做

用python实现了一版，可以多个个体，每行一个：

```python
import numpy as np
import pandas as pd
import statsmodels.api as sm

# 读取等位基因频率数据
allele_freqs = pd.read_table('x', header=None)
# 强制转换为数值类型，错误转换为NaN
allele_freqs = allele_freqs.apply(pd.to_numeric, errors='coerce').fillna(0)
allele_freqs_matrix = allele_freqs.values

# 读取个体基因型数据
genotypes = np.loadtxt('gt.trans')
# 确保genotypes始终是二维的
genotypes = np.atleast_2d(genotypes)

# 检查数据维度，确保它们匹配等位基因频率的行数
if genotypes.shape[1] != allele_freqs_matrix.shape[0]:
    raise ValueError("基因型数据和等位基因频率数据的维度不匹配!")

# 初始化存储每个个体GBC的结果
individual_contributions = pd.DataFrame()

# 循环处理每个个体
for i in range(genotypes.shape[0]):  # 注意这里改为genotypes.shape[0]，因为每行是一个个体
    genotype = genotypes[i, :]  # 获取第i个个体的基因型数据
    
    # 构建线性模型，无截距
    y = genotype
    X = allele_freqs_matrix
    model = sm.OLS(y, X)
    results = model.fit()

    # 提取系数（b向量）
    coefficients = results.params

    # 将负系数转换为0
    coefficients[coefficients < 0] = 0

    # 计算每个品种的遗传贡献比例
    contributions = coefficients / sum(coefficients)

    # 将结果添加到DataFrame中
    individual_contributions[f'Individual_{i+1}'] = contributions

# 输出结果
print(individual_contributions)
```

### final version
优化df写入性能，改成每列一个个体：

```python
#加上了参考品种名字和待测样本名字
# gt一列一个个体
import numpy as np
import pandas as pd
import statsmodels.api as sm

# 读取等位基因频率数据，假设第一行是品种名称
allele_freqs = pd.read_table('RAD.merge_MP.frq.fmt', sep='\s+', header=0, index_col='CHR:POS')  # 使用header=0读取列名
allele_freqs_matrix = allele_freqs.values

#读取基因型文件
genotypes = pd.read_table('FinalSNP.filteredmaf01.rename_chr.ex_dup.phased.keepUN.raw.trans.fmt', sep='\s+', header=0)
genotypes.set_index('CHR:POS', inplace=True)


# 初始化字典来存储每个个体的贡献
contributions_dict = {}


# 循环处理每个个体
for i in range(genotypes.shape[1]):  # 修改为genotypes.shape[1]，因为每列是一个个体
    genotype = genotypes.iloc[:, i].values  # 获取第i个个体的基因型数据
    
    # 检查数据维度
    if len(genotype) != allele_freqs_matrix.shape[0]:
        raise ValueError("Dimensions of genotype data and allele frequency data do not match!")
    
    # 构建线性模型，无截距
    X = allele_freqs_matrix
    model = sm.OLS(genotype, X)
    results = model.fit()

    # 提取系数（b向量）
    coefficients = results.params

    # 将负系数转换为0
    coefficients[coefficients < 0] = 0

    # 计算每个品种的遗传贡献比例
    contributions = coefficients / sum(coefficients)
    
    # 将contributions中小于0.01的系数转换为0
    contributions[contributions < 0.02] = 0
    # 再次计算每个品种的遗传贡献比例
    contributions = contributions / sum(contributions)

    # 存储到字典
    contributions_dict[genotypes.columns[i]] = contributions

# 使用字典创建DataFrame，索引设置为品种名称
individual_contributions_rounded = pd.DataFrame(contributions_dict, index=allele_freqs.columns).round(4)


#print(individual_contributions_rounded)
# 将结果保存到CSV文件，已经四舍五入
individual_contributions_rounded.to_csv('GBC.csv')
```

### 非负最小二乘 (NNLS)求解


```python
# 加上了参考品种名字和待测样本名字
# gt一列一个个体
import numpy as np
import pandas as pd
from scipy.optimize import nnls  # 导入NNLS求解器

# 读取等位基因频率数据，假设第一行是品种名称
allele_freqs = pd.read_table('RAD.merge_MP.frq.fmt', sep='\s+', header=0, index_col='CHR:POS')  # 使用header=0读取列名
allele_freqs_matrix = allele_freqs.values

# 读取基因型文件
genotypes = pd.read_table('FinalSNP.filteredmaf01.rename_chr.ex_dup.phased.keepUN.raw.trans.fmt', sep='\s+', header=0)
genotypes.set_index('CHR:POS', inplace=True)

# 初始化字典来存储每个个体的贡献
contributions_dict = {}

# 循环处理每个个体
for i in range(genotypes.shape[1]):  # 修改为genotypes.shape[1]，因为每列是一个个体
    genotype = genotypes.iloc[:, i].values  # 获取第i个个体的基因型数据
    
    # 检查数据维度
    if len(genotype) != allele_freqs_matrix.shape[0]:
        raise ValueError("Dimensions of genotype data and allele frequency data do not match!")
    
    try:
        # 使用非负最小二乘 (NNLS) 求解
        coefficients, _ = nnls(allele_freqs_matrix, genotype)
        
        # 归一化处理
        total = coefficients.sum()
        contributions = coefficients / total
        
        # 动态阈值（基于数据标准差）
		threshold = 0.5 * np.std(contributions)
		contributions[contributions < threshold] = 0
        
        # 重新归一化（如果总和不为零）
        filtered_sum = contributions.sum()
        contributions /= filtered_sum
   
        # 存储到字典
        contributions_dict[genotypes.columns[i]] = contributions
        
    except Exception as e:
        print(f"Error processing individual {genotypes.columns[i]}: {str(e)}")
        # 存储NaN值作为错误标记
        contributions_dict[genotypes.columns[i]] = np.full(allele_freqs_matrix.shape[1], np.nan)

# 使用字典创建DataFrame，索引设置为品种名称
individual_contributions_rounded = pd.DataFrame(contributions_dict, index=allele_freqs.columns).round(4)

# 将结果保存到CSV文件，已经四舍五入
individual_contributions_rounded.to_csv('GBC_NNLS.csv')

```
