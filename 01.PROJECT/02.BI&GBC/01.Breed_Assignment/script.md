---
title: 机器学习品种鉴定脚本备份
dg-publish: true
created: 2024-06-11 05:48
updated: 2025-06-10 09:55
tags: []
---


```python
import numpy as np
import pandas as pd
import joblib
from sklearn.model_selection import train_test_split 
from sklearn.metrics import accuracy_score 
from sklearn.metrics import confusion_matrix 
from sklearn.impute import KNNImputer
from sklearn.model_selection import KFold 
from sklearn.model_selection import cross_val_score

ori_df = pd.read_table('auto.all_samples.filtered.filter_maf.snp.prune.recode.prune.filter_ml.rm_fid_iid_and_confused_breeds_fid_iid.recodeA.raw', 
                       sep=' ', 
                       )
# 2.前期数据转换
# replace nan str to np.nan
replace_df = ori_df.replace('NA', np.nan)

# 分特征和标签
all_features = replace_df.iloc[:, 6:].values
all_breeds = replace_df.iloc[:, 0].values.ravel()

# 品种名称recode为数字标签
breed2num = {breed: num for num, breed in enumerate(np.unique(all_breeds))}
print(breed2num)
all_labels = np.array([breed2num.get(indv) for indv in all_breeds])

from sklearn.impute import SimpleImputer
imputer = SimpleImputer(missing_values=np.nan, strategy='median')
all_features_imputed_df=imputer.fit_transform(all_features)

from sklearn.model_selection import train_test_split, StratifiedKFold, GridSearchCV

import numpy as np
import pandas as pd
from sklearn.model_selection import StratifiedKFold, GridSearchCV
from sklearn.ensemble import RandomForestClassifier
from sklearn.feature_selection import SelectFromModel
from sklearn.neighbors import KNeighborsClassifier
from sklearn.svm import SVC
from sklearn.metrics import accuracy_score

# 假设 all_features_imputed_df 和 all_labels 已经准备好
num_features = [200, 500, 1000, 2000, 5000, 10000]
skf = StratifiedKFold(n_splits=5, shuffle=True, random_state=0)

# 定义模型及其参数网格
models = [
    (KNeighborsClassifier(), {'n_neighbors': [1, 3, 5]}),
    (SVC(), {'C': [0.01, 0.1, 1, 5, 10, 100]}),
    (RandomForestClassifier(), {'n_estimators': [50, 100, 500], 'max_features': ['sqrt', 'log2']}),
    (MultinomialNB(), {'alpha': [0.01, 0.1, 1.0, 10.0, 100.0]}),
]

results = []

# 对每种特征数进行循环
for n_features in num_features:
    print(f"Processing {n_features} features...")

    for model, param_grid in models:
        fold_results = []

        # GridSearchCV内部使用StratifiedKFold进行参数搜索
        model_name = model.__class__.__name__


        for train_index, test_index in skf.split(all_features_imputed_df, all_labels):
            X_train, X_test = all_features_imputed_df[train_index], all_features_imputed_df[test_index]
            y_train, y_test = all_labels[train_index], all_labels[test_index]

            # 特征选择
            selector = SelectFromModel(RandomForestClassifier(n_estimators=100, random_state=0),
                                       threshold=-np.inf, max_features=n_features)
            X_train_selected = selector.fit_transform(X_train, y_train)
            X_test_selected = selector.transform(X_test)

            # 设置GridSearchCV
            grid_search = GridSearchCV(model, param_grid, cv=5, n_jobs=-1, scoring='accuracy')
            grid_search.fit(X_train_selected, y_train)

            # 最佳模型评估
            best_model = grid_search.best_estimator_
            y_pred = best_model.predict(X_test_selected)
            score = accuracy_score(y_test, y_pred)
            fold_results.append(score)

        # 记录并打印结果
        avg_score = np.mean(fold_results)
        results.append({
            'Model': model_name,
            'Num of Features': n_features,
            'Best Parameters': grid_search.best_params_,
            'Scores': fold_results,
            'Average Score': avg_score
        })
        print(f"{model_name} with {n_features} features: {grid_search.best_params_}, Avg Score: {avg_score:.4f}")

# 创建DataFrame来更好地展示结果
df_results = pd.DataFrame(results)
print(df_results[['Model', 'Num of Features', 'Best Parameters', 'Average Score']])
```