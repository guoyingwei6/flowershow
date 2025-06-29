---
created: 2024-08-19 22:55
updated: 2025-06-10 09:54
dg-publish: true
tags:
  - garden/🌳Evergreen
---
## 01.等方差假设
在很多情况下，可假设
$$
\begin{flalign}
R &= I\sigma_𝑒^2 && 
\end{flalign}
$$
即，所有的随机误差有相同的方差，彼此之间不相关

## 02.估计误差
设$\hat{b}$是$b$的估计值，则y可估计为

$$
\hat{y} = X\hat{b}
$$

估计误差为
$$
\hat{e} = y - \hat{y} = y − Xb
$$
## 03.误差平方和
OLS 估计要求估计误差的平方和最小：
$$
\sum{\hat{e}}_{i}^{2}={\hat{\mathrm{e}}}^{\prime}{\hat{\mathrm{e}}}=\left({\mathrm{y-X\hat{b}}}\right)^{\prime}({\mathrm{y-X}}\mathrm{\hat{b}})=minimum
$$


GPT解释上述公式：

这个公式描述的是线性回归模型中残差平方和（RSS）的计算方法和最小化目标。这里用到了矩阵表示法来展现线性回归的拟合过程及其优化目标。我会一步步地解释这个公式：

### 残差平方和 (RSS)

1. **残差定义**:
   $$
   
   \hat{e} = y - X\hat{b}
   
   $$
   其中:
   - \( y \) 是因变量（响应变量）的向量。
   - \( X \) 是自变量（解释变量）的矩阵。
   - \( \hat{b} \) 是从数据估计得到的回归系数向量。
   - \( \hat{e} \) 是残差向量，即观察值与由模型预测的值之间的差。

2. **残差平方和 (RSS)**:
   $$
   
   \sum \hat{e}_i^2 = \hat{e}^\prime \hat{e}
   
   $$
   这里 \( \hat{e}^\prime \hat{e} \) 是残差向量的转置乘以残差向量本身，得到的是所有残差平方的和。这是因为当你将一个列向量与其自身的转置相乘时，结果是所有元素平方和。

### 最小化 RSS

3. **展开 RSS 公式**:
   $$
   
   \left(y - X\hat{b}\right)^\prime (y - X\hat{b})
   
   $$
   展开这个公式，我们得到：
   $$
   
   y^\prime y - y^\prime X \hat{b} - \hat{b}^\prime X^\prime y + \hat{b}^\prime X^\prime X \hat{b}
   
   $$
   注意 \( y^\prime X \hat{b} \) 和 \( \hat{b}^\prime X^\prime y \) 实际上是等价的，因为它们都是标量（一个数）且它们的转置相等（因为标量的转置是其自身）。

4. **简化**:
   $$
   
   y^\prime y - 2 \hat{b}^\prime X^\prime y + \hat{b}^\prime X^\prime X \hat{b}
   
   $$

5. **优化目标**:
   最终，我们希望找到使 \( \left(y - X\hat{b}\right)^\prime (y - X\hat{b}) \) 最小的 \( \hat{b} \)。这就是线性回归模型的核心目标，即最小化观测值和预测值之间差的平方和。这也就是为什么说 \( \left(y - X\hat{b}\right)^\prime (y - X\hat{b}) = \text{minimum} \)。

### 结论

通过最小化残差平方和，我们实际上是在寻找一个最合适的线性模型，使得模型的预测值尽可能接近实际观测值。这是最小二乘法（Ordinary Least Squares, OLS）的基础，是回归分析中最常用的参数估计方法。




## 04.展开目标函数 Q 
根据求函数最小值的方法

$$
\begin{align*}
Q &= (y - X\hat{b})^{\prime}(y - X\hat{b}) \\
  &= y^{\prime}y - y^{\prime}X{\hat{b}} - \hat{b}^{\prime}X^{\prime}y + \hat{b}^{\prime}X^{\prime}X\hat{b} \\
  &= y^{\prime}y - 2\hat{b}^{\prime}X^{\prime}y + \hat{b}^{\prime}X^{\prime}X\hat{b}
\end{align*}
$$
GPT的解释：

你的推导过程是在最小二乘法（Ordinary Least Squares, OLS）中目标函数 \( Q \) 的展开和简化。这个过程通常用于证明 OLS 解的闭式解，以及推导出一些统计量。让我们一步步分析这个推导过程。

### 1. 定义目标函数 \( Q \)
在最小二乘法中，我们的目标是最小化残差平方和（RSS），即：
$$

Q = (y - X\hat{b})^{\prime}(y - X\hat{b})

$$
这里：
- \( y \) 是观测值的向量，维度为 \( n \times 1 \)。
- \( X \) 是设计矩阵（或称特征矩阵），维度为 \( n \times p \)。
- \( \hat{b} \) 是回归系数的估计值，维度为 \( p \times 1 \)。

### 2. 展开目标函数 \( Q \)
我们先将目标函数 \( Q \) 展开：
$$

Q = (y - X\hat{b})^{\prime}(y - X\hat{b}) = y^{\prime}y - y^{\prime}X\hat{b} - \hat{b}^{\prime}X^{\prime}y + \hat{b}^{\prime}X^{\prime}X\hat{b}

$$
这里使用了向量和矩阵的基本运算性质。

### 3. 简化展开后的公式
注意到 \( y^{\prime}X\hat{b} \) 和 \( \hat{b}^{\prime}X^{\prime}y \) 是标量，并且它们是转置关系，所以它们是相等的：
$$

y^{\prime}X\hat{b} = \hat{b}^{\prime}X^{\prime}y

$$
因此可以将原来的式子简化为：
$$

Q = y^{\prime}y - 2\hat{b}^{\prime}X^{\prime}y + \hat{b}^{\prime}X^{\prime}X\hat{b}

$$
### 4. 含义
- 第一项 \( y^{\prime}y \) 是观测值的平方和。
- 第二项 \( -2\hat{b}^{\prime}X^{\prime}y \) 与回归系数估计值 \( \hat{b} \) 以及观测值和设计矩阵的乘积有关。
- 第三项 \( \hat{b}^{\prime}X^{\prime}X\hat{b} \) 是估计的回归系数与设计矩阵的内积。

这个展开和简化是用于推导最小二乘法中的回归系数 \( \hat{b} \) 的闭式解的一部分。






## 05.求Q关于𝐛的偏导数

求该函数关于 𝐛 的最小值，求Q 关于𝐛 的偏导数，并令偏导数等于0

$$
\frac{\partial Q}{\partial \hat{b}} = -2X^\prime y + 2X^\prime X \hat{b} = 0
$$

可得线性方程组

$$
(X^{\prime}X)\hat{b} = X^{\prime}y
$$
## 06.解方程组

解该方程组，可得

$$
\hat{b} = (X^{\prime}X)^{-1}X^{\prime}y
$$


或

$$
\hat{b} = (X^{\prime}X)^{-}X^{\prime}y
$$
其中$(X^{\prime}X)^{-}$ 是$X^{\prime}X$的 一个广义逆矩阵




