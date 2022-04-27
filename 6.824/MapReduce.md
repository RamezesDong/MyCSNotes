# MapReduce

[MapReduce: Simplified Data Processing on Large Clusters](https://pdos.csail.mit.edu/6.824/papers/mapreduce.pdf)

### 1 Introduction

1. 遇到问题：
   - 谷歌系统需要处理大量数据，许多运算是重复的，但是数据量太大
   - 复杂性: 如何隐藏 并行处理的许多细节，容错，数据分布式存储以及库的均衡负载
2. 抽象：灵感来自Lisp语言的`map` & `reduce`
   - `map`将逻辑 “record” 转化为中间 key/value 对
   - `reduce`对同一个`key`的所有value进行运算
   - 简单且强大的接口

## 2 Programming Model

1. `map` `reduce` 都是用户编写的

2. Example

   ```c#
   map(String key, String value):
   // key: document name
   // value: document contents
   for each word w in value:
   EmitIntermediate(w, "1");
   reduce(String key, Iterator values):
   // key: a word
   // values: a list of counts
   int result = 0;
   for each v in values:
   result += ParseInt(v);
   Emit(AsString(result));
   ```

3. Types

   ```c++
   map (k1,v1) → list(k2,v2)
   reduce (k2,list(v2)) → list(v2
   ```

   

