## CS186_Proj1

### 写在前面

本人在学习cs61a之前对于数据库以及SQL是一无所知的，借由cs186-sp2022开课，准备同步跟进。自从开始刷课，我日益感到自己基础的薄弱，思考能力的欠缺。便向null哥学习，借由写实验笔记与大家交流，共同进步。

另：笔记写得比较随便，请见谅

### 项目环境配置

按着[CS186 Projects](https://cs186.gitbook.io/project/) git book来就行，唯一坑点就是：咱没有学生账号，也进不了piazza，代码框架需要来这个组织下找[UC Berkeley CS 186](https://github.com/berkeley-cs186)。ucb好温柔，代码都让我们白嫖，感动。

其他学习资源参见[PKUFlyingPig的仓库](https://github.com/PKUFlyingPig/CS186)。

### Task 1,2

小试牛刀，不算难，有bug的话可以参考输出信息和diff文件。我遇到的坑点是：

- [数据集信息](http://www.seanlahman.com/files/database/readme2017.txt)

- California 名字应该用简写‘CA’，实验书没说明白
- 注意内连接，外连接使用。我因在INNER前使用了“,”导致bug

### Task 3

是不是被刚开始的一大堆专业文字搞晕了，没关系，咱先来了解一下这一项有趣的运动。

- [恍然大悟，原来棒球规则这么简单！](https://www.bilibili.com/video/BV1ub41187Tg?from=search&seid=7207739961690735891&spm_id_from=333.337.0.0)
- [这可能是你见过最好的棒球科普文（上篇）](https://zhuanlan.zhihu.com/p/20827923)

OK，现在你大概明白赛伯计量学的作用了，就是对每个选手利用数学方法，做最全面最客观的分析，方便之后的训练，比赛，人员补强等等。那么让我们开始吧！

q3i坑点总结： 

- 使用CAST进行类型转换
- 注意使用ROUND保留4位小数

q3ii与q3iii就是q3i的plus版本，使用SQL子查询即可。

### Task 4

1. q4ii：最好创建一个table包含0-9的id，最后结果需要根据binid分组，否则会出现COUNT（*）结果都记到`binid = 0`的元组中都情况。
2. q4v: 注意allstarfull与salaries中都有teamid，要选用allstarfull的。

![image-20220123121859885](https://gitee.com/dongramesez/typora-img/raw/master/img/202201231219003.png)

### 结语

1. 有cs61a的经验，本次实验还算轻松。
2. SQL挺考验思维的，以后可以刷刷leetcode加强一下。
3. 期待下一个proj。





