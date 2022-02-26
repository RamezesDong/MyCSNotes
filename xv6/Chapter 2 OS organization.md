# Chapter 2: OS organization

## 读源码

1. entry.S

   首先将sp设为stack0的地址，在将它与(hartid *  4096)，并且跳转到start.c的start函数。

问题：

- hartid是什么？csrr 什么意思？

  csr指riscv中的控制和状态寄存器，而csr指令用来对其进行操作。`csrr a1 mhartid`是伪指令对应`csrrs a1, mhartid, x0`，用于读取CSR。

  而mhartid（Hart ID Register）：运行当前代码硬件线程(hart)的ID（见[RISC-V 特权模式：机器模式 | BalanceTWK的博客](https://balancetwk.github.io/2020/12/05/hexo-blog/RISC_V_Note/RISC-V%20%E7%89%B9%E6%9D%83%E6%A8%A1%E5%BC%8F%EF%BC%9A%E6%9C%BA%E5%99%A8%E6%A8%A1%E5%BC%8F/)）

  > **Hart** ：英文全称 hardware thread ，表示一个硬件线程，对软件层面来说，就是一个独立的处理器。但是实际上也有可能**它不是完整的独立的一个 CPU 核心**。这里我们需要理解一个概念，那就是超线程技术，超线程技术就是将一个处理器运算单元被多个硬件线程复用，每个硬件线程有自己独立的一套通用寄存器等上下文资源。

- 为什么`csrr a1, mhartid`，能通过硬件线程编号，找到栈顶位置？？

```assembly
.section .text
.global _entry
_entry:
	# set up a stack for C.
        # stack0 is declared in start.c,
        # with a 4096-byte stack per CPU.
        # sp = stack0 + (hartid * 4096)
        la sp, stack0
        li a0, 1024*4
	csrr a1, mhartid
        addi a1, a1, 1
        mul a0, a0, a1
        add sp, sp, a0
	# jump to start() in start.c
        call start
spin:
        j spin

```

此时我们仍然处于机器模式，需要进入监管者模式

![image-20220223150535749](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220223150535749.png)

-----

2. start.c

- `__attribute__ ((aligned (16))) char stack0[4096 * NCPU];` entry.S 需要每一个CPU都有一个栈。而attribute相关用法见[__attribute__的使用](https://blog.csdn.net/qlexcel/article/details/92656797)

- `uint64 timer_scratch[NCPU][5];` （每个CPU）用于机器模式时间中断的暂存区

- ```c
  // entry.S jumps here in machine mode on stack0.
  void
  start()
  {
    // set M Previous Privilege mode to Supervisor, for mret.
    unsigned long x = r_mstatus();
    x &= ~MSTATUS_MPP_MASK;
    x |= MSTATUS_MPP_S;
    w_mstatus(x);  //设置mstatus中表示异常的那一位为管理者模式
  
    // set M Exception Program Counter to main, for mret.
    // requires gcc -mcmodel=medany
    w_mepc((uint64)main); //当我们调用mret就返回main函数处
  
    // disable paging for now.
    w_satp(0);
  
    // delegate all interrupts and exceptions to supervisor mode.
    w_medeleg(0xffff);
    w_mideleg(0xffff);
    w_sie(r_sie() | SIE_SEIE | SIE_STIE | SIE_SSIE);
  
    // configure Physical Memory Protection to give supervisor mode
    // access to all of physical memory.
    w_pmpaddr0(0x3fffffffffffffull); //设置物理地址的内存保护，允许监管者模式访问所有物理内存
    w_pmpcfg0(0xf);
  
    // ask for clock interrupts.
    timerinit();
  
    // keep each CPU's hartid in its tp register, for cpuid().
    int id = r_mhartid(); 
    w_tp(id);
  
    // switch to supervisor mode and jump to main().
    asm volatile("mret");
  }
  ```

- 在进行一系列设置后start才”返回（恢复）“main函数处

----

3. main.c

- 进行完一系列设备和subsystem的初始化后开启第一个进程，userinit
- userinit 运行一段汇编代码，叫`initcode`将系统调用SYS_exec加载入寄存器`a7`，调用ecall返回内核
- 内核使用syscall运行想要的系统调用，在`initcode`中运行了`init`这个程序
- CPU0会先进行初始化，并在初始化时调用`userinit`；其余CPU只有等CPU0把`started`设为1后，才能进行初始化。CPU0与其他CPU初始化时调用的函数并不相同。

4. 最后

- `exec`(initcode.S)运行新的程序“init“（init.c），init将创建console，打开文件描述符0,1,2,之后它在控制器上打开一个shell。系统就这么开始运行了。

5. 额外注意点

- 这些二进制代码对应`initc`![image-20220224111742232](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220224111742232.png)

- ```c
  if(open("console", O_RDWR) < 0){
      mknod("console", CONSOLE, 0);
      open("console", O_RDWR);
    }
  //打开console失败，则新建一个console类型的设备文件
  ```

  //打开console失败，则新建一个console类型的设备文件

- `init`使用`dup(0)`两次打开描述符1和2的。

- 打开shell的代码也非常值得学习

```c
for(;;){
    printf("init: starting sh\n");
    pid = fork();
    if(pid < 0){
      printf("init: fork failed\n");
      exit(1);
    }
    if(pid == 0){
      exec("sh", argv);
      printf("init: exec sh failed\n");
      exit(1);
    }

    for(;;){
      // this call to wait() returns if the shell exits,
      // or if a parentless process exits.
      wpid = wait((int *) 0);
      if(wpid == pid){
        // the shell exited; restart it.
        break;
      } else if(wpid < 0){
        printf("init: wait returned an error\n");
        exit(1);
      } else {
        // it was a parentless process; do nothing.
      }
    }
  }
```

使用for循环等待所有shell子进程结束，若shell退出，则重启shell，否则可以用来接受孤儿进程。