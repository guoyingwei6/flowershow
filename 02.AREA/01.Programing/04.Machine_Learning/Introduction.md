---
created: 2023-10-31 16:40
updated: 2025-06-10 09:55
---


On a high level, the craft of creating machine learning (ML) processes is comprised of a number of steps:  
在高层次上，创建机器学习 （ML） 流程的过程由许多步骤组成：

1. **Decide on the question**. Most ML processes start by asking a question that cannot be answered by a simple conditional program or rules-based engine. These questions often revolve around predictions based on a collection of data.  
    决定问题。大多数 ML 流程都是从提出一个简单的条件程序或基于规则的引擎无法回答的问题开始的。这些问题通常围绕着基于数据集合的预测。
2. **Collect and prepare data**. To be able to answer your question, you need data. The quality and, sometimes, quantity of your data will determine how well you can answer your initial question. Visualizing data is an important aspect of this phase.  
    收集和准备数据。为了能够回答您的问题，您需要数据。数据的质量，有时是数量将决定您回答初始问题的程度。可视化数据是此阶段的一个重要方面。  
    This phase also includes splitting the data into a training and testing group to build a model.  
    此阶段还包括将数据拆分为训练和测试组以构建模型。
3. **Choose a training method**. Depending on your question and the nature of your data, you need to choose how you want to train a model to best reflect your data and make accurate predictions against it.  
    选择一种训练方法。根据您的问题和数据的性质，您需要选择如何训练模型以最好地反映您的数据并对其进行准确预测。  
    This is the part of your ML process that requires specific expertise and, often, a considerable amount of experimentation.  
    这是 ML 流程中需要特定专业知识的部分，并且通常需要大量的实验。
4. **Train the model**. Using your training data, you'll use various algorithms to train a model to recognize patterns in the data. The model might leverage internal weights that can be adjusted to privilege certain parts of the data over others to build a better model.  
    训练模型。使用训练数据，您将使用各种算法来训练模型以识别数据中的模式。该模型可以利用内部权重，这些权重可以调整为使数据的某些部分优先于其他部分，从而构建更好的模型。
5. **Evaluate the model**. You use never before seen data (your testing data) from your collected set to see how the model is performing.  
    评估模型。您可以使用收集集中从未见过的数据（测试数据）来查看模型的性能。
6. **Parameter tuning**. Based on the performance of your model, you can redo the process using different parameters, or variables, that control the behavior of the algorithms used to train the model.  
    参数调优。根据模型的性能，可以使用不同的参数或变量重做该过程，这些参数或变量控制用于训练模型的算法的行为。
7. **Predict**. Use new inputs to test the accuracy of your model.  
    预测。使用新输入来测试模型的准确性。


