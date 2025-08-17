# 虚实映射机制与MMU分析

[TOC]

# 1、介绍

虚实映射：从虚拟地址转换为物理地址的过程，借助硬件MMU完成。

主要流程：（目前多数系统启动都分为两个阶段：在汇编启动阶段完成基础硬件的初始化 + 跳转到C程序启动阶段完成后续初始化）

* 1、汇编启动阶段：
  * 1.1、关闭MMU
  * 1.2、准备一级页表和需要映射的内存段
  * 1.3、填充一级页表的页表项
  * 1.4、构建一级页表
  * 1.5、将当前正在运行的代码区域在临时一级页表中进行恒定映射
    * PA ==> MMU ==> VA
    * PA ==> MMU ==> PA
  * 1.6、开启MMU
  * 1.7、使用一级页表替换临时一级页表
* 2、C程序启动阶段
  * 2.1、切换到临时页表
  * 2.2、设置多级页表并填充页表项
  * 2.3、切换到多级页表

其中Liteos_a内核使用的是二级页表，所以在其C程序启动阶段会进行二级页表及其页表项的设置。

Liteos_a内核具体的MMU初始见思维导图：[Liteos_a内核MMU启动流程分析.zxm](Liteos_a内核MMU启动流程分析.zxm)



# 2、对虚实映射的问题思考

mmap函数的映射：
- 申请虚拟内存且不映射物理内存，
等到cpu访问此虚拟地址时，触发缺页异常，
在异常处理程序中进行虚实映射。
- 在异常处理函数OsVmPageFaultHandler中，
一次只会进行一页的虚实映射，要是mmap申请了不止一页，
那么其他页的内存被访问时，还会再次触发缺页异常进行一页的虚实映射。

mmap是申请一块虚拟内存，并没有申请将这块虚拟内存和物理内存做映射；

在cpu发出访问这块虚拟内存的指令时，会触发缺页异常，在异常处理函数中，会进行虚实映射。
缺页异常（Page Fault）既可能由 Data Abort 触发（数据访问），也可能由 Prefetch Abort 触发（代码执行）。

* Data Abort（数据访问异常）

  - 触发时机：当 CPU 执行“数据访问指令”（如加载、存储指令）时，访问了无效的虚拟地址（如没有建立虚实映射、权限不符、物理页不存在等），MMU 检查失败，就会产生 Data Abort 异常。

  - 典型场景：访问未映射内存、非法写入只读页、缺页异常（Page Fault）等。

  - 处理方式：操作系统会进入 Data Abort 异常处理流程，通常会调用缺页异常处理函数，尝试补页或终止进程。

- Prefetch Abort（预取异常/指令取值异常）
  - 触发时机：当 CPU 取指令（即尝试从内存读取下一条要执行的指令）时，发现该虚拟地址无效（如没有映射、权限不符等），MMU 检查失败，就会产生 Prefetch Abort 异常。
  - 典型场景：跳转到未映射的代码区、执行不可执行区域的指令等。
  - 处理方式：操作系统会进入 Prefetch Abort 异常处理流程，通常会检查是否可以补页，否则终止进程。

CPU发出一个访问虚拟地址的指令之后，在MMU（硬件）中会发现此虚拟地址对应的物理地址无效，
之后MMU（硬件）会触发异常。无需写代码处理发现页表项无效、也无需写代码触发异常。

```c
Prefetch Abort（预取异常/指令取值异常）/ Data Abort（数据访问异常）:
__exception_handlers(异常中断向量表):
	_osExceptPrefetchAbortHdl / _osExceptDataAbortHdl :
		OsArmSharedPageFault
			OsVmPageFaultHandler
				// 1) 根据异常虚拟地址获取所属的虚拟内存空间
				LosVmSpace *space = LOS_SpaceGet(vaddr); 
				// 2) 加锁保护虚拟空间区域操作
				(VOID)LOS_MuxAcquire(&space->regionMux); 
				// 3) 查找异常地址所属的虚拟内存区域
				region = LOS_RegionFind(space, vaddr);   
				// 4) 如果是文件映射区域，进行文件映射缺页处理 OsDoFileFault，之后goto DONE;
				if (LOS_IsRegionFileValid(region)) { 
					...
					status = OsDoFileFault(region, &vmPgFault, flags);
					goto DONE;
					...
				}
				// 5) 不是文件映射区，分配新的物理页
				newPage = LOS_PhysPageAlloc();
				// 6) 获取新物理页物理地址
				newPaddr = VM_PAGE_TO_PHYS(newPage); 
				// 7) 查询原MMU映射
				status = LOS_ArchMmuQuery(&space->archMmu, vaddr, &oldPaddr, NULL); 
				// 7.1) 有映射，先解除原有映射LOS_ArchMmuUnmap，
				//      接着拷贝原页面内容到新页OsPhysSharePageCopy，
				//      再建立新的映射关系LOS_ArchMmuMap
				if (status >= 0) {
					// 解除原映射
					LOS_ArchMmuUnmap(&space->archMmu, vaddr, 1); 
					// 拷贝原页内容到新页
					OsPhysSharePageCopy(oldPaddr, &newPaddr, newPage); 
					// 建立新物理页的映射关系
					status = LOS_ArchMmuMap(&space->archMmu, vaddr, newPaddr, 
                                            1, region->regionFlags);		
				}
				// 7.2) 没有原映射，直接建立新页映射LOS_ArchMmuMap
				else {
					status = LOS_ArchMmuMap(&space->archMmu, vaddr, newPaddr, 
                                            1, region->regionFlags);
				}
				// 8) 解锁
				(VOID)LOS_MuxRelease(&space->regionMux); 
				// 9) 返回处理结果
				return status; 

```

