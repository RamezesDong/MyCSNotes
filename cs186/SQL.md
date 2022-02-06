# CS186 SQL

[toc]

### SQL Pros and Cons

- Declarative
  - Say what you want, not how to get it
- Implemented widely
  - With varing levels of efficiency, completeness
- Constrained
  - Not targeted at Turing-complete tasks
- General-purpose and feature-rich
  - many years of added features
  - extensible: callouts to other languages, data sources

### Relational Terminology

- Database: Set of named Relations
- Relation (Table): 
  - Schema: description(“metadata”)
  - Instance: set of data satisfying the schema

- Attribute (Column, Field)<img src="https://gitee.com/dongramesez/typora-img/raw/master/img/202201221028246.png" alt="image-20220122102830158" style="zoom:50%;" />
- Tuple (Record, Row)<img src="https://gitee.com/dongramesez/typora-img/raw/master/img/202201221028273.png" alt="image-20220122102841227" style="zoom:50%;" />

### Relational Tables

- Schema is fixed:
  - 在MySQL中Schema与database是等价的。
  - Schema是数据库对象的集合，包含表、视图、存储过程、索引等。将database比做仓库，Schema就像一个房间。
- Instance can change often
  - a multiset of “row”(“tuples”)

### SQL Language

- Two sublanguages:

  - DDL: Data Definition Language
  - DML: Data Manipulation Language
    -  Queries can be written intuitively.

- RDBMS responsible for efficient evaluation

  - Choose and run algorithms for declarative queries

  - Choice of algorithm must not affect query answer.

- SQL DDL:
  - <img src="https://gitee.com/dongramesez/typora-img/raw/master/img/202201221225367.png" alt="image-20220122122541303" style="zoom: 50%;" />
- SQL DML:
  - <img src="https://gitee.com/dongramesez/typora-img/raw/master/img/202201221226376.png" alt="image-20220122122624329" style="zoom: 50%;" />
  - 

### Basic Single-Table Queries

- [CS-Notes/SQL 语法.md](https://github.com/CyC2018/CS-Notes/blob/master/notes/SQL%20%E8%AF%AD%E6%B3%95.md#%E4%B8%83%E6%9F%A5%E8%AF%A2)
- <img src="https://gitee.com/dongramesez/typora-img/raw/master/img/202201221329010.png" alt="image-20220122132909952" style="zoom:50%;" />
- <img src="https://gitee.com/dongramesez/typora-img/raw/master/img/202201221329520.png" alt="image-20220122132953455" style="zoom:50%;" />
- <img src="https://gitee.com/dongramesez/typora-img/raw/master/img/202201221340143.png" alt="image-20220122134027075" style="zoom:50%;" />
  - INTERSECT: 交集
  - UNION: 并集
  - EXCEPT: 差集
  - <img src="https://gitee.com/dongramesez/typora-img/raw/master/img/202201221342746.png" alt="image-20220122134241708" style="zoom:50%;" />
  - ALL
  - <img src="https://gitee.com/dongramesez/typora-img/raw/master/img/202201221343124.png" alt="image-20220122134322072" style="zoom:50%;" />
  - <img src="https://gitee.com/dongramesez/typora-img/raw/master/img/202201221343867.png" alt="image-20220122134340806" style="zoom:50%;" />
- Nested Queries: IN / NOT IN
- <img src="https://gitee.com/dongramesez/typora-img/raw/master/img/202201221348202.png" alt="image-20220122134808162" style="zoom:50%;" />
- A Tough One: “Division”:
  - <img src="https://gitee.com/dongramesez/typora-img/raw/master/img/202201221350857.png" alt="image-20220122135003801" style="zoom:50%;" />
- “Inner” Joins: Another Syntax
  - <img src="https://gitee.com/dongramesez/typora-img/raw/master/img/202201221505534.png" alt="image-20220122150516490" style="zoom:50%;" />
- Inner/Natural Joins
  - <img src="https://gitee.com/dongramesez/typora-img/raw/master/img/202201221510211.png" alt="image-20220122151011166" style="zoom:50%;" />
- Outer Join: (Left Right Full Outer)
  - 保留没有关联的行，左连接保留左表。。。

- Views: Named Queries
  - Makes development simpler
  - Often used for security
  -  Not “materialized”
- Subqueries in FROM 
- WITH a.k.a. common table expression
- NULL
  - IS NULL
  - <img src="https://gitee.com/dongramesez/typora-img/raw/master/img/202201221557177.png" alt="image-20220122155728131" style="zoom:50%;" />
  - NULL op NULL is NULL
  - WHERE NULL: do not send to output
  - Boolean connectives: 3-valued logic
  - Aggregates ignore NULL-valued inputs
- <img src="https://gitee.com/dongramesez/typora-img/raw/master/img/202201221600763.png" alt="image-20220122160001701" style="zoom:50%;" />



