---
title: TensorFlow 基础
tags:
  - Python
  - AI  
  - TensorFlow
---

# TensorFlow基础

###  一、张量

​	张量是TensorFlow的基本数据单元，是一个**数学对象**，是对标量、向量和矩阵的泛化。

​	张量可以表示为一个多维数组，0秩的张量就是标量、向量或者数组可以视为一个秩为1的张量，而矩阵是一个秩为2的张量。简而言之，张量可以被认为是一个n维数组。

### 二、计算图与会话

TensorFlow因其TensorFlow Core程序而受欢迎，其有两个主要的作用：

- 在构建阶段建立计算图

- 在执行阶段运行计算图

TensorFlow的工作流程：

- 程序通常被结构化为构建阶段和执行阶段
- 构建阶段搭建具有节点（操作）和边（张量）的图
- 执行阶段使用会话来执行图中的操作
- 最简单的操作是一个常数，没有输入，只是传递给其他计算操作。
- 一个操作的例子就是乘法（或者是取出两个矩阵作为输入并输出另一个矩阵的加法或者减法操作）
- TensorFlow中具有一个默认图来给构建操作添加节点。

因此，TensorFlow程序结构有如下两个阶段：

- 构建阶段：
  - 搭建一个图
  - 构建计算图中的节点（操作）和边（张量）
- 执行阶段：
  - 使用会话进行操作
  - 重复执行一组训练操作

**计算图**是由一系列的TensorFlow操作排成的节点组成的

对比下TensorFlow和Numpy。在Numpy中，如果你打算将两个矩阵相乘，需要生成两个矩阵。但在TensorFlow中，就是建立一个图（默认图，除非你另外创建了一个图）。接下来，创建变量、占位符以及常量值，然后创建会话并初始化变量。最终，把数据赋给占位符以便调用其他操作。

​	实际上，为了计算节点，必须在一个会话里运行计算图。

一个会话里囊括了TensorFlow运行时的控制和状态。

下面的代码生成了一个会话对象

```python
sess = tf.Session()
```

之后就能用它的运行方法来充分运行计算图以计算节点1和节点2

​	计算图定义了计算。但它不执行计算，也不保留任何值。它用来定义代码中提及的操作。同时，创建了一个默认的图。因此，你不必创建图，除非你需要创建图用于多种目的。

​	会话允许你执行图或者只执行部分图。它为执行分配资源（在一个或多个CPU或者GPU上）。而且还保留了中间结果和变量值。

​	在TensorFlow中创建的变量的值，只有在一个会话中是有效的。若尝试在第二个会话中访问，TensorFlow会报错，因为变量不是在那里初始化的。

​	想运行任何操作，都必须先给图创建一个会话。会话会分配内存来存储当前变量值。

示例代码：

```python
import tensorflow as tf
sess = tf.Session()

#Creating a new graph
myGraph = tf.Graph()
with myGraph.as_default():
    variable = tf.Variable(30,name = "navin")
    initialize = tf.global_variables_initializer()
with tf.Session(graph = myGraph) as sess:
    sess.run(initialize)
    print(sess.run(variable))
```



### 三、常量、占位符与变量

​	TensorFlow程序使用张量数据结构表示所有数据，在计算图中，也只有张量在操作之间被传递。

​	将TensorFlow张量想象成一个n维数组或者列表。张量具有静态类型，秩以及形状。在这里，图产生一个不变的结果。变量在整个图的执行过程中维持其状态。

​	通常，在深度学习中，不可避免的需要处理许多图片，因此需要给每一张图赋以像素值，然后对所有图片重复进行此操作。

​	为了训练模型，需要能够修改图以调节一些对象，比如：权重、偏置。简单来说，变量让你能够给模型训练，调整参数。

在TensorFlow中创建一个常量并输出

```python
import tensorflow as tf
x = tf.constant(12,dtype='float32')
sess = tf.compat.v1.Session() #.Session不能用，需要使用该行代码代替
print(sess.run(x))
```

