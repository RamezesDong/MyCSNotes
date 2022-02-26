# Leetcode

[toc]

### 0.0资料

[leetcode题目简评](https://github.com/huangrt01/CS-Notes/blob/master/Notes/Output/leetcode%E9%A2%98%E7%9B%AE%E7%AE%80%E8%AF%84.md)

[leetcode精选题详解](https://github.com/azl397985856/leetcode)

[代码速查表](https://github.com/OUCMachineLearning/OUCML/tree/master/代码速查表)

[图解leetcode](https://github.com/MisterBooo/LeetCodeAnimation)

### 0.1 已收录题目

15，45，56，75, 119, 138，169， 213，238, 240,  287，400, 435, 560, 740

### 1.0 题目

##### 0015. [三数之和](https://leetcode-cn.com/problems/3sum/)

- 排序+双指针

##### 0045.[跳跃游戏II](https://leetcode-cn.com/problems/jump-game-ii/)

- 就是**贪心**算法，我一直执拗于dp

##### 0048.[旋转图像](https://leetcode-cn.com/problems/rotate-image/)

- 方法一：旋转二维图像等价于将图像轴对称两次，一次沿着中线，一次沿对角线
- 二：原地旋转，找四个块之间的关系

##### 0056. [合并区间](https://leetcode-cn.com/problems/merge-intervals)

- sort+遍历

- cmp

  ```cpp
  static bool cmp(vector<int>& a, vector<int>& b){
      return a[0] < b[0] || (a[0] == b[0] && a[1] < b[1]);
  }
  ```

  

##### 0075.[颜色分类](https://leetcode-cn.com/problems/sort-colors/)

-  荷兰国旗问题
- 联想快速排序的partition分治过程。选取主元将数组分为小，主元，大，三部分。
- O（N）时间

##### 0119.[杨辉三角 II](https://leetcode-cn.com/problems/pascals-triangle-ii/)

- C(n)(i) = C(n-1)(i)+C(n-1)(i-1)

  逆序更新序列就可以只用一个数组存第n行的值

##### 0138.[复制带随机指针的链表](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)

- [《剑指offer》第35题](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)
- 我写的朴素算法：时间非常慢O（N^2）
- **迭代+节点拆分**，非常值得学习
  - 每个节点加它的复制![image-20211127161504941](https://gitee.com/dongramesez/typora-img/raw/master/img/202111271615018.png)
  - 遍历，p->next->random = p->random->next
  - 将两个链表分离
  - 时间复杂度O(n),空间复杂度O(1)
- **哈希+回溯**
  - 拷贝节点时，利用回溯使节点创建相互独立。若随机指向的节点未被创建，使用递归法来创建它。
  - 为了防止重复拷贝，使用哈希表来存储指针
  - 时间复杂O（N），空间复杂度O(N)

##### 0169. [多数元素](https://leetcode-cn.com/problems/majority-element/)

- 空间复杂度O(N) :Hashmap法
- 时间复杂度O(NlogN): 排序法（数组中间必然是超半数元素）
- 摩尔投票法：空间O(1),时间O(N)。确定一个候选人（candidate_num）初始化为nums[0]，票数（count）初始化为1。遍历数组，candidate相同，count++，否则count--，count==0，更换candidate。最后得到超半数者，选为leader。

##### 0213.[打家劫舍II](https://leetcode-cn.com/problems/house-robber-ii/)

- 打家劫舍I拓展版，将屋子连成环

##### [238. 除自身以外数组的乘积](https://leetcode-cn.com/problems/product-of-array-except-self/)

- 要求O（N）时间 ，不使用除法
- 解：前缀乘积，一目了然![image-20211203232647433](https://gitee.com/dongramesez/typora-img/raw/master/img/202112032326591.png)

##### 0240.[搜索二维矩阵 II](https://leetcode-cn.com/problems/search-a-2d-matrix-ii)

- [《剑指offer》第4题](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)
- 除普通遍历方法和遍历加二分查找外，还有一种将矩阵近似成二叉搜索树的方法
- 关键在于起点的选取，从左下角或者右上角开始![image-20211201170253991](https://gitee.com/dongramesez/typora-img/raw/master/img/202112011702167.png)

##### 0287 [寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number/)

- 要求使用O（1）空间

  - 方法一：位运算：  计算给定数组（存储在“b”变量中）和数组 [1, 2, 3, ..., n] （存储在“a”变量中）的每 32 位总和。如果“b”大于“a”，则表示重复的数字在当前位位置为1（否则，“b”不能大于“a”）O(logN)
  - 方法二：二分查找，**对值域二分**。在1-n范围二找一个整数，而不是在输入中查找。O（NlogN）
  - 方法三：**快慢指针**：将数组看作链表O（N）

##### 0400.[第 N 位数字](https://leetcode-cn.com/problems/nth-digit/)

- 模拟题。通过直接计算方法得到是几位数，在具体找到那一个数。

##### 0435.[无重叠区间](https://leetcode-cn.com/problems/non-overlapping-intervals/)

- 将问题转化为求最多子区间使他们不重叠。按右边界排序，从左往右遍历。
- 动态规划：O（N^2）
- **贪心**：O(NlogN)每次取一个子区间后，要使剩余最大可能取值大于等于最优解。

#####  [0560. 和为 K 的子数组](https://leetcode-cn.com/problems/subarray-sum-equals-k/)

- 普通前缀和，O(N^2)超时
- **哈希表**优化
  - 注意到`presum[now] - presum[pre] == k`等价`presum[now] - k == presum[pre]`而`presum[pre]`已经被存储在Hashtable中

##### 0740.[删除并获得点数](https://leetcode-cn.com/problems/delete-and-earn/)

- 想出来方法了，hashmap统计一遍每个数出现次数，然后就当做打家劫舍那样做。
- 为了节省空间，取数组长度为maxnum大小
- 时间复杂度O（N+maxnum）空间复杂O（N）

### 2.0 竞赛题目

##### 5939.[半径为k的子数组平均值](https://leetcode-cn.com/problems/k-radius-subarray-averages/)

- 前缀和题目，不难
- 因为一个样例爆范围了，需要用long
- 用python写就不会被卡:cry:

##### 5941.[找出知晓秘密的所有专家](https://leetcode-cn.com/problems/find-all-people-with-secret/)

- 方法一：多源DFS
  - 同一时间会议为一组，每一组会议构成一个图
  - 对图上当前知晓秘密的人进行多源DFS
  - 时间O（N+E+MAXT）空间（N+E）
- 方法二： 并查集
  - 同一时间会议为一组，建立并查集，共同好友相当于一个大组
  
  - 每个时间点的人互相联系。最后将未得知秘密的人断开连接。
  

##### [5942. 找出 3 位偶数](https://leetcode-cn.com/problems/finding-3-digit-even-numbers/)

- 要直接枚举每种情况太复杂，并且很慢
- 暴力： 枚举【100，999】中所有偶数，判断原数组是否能组成

### 3.0 补充知识

#### Kadane's Algorithm

- 用于计算最大连续子数组
- 关注 localmax
  - `localmax[i] = max(A[i],localmax[i-1]+A[i])`

- 全局max
  - `maxans = max (maxans, localmax)`

##### [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

- 基础版

##### [918. 环形子数组的最大和](https://leetcode-cn.com/problems/maximum-sum-circular-subarray/)

- 升级版
- 最大连续子数组有两种情况
  - 如53，单连续子数组
  - 头--尾双区间连续子数组：可以等价为**数组总和-最小连续子数组**
  - 最小连续子数组也可以使用kadane算法（全局最小为所有局部最小的最小值）
  - 注意：可能出现全负情况，则返回maxn即可

#### tarjan算法求SCC（强连通分量）& 缩点

[tarjan算法求SCC（强连通分量）& 缩点](https://www.cnblogs.com/jrdxy/p/13172875.html)

[tarjan模板](https://www.cnblogs.com/jjmg/p/14026900.html)

## 4.0 专辑

### 剑指offer

#### [剑指 Offer 04. 二维数组中的查找 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/):

- O(n+m)做法：一图以蔽之![image-20220223233330284](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220223233330284.png)



#### [剑指 Offer 35. 复杂链表的复制](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)

- 一：哈希表建立原节点和新节点的对应关系
- 二：**拼接+拆分**：构造“原节点->新节点->原节点->新节点”这样的链表，可以在第二次遍历时设置random，在第三次遍历将两个链表拆开。

