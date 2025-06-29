---
created: 2024-08-01 14:36
updated: 2025-06-10 09:54
dg-publish: true
tags:
  - garden/🌳Evergreen
---


> 与[pickle](pickle模块进行对象的序列化.md)的区别，详见[这个笔记](pickle与joblib的区别.md)


joblib是一个Python库，主要用于提供轻松的保存和加载Python对象的功能，特别是针对大数据对象进行优化。它非常适用于处理需要大量内存的对象，如大型numpy数组。joblib的核心功能之一是其有效的内存使用和压缩功能，这使得它在科学计算和机器学习领域尤其受欢迎。


## **主要特点**

  

1. **高效存储**：使用自定义的优化，特别是对于numpy数组，可以更快地读写大型数据。
2. **透明压缩**：自动压缩和解压缩数据，支持多种压缩格式。
3. **内存映射**：支持内存映射（memory mapping）技术，能够处理不适合常规内存处理的大数组。
4. **简化并行计算**：提供简单的工具来并行化任务，便于处理计算密集型任务。


## **使用示例**

  

### **保存和加载对象**

  

joblib最常见的用途是在机器学习中保存和加载训练好的模型。以下示例展示了如何使用joblib来保存和加载一个简单的scikit-learn模型：

```python
from sklearn.datasets import load_iris
from sklearn.ensemble import RandomForestClassifier
from joblib import dump, load

# 加载数据
iris = load_iris()
X, y = iris.data, iris.target

# 训练模型
model = RandomForestClassifier(n_estimators=10)
model.fit(X, y)

# 保存模型到文件
dump(model, 'rf_model.joblib')

# 从文件加载模型
loaded_model = load('rf_model.joblib')

# 使用加载的模型进行预测
predictions = loaded_model.predict(X)
```


### **内存映射功能**

  

对于处理大型numpy数组非常有用，可以将数组保存在磁盘上，并在不完全加载到内存的情况下使用它们。

```python
import numpy as np
from joblib import dump, load

# 创建一个大型数组
large_array = np.random.rand(10000, 10000)

# 使用内存映射保存
dump(large_array, 'large_array.joblib', compress=True)

# 使用内存映射加载
mapped_array = load('large_array.joblib', mmap_mode='r')

# 访问数组不会加载整个数组到内存
print(mapped_array[0, 0])
```


## **适用场景**

  

• **机器学习模型持久化**：用于保存和加载训练好的机器学习模型。
• **大型数据集处理**：用于存储和访问大型数据集，特别是那些不适合常规内存处理的数据。

  

joblib是处理数据密集型任务的强大工具，其简单的API和高效的处理方式使得它在科学研究和工业应用中都非常受欢迎。