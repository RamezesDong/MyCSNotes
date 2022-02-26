# Lab system_calls

## 系统调用的过程

1. （用户模式的系统调用），通过`user/usys.pl`生成`user/usys.S`（事实上的系统调用stub），使用RISC-V ecall指令进入内核。
   - [ 什么是桩代码（Stub）？ - 知乎](https://www.zhihu.com/question/24844900)
2. `kernel/syscall.c/syscall()`读取`a7`的调用号，通过`syscalls[num]()`转入相关系统调用的内核实现
3. 通过一些寄存器读取用户使用系统调用时的参数：内核函数`argint`,`argaddr`,`argfd`从*陷阱框架trap frame*取回第n个是同调用的参数，作为整数，指针或者文件描述符。

## System call tracing

按照实验指导完成。小结：

- 从实验我们可以发现，mask是与proc(线程)绑定在一起的，当某个线程使用系统调用时，便会检测mask，来判断是否输出tracing

## Sysinfo

在这个实验逼着我看了`kalloc.c`的源代码：

- `extern char end[]` 存放kernel后第一个地址
- `struct run` 和`strcut {spinlock...run...} kmem` 等于使用链表来存储空的内存区域，每个区域大小是PGSIZE(4096Bytes)
- `void kinit()` 初始化`PHYSTOP`（2gb + 128mb）大小的内存
- `void freerange(void *pa_start, void *pa_end)`释放区间内的内存
- `void kfree(void *pa)` 释放一块内存
- `void * kalloc(void)`申请一块内存
- `uint64 free_memory()` 统计空内存大小

`nums_proc()`很好实现，但是我因为疏忽，造成了一个bug

- `for (p = proc; p < &proc[NPROC]; p++)`写成了`for (p = proc; p < &p[NPROC]; p++)`一时间没有发现。指针作为迭代量，进行循环，确实很容易犯错。

## 总结

细心，阅读，思考，再编码。

