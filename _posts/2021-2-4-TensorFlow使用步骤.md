---
title: TensorFlow使用步骤
tags:
  - Python
  - AI
---

# TensorFlow使用步骤

```python
import tensorflow as tf
import pandas as pd

# 读取数据集
data = pd.read_csv('C:/Users/Lenovo/Desktop/JODE/TensorDlowDataBase/tensorflow入门与实战-基础部分数据集/Income1.csv')
print(data)

# 以下注释部分能将加载的数据用笛卡尔图表示出来
# import matplotlib.pyplot as plt

# plt.scatter(data.Education, data.Income)
# plt.show()

#定义两个变量，分别表示数据里的哪一项
x = data.Education
y = data.Income

#使用	层的堆叠Sequential模型
model = tf.keras.Sequential()   #Sequential顺序模型

#Layers里面的Dense层，建立f(x) = ax + b 的线性模型
# f(x) = ax + b 输出的维度为1（f(x)），输入的维度（ax）也为1
# 左边为输出维度，右边为输入维度
model.add(tf.keras.layers.Dense(1, input_shape=(1,)))  #有逗号才叫一个元组

#输出中，outputshape第一个维度代表样本的个数，第二个维度代表输出的维度   Param代表参数个数
#此处为2，即为ax+b a,b两个参数的
model.summary()

model.compile(
    optimizer='adam',
    loss='mse'
) #类似于配对的过程    adam优化方法 和 mse（方差）损失函数

#将训练过程打印出来
history = model.fit(x, y, epochs=5000)

#预测现有的x
print(model.predict(x))

#打印预测结果
print(model.predict(pd.Series([20])))
```
