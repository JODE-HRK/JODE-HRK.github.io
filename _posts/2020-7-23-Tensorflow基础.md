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
import tensorflow as tf # 导入
tf.compat.v1.disable_eager_execution() # 标记
x = tf.constant(12,dtype='float32') #创建常量值（x），并指定为12
sess = tf.compat.v1.Session() # 标记	创建会话来计算
print(sess.run(x))	#仅运行变量x，进行输出
#使用tensorflow2.0的小伙伴，需要将大佬标记的两行才能正常运行
```

另一种写法：

```python
import tensorflow as tf
tf.compat.v1.disable_eager_execution()
x = tf.constant(12,dtype='float32')
with tf.compat.v1.Session() as sess:
    print(sess.run(x))
```

如何创建变量并将其初始化

```python
import tensorflow as tf
tf.compat.v1.disable_eager_execution()
x = tf.constant([14,23,40,30])
y = tf.Variable(x*2+100)
#初始化变量
model = tf.compat.v1.global_variables_initializer() #此处也会因为版本问题导致错误
with tf.compat.v1.Session() as sess:
    sess.run(model)
    print(sess.run(y))
```

### 四、占位符

可以在之后再对其进行赋值。占位符用来接收外部输入，其可以为一维或者多维，存储n维数组

```python
import tensorflow as tf
tf.compat.v1.disable_eager_execution()
x = tf.compat.v1.placeholder("float",None) 
# x = tf.compat.v1.placeholder("float",[None,4]) 表示能开二维数组，但只有4列
y = x*10+500
with tf.compat.v1.Session() as sess:
    placeX = sess.run(y,feed_dict = {x: [0,5,15,25]})
    print(placeX)
```

解释：

- 导入
- 创建占位符x并指定类型

- 创建张量y，注意此时x初始值未确定
- 创建会话来计算
- 再feed_dict中给定x的值来运行y
- 输出运行过后的值

Tips：当调用 ```tf.constant```时，常量会被初始化，并且其值也不会再发生变化。调用```tf.Variable```时，变量不会被初始化。想在Tensorflow里初始化所有变量，必须显式地调用如下特殊操作

```python
sess.run(tf.global_variables_initializer()) #初始化所有的全局变量
```



### 五、创建张量

​	图片是一个三阶的张量，其维度为 高、宽以及通道数（红、蓝、绿）

```python
import tensorflow as tf
tf.compat.v1.disable_eager_execution()
image = tf.image.decode_jpeg(tf.compat.v1.read_file("D:/Desktop/1.jpg"),channels=3)
sess = tf.compat.v1.InteractiveSession()
print(sess.run(tf.shape(image)))
print(sess.run((image[10:15,0:4,1])))
```



- 固定张量

  ```python
  A = tf.zeros([2,3])
  B = tf.ones([4,3])
  C = tf.fill([2,3],21)
  D = tf.diag([4,-3,2])
  E = tf.constant([5,4,2,1,3])
  ```

- 序列张量

  ```python
  G = tf.range(start = 6, limit = 45, delta =3) #给定增量
  H = tf.linspace(10.0,92.0,5) #均分成5段
  ```

  

- 随机张量

```python
R1 = tf.compat.v1.random_uniform([2,3],minval = 0, maxval =4) #从给定范围生成均匀分布的随机值
R2 = tf.compat.v1.random_normal([2,3],mean=5,stddev=4) # 从给定的均值和标准差的正态分布生成随机值
R3 = tf.compat.v1.random_shuffle(tf.compat.v1.diag([3,-2,4])) #生成对角张量，并随机排列
```

### 六、矩阵操作

```python
import tensorflow as tf
import numpy as np
tf.compat.v1.disable_eager_execution()
sess = tf.compat.v1.Session()
A = tf.compat.v1.random_uniform([3,2])
B = tf.fill([2,4],3.5)
C = tf.compat.v1.random_normal([3,4])
print(sess.run(A))
print(sess.run(B))
print(sess.run(C))
print(sess.run(tf.matmul(A,B)+C))  #矩阵的乘法和加法
```

### 七、激活函数

​	激活函数的想法来自于对人脑神经元工作机理的分析。神经元在某个阈值（活化电位）之上会被激活。大多数情况下，激活函数还意在将输出限制再一个小的范围内。

Sigmoid、双曲正切，ReLU和LU是流行的激活函数

* 双曲正切与Sigmoid
  * 双曲正切无论输入什么，输出都在-1~1之间
  * Sigmoid，无论输入什么，输出在0~1之间

```python
import tensorflow as tf
import numpy as np
tf.compat.v1.disable_eager_execution()
sess = tf.compat.v1.Session()
E = tf.nn.tanh([10,2,1,0.5,0,-0.5,-1,-2.,-10.])
print(sess.run(E))
F = tf.nn.sigmoid([10,2,1,0.5,0,-0.5,-1,-2.,-10.])
print(sess.run(F))
```

- ReLU与ELU

![](\JODE-HRK.github.io\assets\image\激活函数1.png)

Relu函数，将所有小于0的值都变为0，其余不变；而ELU函数，在小于0的部分，将无限趋近于-1

- ReLU6

除了输出不超过6以外，与ReLU相同

### 八、损失函数（代价函数）

​	用来最小化以得到模型每个参数的最优值。举个栗子：为了用预测器（X）来预测目标（y）的值，需要获得权重值（斜率）和偏置量（y的截距）。得到斜率和截距最优值的方法是最小化代价函数/损失函数/平方和。对于任何一个模型来说，都有很多参数，而且预测或进行分类的模型结构也是通过参数的数值来表示的。

​	你需要计算模型，并且为了达到这个目的，需要定义代价函数（损失函数）。最小化损失函数就是为了寻找这个参数的最优值。对于回归/数值预测问题来说，L1或L2是很有用的损失函数。对于分类来说，交叉熵是很有用的损失函数。Softmax或者Sigmoid交叉熵都是非常流行的损失函数。

- 损失函数实例

```python
import tensorflow as tf
import numpy as np
tf.compat.v1.disable_eager_execution()
sess = tf.compat.v1.Session()
#Assuming prediction model
pred = np.asarray([0.2,0.3,0.5,10.0,12.0,13.0,3.5,7.4,3.9,2.3])
#convert ndarray into tensor
x_val = tf.convert_to_tensor(pred)
#assuming actual values
actual = np.asarray([0.1,0.4,0.6,9.0,11.0,12.0,3.4,7.1,3.8,2.0])
#L2 loss:L1 = (pred-actual)^2
l2 = tf.square(pred-actual)
l2_out = sess.run(tf.round(l2))
print(l2_out)

