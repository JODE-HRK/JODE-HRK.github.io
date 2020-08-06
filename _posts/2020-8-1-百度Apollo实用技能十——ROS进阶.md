---
title: ROS进阶
tags:
  - 百度
  - 无人驾驶
---

# ROS进阶

### 创建项目

ROS Packages

- 创建一个ROS开发环境和写一个C++工程有点类似，通过`catkin create`可以创建一个简单的工程
- 文件组织方式：
  - `src`存放源文件；
  - `msg`存放节点之间进行通信的消息定义；
  - `srv`存放节点之间进行服务通信的时候的服务定义；
  - `config`存放配置文件相关的信息；
  - `include/package_name`存放头文件相关的信息；
  - `launch`存放节点启动和它相关的节点之间的启动文件。
- Package.xml
  - Package.xml定义了可执行文件依赖的一些库，包括编译和运行时依赖库，同时定义了软件版本信息等常见的描述文件。
- Cmakelists.txt，它定义了怎么编译ROS工程的规则，主要定义了以下几个部分：
  - 指定Cmake的版本
  - 工程Package name
  - 工程的头文件信息
  - 指定所依赖的库，与在Package.xml里面所指定的依赖库是一一对应的
  - install命令，将编译出来的临时文件放在指定的目录里

### 编译和运行

- Eclipse下编译ROS基本工程
  - 首先是设置工作ROS工作区，然后将ROS package导入到Eclipse设置的工作区
  - 通过Eclipse提供的build或者run等功能去调试ROS工程。同时Eclipse里面提供的快捷键在编译程序里面同样适用。
- 通过hello world了解ROS基本的运行逻辑
  - Main函数里面有四行需要重点注意：
  - `ros::init(argc, argv, "hello_world");` 引入ROS的一个基本环境，指定节点使用的node名字和一些参数信息
  - `ros::NodeHandle nodeHandle;` node和整个ROS框架进行通信所使用的一个句柄指针
  - `ros::Rate loopRate(10);` 以10赫兹发送消息
  - `ros::spinOnce();` 有一帧消息就把这消息马上发送出去，同时等待下一轮的消息

### 日志系统

ROS的日志系统

- ROS的日志系统是分级的，分级的作用是为了帮助开发者快速地定位到关键信息，不会对整个节点的逻辑产生实质性的影响
- 日志系统提供了两种格式`ROS_INFO`与`ROS_INFO_STREAM`
- ROS_INFO：默认把信息打印到当时运行的屏幕上。
- ROS_INFO_STREAM：它是流式数据，默认输出到后台这个节点所对应的日志文件。

### subscriber和publisher功能

Subscriber与Publisher有三点明显的区别：

- 回调函数：subscriber作为信息的接收方有一个回调函数，回调函数定义了它接收到的每一帧信息如何使用；上图listener回调函数比较简单，它接收到信息后只是进行了打印处理。Publisher没有回调函数，它不需要对消息进行处理。
- 声明的时候：subscriber把回调函数传入到对应的node初始化程序里面。publisher声明的时候只需要注册要往哪一个topic上去发信息，同时还设置队列长度。
- Rosspin：在ROS构架里所有的回调函数都不是主动触发的。Rosspin是阻塞性的，声明Rosspin之后，就阻塞在此，程序不会退出，它会一直监听自己对应的队列里面是否有新消息的到达，若有新消息到达就会触发回调函数处理。

### Message意以外的通信方式

- Service
  - 节点可以启动service去注册一项服务，另外一个节点在使用这项服务的时候可以直接call service完成一些实时的数据通讯交互
  - Message是一个被动的消息行为，发送和接收之间是一个什么状态是不知道的，Service弥补了这种通信方式的不足，它需要及时回应
- Parameter
  - Parameter通信方式借鉴了service的原理
  - 它启动了Parameter service，Parameter service是一个全局的服务器，各个节点在进行参数设置和获取的时候可以通过Parameter service的方式轻易完成

### 可视化工具

- ROS提供了一些比较好用的可视化工具立体化展示某一个拓扑结构里面的拓扑网络，RViz就是其中之一
- RViz在整个ROS生态里可以看成是一个节点，它定义了整个拓扑结构里面所有的消息，然后按照固定的格式进行图形化展示