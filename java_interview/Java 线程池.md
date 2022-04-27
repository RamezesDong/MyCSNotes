# Java 线程池

### 1. 线程池的优点

使用好处：

- 降低资源的消耗。线程本身是一种资源，创建和销毁线程会有CPU开销；创建的线程也会占用一定的内存。
- 提高任务执行的响应速度。任务执行时，可以不必等到线程创建完之后再执行。
- 提高线程的可管理性。线程不能无限制地创建，需要进行统一的分配、调优和监控。

不用线程池的坏处：

- 频繁的线程创建和销毁会占用更多的CPU和内存
- ---------会对GC造成较大压力
- 线程太多，线程切换带来的开销不可忽视
- 线程太少，多核CPU得不到充分利用，是一种浪费

### 2. 线程池实现原理

![image-20220315231352214](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220315231352214.png)

1. `corePool` -> 核心线程池
2. `maximumPool` -> 线程池
3. `BlockQueue` -> 队列
4. `RejectedExecutionHandler` -> 拒绝策略

### 3. Executors 线程池工厂

``` java
// 创建单一线程的线程池
public static ExecutorService newSingleThreadExecutor();
// 创建固定数量的线程池
public static ExecutorService newFixedThreadPool(int nThreads);
// 创建带缓存的线程池
public static ExecutorService newCachedThreadPool();
// 创建定时调度的线程池
public static ScheduledExecutorService newScheduledThreadPool(int corePoolSize);
// 创建流式（fork-join）线程池
public static ExecutorService newWorkStealingPool();
```

`Executors` 实现使用了默认的线程工厂`DefaultThreadFactory`

### 3.提交任务的几种方式

- `execute()`不需要返回结果
- `submit()` 需要返回结果，调用`get()`方法，可以获得结果，会一直阻塞。我们可以重载方法加入等待时间



## 4. 关闭线程池

1. `shutdown()`会将线程池状态置为`SHUTDOWN`，不再接受新的任务，同时会等待线程池中已有的任务执行完成再结结束。尝试关闭没有执行的线程

2. `shutdownNow()`会将线程池状态置为`SHUTDOWN`，对所有线程执行`interrupt()`操作，清空队列，并将队列中的任务返回回来。

   

