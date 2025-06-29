---
created: 2024-08-14 00:23
updated: 2025-06-10 09:55
dg-publish: true
---
LaTeX提供了丰富的数学符号和公式环境，能够用于撰写从简单到复杂的所有数学内容。下面是一些在LaTeX中最常用的数学公式以及与线性代数相关的公式示例：

## 最常用的LaTeX数学符号和表达式：

1. **希腊字母**：
    - 小写：`\alpha`, `\beta`, `\gamma`,`\epsilon` ... （例如：$\alpha, \beta, \gamma, \epsilon$）
    - 大写：`\Alpha`, `\Beta`, `\Gamma`, ... （例如：$A, B, \Gamma$）
2. **上标和下标**：
    - 上标：`^`（例如：$x^2$）
    - 下标：`_`（例如：$x_i$）
3. **分数**：
    - `\frac{numerator}{denominator}`（例如：$\frac{a}{b}$）
4. **根号**：
    - 平方根：`\sqrt{x}`（例如：$\sqrt{x}$）
    - n次根：`\sqrt[n]{x}`（例如：$\sqrt[3]{x}$）
5. **求和与积分**：
    - 求和：`\sum_{i=1}^n i`（例如：$\sum_{i=1}^n i$）
    - 积分：`\int_a^b f(x) \, dx`（例如：$\int_a^b f(x) , dx$）
6. **矩阵**：
    - 基础矩阵：
		```latex
		\begin{matrix}
		a & b \\
		c & d \\
		\end{matrix}
		```

		展示效果：

$$
\begin{matrix}
a & b \\
c & d \\
\end{matrix}
$$
		- 带括号：

```latex
		\begin{pmatrix}
		a & b \\
		c & d \\
		\end{pmatrix}
```



$$ \begin{pmatrix} 
 a & b \\ 
 c & d \\ 
 \end{pmatrix}
$$
### 希腊字母
希腊字母的表达方式是`\字母`名称，比如说 （输出结果：指令）
- α: `\alpha`
- β: `\beta`
- γ: `\gamma`
- σ: `\sigma`
- ϵ: `\epsilon`
如果你想要大写的某个希腊字母，只需要把指令中的首字母大写，比如说
- Σ: `\Sigma`
- Γ: `\Gamma`

### 数学符号
注意上下标的内容是在{}里面。读者可以试一下如果忘记括号输出的是什么结果。
- ≠: \neq
- ≥: \geq
- ≤: \leq
- ≈: \approx
- ≡: \equiv
- ∫: \int
- ∀: \forall
- ∃: \exists
- ∂: \partial
- : \mathcal{N}
- ∼: \sim
- 412: 4^{12}
- C60: C_{60}
- 12: \frac{1}{2}

- 常见表达式 注意下面的几个例子都是在数学环境中，需要在 \[ \]之间。
    - 求和  
        $\sum_{i=0}^{10} i = 0 + 1 + 2 + \ldots + 10$
    - 求乘积  
        $\prod_{i=1}^5 i = 1 \times 2 \times 3 \times 4 \times 5$
        
        
    - 求积分  
        $\int_{x=1}^{10}\frac{1}{x^2}$

    - 求导  
        $\frac{d}{dy} y^2$
        
    - 求极限  
        $\lim_{n\to \infty} \frac{1}{n}$
        
        
    - 求余数  
        $10 \equiv 1 \text{ (mod 3)}$
        
        
    - 开根号  
        $\sqrt{\frac{a}{b+c}}(b+a)$


## 线性代数中常用的LaTeX表达式：

1. **向量**：
    - 行向量：
	    - `(a_1, a_2, \ldots, a_n)`
	    - $(a_1, a_2, \ldots, a_n)$
    - 列向量：
```latex
        \begin{pmatrix} 
        \a_1 \\ 
        \a_2 \\ 
        \vdots 
        \\ a_n \\ 
        \end{pmatrix}`
```
        $$
        \begin{pmatrix} 
        a_1 \\ 
        a_2 \\ 
        \vdots
        \\ a_n \\ 
        \end{pmatrix}
        $$
1. **矩阵操作**
    - 矩阵乘法：`A \times B` $A \times B$
    - 矩阵转置：`A^T` $A^T$
    - 矩阵逆：`A^{-1}` $A^{-1}$
2. **行列式**：
    - 行列式表示：`\det(A)`  $\det(A)$ 或 `|A|` $|A|$
3. **特征值与特征向量**：
    - 特征值：`\lambda` $\lambda$
    - 特征向量：通常表示为向量，例如：`\vec{v}` $\vec{v}$
4. **线性方程组**：
    - 线性方程组表示：`Ax = b` $Ax = b$
    - 方程组解的表示：使用 `\text{}` 来插入文本说明

通过这些基础和专业的符号与表达式，你可以在LaTeX中构建几乎所有类型的数学公式，尤其是对于专业的学术文档和学校作业来说非常实用。



$$
\begin{bmatrix}
0 & 0 & 1 \\ 
1 & 0 & 0 \\ 
\end{bmatrix}

\begin{pmatrix}
1 & 4 & 0 \\ 
2 & 5 & 8 \\
\end{pmatrix}

\begin{vmatrix}
1 & 4 & 0 \\ 
2 & 5 & 8 \\
\end{vmatrix}
$$



## 保留字符

LaTeX 环境中具有特殊含义的保留字符，不能直接使用，必须通过指定的语法实现：

| 序号    | 符号  | LaTeX     | 序号    | 符号  | LaTeX        |
| ----- | --- | --------- | ----- | --- | ------------ |
| **1** | #   | `\#`      | **6** | {   | `\{`         |
| **2** | %   | `\%`      | **7** | }   | `\}`         |
| **3** | ∧   | `^\wedge` | **8** | ∼   | `\sim`       |
| **4** | &   | `\&`      | **9** | ∖   | `\backslash` |
| **5** | _   | `\_`      |       |     |              |