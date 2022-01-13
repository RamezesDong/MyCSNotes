# cs61a

[toc]

## W1

### Function

1. values

   | Data type | Example values      |
   | --------- | ------------------- |
   | Integers  | 2 44 -3             |
   | Floats    | 3.14 4.5            |
   | Booleans  | True False          |
   | Strings   | 'hola' 'its python' |
   | Ranges    | range(11)           |
   | Lists     | []                  |

2. **Expressions** (with operators)

   `18 + 69`

   `2 ** 100`

   Call expressions

​		`pow(2,100)`

3. Names

   One way to bind a name is with an **assignment statement**.

4. Function

   A **function** is a sequence of code that performs a particular task and can be easily reused. ♻️

5. Environment 

   - Global frame
   - Local frame

### HW1

##### Quine(自产生程序)

A quine program is a program which can print a copy of its own as the only output.

```c
main() { 
    char *s="main() { char *s=%c%s%c; printf(s,34,s,34); }"; 	printf(s,34,s,34); 
}

```

实现原理：

> A=”储存<B>”
> B=”对于输入<M>,而M作为一段程序码。
>    一、计算出q(<M>)
>    二、把计算结果和<M>结合起来
>    三、打印出所求出的结果”

自产生程序的实际执行过程：

1. 首先A先执行，会得到 < B > 。
2. B开始执行，然后被输入< B >（即M=B）。
3. B利用 < B > ,可以计算出 q(< B >)，并以此计算出 < A >。将 < A > 与 < B > 组合成一个新的程式的描述 < SELF > 。
4. 输出< SELF >。

- `str()repr()`

  ```
  >>> print(str'Hello')
  Hello
  >>> print(repr('Hello'))
  'Hello'
  ```

  

## W2

### Control

- The None value

  The special `None` represents nothingness in Python. Any function which has nothing to return will return `None` 

- Default arguments
  - In the function signature, a parameter can specify a **default value**.

- Multiple return value
  - A function can specify multiple return values,separated by commas.

- Doctests

  - Doctests check the input/output of functions.

- Logical operators 

  `and or not`

  - Type is about the last after `and or`

  `True and 13` -> 13

  `False and 0` -> 0

  `not 10` -> False

  'not None' ->True

