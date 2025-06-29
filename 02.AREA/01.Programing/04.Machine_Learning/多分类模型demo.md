---
tags:
created: 2024-06-04 20:58
updated: 2025-06-10 15:00
---


![](Pasted%20image%2020231104175344.png)
```python


from sklearn.metrics import confusion_matrix

  

# Print the confusion matrix

mcm = confusion_matrix(y_penguin_test, penguin_predictions)

print(mcm)




import numpy as np

import matplotlib.pyplot as plt

%matplotlib inline

  

plt.imshow(mcm, interpolation="nearest", cmap=plt.cm.Blues)
plt.colorbar()
tick_marks = np.arange(len(penguin_classes))
plt.xticks(tick_marks, penguin_classes, rotation=45)
plt.yticks(tick_marks, penguin_classes)
plt.xlabel("Predicted Species")
plt.ylabel("Actual Species")
plt.show()
```



The darker squares in the confusion matrix plot indicate high numbers of cases, and you can hopefully see a diagonal line of darker squares indicating cases where the predicted and actual label are the same.  
混淆矩阵图中较暗的方块表示事例数较多，您希望看到一条较暗方块的对角线，表示预测标签和实际标签相同的事例。

In the case of a multiclass classification model, a single ROC curve showing true positive rate vs false positive rate is not possible. However, you can use the rates for each class in a One vs Rest (OVR) comparison to create a ROC chart for each class.  
在多类分类模型的情况下，不可能使用显示真阳性率与假阳性率的单个 ROC 曲线。但是，您可以在 One vs Rest （OVR） 比较中使用每个类的费率为每个类创建 ROC 控制图。


```python
from sklearn.metrics import roc_curve

from sklearn.metrics import roc_auc_score

  

# Get class probability scores

penguin_prob = multi_model.predict_proba(x_penguin_test)

  

# Get ROC metrics for each class

fpr = {}

tpr = {}

thresh ={}

for i in range(len(penguin_classes)):

fpr[i], tpr[i], thresh[i] = roc_curve(y_penguin_test, penguin_prob[:,i], pos_label=i)

# Plot the ROC chart

plt.plot(fpr[0], tpr[0], linestyle='--',color='orange', label=penguin_classes[0] + ' vs Rest')

plt.plot(fpr[1], tpr[1], linestyle='--',color='green', label=penguin_classes[1] + ' vs Rest')

plt.plot(fpr[2], tpr[2], linestyle='--',color='blue', label=penguin_classes[2] + ' vs Rest')

plt.title('Multiclass ROC curve')

plt.xlabel('False Positive Rate')

plt.ylabel('True Positive rate')

plt.legend(loc='best')

plt.show()
```

To quantify the ROC performance, you can calculate an aggregate area under the curve score that is averaged across all of the OVR curves.  
要量化 ROC 性能，您可以计算曲线下的总面积，该分数是所有 OVR 曲线的平均值。

```python
auc = roc_auc_score(y_penguin_test,penguin_prob, multi_class='ovr')

print('Average AUC:', auc)
```


