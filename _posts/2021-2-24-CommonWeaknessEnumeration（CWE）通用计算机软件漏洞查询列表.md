---
title: CommonWeaknessEnumeration（CWE）通用计算机软件漏洞查询列表
tags:
  - 安全
---

# CommonWeaknessEnumeration（CWE）通用计算机软件漏洞查询字典

## 一、是什么

由MITRE运行和维护，并邀请各企业、学术机构和政府部门等多个国际专家组参与完善的**在线计算机软件缺陷查询字典**

CWE是一种通用的标准化术语，是软件安全工具的衡量标准，也是识别、修复和预防缺陷的基准。对于服务提供商，在发现特定的潜在缺陷时，可以借助CWE通知用户并提出解决建议方案；对于软件买家，可以利用CWE比较多个厂商提供的相似产品；对于开发人员，可以学习利用其中的内容，尽可能地避免引入代码缺陷。 

## 二、如何使用（阅读）

CWE提供了三种概念，并基于这三种概念设计出来研究、软件开发和硬件设计的三种类型的视图，每一种图都是一个多层次的树状图体系，每种体系下都有如下类似的结构。

![](C:\Users\Lenovo\Desktop\JODE\Blog\JODE-HRK.github.io\assets\image\CWE三种概念.png)

![](C:\Users\Lenovo\Desktop\JODE\Blog\JODE-HRK.github.io\assets\image\CWE树状图体系.png)

### Research Concepts 研究概念视图

研究型视图主要面向学术研究人员、漏洞分析人员和评估工具厂商，旨在促进对缺陷的研究，以及缺陷与缺陷之间的依赖关系。这种分类方式并不关心缺陷的检测方法，缺陷在代码的位置和在软件开发生命周期中何时引入缺陷。**研究型视图主要基于对软件行为进行抽象描述的方法组织归类**。

下面给出研究型视图样例，此图中每个类别下包含基础漏洞和变体漏洞等

![](C:\Users\Lenovo\Desktop\JODE\Blog\JODE-HRK.github.io\assets\image\CWE研究型概念图示意.png)

将每一种具体的漏洞进行分类，并为其建立详细的条目，再以divide by zero为例

![](C:\Users\Lenovo\Desktop\JODE\Blog\JODE-HRK.github.io\assets\image\CWE条目的示例.png)

### SoftwareDevelopment软件开发概念视图

软件开发视图因为是面向软件开发人员、教育人员和评估工具厂商，其对漏洞的分类基础是软件开发中经常使用和遇到的概念。

给出示例，可以看出每一个类别下包含其他类目、类别、基础和变体漏洞等

![](C:\Users\Lenovo\Desktop\JODE\Blog\JODE-HRK.github.io\assets\image\CWE软件开发型示例.png)

同样，这里也为每一个类目创建了内容页面

![](C:\Users\Lenovo\Desktop\JODE\Blog\JODE-HRK.github.io\assets\image\CWE软件开发型条目示例.png)

### 硬件设计概念视图

主要面向硬件设计人员识别在软件设计过程中可能造成的错误。

基本结构同上述两种概念视图相似，且更为简单