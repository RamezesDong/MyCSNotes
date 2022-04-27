# Go

[toc]

## fmt

#### printing format ‘verbs’

[fmt package - fmt - pkg.go.dev](https://pkg.go.dev/fmt)

General:

> ```go
> %v	the value in a default format
> 	when printing structs, the plus flag (%+v) adds field names
> %#v	a Go-syntax representation of the value
> %T	a Go-syntax representation of the type of the value
> %%	a literal percent sign; consumes no value
> ```

## Slices

#### Exercise: Slices

> Implement `Pic`. It should return a slice of length `dy`, each element of which is a slice of `dx` 8-bit unsigned integers. When you run the program, it will display your picture, interpreting the integers as grayscale (well, bluescale) values.
>
> The choice of image is up to you. Interesting functions include `(x+y)/2`, `x*y`, and `x^y`.
>
> (You need to use a loop to allocate each `[]uint8` inside the `[][]uint8`.)
>
> (Use `uint8(intValue)` to convert between types.)

```go
package main

import "golang.org/x/tour/pic"

func Pic(dx, dy int) [][]uint8 {
	var xp = make([][]uint8, dy)
	for y := range xp {
		xp[y] = make([]uint8, dx)
		for x := range xp[y]{
			xp[y][x] = uint8((x^y)*(y^x))
		}
	}
	return xp
}

func main() {
	pic.Show(Pic)
}

```

之前没有使用过go，需要先make一个新的二维slice，每个子slice也需要make，之后运算，如上结果为：![image-20220305115733479](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220305115733479.png)

## Method and Interface

[Exercise: rot13Reader](https://go.dev/tour/methods/23)

[Exercise: Images](https://go.dev/tour/methods/25)

## **Concurrency**

#### [等价二叉查找树](https://tour.go-zh.org/concurrency/8)

- Walk()函数先序遍历二叉树（使用管道）

- ```go
  // Walk 步进 tree t 将所有的值从 tree 发送到 channel ch。
  func Walk(t *tree.Tree, ch chan int){
  	defer close(ch)
  
  	// requred "forward declaration" for recurive calls
  	var walk func(t *tree.Tree)
  
  	walk = func(t *tree.Tree) {
  		if t == nil {
  			return
  		}
  		walk(t.Left)
  		ch <- t.Value
  		walk(t.Right)
  	}
  
  	walk(t)
  }
  ```

- 使用defer来关闭管道

- Same（）函数判断是否相等

- [Go Tour Exercise #7: Binary Trees equivalence - Stack Overflow](https://stackoverflow.com/questions/12224042/go-tour-exercise-7-binary-trees-equivalence)

#### [Exercise: Web Crawler](https://go.dev/tour/concurrency/10)

- [Tour of Go exercise #10: Crawler - Stack Overflow](https://stackoverflow.com/questions/13217547/tour-of-go-exercise-10-crawler)