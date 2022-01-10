# cs61c

[toc]

## FOREWORD

In the chat group of cs61abc, the teaching assistant  came from THU says that she/he is learning cs61abc together. Don’t be a giant in words, a short in action.  Let’s set sail for the new continent immediately.

Written in 2021/12/13



### Useful Docs

[RISC-V CARD](https://github.com/jameslzhu/riscv-card/blob/master/riscv-card.pdf)



## W1/W2/W3 C

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

### disc02

[非常好的面试题，:laughing:](https://cs61c.org/fa21/pdfs/discussions/disc02-sols.pdf)

- `gets()` 

### disc03

- The distance between floating point numbers increase as the absolute value of the numbers increase.

  True. If the exponent increase, the difference between two neighboring floats will increase.

- Floating Points addition is associative.

  False. Because of rounding errors, you can find Big and Small numbers such that:
  (Small + Big) + Big != Small + (Big + Big)

### Proj1-Philphix:star:

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

### Lab2

1. [valgrind](https://valgrind.org/docs/manual/mc-manual.html)
2. [make](https://www.gnu.org/software/make/manual/make.html#Reading)

## W4/W5/W6 RISC-V

### RISC-V Intro

1. Instruction Set Architecture (ISA): Job of  a CPU is execute instructions 

   **Instructions**: CPU’s primitive operations 
   •Instructions performed one after another in sequence 
   •Each instruction does a small amount of work (a tiny part of a larger program). 
   •Each instruction has an operation applied to operands, 
   •  and might be used to change the sequence of instructions

2. Complex Instruction Set Computer vs. Reduced Instruction Set Computer

   1. x86/x64(servers) and Arm(phones/embedded)

3. Assembly language programming :Each assembly language is tied to a particular ISA (its just a human readable version of machine language).

   For us ... learn to program in assembly language 
   • Best way to understand what compilers do to generate machine code 
   • Best way to understand what the CPU hardware does

4. Assembly Variables: Registers![image-20211224144207148](https://gitee.com/dongramesez/typora-img/raw/master/img/202112241442243.png)

5. Speed of Registers vs. Memory
   - Registers: 32 words(128Bytes)
   - Memory(DRAM): Billions of bytes 2GB to 16GB on laptop
   - registers is faster than DRAM 100-500 times.

6. 32 registers in RISC-V, referred to by number x0 – x31
   - Each RISC-V register is 32 bits wide (RV32 variant of RISC-V ISA) 
   - Groups of 32 bits called a word in RISC-V ISA 
   - P&H CoD textbook uses the 64-bit variant RV64 (explain differences later)
   - x0 is special, always hold zero and can’t be changed 

7. Registers have on type and operations determines how register contents are interpreted.

8. RISC-V Instructions are fixed 32b long.

9. RISC-V Addition and so on...
   - `add x1, x2, x3` x1 = x2+x3
   - `sub x3,x4,x4` x3 = x4 +x5
   - Immediates 
     - `addi x3,x4,10` x3 = x4 +10
   - Loading and Storing Bytes
     - load byte: `lb` word `lw`
     - store byte: `sb` word `sw`
     - unsigned byte loads `lbu`
   - **Remember, RISC-V is “little endian”**
   - `lb x10,3(x11)` contents of memory location with address  = sum of “3” +contents of register x11 is copied to the low byte position of register x10.
   - :star:In RISC-V immediates are sign extended, for example![image-20211224153329933](https://gitee.com/dongramesez/typora-img/raw/master/img/202112241533993.png) If we did `lbu` we’d instead get `0xf8`
   - RISC-V Logical Instructions
     - `and or xor sll srl/sra`
     - `slli x11,x12,2` x11 = x12 <<2 
     - Shift Right Logical `srli`
     - Arithmetic Shifting `srai`
   - Decision Making /Control Flow Instructions
     - `beq register1,register2,L1` go to instruction labeled L1 if reg1 == reg2
     - `bne` for branch if not equal
     - `j`  jump
     - `blt reg1,reg2, label`  <
     - `bltu` < and treat registers as unsigned integers
     - `bgt` > `bge` >=
     - `jal rd immd` use immediate(20bits) encoding for destination address and can jump +- 1MB range. And actual `address + 4` in register `rd`
     - `jalr rd immd(x1)` use indirect address (x1 in example) plus a constant of 12 bits. It save the actual `address + 4` in register `rd`
   
10. Register Conventions
    - Symbolic register names :a0-a7 for argument registers (x10-x17);zero for x0
    
    - The RISC-V Registers and Convention
    
    - The "Application Binary Interface" defines our 'calling  convention'
    
    - ![image-20211224163237469](https://gitee.com/dongramesez/typora-img/raw/master/img/202112241632532.png)Yes in the column referred to as **callee-saved** and those with No as **caller-saved**
    
      函数调用中的其他寄存器，要么被当做保存寄存器前后值不变，要么当临时寄存器，在函数调用中不保留。YES---callee-saved 表示被保留的
    
    - ![image-20211224164435595](https://gitee.com/dongramesez/typora-img/raw/master/img/202112241644646.png)
    
11. RV32 Memory Allocation![image-20211224170102093](https://gitee.com/dongramesez/typora-img/raw/master/img/202112241701175.png)
    - static data segment (constants and other static variables) above text for static variables 
      •RISC-V convention global pointer (gp) points to static 
      •RV32 gp = 1000_0000hex

### How to Implement Function in RISC-V

1. Six Fundamental Steps in Calling a Function
   1. Put parameters in a place where function can access them 
   2. Transfer control to function 
   3.  Acquire (local) storage resources needed for function 
   4.  Perform desired tas of the function 
   5.  Put result value in a place where calling code can access it and maybe restore any registers you used
   6.  Return control to point of origin.  
      (Note: a function can be called from several points in a program, including from  itself.)
2. **Caller**: a procedure that calls one or more more subsequent procedure(s).
3. **Callee**: a procedure that is called by another.
4. **Application Binary Interface (ABI)**: a standard for register usage and memory layout that allows for programs that are not compiled together to interact effectively.
5. **Calling Conventions**: a subset of an ABI specifically focused on how data is passed from one procedure to another.
6. Use `jar` return address is saved in register `ra`

### RISC-V Instruction Formats

1. Why?

   - Consequence #1: Everything has a memory address
     - instructions, data words, have a memory address
     - PC: program counter is basically a pointer to memory which be called Instruction Pointer by Intel
   - #2: Binary Compatibility
     - New machines in the same family want to run old programs  (“binaries”) as well as programs compiled to new instructions

2.  Instruction as Numbers

   - Divide 32-bit instruction word into “field”
   - Each field tells processor something about instruction 
   - For hardware simplicity, group possible instructions into six basic types of instruction formats:

     - R-format for register-register arithmetic/logical operations
     - I-format for register-immediate ALU operations and loads
     - S-format for stores
     - B-format for branches
     - U-format for 20-bit upper immediate instructions 
     - J-format for jumps
     - ![image-20211224230500138](https://gitee.com/dongramesez/typora-img/raw/master/img/202112242305238.png)
   - R-Format Instructions register specifiers
   
     - rs1(Source Register 1) rs2 rd(Destination Register)
     - ![image-20211225000750312](https://gitee.com/dongramesez/typora-img/raw/master/img/202112250007424.png)
   - I-format instruction Layout
   
     - Immediate is always sign-extended to 32-bits before use in an arithmetic/logic operation
     - ![image-20211225001453196](https://gitee.com/dongramesez/typora-img/raw/master/img/202112250014303.png)
   
     - **Load Instructions are also I-Type**
     - ![image-20211225001603531](https://gitee.com/dongramesez/typora-img/raw/master/img/202112250016613.png)
   - S-Format Used for Stores

     - Store needs to read two registers,`rs1` is base memory address and `rs2` for data to be stored, as well as need immediate offset!
     - ![image-20211225002259952](https://gitee.com/dongramesez/typora-img/raw/master/img/202112250023015.png)
   - B - Format
     - Branches typically used for loops (if-else, while, for ). Loops are generally small (<50 instructions)
     - `BEQ x1, x2, Label`
       - If we don’t take the branch:  PC = PC + 4 (i.e., next instruction)
       - If we do take the branch: PC = PC + immediate
     - 12-bits immediate , could specify +- 2^11 byte address offset from the PC. However, extensions to RISC-V base ISA support 61-bit compressed instructions and also variable-length instructions that are multiples of 2-Bytes in length.
     - **RISC-V scales the branch immediate by 2 bits--- so RISC-V conditional branches can only reach +-2^10 *32 -bit(+-2^10 for 4-byte instructions)**
     - ![image-20211225101922323](https://gitee.com/dongramesez/typora-img/raw/master/img/202112251019433.png)
   - U-Format for “Upper Immediate” instructions
     - ![image-20211225102403742](https://gitee.com/dongramesez/typora-img/raw/master/img/202112251024787.png)
     - LUI to create long immediates
     - How to set 0xDEADBEEF![image-20211225102823359](https://gitee.com/dongramesez/typora-img/raw/master/img/202112251028403.png)
   - J- Format for Jump Instructions
     - ![image-20211225103430920](https://gitee.com/dongramesez/typora-img/raw/master/img/202112251034991.png)
     - ![image-20211225103529434](https://gitee.com/dongramesez/typora-img/raw/master/img/202112251035487.png)

### CALL (Compiler/ Assembler/ Linker/ Loader)

1.Interpretation vs. Compilation

- An Interpreter is a program that executes other programs![image-20211226165657564](https://gitee.com/dongramesez/typora-img/raw/master/img/202112261656628.png)
- Language translation gives us another option
- In general, we interpret a high-level language when efficiency is not critical and translate to a lower-level language to increase performance.
- How do we run a program written in a source langugae?
  - Interpreter: Directly executes a program in the source language
  - Translator: Converts a program from the source language to an equivalent program in another language.
- Translator reaction: add extra information to help debugging (line  numbers, names): 
  This is what gcc -g does, it tells the compiler to add all the debugging information
- Interpreter closer to high-level, so can give better error messages. slower(10x? ), code smaller(2x or not?) And it provides instruction set independence: run any machine
- complied/ translated code almost always more efficient and therefore higher performance. -- Using for operating system
- Compiled  code does the hard work once: during compilation.
  - Which is why most “interpreters” these days are really “just in time compilers”: 
    don’t throw away the work processing the program when you reexecute a function
  - Especially of web browsers

2. CALL chain

   - ![image-20211226210804780](https://gitee.com/dongramesez/typora-img/raw/master/img/202112262108868.png)
   - High-level language code transform to assembly language code (foo.s for RISC-V)
     - Note: output may contain **pseudo-instructions** (assembler understand but not in machine)
     - Steps in the complier![image-20211226211202661](https://gitee.com/dongramesez/typora-img/raw/master/img/202112262112719.png)
   - Assembler: A dumb complier for assembly language 
     - foo.s -> foo.o (object code, information tables)
     - Read and use Directives
     - replace pseudo-instructions
     - Produce **Machine Language** rather than just **Assembly Language**
     - Assembler Directives![image-20211226211559314](https://gitee.com/dongramesez/typora-img/raw/master/img/202112262115375.png)
     - `tail offset` ![image-20211226212113975](https://gitee.com/dongramesez/typora-img/raw/master/img/202112262121029.png)

3. Producing Machine Language

   - What about branches? Forward Reference problem

   - Solved by taking 2 passes over the program

     - First pass remembers position of labels (Can do this when we expand pseudo instructions )
     - Second pass uses label positions to generate code

   - What about jumps(`j jal`)

     - Jumps within a file are PC relative (and we can easily compute) 
     - Jumps to other files we can’t

   - What about reference to static data?

     - `la`

   - The two questions can’t be determined yet, so we can create two tables

     - **Symbol Table**: may be used by other file

        Lables (function calling) Data: anything in .data section

     - **Relocation Table**: List of  “items” files need the address of later

       the `jal` labels and any piece of data in static section(`la`)

   - Object File Format![image-20211226215105763](https://gitee.com/dongramesez/typora-img/raw/master/img/202112262151821.png)

4. Linker!!

   - Input: object code files with information tables
   - output: Executable code (a.out)
   - Enable separate compilation of files
   - ![image-20211226215643145](https://gitee.com/dongramesez/typora-img/raw/master/img/202112262156199.png)
   - After taking each .0 file text and data segment, Resolve references
     - Go through Relocation Table; handle each entry 
     - That is, fill in all absolute addresses
   - Three Types of addresses
     - PC- relative (**Never relocate**)
     - extern function reference (only external jumps)
     - static data reference (auipc addi)
   - Resolving References![image-20211226220229148](https://gitee.com/dongramesez/typora-img/raw/master/img/202112262202198.png)

5. Loader

   - In reality, loader is the operating system (OS)

6. Static vs. Dynamically Linked Libraries

### disc04

1. `slti rd, rs1, imm` place the value 1 in register `rd` if register `rs1` less than the sign extended immediate number, else 0 is written to `rd` 
2.  notice that using `addi`to add immediate rather than `add`. 
3. When do we save the registers maybe overriding? To write a **prologu**e and
   **epilogue** to account for the registers you used, when you call the callee-function.

### lab3

- [RISCV-CARD](https://inst.eecs.berkeley.edu/~cs61c/resources/riscvcard.pdf)

- venus is  an interesting web-service for risc-v assembly language.

- `la` pseudo-instructions

  `la rd, symbol` means that `auipc rd, symbol[31:12]; addi rd, rd, symbol[11:0]` For loading address. 

- difference from the `jal` and `jalr`
  - `jal` use immediate (20bits) encoding for destination address
  - `jalr` use indirect address `x1` plus 12bits constant `jalr x0,0(x1)`

- linked-list To be noticed the `jalr` and the contrast between address and value.

  The a0 (a linked-list node is address) but the a1(function) is the label bias which exactly is a value. 

### Proj2-Classify

1. [A list of standard RISC-V pseudo instructions](https://github.com/riscv-non-isa/riscv-asm-manual/blob/master/riscv-asm.md#a-listing-of-standard-risc-v-pseudoinstructions)
2. `li rd, immediate` (Based instructions are Myriad sequences ) Load immediate
3. Need to learn use the pseudo instructions, such as `mv li j `
4. Using a skill to write the exit function. 
4. The `classify.s` is not completed, whose bugs bother me.

### Lab4

The exercise 4 is not finished. I am too undisciplined to write my own tests.

### Dis05

1. [von Neumann architecture - Wikipedia](https://en.wikipedia.org/wiki/Von_Neumann_architecture)
   - A processing unit that contains an arithmetic logic unit and processor registers
   - A control unit that contains an instruction register and program counter
   - Memory that stores data and instructions
   - External mass storage
   - Input and output mechanisms
   
2. Stored-program computer is a computer that stores program instructions in electronically or optically accessible memory. This contrasts with systems that stored the program instructions with [plugboards](https://en.wikipedia.org/wiki/Plugboard) or similar mechanisms. 

3. ![image-20220103165218008](https://gitee.com/dongramesez/typora-img/raw/master/img/202201031652068.png)

4. What is the maximum range of 32-bit instructions that can be reached from the current PC using a jump instruction?

   This question is like that what is the range of 32-bit instructions ......from the current PC using a **branch** instruction? The difference between them is the immediate bits numbers.

## W7/W8/ Digital Systems

### Into to Digital Circuits or Synchronous Digital Systems

1. Synchronous

   Heartbeat of the system!

2. Why Binary Representation?
   - Reliability - good noise immunity.

3. Logic Gates![image-20211228164858556](https://gitee.com/dongramesez/typora-img/raw/master/img/202112281648617.png)

4. **clk-to-q delay** Like logic gates, registers also have a delay associated with them before their output will reflect the input that was sampled.

5. Single bit compare circuit: aka exclusive-nor (xnor) aka is also known as

6. ![image-20211228165430887](https://gitee.com/dongramesez/typora-img/raw/master/img/202112281654956.png)

7. State Elements:

8. Memory elements (aka “state elements”) allow our circuit to “remember” - retain values from one time to the next.

9. Only two types of circuits exist
   - Combinational Logic Blocks (CL)
   - State Elements (registers, memories)

10. Finite State Machine(FSM): Adding registers to CL , aka “sequential circuits”
   - Example sequential circuits (Program counter) PC ![image-20211228170256624](https://gitee.com/dongramesez/typora-img/raw/master/img/202112281702672.png)
   - The cMOS register circuits in common 
     use are “edge-triggered” 
     - ![image-20211228171332627](https://gitee.com/dongramesez/typora-img/raw/master/img/202112281713664.png)
   - Sequential circuits with feedback are often modeled as finite state machines

11. ![image-20211228171142526](C:/Users/Ramezes%20Dong/AppData/Roaming/Typora/typora-user-images/image-20211228171142526.png)

12. Introduction the “multiplexor”, aka “mux”

    - ![image-20211228172100756](https://gitee.com/dongramesez/typora-img/raw/master/img/202112281721801.png)
    - ![image-20211228172434540](https://gitee.com/dongramesez/typora-img/raw/master/img/202112281724596.png)

13. MOS transistor: nFET (Three electrical terminals: gate, source, drain)

    - ![image-20211228172618825](https://gitee.com/dongramesez/typora-img/raw/master/img/202112281726874.png)

    - ![image-20211228173003976](https://gitee.com/dongramesez/typora-img/raw/master/img/202112281730038.png)

14. Nasty realities: 
    - Delays in CMOS circuits
      - transistors as water values
      - Consequences:  the delay time 
      - ![image-20211228173941657](https://gitee.com/dongramesez/typora-img/raw/master/img/202112281739688.png)
    - CMOS circuits use electrical energy (consume power)

### DIS6

1. The fewer gates the faster the circuit (assuming they all have the same delay).
   - False. A wide circuit with more gates can have less delay than just a few gates arranged in sequence.
2. What is hold time, setup time and **tlk-2-q** time ? 保持时间需要足够长，来保证采样的正确性,保持时间算在`tlk-to-q` 中，所以`tlk-2-q` >=保持时间

### RISC-V Processor Design

#### Part 1 :The Datapath

You COU in two parts:

- Data path: contains the hardware necessary to 
  performoperations required by the processor
- Control: decides what each piece of the datapath should do 

1. State required by RV32I ISA

   - Registers(x0 x31)
   - Program Counter
   - Memory (MEM)

2. Basic Phase of Instruction Execution

   ![image-20211229094713122](https://gitee.com/dongramesez/typora-img/raw/master/img/202112290947206.png)

3. ![image-20211229101001070](https://gitee.com/dongramesez/typora-img/raw/master/img/202112291010142.png)

3. `BrEq BrLT` is used in the Branch Compare. `BrUn = 1` selects unsigned comparison for `BrLT`, 0=signed. 

3. ![image-20220108103434171](https://gitee.com/dongramesez/typora-img/raw/master/img/202201081034270.png)

4. Conclusion![image-20211229110739214](https://gitee.com/dongramesez/typora-img/raw/master/img/202112291107287.png)

#### Part 2: RICS-V Control & Operating Speed

1. Controller
   - ![image-20220107163133724](https://gitee.com/dongramesez/typora-img/raw/master/img/202201071631847.png)
   - Notes: Instruction type encoded using only 9 bits `inst[30] inst[14:21], inst[6:2]`
   - ![image-20220107163307602](https://gitee.com/dongramesez/typora-img/raw/master/img/202201071633683.png)
   - Rom Controller Implementation 
     - ROM is common for CISC ISAs (like x86) but is less common for RISC ISAs (like RISC-V). 
     - Can be easily reprogrammed during the design process to `fix errors and add instructions`
   - ![image-20220107163355075](https://gitee.com/dongramesez/typora-img/raw/master/img/202201071633152.png)
   - Combinatorial Logic
     - Used in real-world RISC-V systems.
2. Instruction Timing
   1. Typical Approximate Worst -Case Instruction Timing
      - ![image-20220107164039373](https://gitee.com/dongramesez/typora-img/raw/master/img/202201071640447.png)
   2. Performance Measure
      1. The Iron Law of processor performance .
      2. ![image-20220107164325374](https://gitee.com/dongramesez/typora-img/raw/master/img/202201071643419.png)
      3. ![image-20220107164452352](https://gitee.com/dongramesez/typora-img/raw/master/img/202201071644428.png)
      4. ![image-20220107164510602](https://gitee.com/dongramesez/typora-img/raw/master/img/202201071645660.png)
      5. ![image-20220107175514558](https://gitee.com/dongramesez/typora-img/raw/master/img/202201071755621.png)
      6. ![image-20220107175628385](https://gitee.com/dongramesez/typora-img/raw/master/img/202201071756453.png)
      7. ![image-20220107175726036](https://gitee.com/dongramesez/typora-img/raw/master/img/202201071757102.png)


#### Part3: Pipelining

1. A familiar example:
   - ![image-20220107180054232](https://gitee.com/dongramesez/typora-img/raw/master/img/202201071800330.png)
   - ![image-20220107180125334](https://gitee.com/dongramesez/typora-img/raw/master/img/202201071801393.png)

2. Pipelined RISC-V DataPath
   - ![image-20220107180411898](https://gitee.com/dongramesez/typora-img/raw/master/img/202201071804963.png)

3. ![image-20220107200609455](https://gitee.com/dongramesez/typora-img/raw/master/img/202201072006576.png)
4. ![image-20220107200627749](https://gitee.com/dongramesez/typora-img/raw/master/img/202201072006851.png)
5. Pipeline Hazards
   - ![image-20220107200756059](https://gitee.com/dongramesez/typora-img/raw/master/img/202201072007139.png)
   - ![image-20220107201749713](https://gitee.com/dongramesez/typora-img/raw/master/img/202201072017800.png)
   - ![image-20220107201822975](https://gitee.com/dongramesez/typora-img/raw/master/img/202201072018042.png)
   - ![image-20220107215942343](https://gitee.com/dongramesez/typora-img/raw/master/img/202201072159425.png)
   - ![image-20220107221733421](https://gitee.com/dongramesez/typora-img/raw/master/img/202201072217510.png)
     - Solution 1: Stalling
     - Solution 2: Forwarding(grab operand from pipeline stage, rather than register file)
   - ![image-20220107222325290](https://gitee.com/dongramesez/typora-img/raw/master/img/202201072223373.png)
   - ![image-20220107222910679](https://gitee.com/dongramesez/typora-img/raw/master/img/202201072229760.png)
6. Superscalar Processors
   - What is the CPI? Cycles Per Instruction
   - ![image-20220107230928389](https://gitee.com/dongramesez/typora-img/raw/master/img/202201072309468.png)
7. 5 Phase of execution
   - IF, ID ,EX, MEM, WB	

### Dis 07

1. Explain the 5 phase of execution
   1. IF: Instruction Fetch: Send address to instruction memory (IMEM), and read IMEM at that address.
   2. ID Instruction Decode: Generate control signals from the instruction bits, generate the immediate, and read registers from the RegFile.
   3. EX: Execute: Perform ALU operations, and do branch comparison
   4. MEM Memory: Read from or write to the data memory (DMEM)
   5. WB Write Back: Write back either PC+4, the result of the ALU operation, or data from memory to the RegFile.

### Proj3 CPU :star:

1. How to resolve the immediate selection and register selection with a explicate structure?
   - 串联多路选择器
   - ![image-20220108160421688](https://gitee.com/dongramesez/typora-img/raw/master/img/202201081604741.png)
   - Immediate Generator 
   - ![image-20220108221503009](https://gitee.com/dongramesez/typora-img/raw/master/img/202201082215084.png)
   - 

## W9/W10 Memory Hierarchy, Caches

### Memory Hierarchy Overview

1. Principle of Locality: Programs access only a small portion of the full address space at any instant of time
2. Temporal Locality (locality in time)
3. Spatial Locality (locality in space)
4. ![image-20220109155854748](https://gitee.com/dongramesez/typora-img/raw/master/img/202201091558848.png)
5. ![image-20220109160026665](https://gitee.com/dongramesez/typora-img/raw/master/img/202201091600725.png)
6. ![image-20220109173053188](https://gitee.com/dongramesez/typora-img/raw/master/img/202201091730264.png)
7. Fully Associative Caches
   1. Each memory block can map anywhere in the cache

