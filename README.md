# 操作系统技术文章
[TOC]

# 零、介绍

本仓库主要分享操作系统相关的技术文章，目标是对技术的核心原理进行剖析。

（欢迎各位网友批评指正，如发现错误，请给出正确版本，验证之后会进行修改更新）



文章大致使用以下架构进行编写：

* 1、对技术的概念进行介绍。

* 2、分析该技术的通用实现流程。

* 3、借助具体的工程项目代码，按照通用流程进行分析，并总结在该工程项目中的具体流程。



# 一、进程、线程/任务、调度

通用分析：

Liteos_a 内核实现分析：

* 1、 [进程及Liteos_a内核进程分析.md](docs/进程及Liteos_a内核进程分析.md) 
* 2、 [线程及Liteos_a内核线程分析.md](docs/线程及Liteos_a内核线程分析.md) 
* 3、 [调度及Liteos_a内核调度分析.md](docs/调度及Liteos_a内核调度分析.md) 

Linux 内核实现分析：

* 1、[Linux内核进程线程模型及其创建流程分析.md](docs/Linux内核进程线程模型及其创建流程分析.md)
* 2、[Linux内核调度模型初始化和运行流程分析.md](docs/Linux内核调度模型初始化和运行流程分析.md) 
*  3、[Linux内核CFS调度算法原理与源码分析.md](docs/Linux内核CFS调度算法原理与源码分析.md) 



# 二、ELF 可执行文件

通用分析：

Liteos_a 内核实现分析：

1、 [ELF可执行文件及Liteos_a内核加载ELF文件分析.md](docs/ELF可执行文件及Liteos_a内核加载ELF文件分析.md) 



# 三、IPC 进程间通信机制

1、事件 Event： [事件Event机制与Liteos_a内核事件机制分析.md](docs/事件Event机制与Liteos_a内核事件机制分析.md) 

2、信号量 Semaphore： [信号量Semaphore机制与Liteos_a内核信号量机制分析.md](docs/信号量Semaphore机制与Liteos_a内核信号量机制分析.md) 

3、互斥锁 Mutex： [互斥锁Mutex机制与Liteos_a内核互斥锁机制分析.md](docs/互斥锁Mutex机制与Liteos_a内核互斥锁机制分析.md) 

4、读写锁： [读写锁RWLock机制与Liteos_a内核读写锁机制分析.md](docs/读写锁RWLock机制与Liteos_a内核读写锁机制分析.md) 

5、用户态快速互斥锁：

6、消息队列：

7、信号：

8、共享内存：

```c
# 互斥锁Mutex机制与Liteos_a内核互斥锁机制分析

[TOC]

# 0、一句话总结

# 1、互斥锁的通用知识点

## 1.1、互斥锁的概念

## 1.2、互斥锁的通用结构

## 1.3、互斥锁的运行时序图

# 2、Liteos_a内核中互斥锁的实现

## 2.1、Liteos_a内核中互斥锁的概念

## 2.2、Liteos_a内核的互斥锁运行机制

## 2.3、Liteos_a内核互斥锁模块的总结

# 3、Liteos_a内核互斥锁开发案例

## 3.1、接口说明

## 3.2、开发流程

### 3.3、编程实例

### 3.3.1、实例描述

### 3.3.2、编程示例

# 4、Liteos_a内核互斥锁的源码分析

## 4.1、互斥锁控制块 LosMux

# 5、对Liteos_a内核互斥锁机制的思考

## 5.1、OH工程项目中哪些地方使用到互斥锁

## 5.2、这些地方为什么要使用互斥锁

## 5.3、在这些地方使用互斥锁带来哪些好处

## 5.4、在这些地方使用互斥锁带来哪些坏处

## 5.5、有坏处为什么还要使用

## 5.6、如何在工程中取舍

```



# 四、物理内存、虚拟内存、虚实映射







# 五、中断、异常







# 六、文件系统











[![Star History Chart](https://api.star-history.com/svg?repos=https://github.com/ShareTechnologyForFree/OperatingSystemTechnicalArticles&type=Date)](https://star-history.com/#https://github.com/ShareTechnologyForFree/OperatingSystemTechnicalArticles&Date)