- Statements![image-20211210233527486](https://gitee.com/dongramesez/typora-img/raw/master/img/202112102335642.png)

### Debugging

- Traceback
  - Remember that the traceback is organized with the "most recent call last".

- Debugging Techniques
  - Running doctests
  - Writing your own tests
  - Using `print` statements
  - Using `assert` statements
  - Error Types

### Higher-Order Functions

- What are higher-order functions?

  - Takes another function as an argument
  - Return a function as its result

  All other functions are considered first-ordered functions.

- Lambda syntax

  A **lambda expression** is a simple function definition that evaluates to a function.

  The syntax:

  ```python
  lambda <parameters>: <expression>
  ```

  A function that takes in `parameters` and returns the result of `expression`.

  A lambda version of the `square` function:

  ```python
  square = lambda x: x * x
  ```

- Only the `def` statement gives the function an **intrinsic name**, which shows up in environment diagrams but doesn't affect execution (unless the function is printed).

- Lambdas with conditionals

  - This is invalid syntax: :disappointed:

    **lambda** x: **if** x > 0: x **else**: 0

  -  Conditional expressions to the rescue!

    **lambda** x: x **if** x > 0 **else** 0

- example:

  In the Hog, `say` has a recycled use of function total and echo one by one  .

  ```python
  def total(x,y):
  	print(x+y)
      return echo
  def echo(x,y):
      print(x,y)
      return total
   s0, s1 = play(always_roll(1), always_roll(1), dice=make_test_dice(2, 5), goal=10, say=echo)
  ```

### Hog:star:

- contain `str` **error**. The python version need 3.8 or higher.

### Environment

- Multiple Environment

  Names have no meanings without environment

- Environment for higher-order functions

  - Every user-defined **function** has a parent frame

  - The parent of a **function** is the **frame in which it was defined**
  - Every local **frame** has a parent frame
  - The parent of a **frame** is the **parent of the called function**

- Self-reference
  - ![image-20211212092400472](https://gitee.com/dongramesez/typora-img/raw/master/img/202112120924547.png)

- Currying

  Converting a function that takes multiple arguments into a single-argument higher-order function.

  A function that currys any two-argument function:

```python
def curry2(f):
    def g(x):
        def h(y):
            return f(x, y)
        return h
    return g
```

### Church Numerals(丘奇数):star:

[good math](http://goodmath.blogspot.com/)

[λ演算(Lambda Calculus)入门基础（一）：定义与归约](https://www.jianshu.com/p/ebae04e1e47c)

1. `lambda x:M` can represent like  $\lambda$ x.M

   `(λx.x) y, (λx.x) (λx.x)`The first means that $\lambda$ x,x assignment the y. And the (λx.x) is an identity function.

   函数应用是左结合的

2. Currying

   **`currying: λx y.xy = λx.(λy.xy)`**

3. Church numerals

   - **皮亚诺公理(Peano Axioms)**皮亚诺公理规定了自然数集就是从`0`开始通过后继函数`S`构成的集合

   - 0 = $\lambda$f.$\lambda$x.x

   - S(successor) = $\lambda$n.$\lambda$f. $\lambda$​x.f(n f x)

   - ```haskell
     0 ≡ λf.λx.x
     1 ≡ λf.λx.f x
     2 ≡ λf.λx.f (f x)
     3 ≡ λf.λx.f (f (f x))
     ...
     ```

   - **`n ≡ λf.λx.f^n x`** every church number is a higher-order function.

4. Calculate

   - Addition: n+m is n times Successor of m.

     ```haskell
     PLUS ≡ λm.λn.λf.λx.m f (n f x)
     ```

   - Multiple: n times m Successor

     1. 将`n`先应用于`f`上，得到将`f`应用于参数`x`上`n`次的中间函数`TMP`；

     2. 然后将`m`应用于`TMP`上，即再把`TMP`应用于参数`x`上`m`次，这样就可以得到将`f`应用于`x`上`m * n`次的结果`p`。

     ```haskell
     MULTIPLE = λm.λn.λf.m (n f)
     MULTIPLE = λm.λn.λf.λx.m (n f) x
     ```

   - Pow : pow(m,n) is m^n^ ,which means that m * m * m * m ...m . The number of m is n.

     乘方乍一看十分的抽象，将它补全得到 (m n f) x = (n . n. n ... .n ) f x 

     ```haskell
     POW = λm.λn.n (m)
     ```

     

## W3

### Design

- Functional abstractions

- Names
  - *There are only two hard things in Computer Science: cache invalidation and naming things. --Phil Karlton*
- Errors
  - Logic errors
  - Syntax errors
    - `IndentationError`/`TabError`
  - Runtime errors

### Function Examples

1. Decorators

The notation:

```python
@ATTR
def aFunc(...):
    ...
```

is essentially equivalent to:

```python
def aFunc(...):
    ...
aFunc = ATTR(aFunc)
```

## W4

### Recursion

### Tree Recursion

### HW3

- It is hard to handle some problems, so that I have to learn from others solutions.
- Count Coins and Missing Digits :star:
- **Anonymous factorial**--Having a good comprehension of lambda is not easy.

## W5

### Containers

1. List: A list is contained that holds a sequence of related pieces of information.

   - `member = []` empty list
   - `len()` get the length of a list 
   - `letters[0]` Each list item has a index, starting from 0.`letters[-1]`Negative indices are also possible.
   - `+` and `add()` can add two lists together .
   - `*` or `mul()` can concatenate the same list multiple times.
   - Nested Lists: list can contains lists as items.

2. Containment

   - Use `in` operator to test if value is inside a container.

3. For Statement

   - for <value> in <sequence>

4. Ranges

   - `range(-2,3)` -2,-1,0,1,2
   - `range(3)` 0,1,2

5. List comprehensions

   - `[<map exp> for <name> in <iter exp>]`

     ```python
     def front(s, f):
     	return [e for e in s if f(e)] + [e for e in s if not f(e)]
     ```

6. String literals

   - Strings are similar to lists

### Lab4

- Riffle Shuffle

  ```python
  def riffle(deck):
      """Produces a single, perfect riffle shuffle of DECK, consisting of
      DECK[0], DECK[M], DECK[1], DECK[M+1], ... where M is position of the
      second half of the deck.  Assume that len(DECK) is even.
      >>> riffle([3, 4, 5, 6])
      [3, 5, 4, 6]
      >>> riffle(range(20))
      [0, 10, 1, 11, 2, 12, 3, 13, 4, 14, 5, 15, 6, 16, 7, 17, 8, 18, 9, 19]
      """
      return [row[i] for row in zip(deck[:len(deck)//2],deck[len(deck)//2:]) for i in range(2)]
  ```

- deck 有2M元素。将deck氛围前后两个部分，zip打包得到`[(deck[0],deck[M]),(deck[1],deck[M+1])...(deck[M-1],deck[2M-1])]`

### Sequences

1. Box + Pointer

2. Slicing: Slicing a list creates a new list with a subsequences of the original list.

   ```python
   compound_word = "cortaúñas"
   
   word1 = compound_word[:5]    # "corta"
   word2 = compound_word[5:]   # "úñas"
   ```

   `list()` creates a new list containing exiting elements from any iterable

3. Recursive exercises

   ```python
   def reverse(s):
       """Returns a string with the letters of S
       in the inverse order.
       >>> reverse('ward')
       'draw'
       """
       if len(s) == 1:
           return s
       else:
           return reverse(s[1:]) + s[0]
   ```

   

4. Built-ins for iterables![image-20211212191620156](https://gitee.com/dongramesez/typora-img/raw/master/img/202112121916273.png)

   ```python
   gymnasts = [ ["Brittany", 9.15, 9.4, 9.3, 9.2],
       ["Lea", 9, 8.8, 9.1, 9.5],
       ["Maya", 9.2, 8.7, 9.2, 8.8] ]
   min(gymnasts, key=lambda scores: min(scores[1:]))    # ["Maya", ...]
   max(gymnasts, key=lambda scores: sum(scores[1:], 0)) # ["Brittany", ...]
   ```

### Data Abstraction

1. Data abstraction

   - `pair` If we need to frequently manipulate “pair” of values in our program,we could use  a pair data abstraction.

     `first(pair) second(pair)` 

   - Rational numbers

     If we needed to represent fractions exactly... $\frac{numerator}{denominator}$

2. Dictionaries
   - A `dict` is a mutable mapping of key-value pairs.
   - `words = {"mas":1,"pig":2}`
   - `get(key,value)`
   - A key **cannot** be a list or dictionary (or any mutable type)

### Cats

- The project is easier than the Hog, because there is no high-order function. But what I should to learn are a lot, such as list comprehensions, recursion, etc.
- The cats is a game that tests player typing speed. It’s well known that the higher typing speed, the more information that we present, express and share. 

## W6

### Trees

- Recursive description: A tree has a root label and a list of branches, which are itself a tree.
- Relative description: Each location in a tree is called a node that has a label with any value. One node can be the parent/child of another
- **Suddenly,  I wake up that I don’t need to write down everything I had known. So, I will skip many slides and find out what is new or interesting for me.**
- Parse trees: For natural language![image-20211213114752917](https://gitee.com/dongramesez/typora-img/raw/master/img/202112131148056.png)

### Mutability

1. Object & methods
   - An object is a bundle of data and behavior. A type of object is called a class. Every value in Python is an object.
2. List mutation & methods
   - `append()` adds a single element to a list.
   - `extend()` adds all the elements in one list to a list.
   - `pop()` removes and return the last element
   - `remove()` removes the first element equal to the argument
3. Tuples
   1. Tuple is an immutable sequence.
   2. Many of list’s read-only operations work on tuples.
4. Mutability
   1. Immutable types: int, float, string, tuple
   2. mutable types: list, dict
   3. Equality(==) & Identity(is)判断身份表示是否相等
5. Beware of mutation

### Syntax

1. Syntax trees
   - Using the tree abstraction
2. Data abstractions
3. Parsing syntax trees
4. Sentence generation

## W7

### Iterators

1. Iterators: An iterator is an object that provides sequential access to values. one by one.
   - `iter(iterable)` returns an iterator over the elements of an iterable.
   - `next(iterable)` return the next element in an iterator.
   - Lists, tuples, dictionaries, strings and ranges are all iterable objects.

2. Functions
   - ![image-20211215095426386](https://gitee.com/dongramesez/typora-img/raw/master/img/202112150954607.png)
   - ![image-20211215095456785](https://gitee.com/dongramesez/typora-img/raw/master/img/202112150954952.png)
3. Reasons for using iterators
   - A code that process an iterator using `iter` or `next` makes few assumptions about the data itself.
   - An iterator bundles together a sequence and a position with the sequence in a single object.

### Generators

- A generator function uses `yeild` instead of `return`. A generator is a type of a iterator that yields results from a generator functions.

- When `next()`is called on the iterator, it executes the body of the generator from the last stopping point up to the next `yield` statement.**停在前一个yield处**

- If it doesn't reach a yield statement, it stops at the end of the function and raises a `StopIteration` exception.

- Virahanka-Fibonacci generator 

  ```python
  def generate_virfib():
      """Generate the next Virahanka-Fibonacci number.
      >>> g = generate_virfib()
      >>> next(g)
      0
      >>> next(g)
      1
      >>> next(g)
      1
      >>> next(g)
      2
      >>> next(g)
      3
      """ 
      prev = 0  # First Fibonacci number
      curr = 1  # Second Fibonacci number
      while True:
          yield prev
          (prev, curr) = (curr, prev + curr)
  ```

- `yield from`statement can be used to yield the values from an iterable one at a time.:sa:

- Counting partitions with yield

  ```python
  def partitions(n, m):
      """List partitions.
  
      >>> for p in partitions(6, 4): print(p)
      4 + 2
      4 + 1 + 1
      3 + 3
      3 + 2 + 1
      3 + 1 + 1 + 1
      2 + 2 + 2
      2 + 2 + 1 + 1
      2 + 1 + 1 + 1 + 1
      1 + 1 + 1 + 1 + 1 + 1
      """
      if n < 0 or m == 0:
          return
      else:
          if n == m:
              yield str(m)
          for p in partitions(n-m, m):
              yield str(m) + " + " + p
          yield from partitions(n, m - 1)
  ```

### Objects

1. Object-oriented programming

   - OOP is a method for organizing programs which includes:

     - Data abstractions

     - Bundling together information and related behavior

2. The class statement/method

   - `__init__` method of class is called with the new object as its first argument (named `self`), along with any additional arguments provided in the call expression
   - `pina.increase(2)` is equivalent to `Product.increase(pina,2)`

3. Class variables

   - A **class variable** is an assignment inside the class that isn't inside a method body.

4. Accessing attributes

   - `getattr/hasattr`. We can use them to look up an attribute using a string.

   - ```python
     getattr(pina_bar, 'inventory')   # 1
     
     hasattr(pina_bar, 'reduce_inventory')  # True
     ```

5. Private attributes

   - To communicate the desired access level of attributes, Python programmers generally use this convention:
     - `__` (double underscore) before very private attribute names
     - `_` (single underscore) before semi-private attribute names
     - no underscore before public attribute names

### Lab6

- repeated: 使用iterator来找相邻重复出现k次的数
  - 我纠结是否需要定义函数存储之前状态
  - 直接使用while就行了，iterator本来就只会遍历一遍

### HW5

1. `gen_perms()` use the `yield` recursion. Generating all the possibilities of `seq[1:]` add `seq[0]` in every position that can be placed.
2. Remainder Generator: The function(m) create a function that can give a generator with size K. 我用英语表达不出来:sweat: ，可以看代码，找在自然数范围内的解。  

## W8

### Inheritance

1. Base classes and subclasses
   - base classes is also known as the superclass.

2. Overriding 
3. `spuer().attribute` refers to the definition of `attribute` in the superclass of the first parameter to the method
4. `object` : Every Python3 class implicitly extends the `object` class.
5. Multiple inheritance 
6. Composition vs. Inheritance
   - Inheritance is best for representing “is-a” relationships.
   - Composition is best for representing “has-a” relationships

### Lab7

1. [TypeError: method() takes 1 positional argument but 2 were given](https://stackoverflow.com/questions/23944657/typeerror-method-takes-1-positional-argument-but-2-were-given)

   - In python, this 

     ```python
     my_object.method("foo")
     #is syntactic sugoar, which the interpreter translates behind the scense into:
     Myclass.method(my_object, "foo")
     ```

   - as you can see, does indeed have two arguments. So that  `my_object.method(slef,"foo")` is wrong.

### Representation

- string interpolation

  `f strings` 

  ```python
  artist = "Lil Nas X"
  song = "Industry Baby"
  place = 2
  
  print(f"Debuting at #{place}: '{song}' by {artist}")
  ```

- `dir`(object) , a built-in function that returns a list of all the attributes on an object
- `__str__` returns a human readable string representation of an object. `__repr__` returns a string that would evaluate to an object with the same values.
- Polymorphism
  - Type dispatching

### Recursive objects

1. Linked List
2. Trees (is a class)

### hw6

1. `deep_map_mut` deep search and mutate. Just remember that the link like a pionter changed directly. 

2. VirFib  use the method of object-oriented programming to solve the VirFib problems.   The `next` method return a `VirFib` instance with only constant time.

## W9

### Efficiency

1. Exponentiation ((in a [mathematical](https://www.collinsdictionary.com/zh/dictionary/english/mathematical) [equation](https://www.collinsdictionary.com/zh/dictionary/english/equation)) the use of an [exponent](https://www.collinsdictionary.com/zh/dictionary/english/exponent) to [raise](https://www.collinsdictionary.com/zh/dictionary/english/raise) the [value](https://www.collinsdictionary.com/zh/dictionary/english/value) of the [base](https://www.collinsdictionary.com/zh/dictionary/english/base) number to a [power](https://www.collinsdictionary.com/zh/dictionary/english/power))
   - 
2. Orders of Growth
   - Exponential growth
   - Quadratic growth
   - Linear growth
   - Logarithmic growth
   - Constant growth
3. Memorization is a strategy to reduce redundant computation by remembering the results of previous function calls in a “memo”

### Decomposition

1. Modules

   ```python
   import link #importing a whole module
   ll = link.Link(3,link.Link(4))
   from link import Link # Importing specific namesll = link.Link(3,link.Link(4))
   ll = Link(3,Link(4))
   from link import * #importing all the names
   ```

- alias `from link import Link as LL`

- Running a module `python module.py` When run like that, Python sets a global variable `__name__` to ‘’main”.

  ```python
  if __name__ == "__main__":
  ```

  The code inside the condition will be executed as well, but only the module is run directly.

2. Packages

   - Importing a whole path: 

     ```python
      import sound.effects.echo 
      sound.effects.echo
     ```

   - `from sound.effects import echo`

3. Modular design: A design principle: Isolate different parts of a program that address different concerns

   The way to isolate in Python:

   - Functions
   - Classes
   - Modules
   - Packages

### Data Examples

### Lab08

1. Prune Small. What is the most difficult for me is that the question is a fixed format fill-in-blank questions.

   ```python
   def prune_small(t, n):
   	while len(t.branches) > n:
   		largest = max(t.branches, key=lambda x: x.label)
    		t.branches.remove(largest)
   	 for b in t.branches:
    		prune_small(b, n)
   ```

2. `max(iterable, *iterables, key, default)`

   - **iterable** - an iterable such as list, tuple, set, dictionary, etc.
   - ***iterables (optional)** - any number of iterables; can be more than one
   - **key (optional)** - key function where the iterables are passed and comparison is performed based on its return value
   - **default (optional)** - default value if the given iterable is empty

   - We can have passed a lambda function to the `key` parameter.

## W10

### Lab 09: Midterm Review

## W11

### Scheme

1. Scheme expressions

   - [Genealogical tree of programming languages](https://upload.wikimedia.org/wikipedia/commons/2/25/Genealogical_tree_of_programming_languages.svg)
   - [Scheme Built-In Procedure Reference | CS 61A Spring 2022](https://cs61a.org/articles/scheme-builtins/#general)
   - Scheme is a good example of a functional programming language.
   - Scheme programs consist of expressions, which can be:
     - Primitive expressions: `2` `3.3` `#t` `#f` `+` `quotient`
     - Combinations: `(quotient 10 2)` `(not #t)`

2. Special forms

   - [CS 61A Scheme Specification | CS 61A Spring 2022](https://cs61a.org/articles/scheme-spec/#special-forms-2)

   - define form:  Evaluates `<expression>` and binds the value to `<name>` in the current environment. `<name>` must be a valid Scheme symbol.

     ```scheme
     (define x 2)
     ```

   - define procedure

   - If expression: `if <predicate> <consequent> <alternative>`

     ```scheme
     (define nums '(1 2 3))
     (if (null? nums) 0 (length nums))
     ```

   - and form: `(and [test] ...)`

   - or form:  `(or [test] ...)`

   - lambda expressions: `(lambda ([param] ...) <body> ...)`

     - Two equivalent expressions:

     - ```scheme
       (define (plus4 x) (+ x 4))
       (define plus4 (lambda (x) (+ x 4)))
       ```

   - Cond form

     - ```scheme
       (cond ((> x 10) (print 'big))
           ((> x 5) (print 'medium))
           (else (print 'small)))
       ```

   -  The begin form
   - let form: The `let` special form binds symbols to values temporarily; just for one expression

3. Scheme List

   - Scheme lists are linked lists.
   - Scheme use the cons form`(cons 1 (cons 2 nil))`. `nil` is the empty list.
   - `car`: Procedure that returns the first element of a list
   - `cdr`: Procedure that returns the rest of the list

4. Quotation

   Symbolic programming. Symbols typically refer to values. **Quotation** is used to refer to symbols directly. `(list 'a 'b)  ; (a b) (list 'a b)   ; (a 2)` The `‘` is shorthand for the `quote` form.



### Lab10

scheme这种函数式语言，运算符在左，比较抽象难以适应。Scheme作为Lisp的一种方言,有很强的代表意义,影响了在它之后的许多语言。

Lisp诞生之时，就包含了9种新思想。而这九种新思想也在各种语言中出现。

- 条件结构
- 函数也是一种数据类型。
- 递归
- 变量的动态类型。
- 垃圾回收机制。
- 程序由表达式组成。
- 符号（symbol）类型。
- 代码使用符号和常量组成的树形表示法（notation）。
- 无论什么时候，整个语言都是可用的。

### HW 07

[介绍 | Scheme 语言简明教程](https://wizardforcel.gitbooks.io/teach-yourself-scheme/content/index.html)

### Exceptions

1. A Scheme Expression is a Scheme List
2. Quasiquotation
   1. ![image-20220113124412417](https://gitee.com/dongramesez/typora-img/raw/master/img/202201131244516.png)
3. ![image-20220113124503317](https://gitee.com/dongramesez/typora-img/raw/master/img/202201131245364.png)
4. Exceptions: An **exception** is a built-in mechanism in a programming language to declare and respond to "exceptional" conditions.
5. Exception in Python [exceptions docs](https://inst.eecs.berkeley.edu/~cs61a/fa21/assets/slides/28-Exceptions.html#/10)
6. Assert statements: Assert statements raise an exception of type `AssertionError`.
7. Raise statements

### Calculator

- Programming languages
  - ![image-20220113130045288](https://gitee.com/dongramesez/typora-img/raw/master/img/202201131300348.png)
- Parsing a language
  - A parser takes text and returns an expression object.
  - ![image-20220113130336762](https://gitee.com/dongramesez/typora-img/raw/master/img/202201131303814.png)
  - ![image-20220113130527474](https://gitee.com/dongramesez/typora-img/raw/master/img/202201131305547.png)
  - 
- The Calculator language
  - ![image-20220113130710692](https://gitee.com/dongramesez/typora-img/raw/master/img/202201131307763.png)
  - ![image-20220113130807745](https://gitee.com/dongramesez/typora-img/raw/master/img/202201131308815.png)
- Evaluating a language
  - ![image-20220113131000294](https://gitee.com/dongramesez/typora-img/raw/master/img/202201131310375.png)
- Interactive interpreters
  - ![image-20220113132214547](https://gitee.com/dongramesez/typora-img/raw/master/img/202201131322640.png)

