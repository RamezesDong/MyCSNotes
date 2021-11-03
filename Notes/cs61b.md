# cs61b



[toc]

---

[课程主页](https://sp18.datastructur.es/)

直接记一下笔记吧

#### w2's Discussion

- ![image-20211024231034681](cs61b/image-20211024231034681.png)

  定义两个猫，赋值不同的noisy，他们改变的是Cat下的同一个noisy，也就是说noisy被后操作的所决定

- 递归和非递归的方法求整数链表的平方

  **对链表理解还不够深，这个问题很经典**

- 全局变量和 local 变量![image-20211025094345645](cs61b/image-20211025094345645.png)

  swaperoo(f1,f2)则f1与f2都变为f2。

  注意传递的变量是一个值还是一个地址（java没有用指针)

- ![image-20211025095842001](cs61b/image-20211025095842001.png)

  multiplyBy3 是去A 中各元素的值，变量名叫 x ，改变的实际上是变量 x。

#### lab2

dcatenate() and catenate() 加一句判断A是否为null ，解决 test null arguments 的问题

#### proj0

花了整整一天时间，上午在配置IntelliJ IDEA 相关的环境；下午开始写代码，测试文件一直无法单独执行，后来又查了好久。到晚上九点，终于写完代码，一运行发现行星都飞走了。**物理函数写错了**！！！忘记dx是矢量，我求了绝对值。

运行出动画，成就感满满呀！实际上这次难度并不算太大，而且老师的doc真的太详细了。感动 :joy:

![image-20211025214422015](cs61b/image-20211025214422015.png)

#### Dis3

- reverse SLList (a very significant problem which had been done in leetecode )

  ![image-20211027000414432](cs61b/image-20211027000414432.png)

- Must to know: **special cases** 
- ![image-20211027000527683](cs61b/image-20211027000527683.png)

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

  ![image-20211028102912325](cs61b/image-20211028102912325.png)

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

![image-20211031110015241](cs61b/image-20211031110015241.png)



![image-20211031110027995](cs61b/image-20211031110027995.png)



![image-20211031110153218](cs61b/image-20211031110153218.png)

![image-20211031110212566](cs61b/image-20211031110212566.png)

- The problem Creating Cats uses the super syntax for calling superclass' constructor
- 动态方法选择此题，用了末尾节点来确保max函数不会出错，用了继承和override(重写)



  



















