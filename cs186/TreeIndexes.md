# Tree Indexes

## Review

- Data structure in RAM: Search trees (Binary, AVL, Red-Black,..) Hash tables
- Needed: disk-based data structures: “paginated”: made up of disk pages!

## Index

![image-20220201173209245](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220201173209245.png)

![image-20220201173459601](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220201173459601.png)

![image-20220201173555650](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220201173555650.png)

ISAM Indexed Sequential 
Access Method (Early IBM Indexing Technology)

## B+ TREE

B+ Tree:

- Dynamic Tree Index
- “+”?B-tree that stores data entries in leaves only

![image-20220201183407262](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220201183407262.png)

### B+ Tree Properties

A B+Tree is an M-way search tree with the following properties:

- Every leaf node is at the same depth in tree
- Every node other than the root, is at least half-full `M/2-1 <= #KEY <=M-1`
- Every inner node with `k`  keys has `k+1`non-null children

![image-20220206093954833](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220206093954833.png)

## Clustered indexes

1. heap table: not in ordered .
2. clustered indexes: The table is stored in the sort order specified by the primary key (Can be either heap- or index- organized storage)
3. Retrieving tuples in the order that appear in an unclustered index is inefficient.

#### Node size

The slower the storage device, the larger the optimal node size for a B+ Tree.

- HDD 1MB
- SSD 10KB
- In-Memory: 512B

### Variable length keys

Approach:

1. Pointers
2. Variable Length Nodes
3. Padding
4. Key Map/Indirection

### Optimizations

1. Prefix Compression

   ![image-20220206110050882](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220206110050882.png)

2. Deduplication

   ![image-20220206110108155](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220206110108155.png)

3. Suffix Truncation

   ![image-20220206110222938](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220206110222938.png)

   ![image-20220206110229813](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220206110229813.png)

4. Bulk Insert

   ![image-20220206110310210](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220206110310210.png)

5. Pointer Swizzling

   ![image-20220206110907020](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220206110907020.png)

   

## Additional Index Magic

1. partial indexes
2. covering indexes
3. index include columns
4. functional/expression indexes

### Trie index

### inverted index

### query types

1. Phrase Searches→ Find records that contain a list of words in the given 
   order.
2. Proximity Searches→ Find records where two words occur within n words of 
   each other.
3. Wildcard Searches→ Find records that contain words that match some pattern 
   (e.g., regular expression).