# cs61b



[toc]

---

[课程主页](https://sp18.datastructur.es/)

直接记一下笔记吧

#### w2's Discussion

- ![image-20211024231034681](https://gitee.com/dongramesez/typora-img/raw/master/img/202111121801598.png)

  定义两个猫，赋值不同的noisy，他们改变的是Cat下的同一个noisy，也就是说noisy被后操作的所决定

- 递归和非递归的方法求整数链表的平方

  **对链表理解还不够深，这个问题很经典**

- 全局变量和 local 变量![image-20211025094345645](https://gitee.com/dongramesez/typora-img/raw/master/img/202111121801599.png)

  swaperoo(f1,f2)则f1与f2都变为f2。

  注意传递的变量是一个值还是一个地址（java没有用指针)

- ![image-20211025095842001](https://gitee.com/dongramesez/typora-img/raw/master/img/202111121801601.png)

  multiplyBy3 是去A 中各元素的值，变量名叫 x ，改变的实际上是变量 x。

### lab2

dcatenate() and catenate() 加一句判断A是否为null ，解决 test null arguments 的问题

### proj0

花了整整一天时间，上午在配置IntelliJ IDEA 相关的环境；下午开始写代码，测试文件一直无法单独执行，后来又查了好久。到晚上九点，终于写完代码，一运行发现行星都飞走了。**物理函数写错了**！！！忘记dx是矢量，我求了绝对值。

运行出动画，成就感满满呀！实际上这次难度并不算太大，而且老师的doc真的太详细了。感动 :joy:

![image-20211025214422015](https://gitee.com/dongramesez/typora-img/raw/master/img/202111121801602.png)

#### Dis3

- reverse SLList (a very significant problem which had been done in leetecode )

  ![image-20211027000414432](https://gitee.com/dongramesez/typora-img/raw/master/img/202111121801603.png)

- Must to know: **special cases** 
- ![image-20211027000527683](https://gitee.com/dongramesez/typora-img/raw/master/img/202111121801604.png)

#### proj1 a

- 一直报 style 错误，而且check style 无法运行， 后来问了群里的大佬，设置一下check即可

- 128！=128 直接引用一下

  > "在-128~127的Integer值并且以Integer x = value;的方式赋值的Integer值在进行==和equals比较时，都会返回true，因为Java里面对处在在-128~127之间的Integer值，用的是原生数据类型int，此时调用的是Integer.valueOf()方法，会在内存里供重用，也就是说这之间的Integer值进行==比较时只是进行int原生数据类型的数值比较，而超出-128~127的范围，进行==比较时是进行地址及数值比较。"


#### W4.1: Inheritance , Implements

- interface(接口) and implements（使用接口）

  	- interfaece 不能被用于创建一个对象
  	- interface 中需要将method重写，但不能有函数体

- Interface Inheritance(继承)

  - All sub-class (hyponyms) are said to 'inherat' the interface from their super-class (hypernym)
  - Interface consists of all the method signatures

- GRoE (Golden Rule of Equals)

  we copy the bits from b into a, with the requirement that b is the same type as a.

- Implementation Inheritance 

  We can define the default func in List61B

  ```java
  default public void print() {
      for (int i = 0; i < size(); i += 1) {
          System.out.print(get(i) + " ");
      }
      System.out.println();
  }
  ```

- Static Type vs. Dynamic Type

  ![image-20211028102912325](https://gitee.com/dongramesez/typora-img/raw/master/img/202111121801605.png)

  When Java runs a method that is overriden, it searches for the appropriate method signature in it's **dynamic type** and runs it.

  **IMPORTANT: This does not work for overloaded methods!**

- Interface Inheritance vs. Implementation Inheritance

  - Interface inheritance (what): Simply tells what the subclasses should be able to do.
    - EX) all lists should be able to print themselves, how they do it is up to them.
  - Implementation inheritance (how): Tells the subclasses how they should behave.
    - EX) Lists should print themselves exactly this way: by getting each element in order and then printing them.

#### W4.2 :Extend, Casting, Higher Order Function

- Extends

  By using the `extends` keyword, subclasses inherit all **members** of the parent class. "Members" includes:

  - All instance and static variables
  - All methods
  - All nested classes

- `super` keyword: call the super class constructor

  - While constructors are not inherited, Java requires that all constructors **must start with a call to one of its superclass's constructors**.

- Object Class ：is the parent class of all the classes in java by default

- Encapsulation: It is a module-abstracting

- Casting: Type casting is when you assign a value of one primitive data type to another type

#### Subtype Polymorphism

- Interface quiz (这一章的内容好难理解)

  [**复习(五星级)**](https://www.youtube.com/watch?v=dbdbcbhe3Jk)

- **Comparable and Comparator**

  They are hard to understand. I will review after completing the project 1B.

#### Proj1B

- 遇到了个bug一直没找出来，看别人的代码才懂了。
  - 我用了`removeFirst()` and `removeLast()`来比较头尾, 却忘记了可能出现奇数情况：取头是个字符，取尾是`“”`，会判定为false。

#### Dis4

[PPT](https://docs.google.com/presentation/d/1uN5LwebE8ntlJHJTsCixhP4squ-r_xVUC2vcq8FfUXM/edit#slide=id.g306ea6a40e_0_70)

The picture which is shown in the discussion clearly explains the concept .

![image-20211031110015241](https://gitee.com/dongramesez/typora-img/raw/master/img/202111121801606.png)



![image-20211031110027995](https://gitee.com/dongramesez/typora-img/raw/master/img/202111121801607.png)



![image-20211031110153218](https://gitee.com/dongramesez/typora-img/raw/master/img/202111121801608.png)

![image-20211031110212566](https://gitee.com/dongramesez/typora-img/raw/master/img/202111121801609.png)

- The problem Creating Cats uses the super syntax for calling superclass' constructor
- 动态方法选择此题，用了末尾节点来确保max函数不会出错，用了继承和override(重写)

#### Asymptotic Analysis

- O, Ω, and Θ

  	- f(n)=O(g(n)): c*g(n) is upper bound on f(n)

   -  f(n)=Ω(g(n)) : c*(g(n)) is lower than f(n)
   - f(n) = Θ(g(n)): c*g(n) is upper than f(n) and lower than c2 * f(n)

##### **CTCI**(I have no idea) :star:

Union Write the code that returns an array that is the union between two given arrays. The union of two arrays is a list that includes everything that is in both arrays, with no duplicates. Assume the given arrays do not contain duplicates. For example, the union of {1, 2, 3, 4} and {3, 4, 5, 6} is {1, 2, 3, 4, 5, 6}. Hint: The method should run in O(M + N) time where M and N is the size of each array.

- SULOTION: use HashSet of ADTs 

#### Disjoint Sets

- path compression and Weighted Quick Union（启发式合并）
- **WQU's Maximum height: Log N**

##### Application

朋友圈， 团伙问题， 连通性问题（链接孤岛的最短路），[OIwiki](https://oi-wiki.org/topic/dsu-app/)

#### BST

- delete: 让被删除的右子树minNode或左子树maxNode代替被删除节点
- Iterator ：how to  implement 
  - 创建一个stack，存**root和root所有左节点**
  - 遍历，pop一个，将这个节点右节点及其所有左节点push进stack

#### B Tree

- set a limit on the number of elements in the single node(called **L**) min degree
- such as limit is 3.The trees are called B-trees or 2-3-4/2-3 trees
  	- for 2-3-4 trees
  	- After adding the node to the leaf node, if the new node has 4 nodes, than pop up the middle left node and re-arrange the children accordingly.
- 关于B树叶子节点个数问题
  - n个关键字的B树，查找失败的情况有n+1种，所以叶结点个数为n+1。

- B-Tree invariants
  - all leaves must be the same height from the root
  - no-leaves node with k items must be have exactly k+1 children (A non-leaf node with k children contains k−1 keys)
  - Every non-leaf node (except root) has at least ⌈m⁄2⌉ children.
- B树的高度范围![image-20211115125947399](https://gitee.com/dongramesez/typora-img/raw/master/img/202111270006895.png)
- B树各个节点关键码要求
  - 根节点至少含有一个关键码
  - 每个节点至多含m-1个关键码
  - 除了根节点，其他分支节点至少含⌈*m*/2⌉-1个节点
- B树与普通树不同，计算树高度需要把外部节点考虑进来（没有关键字的节点，国外的教材没有那么关注外部节点）

​		高度为3的5阶B-树,只多存放?个关键码,至少存放(?)个?

​		最多4 * (1+5+5 * 5) = 124 :最少 1+2 * 2 +2 * 3 * 2 =17![image-20211115130550034](https://gitee.com/dongramesez/typora-img/raw/master/img/202111270006908.png)



#### Rotating Trees

With rotations, we can actually completely  balance a tree.

```java
private Node rotateRight(Node h) {
    // assert (h != null) && isRed(h.left);
    Node x = h.left;
    h.left = x.right;
    x.right = h;
    return x;
}

// make a right-leaning link lean to the left
private Node rotateLeft(Node h) {
    // assert (h != null) && isRed(h.right);
    Node x = h.right;
    h.right = x.left;
    x.left = h;
    return x;
}
```

![image-20211115100347594](https://gitee.com/dongramesez/typora-img/raw/master/img/202111151003775.png)



#### Red-Black Trees

Build a BST that is structurally identical to a 2-3 tree .

A 2-3 tree with only 2-nodes is trivial,which is exactly same to BST. What do we do about 3-nodes?

- We choose arbitrarily to make the left element a child of the right one. This results in a **left-leaning** tree. 
- We use a glue link by making it red. Normal links are black.
- left-leaning red-black trees (LLRB)
- Properties of LLRB's
  - 1-1 correspondence with 2-3 trees.
  - No node has 2 red links.
  - There are no red right-links.
  - Every path from root to leaf has same number of black links (because 2-3 trees have same number of links to every leaf).
  - Height is no more than 2x height of corresponding 2-3 tree.
- every path from the root to a leaf has the **same number of black links**
- Root is black
- Leaves are black
- Red nodes have black children

Rules

- Right red link -> rotate left
- Two consecutive left links -> rotate right
- Red left and red right -> color flip
- ![image-20211115112212815](https://gitee.com/dongramesez/typora-img/raw/master/img/202111270006909.png)



#### Hashing

字符串哈希，给字母编码，例如小写字母，a-z编码1-26；`hash（"abc"）= 1*26^2+2*26+3*1`

##### HashTable

- Deal with Runtime
  - Dynamically growing our hashtable.
  - Improving our Hashcodes

- 拉链法（开散列表法）

  - 索引范围1-M，哈希表大小N，插入/查询期望为O(N/M)

- 闭散列法 （直接将记录存在散列表，冲突则按某种方式探查）

  - 线性探测法：，冲突则查询d+1，d+2，d+3----

    

### HW3

- A Simple Hashcode

  - equals: use the method like algorithm4 page 65
  - Hashcode (perfect version) : red* 53* 53+green *53+blue
- Complex hashcode's bug
  - if the hashcode exceeds 256e3, it will overflow to 0.
  - use add(i) rather than add(some_constant) will built-in hashCode() to pass.


### How to use ListIterator() method in Java

- 多种方法遍历 List

```java
public class CrunchifyIterateThroughList {
 
    public static void main(String[] argv) {
 
        // create list
        List<String> crunchifyList = new ArrayList<String>();
 
        // add 4 different values to list
        crunchifyList.add("Facebook");
        crunchifyList.add("Paypal");
        crunchifyList.add("Google");
        crunchifyList.add("Yahoo");
 
        // Other way to define list is - we will not use this list :)
        List<String> crunchifyListNew = Arrays.asList("Facebook", "Paypal", "Google", "Yahoo");
 
        // Simple For loop
        System.out.println("==============> 1. Simple For loop Example.");
        for (int i = 0; i < crunchifyList.size(); i++) {
            System.out.println(crunchifyList.get(i));
        }
 
        // New Enhanced For loop
        System.out.println("\n==============> 2. New Enhanced For loop Example..");
        for (String temp : crunchifyList) {
            System.out.println(temp);
        }
 
        // Iterator - Returns an iterator over the elements in this list in proper sequence.
        System.out.println("\n==============> 3. Iterator Example...");
        Iterator<String> crunchifyIterator = crunchifyList.iterator();
        while (crunchifyIterator.hasNext()) {
            System.out.println(crunchifyIterator.next());
        }
 
        // ListIterator - traverse a list of elements in either forward or backward order
        // An iterator for lists that allows the programmer to traverse the list in either direction, modify the list during iteration,
        // and obtain the iterator's current position in the list.
        System.out.println("\n==============> 4. ListIterator Example...");
        ListIterator<String> crunchifyListIterator = crunchifyList.listIterator();
        while (crunchifyListIterator.hasNext()) {
            System.out.println(crunchifyListIterator.next());
        }
 
        // while loop
        System.out.println("\n==============> 5. While Loop Example....");
        int i = 0;
        while (i < crunchifyList.size()) {
            System.out.println(crunchifyList.get(i));
            i++;
        }
 
        // Iterable.forEach() util: Returns a sequential Stream with this collection as its source
        System.out.println("\n==============> 6. Iterable.forEach() Example....");
        crunchifyList.forEach((temp) -> {
            System.out.println(temp);
        });
 
        // collection Stream.forEach() util: Returns a sequential Stream with this collection as its source
        System.out.println("\n==============> 7. Stream.forEach() Example....");
        crunchifyList.stream().forEach((crunchifyTemp) -> System.out.println(crunchifyTemp));
    }
}
```



#### Priority Queue

- interface / implementation
  - Ordered Array
    - `add`: Θ(*N*)
    - `getSmallest`:Θ(1)
    - `removeSmallest`: Θ(*N*)
  - Bushy BST
    - `add`: Θ(log*N*)
    - `getSmallest`: Θ(log*N*)
    - `removeSmallest`: Θ(log*N*)
  - HashTable
    - `add`: Θ(1)
    - `getSmallest`: Θ(*N*)
    - `removeSmallest`:Θ(*N*)

##### Application

LeetCode347（数组出现频率前k个）Leetcode239. Sliding Window Maximum

#### Heap （堆）

#### Tries(单词查找树)字典树/前缀树

- 优点：
  - 1，最坏情况时间复杂度比hash表好
  - 2，没有冲突，除非一个key对应多个值（除key外的其他信息）
  - 3，自带排序功能（类似Radix Sort），中序遍历trie可以得到排序。

- 应用：

  - **字符串检索**：事先将已知的一些字符串（字典）的有关信息保存到trie树里，查找另外一些未知字符串是否出现过或者出现频率。

    - 1，给出N 个单词组成的熟词表，以及一篇全用小写英文书写的文章，请你按最早出现的顺序写出所有不在熟词表中的生词。
    - 2，给出一个词典，其中的单词为不良单词。单词均为小写字母。再给出一段文本，文本的每一行也由小写字母构成。判断文本中是否含有任何不良单词。例如，若rob是不良单词，那么文本problem含有不良单词。
    - 3，1000万字符串，其中有些是重复的，需要把重复的全部去掉，保留没有重复的字符串。

  - **文本预测**，拼写检查

  - **词频统计**：

    - 1，有一个1G大小的一个文件，里面每一行是一个词，词的大小不超过16字节，内存限制大小是1M。返回频数最高的100个词。

    - 2，一个文本文件，大约有一万行，每行一个词，要求统计出其中最频繁出现的前10个词，请给出思想，给出时间复杂度分析。

    - 3，寻找热门查询：搜索引擎会通过日志文件把用户每次检索使用的所有检索串都记录下来，每个查询串的长度为1-255字节。假设目前有一千万个记录，这些查询串的重复度比较高，虽然总数是1千万，但是如果去除重复，不超过3百万个。一个查询串的重复度越高，说明查询它的用户越多，也就越热门。请你统计最热门的10个查询串，要求使用的内存不能超过1G。
      (1) 请描述你解决这个问题的思路；
      (2) 请给出主要的处理流程，算法，以及算法的复杂度。

      ==》若无内存限制：Trie + “k-大/小根堆”（k为要找到的数目）。

      否则，先hash分段再对每一个段用hash（另一个hash函数）统计词频，再要么利用归并排序的某些特性（如partial_sort），要么利用某使用外存的方法。参考

##### [[海量数据问题的处理-六种解决思路 ](https://www.cnblogs.com/GarrettWale/p/14478347.html)] :star:

### A*(A star)

- 为了优化类似平面图这样的找最短路径问题。使用启发式搜索。![image-20211129172818699](https://gitee.com/dongramesez/typora-img/raw/master/img/202111291728797.png)

- f(n) = g(n) + h(n)

  f(n)综合优先级，g(n)n到起点代价,h(n)n到终点的预计代价。

  - h(n) -> 0，退化成dijkstra算法
  - 如果h(n)的值比节点n到终点的代价要大，则A*算法不能保证找到最短路径，不过此时会很快
  - h（n）曼哈顿距离，对角距离，欧几里得距离
  - 用优先队列维护一个备用搜索路线很好

#### lab11

- A* 不知道怎么写
- 大佬思路：
  - 定义Node，将节点与f(n)绑定在一起
  - override `Comparator`
  - 使用**优先队列**维护栈

#### HW4-A*

- 应用A*，解决谜题

- 定义了interface WorldState，将所有的问题都转化成

  - 估计距离
  - 相邻情况
  - 是否到达终点

  使用**Solver**类使用类似Lab11的方法，解决WorldState类的问题。而word，board作为sub-class of WorldState，重写neighbors和estimatedDistanceToGoal。


##### Radix Sort

- LSD && MSD (the least significant digit && the most significant digit) Radix Sort
-  MSD is more efficient than LSD because it may not have examine the every digit of each integer.
- MSD can be used to sort strings of variable length, unlike LSD.
- **Time Complexity:** LSD is `O(N*M)` where M is length of longest string. MDS is `O(N)`
- **Auxiliary Space:** LSD `O(N+B)`,MSD `O(N + MB)`

### Proj3:star:

##### 1.Setup

- getting the skeleton files
- make the `src/main` directory as a sources root
- After opening the `localhost:4567/map.html`, I get a map page with no map.

##### 2.The  `getMapRaster` Method

- The hardest part of this project, which return query results of the given `params`![image-20211204205545752](https://gitee.com/dongramesez/typora-img/raw/master/img/202112042055852.png)

-  suppose params = *{ullon=-122.241632, lrlon=-122.24053, w=892.0, h=875.0, ullat=37.87655, lrlat=37.87548}*

  Means: longitude & latitude , and display 892 pixels wide and 875 pixels tall

- At this latitude (38 degree north), each degree of longitude is **S = 288,200 feet**.

  User want a box (122.241632 - 122.....) x S = 317 feet wide, while the screen space is 892 pixels wide. So, the image is roughly 317/892 = 0.355 feet per pixel.

- personal comprehension 
  - data structure: put all the image
  - With the detaX & detaY, I can calculate the feet/pixel
  - ![image-20211209105554771](https://gitee.com/dongramesez/typora-img/raw/master/img/202112091055917.png)
  -    choose the depth which make  resolution is perfect
    - Resolution = 49(approximate) / 2^(D-1)
