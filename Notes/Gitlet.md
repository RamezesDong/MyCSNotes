# Gitlet上

## 写在最前

我之前把cs61b-sp18的lab和hw做完了，但是项目迟迟没有推进。听闻新项目gitlet含金量超高，而且autograder即将到期，赶紧来补票。

## 预备

lab6：实验是利用java文件读取与存储，以此来实现保存程序信息的功能。

[Git Pro book -- git Internals](https://git-scm.com/book/en/v2/Git-Internals-Plumbing-and-Porcelain): 非常好的官方文档，且有中文版，看了以后对git内部实现有了初步理解。

[超长的文档](https://sp21.datastructur.es/materials/proj/proj2/proj2#overview-of-gitlet)：61b的项目文档，一万多字，真的太长了。

[Lecture 12: Gitlet](https://youtu.be/fvhqn5PeU_Q)：

- 版本控制的几种方法：从备份所有文件，到只备份被改变的文件。再考虑到使用提交时间为版本号，以及多人同时提交的问题。最终，引出使用SHA1哈希的方案。
- git-SHA1哈希有160bits。Git会创建以哈希值前两位为名字的文件夹，存放着改变文件的压缩形式（zlib）。
- 每一个commit信息也有一个SHA1哈希值，`git log --gragh`可以看到每次提交的信息和父节点信息。
- 最后，讲了一点branching的知识。这部分我曾了解过，hug教授也讲的不深。最后，commit信息应该使用图来表示（而不是链表）。

## 思考

本项目只给了思路指导，代码框架数据结构等等实现细节都没有给出。这些都得自己想，自己实现。对于我这种菜鸡码工，靠自己想构造实在太难了。

以下是我提出的问题，以及自己给出的答案：

- 数据结构应该是怎么样的？

  看了看git源码，linus当时写git主要使用了三种结构: tree,blob,commit。commit对象包含了快照内容指针，提交者，时间，提交信息，以及一或多个指向祖先的指针。tree对象则是文件列表，每一行描述一个文件。而blob对象指向具体文件内容（不包括名称文件类型）。

  所以我决定效仿linus，使用同样的三个类。

- 如何知道当前的header？

  观察`.git`文件列表，可以看到一个HEAD文件，文件内容是路径，内容是refs文件下的分支。

  例如`master`内容就是SHA1哈希，指向具体的commit

- add是如何工作的？

  观察到`.git`文件夹下有一个文件为index，是二进制文件。通过`git ls-files --stage`可以查看其内容。实际上，add将最新修改的代码加入暂存区，创建blob对象保存快照，index就是这些快照的索引文件。

- 具体实现应该是怎么样的呢？

  - commit内容

    ![image-20220129160756033](https://gitee.com/dongramesez/typora-img/raw/master/img/202201291607232.png)

  - tree内容

    ![image-20220129160834497](https://gitee.com/dongramesez/typora-img/raw/master/img/202201291608556.png)

    tree对象内容是一个tree和blob对象的列表

  - blob内容

    ![image-20220129161401632](https://gitee.com/dongramesez/typora-img/raw/master/img/202201291614688.png)

  - index文件

    ![image-20220129172752727](https://gitee.com/dongramesez/typora-img/raw/master/img/202201291727769.png)

    100代表regular file，644代表文件权限；sha-1码后面的数字0表示文件的状态，当有冲突时不为0，其他一般状态为0。

  - 现在我的心里已经有项目的初始目标了，接下来就是实现它。

## 实践

- 暂存区怎么实现？

  原本我想用一个tree来实现它。后来发现tree比较难实现，而文件名与sha1哈希值正好是一对映射，符合hashmap的特征，项目doc其实也给了相关提示：

  > Be careful using a `HashMap` when serializing! The order of things within the `HashMap` is non-deterministic. The solution is to use a `TreeMap` which will always have the same order. 

- stage for removal 又如何？

  问了群友，rm操作就是将INDEX的内容删除，取消追踪，列为removed队列中。如图，git的rm是这样实现的，但git似乎将stage for add and removal结合到了一个文件。

  ![image-20220130162541306](https://gitee.com/dongramesez/typora-img/raw/master/img/202201301625560.png)

  我计划用一个hashmap表示add stage，另一个hashset表示removal stage。

- global-log如何实现？

  需要与number of commits时间线性相关，所以说不可能遍历所有的objects找其中的commit对象。观察到`.git/logs`文件夹下有`refs/heads/main`,`refs/remote/origin`和`HEAD`文件。所以，我决定将所有的commit操作写入一个文件，之后取其内容即可。

- Utils中的write函数是用`BufferOutputStream`实现的，若文件不存在，自动创建文件；若文件存在，默认会覆盖原文件内容。这正好满足我们的需要，因为gitlet是通过取数据，修改数据，写回的过程实现的，直接覆盖原内容方便省事。

### 未完待续

- 今天除夕夜，项目还没做完，gradescope得分400，争取这几天搞完。
