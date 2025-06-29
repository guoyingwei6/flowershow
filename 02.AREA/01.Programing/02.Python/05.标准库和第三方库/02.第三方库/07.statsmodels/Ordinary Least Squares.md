---
created: 2024-08-20 10:05
updated: 2025-06-10 09:54
tags:
  - garden/🌱Seedling
---
OLS估计的python实现，具体的求解细节见[常规最小二乘(ordinary least squares, OLS)估计](常规最小二乘(ordinary%20least%20squares,%20OLS)估计.md)

```python
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import statsmodels.api as sm

np.random.seed(9876789)

```


```python
nsample = 100
x = np.linspace(0, 10, 100)
X = np.column_stack((x, x ** 2))
beta = np.array([1, 0.1, 10])
e = np.random.normal(size=nsample)

```

```python
# 添加截距intercept
X = sm.add_constant(X)
y = np.dot(X, beta) + e

```

```python
model = sm.OLS(y, X)
results = model.fit()
print(results.summary())

```


```
                            OLS Regression Results
==============================================================================
Dep. Variable:                      y   R-squared:                       1.000
Model:                            OLS   Adj. R-squared:                  1.000
Method:                 Least Squares   F-statistic:                 4.020e+06
Date:                Thu, 14 Dec 2023   Prob (F-statistic):          2.83e-239
Time:                        14:45:11   Log-Likelihood:                -146.51
No. Observations:                 100   AIC:                             299.0
Df Residuals:                      97   BIC:                             306.8
Df Model:                           2
Covariance Type:            nonrobust
==============================================================================
                 coef    std err          t      P>|t|      [0.025      0.975]
------------------------------------------------------------------------------
const          1.3423      0.313      4.292      0.000       0.722       1.963
x1            -0.0402      0.145     -0.278      0.781      -0.327       0.247
x2            10.0103      0.014    715.745      0.000       9.982      10.038
==============================================================================
Omnibus:                        2.042   Durbin-Watson:                   2.274
Prob(Omnibus):                  0.360   Jarque-Bera (JB):                1.875
Skew:                           0.234   Prob(JB):                        0.392
Kurtosis:                       2.519   Cond. No.                         144.
==============================================================================

Notes:
[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
```

可以直接从拟合模型中提取感兴趣的数量。输入`dir(results)`以获得完整列表。以下是一些示例：

```python
print("Parameters: ", results.params)
print("R2: ", results.rsquared)

```

```
Parameters:  [ 1.34233516 -0.04024948 10.01025357]
R2:  0.9999879365025871
```


