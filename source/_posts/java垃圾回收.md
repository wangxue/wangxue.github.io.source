---
title: java垃圾回收
date: 2017-05-02 18:04:43
tags:
category: java
---
在开发java应用时，开发人员往往不需要关心对象的释放，java自带内存回收机制，即garbage collection，但作为开发人员有必要了解gc的一些基本概念，如java的内存模型，gc在什么时候开始回收内存，如何回收等。
### jvm模型
![hotspot architecture](http://onondv8t2.bkt.clouddn.com/hotspot_arch.PNG)
hotspot jvm主要包括上图中的组件：classloader, 运行时数据区，执行引擎。

其中jvm性能调优时有三个部分是需要注意的，即图中heap, jit compiler, Garbage Collector。 heap为存储对象数据的地方，由garbage collector负责管理。大部分的优化思路为调整heap大小和选择合适的garbage collector，jit compiler通常不需要调整。
### 自动回收(Automatic Garbage Collection)流程
自动回收的过程为检查heap内存，确定无用对象并删除对象释放内存，然后整理内存，将使用的对象移到一起，这样为新对象分配内存更快。
jvm heap是分为几个部分的:Young Generation，Old or Tenured Generation，Permanent Generation。
![heap generations](http://onondv8t2.bkt.clouddn.com/heap_generations.PNG)
Young Generation用来存放刚分配的以及长大的(aged)对象，当Young Generation填满时就会引发minor gc。
"stop the world"事件：所有的minor gc都是"stop the world"事件
### garbage collector类型
### gc root