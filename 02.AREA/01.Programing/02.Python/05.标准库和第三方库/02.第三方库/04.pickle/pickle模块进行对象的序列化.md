---
created: 2024-07-24 15:28
updated: 2025-06-10 09:54
dg-publish: true
tags: []
---
> 也可以用[joblib对象序列化和反序列化](joblib对象序列化和反序列化.md)，二者的区别见[pickle与joblib的区别](pickle与joblib的区别.md)



## 把对象存入文件


```python
import pickle
with open('all_features_imputed_df.pkl', 'wb') as file:
    pickle.dump(all_features_imputed_df, file)
with open('all_labels.pkl', 'wb') as file:
    pickle.dump(all_labels, file)
```


## 从文件中加载对象


```python
import pickle

 #打开文件，从二进制模式读取
with open('all_features_imputed_df.pkl', 'rb') as file:
    all_features_imputed_df = pickle.load(file)
    
with open('all_labels.pkl', 'rb') as file:
    all_labels = pickle.load(file)
```


