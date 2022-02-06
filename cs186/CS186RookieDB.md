## CS186RookieDB

### Code Overview

#### cli

cli 文件夹包含了处理数据库命令行接口的文件。运行`CommandLineInterface`将要创建一个数据库实例，可以支持简单的查询操作。此部分代码不做课程要求，当然可以借鉴sql解释器写法。

##### cli/parser

此子目录包含巨量代码。但别怕，这都是`RookieParser.jjt`自动生成的。这部分代码将用户输入查询转换为语法分析树形式。

##### cli/visitor

此子目录包含遍历语法分析树和创建数据库对象的类。

##### common

common文件夹包含一些有用代码和通用接口不限于代码的某一部分。

##### concurrency

并发文件夹包含一个数据库多粒度锁的框架代码，你将在project4完成它。

#### databox

像大多数DSMS一样，我们的数据库也有一个类型系统。

`databox`文件夹包含表示存储在数据库中值（以及类型）的类，各种`databox`类表示某种类的值，而`Type`表示数据库的类型。例如：

```java
DataBox x = new IntDataBox(42); // The integer value '42'.
Type t = Type.intType();        // The type 'int'.
Type xsType = x.type();         // Get x's type, which is Type.intType().
int y = x.getInt();             // Get x's value: 42.
String s = x.getString();       // An exception is thrown, since x is not a string.
```

##### index

索引包含B+树实现的索引框架，将在project2实现。

##### memory



