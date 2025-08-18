# 操作系统技术文章
[TOC]

# 零、介绍

本仓库主要分享操作系统相关的技术文章，目标是对技术的核心原理进行剖析。

（欢迎各位网友批评指正，如发现错误，请给出正确版本，验证之后会进行修改更新）



文章大致使用以下架构进行编写：

* 0、一句话总结该技术。

* 1、对技术的概念进行介绍。

* 2、分析该技术的通用实现流程。

* 3、借助具体的工程项目代码，按照通用流程进行分析，并总结在该工程项目中的具体流程。



文档写作工具：

* markdown工具：[typora](https://typora.io/)
* 思维导图工具：[知犀](https://www.zhixi.com/)（这个能在里面插入代码块）



# 一、进程、线程/任务、调度

通用分析：

* 1、 [协程Coroutine和C语言协程库libaco分析.md](other/协程Coroutine和C语言协程库libaco分析.md) 

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

* 1、 [ELF可执行文件及Liteos_a内核加载ELF文件分析.md](docs/ELF可执行文件及Liteos_a内核加载ELF文件分析.md) 



# 三、IPC 进程间通信机制

通用分析：

Liteos_a 内核实现分析：

* 1、事件 Event： [事件Event机制与Liteos_a内核事件机制分析.md](docs/事件Event机制与Liteos_a内核事件机制分析.md) 

* 2、信号量 Semaphore： [信号量Semaphore机制与Liteos_a内核信号量机制分析.md](docs/信号量Semaphore机制与Liteos_a内核信号量机制分析.md) 

* 3、互斥锁 Mutex： [互斥锁Mutex机制与Liteos_a内核互斥锁机制分析.md](docs/互斥锁Mutex机制与Liteos_a内核互斥锁机制分析.md) 

* 4、读写锁 RWLock： [读写锁RWLock机制与Liteos_a内核读写锁机制分析.md](docs/读写锁RWLock机制与Liteos_a内核读写锁机制分析.md) 

* 5、用户态快速互斥锁 Futex： [用户态快速互斥锁Futex机制与Liteos_a内核Futex机制分析.md](docs/用户态快速互斥锁Futex机制与Liteos_a内核Futex机制分析.md) 

* 6、消息队列 Queue： [消息队列Queue机制与Liteos_a内核消息队列机制分析.md](docs/消息队列Queue机制与Liteos_a内核消息队列机制分析.md) 

* 7、信号 Signal： [信号Signal机制与Liteos_a内核信号机制分析.md](docs/信号Signal机制与Liteos_a内核信号机制分析.md) 

* 8、共享内存 ShareMemory： [共享内核Shm机制与Liteos_a内核Shm机制分析.md](docs/共享内核Shm机制与Liteos_a内核Shm机制分析.md) 

Linux 内核实现分析：

* 1、用户态快速互斥锁 Futex： [Linux内核Futex机制分析.md](docs/Linux内核Futex机制分析.md) 



# 四、物理内存、虚拟内存、虚实映射

通用分析：

Liteos_a 内核实现分析：

* 1、物理内存 Physical Memory： [物理内存与Liteos_a内核物理内存分析.md](docs/物理内存与Liteos_a内核物理内存分析.md) 
* 2、虚拟内存 Virtual Memory： [虚拟内存与Liteos_a内核虚拟内存分析.md](docs/虚拟内存与Liteos_a内核虚拟内存分析.md) 
  * 2.1、虚拟内存区域的红黑树RBTree管理方式： [红黑树RBTree分析.zxm](docs/红黑树RBTree分析.zxm) 
  * 2.2、使用虚拟内存的堆内存管理算法TLSF： [堆内存与Liteos_a内核堆内存TLSF机制分析.md](docs/堆内存与Liteos_a内核堆内存TLSF机制分析.md) 

* 3、虚实映射 Virtual-Physical Mapping： [虚实映射机制与MMU分析.md](docs/虚实映射机制与MMU分析.md) 



# 五、中断、异常

通用分析：

Liteos_a 内核实现分析：

* 1、中断与异常 Interrupt and Exception： [中断异常机制与Liteos_a内核中断异常机制分析.md](docs/中断异常机制与Liteos_a内核中断异常机制分析.md) 
* 2、中断控制器 GIC：  [中断控制器GICv2以及Liteos_a内核使用流程分析.md](docs/中断控制器GICv2以及Liteos_a内核使用流程分析.md) 



# 六、文件系统

通用分析：

Liteos_a 内核实现分析：

* 1、文件系统 File System： [文件系统及Liteos_a内核文件系统分析.md](docs/文件系统及Liteos_a内核文件系统分析.md) 
  * FATFS分析： [FAT文件系统分析.md](other/FAT文件系统分析.md) 