#L2 loss:L1 = abs(pred - actual)
l1 = tf.abs(pred-actual)
l1_out = sess.run(l1)
print(l1_out)

#cross entropy loss
softmax_xentropy_variable = tf.nn.sigmoid_cross_entropy_with_logits(logits=l1_out,labels=l2_out)
print(sess.run(softmax_xentropy_variable))
```

### 九、优化器

你假定了模型中的权重和偏置量的初始值。现在需要你找到抵达参数最优值的方法。优化器就是找到参数最优值的方法。在每一次迭代中，参数值朝优化器指明的方向去更新。举个栗子：你有16个权值（w1~w16）和4个偏置量（b1~b4）。开始你可以指定每个权重值和偏置量为0（或者是1，其他任意数值均可）。优化器时刻谨记着最小化的目标，决定w1以及其他参数在下一次迭代中应该是增加还是减少。在很多次迭代之后，w1或者其他参数将会趋于稳定而达到最优值。

优化器的目标就是给定权重值和偏置量下一次迭代时变化的方向。假定有64个权重值和16个偏置量，尝试在每次迭代中改变其值（在反向传播中），在尝试最小化损失函数的很多次迭代之后，应该能够得到正确的权值和偏置量。

为模型选择一个好的优化器，收敛快并且能够学到合适的权重和偏置量，是一个需要技巧的事情

自适应技术，对于复杂的神经网络模型来说是很好的优化器，收敛更快。大多数情况下，Adam可能是最好的优化器。Adam还优于其他的自适应技术，但其计算成本很高。对于稀疏数据集来说，一些方法，如SGD、NAG以及momentum，都不是最好的选择，能自适应调整学习率的方法才是。一个附加的好处就是不需要调整学习率，实用默认的学习率就可以达到最优解。

- 优化器实例

```python
#-*- coding:utf-8 -*-
#@Time : 2020/6/9 19:44
#@Athor : JODE
#@File : NeedSighin.py
#@Software: PyCharm
import tensorflow as tf
tf.compat.v1.disable_eager_execution()
#Assign the value into variable
x = tf.Variable(3,name='x',dtype=tf.float32)
log_x = tf.compat.v1.log(x);
log_x_squared = tf.square(log_x)

#Apply GradientDescentPotimizer
optimizer = tf.compat.v1.train.GradientDescentOptimizer(0.7)
train = optimizer.minimize(log_x_squared)

#Initialize Variables
init = tf.compat.v1.global_variables_initializer()

#Finally running computation
with tf.compat.v1.Session() as session:
    session.run(init)
    print("starting at","x:",session.run(x),"log(x)^2:",session.run(log_x_squared))
    for step in range(10):
        session.run(train)
        print("step",step,"x:",session.run(x),"log(x)^2:",session.run(log_x_squared))
```

### 度量

