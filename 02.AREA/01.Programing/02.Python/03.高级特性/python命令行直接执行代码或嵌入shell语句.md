---
created: 2024-09-24 18:03
updated: 2025-06-10 09:55
dg-publish: true
tags:
  - garden/🌱Seedling
---

## 介绍

`python -c` 命令允许你直接从命令行运行一小段 **Python 代码**，而不需要创建一个脚本文件。这个命令特别适合运行短小的 Python 代码片段，进行快速测试或执行一次性的操作。

## 功能和用法示例

### **打印输出**：

你可以使用 python -c 运行简单的 Python 代码，例如打印输出：
```
python -c 'print("Hello, World!")'
```

###  **数学运算**：

直接进行一些数学运算并输出结果：

```
python -c 'print(2 + 3)'
```

### **导入模块并调用函数**：

你可以在命令中导入 Python 模块，并使用其中的函数：

```
python -c 'import math; print(math.sqrt(16))'
```


### **多条语句**：

如果需要执行多条语句，可以用分号 ; 将代码分隔开：
```
python -c 'x = 10; y = 20; print(x + y)'
```


### **读取文件或进行网络操作**：

你可以通过 python -c 运行读取文件、进行网络请求等操作。例如：

```
python -c 'import urllib.request; response = urllib.request.urlopen("http://example.com"); print(response.read())'
```

### **调用系统命令**：

可以结合 Python 的 os 模块来调用系统命令：

```
python -c 'import os; os.system("ls")'
```

### 嵌入带参数的命令：

```shell
#这一段是python代码 
script=" import sys 
a=sys.argv 
for i in a: 
	print(i) " 
#这一段shell调用python执行命令 
python -c """$script""" hello world 
#输出效果 
-c 
hello 
world
```

## **注意事项：**

  

• 代码片段必须放在引号 '' 内，通常使用单引号或双引号包裹代码，确保代码片段被正确传递。

• 对于较长或复杂的代码，最好还是使用脚本文件，而不是通过 python -c 来运行，以便调试和维护。

  

**常见用途：**

  

• **快速测试**：用于运行小段代码、调试或验证某些 Python 行为。

• **自动化脚本**：在系统管理或自动化过程中，结合 Shell 脚本快速执行 Python 任务。

• **网络操作或文件操作**：结合网络请求、文件读写等 Python 功能，在命令行环境中快速执行操作。

  

python -c 非常适合处理简单、临时的任务，而不需要创建独立的 Python 文件。