# cs61c

[toc]

## FOREWORD

In the chat group of cs61abc, the teaching assistant  came from THU says that she/he is learning cs61abc together. Don’t be a giant in words, a short in action.  Let’s set sail for the new continent immediately.

Written in 2021/12/13



## W1/W2

### Intro

- ![image-20211213161207392](https://gitee.com/dongramesez/typora-img/raw/master/img/202112131612578.png)
- Great Idea![image-20211216104054462](https://gitee.com/dongramesez/typora-img/raw/master/img/202112161040557.png)
- After taking this class students should be able to:
  -  Identify and explain the various layers of abstraction that allow computer users to perform complex software tasks without understanding what the computer hardware is actually doing
  - Judge the effect of changing computer components (e.g. processor, RAM, HDD, cache) on the performance of a computer program
  - Explain how the memory hierarchy creates the illusion of being almost as fast as the fastest type of memory and almost as large as the biggest memory
  - Construct a working CPU from logic gates for a specified instruction set architecture
  - Identify the different types of parallelism and predict their effects on different types of applications
- Six Great Ideas in Computer Architecture
  1. Abstraction
  2. Technology Trends
  3. Principle of Locality/Memory Hierarchy
  4. Parallelism
  5. Performance Measurement & Improvement
  6. Dependability via Redundancy

### Number Representation

1. Analog signal Digital signal
2. Commonly used number bases
   - Decimal
   - Binary
   - Hexadecimal
3. Only one constraint: n digits (Based B) = B^n^ things
3. N bit Bias notation number: for example 8bits with a bias of -127.
4. Sign and magnitude(原码) One’s Complement(反码) Two’s complement(补码)
5. ![image-20211213165336462](https://gitee.com/dongramesez/typora-img/raw/master/img/202112131653503.png)
6. Sign Extension
   1. easy for positive, more complicated for negative 
   2. One’s/Two’s complement: copy MSB
7. Overflow

### C: Introduction, Pointers

1. Basic C Concepts

   1. Compilation
      - C Compiler map C program into architecture-specific machine code (string of 0s and 1s)
      - Compilation Advantages
        - Excellent run-time performance, because it optimizes for the given architecture
        - Fair compilation time: enhancements in compilation procedure (Makefiles) allow us to recompile only the modified files.
      - Compilation Disadvantages
        - Compiled files, including the executable, are architecture-specific (CPU type and OS)
          –Executable must be rebuilt on each new system
          –i.e. “porting your code” to a new architecture
        - “Edit→ Compile → Run [repeat]” iteration cycle can be slow

   2. C Pre-Processor(CPP)

      - ![image-20211216104811979](https://gitee.com/dongramesez/typora-img/raw/master/img/202112161048013.png)

      - macro Function bugs

        - ```c
          #define twox(x) (x+x)
          twox(3); //3+3
          twox(y++);//(y++ + y++)
          ```

   3. Variable Types
      1. Typecast
      2. struct in c
         - Structs Alignment and Padding in C: From CSAPP and this lecture, we know that c provide enough space and aligns the data with padding!

2. C Syntax and Control Flow

   - C Syntax: main
     - `argc` argument count
     - `argv` argument value
   - C Syntax: Control Flow
     - `while for switch break`
     - C has `goto`

3. Pointers

   1. Address vs. Values

      A pointer is a variable that contains an address

      ![image-20211213173640246](https://gitee.com/dongramesez/typora-img/raw/master/img/202112131736289.png)

      Pointers are used to point to one kind of data (int, char, a struct, etc.) pointer to a pointer (int **pp)
      •Exception is the type void *, which can point to anything (generic pointer)



### C Array, pointers, Strings

1. `sizeof()`returns number of bytes in object
2. `union` and `struct` : union provides enough space for the largest element.

### C Memory management, Usage 

1. Where are variables Allocated?
   - Static variables: Static data
   - Local variables: Stack
   - Global variables
   - Constants: Code, static `#define y 5`, or stack
   - Machine Instructions: Code
   - Result of malloc : Heap
   - String Literals: Static `s = "hello"`

- ![image-20211216171727484](https://gitee.com/dongramesez/typora-img/raw/master/img/202112161717569.png)
- If declared outside a function, allocated in “static” storage. If declared inside function, allocated on the “**stack**” and freed when function returns.
- `main` is treated like a function

2. Managing the Heap

-  **malloc**()  allocate a block of uninitialized memory
-  **calloc**()  allocate a block of zeroed memory 
-  **free**()   free previously allocated block of memory 
-  **realloc**()   change size of previously allocated block 
  - careful – it might move! 
  - And it will not update other pointers pointing to the same block of memory
    53

3. Three ways to find free space when given a request:
   1. First fit
   2. Next fit
   3. Best fit: finds most “snug” free space
4.   don’t double `free`
5. Heap data is biggest source of bugs in C code

### What is more for C?

1. Storage Classes
   - just like public in java, there are four of them:
   - `extern static auto register`
   - `auto` is default for local and not permitted to be used for global. `register` is never seen in practice, in new C code, because modern compilers are much better than human beings at deciding which local variables to put in processor registers for optimal perfomance.
   - `extern int x;/*declaration,explicityly not a defination*/` 
   - [What is the difference between static and extern in C? - Stack Overflow](https://stackoverflow.com/questions/3684450/what-is-the-difference-between-static-and-extern-in-c/49093474)

### disc01 

1. When does two tow’s complement numbers calculate to a result with overflow?

   0b100011+0b111010 = 29 overflow !

   0x3B +0x06 = 1 not an overflow 尽管有进位但值没有改变，overflow**只有**发生在同符号运算后符号改变

### disc2

[非常好的面试题，:laughing:](https://cs61c.org/fa21/pdfs/discussions/disc02-sols.pdf)

- `gets()` 

## Proj1-Philphix

1. 环境配置：
   - 使用wsl (windows subsystem for linux)
   - 配置单元测试环境CUnit库
   - 在google查了很多资料查解决了环境配置问题

2. Task1/2

   - C language is hard to write for me. When I wrote the code for the first time, I encountered a memory leak problem which is called `segment fault` . Because I am unfamiliar to struct and pointers.

   - `insertData`: Don’t use iterator to find the last item in buckets. Just put the new Data in the first and put old item at the `next`.

     ```c
     void insertData(HashTable *table, void *key, void *data) {
       unsigned int location = table->hashFunction(key) % table->size;
       HashBucketEntry* where = (HashBucketEntry*) malloc(sizeof(HashBucketEntry));
       where->next = table->buckets[location];
       where->key = key;
       where->data = data;
       table->buckets[location] = where;
       // HINT:
       // 1. Find the right hash bucket location with table->hashFunction.
       // 2. Allocate a new hash bucket entry struct.
       // 3. Append to the linked list or create it if it does not yet exist. 
     }
     ```


	3. Task3: using the function in `ctype.h`
	- `int isalnum(int c)` : checks whether the passed character is alphanumeric.
	- `int isalpha(int c)`
	- `int isprint(inc c)` checks whether the passed character is printable, for choosing the character without space and tab.
	4. Task4:
	- Just like task 3, input the word by one and one character with `getchar()`. The word malloc 60 bytes firstly, and increase double.
	5. Let me make a summary of this project. I found my shortcomings in programming in C, because it is common for me to type wrong code with segment fault bugs. In addition, gdb is also a huge weakness for me.

### Lab1

1. [GDB reference card](http://inst.eecs.berkeley.edu/~cs61c/resources/gdb5-refcard.pdf)

2. segmentation fault: when you try to access a piece of memory that “does not belong to you”.

   - Accessing an array out of bounds.

   - Derefrencing a null pointer.

   - Accessing a pionter that has been free’d.

   - Attempting to write to read-only memory.

   - ```c
     char *my_str = "hello"; it is immutable
     char my_str[] = "hello";it is mutabble
     ```

   - The first string is stored in the **data portion** of memory which is read-only while the second string is stored on the **stack**.
