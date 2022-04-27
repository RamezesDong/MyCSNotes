## LSTM

#### 长期依赖(Long Term Dependencies)

在深度学习领域中（尤其是RNN），“长期依赖“问题是普遍存在的。长期依赖产生的原因是当神经网络的节点经过许多阶段的计算后，之前比较长的时间片的特征已经被覆盖，例如下面例子

```text
eg1: The cat, which already ate a bunch of food, was full.
      |   |     |      |     |  |   |   |   |     |   |
     t0  t1    t2      t3    t4 t5  t6  t7  t8    t9 t10
eg2: The cats, which already ate a bunch of food, were full.
      |   |      |      |     |  |   |   |   |     |    |
     t0  t1     t2     t3    t4 t5  t6  t7  t8    t9   t10
```

我们想预测'full'之前系动词的单复数情况，显然full是取决于第二个单词’cat‘的单复数情况，而非其前面的单词food。根据图1展示的RNN的结构，随着数据时间片的增加，RNN丧失了学习连接如此远的信息的能力。

#### LSTM

LSTM的全称是Long Short Term Memory，顾名思义，它具有记忆长短期信息的能力的神经网络。LSTM首先在1997年由Hochreiter & Schmidhuber 提出，之后经过若干代的发展，逐渐形成了比较系统且完整的LSTM框架，并且在很多领域得到了广泛的应用。

LSTM 的基础结构与 RNN 没有差别，LSTM 将每个时间节点视作一个“细胞状态”，信息在每个细胞上顺序流动。LSTM中的重复模块包含四个相互作用的激活函数(三个sigmoid，一个tanh):（图中每条线表示一个完整向量，从一个节点的输出到其他节点的输入。粉红色圆圈代表逐点操作，比如向量加法，而黄色框表示门限激活函数。线条合并表示串联，线条分差表示复制内容并输出到不同地方。）

![image-20220322211658335](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220322211658335.png)

正向传递第一步：决定哪些信息从单元状态中抛弃。公式中，括号左边的是sigmoid函，W是连接权重矩阵，其下标对应那条连接，W_f是遗忘门（forget）连接的权重矩阵，h_t-1是隐含层上一个时间节点的输出，x_t是t时刻输入层的输出，b_f是遗忘门的偏移值。

![image-20220322212058896](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220322212058896.png)

第二步：决定单元状态中保存哪些新信息。分为两步：生成临时新状态、更新旧状态。i为输入门的输出，决定哪些值需要更新； C't 是临时状态，包含新候选值。

![image-20220322212240554](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220322212240554.png)

旧状态Ct-1 乘以ft，忘记决定要忘记的，再加上新的候选值it*C't ，就得到新的状态。

![image-20220322212301971](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220322212301971.png)

第三步决定要输出什么：首先通过sigmoid函数决定哪些需要输出，再将单元状态输入到tanh函数（将值转化为-1到1之间），再乘以sigmoid门限值，得到输出。

![image-20220322212337740](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220322212337740.png)

#### LSTM缺陷

1. RNN的梯度问题在LSTM及其变种里面得到了一定程度的解决，但还是不够。它可以处理100个量级的序列，而对于1000个量级，或者更长的序列则依然会显得很棘手。
2. 计算费时。每一个LSTM的cell里面都意味着有4个全连接层(MLP),如果LSTM的时间跨度很大，并且网络又很深，这个计算量会很大，很耗时