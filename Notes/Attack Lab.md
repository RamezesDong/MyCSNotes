# Attack Lab

### 准备工作

又花了很久才懂得怎么操作。大概流程，读反汇编，通过hex2raw生成字符串，作为输入，ctarget会运行test函数，执行完getbuf后，程序不执行test，直接执行touch函数。