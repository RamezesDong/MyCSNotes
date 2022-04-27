# NJU PA

[toc]

## PA0 世界诞生的前夜

尝试用wsl作为实验环境失败，改用ubuntu虚拟机

- RTFM, STFW, RTFSC：老师用心良苦呀

1. 必答题：独立解决问题是作为码农的一项十分重要的生存技能......请仔细阅读[提问的智慧](https://github.com/ryanhanwu/How-To-Ask-Questions-The-Smart-Way/blob/master/README-zh_CN.md)和[别像弱智一样提问](https://github.com/tangx/Stop-Ask-Questions-The-Stupid-Ways/blob/master/README.md)，.......写一篇不少于800字的读后感.

   写八百字就不必写了，简单谈谈我自己学习过程的感受。过去的搜索引擎是百度，搜到的内容都是csdn和博客园的blog。我觉得csdn等等的好处是：

   - 对于落后的计算机课程(实验)，能够立刻搜索到答案（注意是答案）。所以能够帮助我快速完成某些乐色实验。

   但当想解决一个bug时，使用csdn效率会很低。中文互联网的抄袭实在是过多，往往一次搜索，得到的都是反复抄袭后的雷同内容。所以，推荐使用搜索引擎为google(可以先用bing)，推荐的搜索语言为英文。

   关于提问：我认为在学习计算机课程中，提问的最好对象当然是老师，助教和同学，如果有一个piazza能够保留大家的提问信息，更能提高所有人的问答效率。

## PA1 开天辟地的篇章

### RTFSC

1. 配置ccache：修改`.bashrc`的path时要注意写成`export PATH=/.../..:$PATH`,防止丢失root创建的默认path

2. PA可以选择不同的ISA来完成实验，我选择riscv，因为学过。。且简单

3. 计算机可以没有寄存器吗？

   - 关于编程模型店理解，我STFW，没有找到一个很完美的解释。
   - 计算机当然可以没有寄存器。按照程序是个状态机这么理解，计算机就可以只用内存来存储状态。
   - stack machines：编写函数式语言程序，通过栈来存储operands，FILO，通过递归来处理程序。
   - 没有寄存器，所以都操作数都从内存读取，速度一定比有寄存器的机器慢很多。

4.  kconfig生成的宏与条件编译，宏是如何工作的？

   - macro定义一段代码，在编译（准确的说是预处理器）时用定义值替代这段代码

5. `init_monitor()`为什么全是函数？

   - 便于代码的编写，阅读理解和调试

6. RTFSC:

   - ```c
     void init_rand() {
       srand(MUXDEF(CONFIG_TARGET_AM, 0, time(0)));
     	//如果target_am有定义用0，否则使用现在都时间
     }
     ```

   - `parse_args()`需要RTFM

   - ![image-20220423130202875](https://cdn.jsdelivr.net/gh/RamezesDong/Pictures-for-Markdown@main/img/image-20220423130202875.png)

   - 看手册的我表示懵逼的很呀，但对照代码看就清晰了

   - ```c
     const struct option table[] = {
         {"batch"    , no_argument      , NULL, 'b'},
         {"log"      , required_argument, NULL, 'l'},
         {"diff"     , required_argument, NULL, 'd'},
         {"port"     , required_argument, NULL, 'p'},
         {"help"     , no_argument      , NULL, 'h'},
         {0          , 0                , NULL,  0 },
       };
     // name表示参数选项的名字，log为是否需要参数，flag=NULL，则用val来代表参数选项的名字，实验代码用来字母简称
     ```

   - `getopt_long(...)`的返回值是上面结构体的`val`。根据输入的参数进行相应的配置

   - `init_log()`很简单，配置log对应的文件

   - `init_mem()`malloc一个`CONFIG_MSIZE 0x8000000`(128MB)的内存。如果配置了内存随机化，进行内存随机化处理

7. 另外的一个问题是, 这些参数是从哪里来的呢?

   - 不知道，之后再看看

8. `sudo dmesg`打印内核信息，打印出的信息很夸张，暂时搁置。

9. 在`cmd_c()`函数中, 调用`cpu_exec()`的时候传入了参数`-1`, 你知道这是什么意思吗?

   - -1的二进制是`1111...11`，64位1，则传入变量-1，代表执行2^64 - 1次。

10. "调用`cpu_exec()`的时候传入了参数`-1`", 这一做法属于未定义行为吗? 请查阅C99手册确认你的想法.

    - 我觉得应该合法。不知道看手册哪里
    - 搁置中

11.  谁来指示程序的结束?

    - STFW和之前看的apue。
    - 在main函数中`return `和`exit()`作用是一样的。都会进行清楚过程--关闭指针，打开的文件描述符，刷新缓冲区。
    - 而`_Exit()`直接退出程序，不进行清楚操作

12. 优美地退出

    - 报错如下，检查发现与makefile无关![image-20220423145628421](https://cdn.jsdelivr.net/gh/RamezesDong/Pictures-for-Markdown@main/img/image-20220423145628421.png)

    - 在`sdb.c`中找到了关键代码![image-20220423145740512](https://cdn.jsdelivr.net/gh/RamezesDong/Pictures-for-Markdown@main/img/image-20220423145740512.png)

      `cmd_q`直接返回-1，最后回到`nemu-main.c`，那问题很可能就在`is_exit_status_bad()`中

    - 使用`grap`找到文件。之后就很简单了。

### 基础设施：简单调试器

1. `sscanf`与`scanf`类似，但不是从标准输入读入，可以自己指定字符串
2. 单步执行，打印寄存器都很简单。使用`printf("%0*X")`能保存前缀0
3. 扫描内存，我的结果只能打印`0x80000000`附近的内存，不能打印`0x100000`的内存

### 表达式求值

1. 我先完成了词法分析和递归求值的代码，却没有良好的测试，感觉已经算是犯了大忌。

2. 为什么printf()的输出要换行?

   - unix上的标准输入输出是有缓冲的，一般是行缓冲。如果printf中没有“\n”，输出将先被缓存起来，此时要遇到问题,则缓冲区信息就会无法显示。

3. 递归求值实现

   - 手册讲解的很清楚，但实现出来也挺不容易的。
   - 我有个问题，如果遇到bad expressions，应该如何退出？

4. 测试代码, 自己写测试代码：

5. sizeof vs strlen()

   - sizeof 是一个一元运算符，而`strlen()`是一个c定义的函数

   - sizeof 给出任意数据类型的实际大小(in bytes)

     ```c
     printf("Length of String is %lu\n", strlen(str));
     printf("Size of String is %lu\n", sizeof(str));
     //Length of String is 8
     //Size of String is 9
     ```

6. `sprintf()`与`sscanf()`类似，`int sprintf(char *str, const char *format, ...)`
7. `system()`执行一个shell命令，通过`fork()`创建子进程执行`execl("/bin/sh","sh","-c",commad,(char *) NULL)`
8. `popen, pclose`- pipe stream to or from a process: 

