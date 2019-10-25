## 1.切片 a、b、c 的长度和容量分别是多少？
```go
func main() {

    s := [3]int{1, 2, 3}
    a := s[:0]
    b := s[:2]
    c := s[1:2:cap(s)]
}
```
//fmt.Println(len(a),len(b),len(c)) 
  0 2 1
// 	fmt.Println(cap(a),cap(b),cap(c))
  3 3 2

解析： 知识点：数组或切片的截取操作。
截取操作有带 2 个或者 3 个参数，形如：[i:j] 和 [i:j:k]，
假设截取对象的底层数组长度为 l。在操作符 [i:j] 中，
如果 i 省略，默认 0，如果 j 省略，默认底层数组的长度，
截取得到的切片长度和容量计算方法是 j-i、l-i。操作符 [i:j:k]，
k 主要是用来限制切片的容量，但是不能大于数组的长度 l，
截取得到的切片长度和容量计算方法是 j-i、k-i。

|                 |                                                              |
| --------------- | ------------------------------------------------------------ |
| s[n]            | 切片s中索引位置为n的项                                       |
| s[:]            | 从切片s的索引位置0到len(s)-1处所获得的切片                   |
| s[low:]         | 从切片s的索引位置low到len(s)-1处所获得的切片                 |
| s[:high]        | 从切片s的索引位置0到high处所获得的切片，len=high             |
| s[low:high]     | 从切片s的索引位置low到high处所获得的切片，len=high-low       |
| s[low:high:max] | 从切片s的索引位置low到high处所获得的切片，len=high-low，cap=max-low |
| len(s)          | 切片s的长度，总是<=cap(s)                                    |
| cap(s)          | 切片s的容量，总是>=len(s)                                    |

## 2.下面代码中 A B 两处应该怎么修改才能顺利编译？
```go
func main() {
    var m map[string]int        //A
    m["a"] = 1
    if v := m["b"]; v != nil {  //B
        fmt.Println(v)
    }
}
```
解析：
在 A 处只声明了map m ,并没有分配内存空间，不能直接赋值，
需要使用 make()，都提倡使用 make() 或者字面量的方式直接初始化 map。

B 处，v,k := m["b"] 当 key 为 b 的元素不存在的时候，
v 会返回值类型对应的零值，k 返回 false。


## 3.下面代码输出什么？
```go
type A interface {
	ShowA() int
}

type B interface {
	ShowB() int
}

type Work struct {
	i int
}

func (w Work) ShowA() int {
	return w.i + 10
}

func (w Work) ShowB() int {
	return w.i + 20
}

func main() {
	c := Work{3}
	var a A = c
	var b B = c
	fmt.Println(a.ShowB())
	fmt.Println(b.ShowA())
}
```
    A. 23 13
    B. compilation error
    
解析：  知识点：接口的静态类型。a、b 具有相同的动态类型和动态值，分别是结构体 work 和 {3}；
a 的静态类型是 A，b 的静态类型是 B，接口 A 不包括方法 ShowB()，接口 B 也不包括方法 ShowA()，
编译报错。看下编译错误：
a.ShowB undefined (type A has no field or method ShowB)
b.ShowA undefined (type B has no field or method ShowA)
   

## 4.下面代码中，x 已声明，y 没有声明，判断每条语句的对错。

    1. x, _ := f()
    2. x, _ = f()
    3. x, y := f()
    4. x, y = f()
   
解析： 知识点：变量的声明。1.错，x 已经声明，不能使用 :=；
2.对；3.对，当多值赋值时，:= 左边的变量无论声明与否都可以；
4.错，y 没有声明。  
    
## 5.下面代码输出什么？    
```go
func increaseA() int {
    var i int
    defer func() {
        i++
    }()
    return i
}

func increaseB() (r int) {
    defer func() {
        r++
    }()
    return r
}

func main() {
    fmt.Println(increaseA())
    fmt.Println(increaseB())
}
```
    A. 1 1
    B. 0 1
    C. 1 0
    D. 0 0
    
解析：  B 0 1 
 B。
知识点：defer、返回值。注意一下，increaseA() 的返回参数是匿名，increaseB() 是具名。
关于 defer 与返回值的知识点，
  
    
    
## 6. 下面代码输出什么？
```go
type A interface {
    ShowA() int
}

type B interface {
    ShowB() int
}

type Work struct {
    i int
}

func (w Work) ShowA() int {
    return w.i + 10
}

func (w Work) ShowB() int {
    return w.i + 20
}

func main() {
    var a A = Work{3}
    s := a.(Work)
    fmt.Println(s.ShowA())
    fmt.Println(s.ShowB())
}
```    
    A. 13 23
    B. compilation error
解析：    A 类型断言。

## 7.f1()、f2()、f3() 函数分别返回什么？
```go
func f1() (r int) {
    defer func() {
        r++
    }()
    return 0
}

func f2() (r int) {
    t := 5
    defer func() {
        t = t + 5
    }()
    return t
}

func f3() (r int) {
    defer func(r int) {
        r = r + 5
    }(r)
    return 1
}
```
解析：
//1
  5
  1
  
## 8.下面代码段输出什么？
```go
type Person struct {
	age int
}

func main() {
	person := &Person{28}

	// 1.
	defer fmt.Println(person.age)

	// 2.
	defer func(p *Person) {
		fmt.Println(p.age)
	}(person)

	// 3.
	defer func() {
		fmt.Println(person.age)
	}()

	person.age = 29
}

//29
//29
//28
``` 
解析：变量 person 是一个指针变量 。
   
   1.person.age 此时是将 28 当做 defer 函数的参数，会把 28 缓存在栈中，等到最后执行该 defer 语句的时候取出，即输出 28；
   
   2.defer 缓存的是结构体 Person{28} 的地址，最终 Person{28} 的 age 被重新赋值为 29，所以 defer 语句最后执行的时候，依靠缓存的地址取出的 age 便是 29，即输出 29；
   
   3.闭包引用，输出 29；
   
   
加油站：
什么是 defer

defer 是 Go 语言提供的一种用于注册延迟调用的机制，
每一次 defer 都会把函数压入栈中，当前函数返回前再把延迟函数取出并执行。
defer 语句并不会马上执行，而是会进入一个栈，函数 return 前，
会按先进后出（FILO）的顺序执行。也就是说最先被定义的 defer 语句最后执行。
先进后出的原因是后面定义的函数可能会依赖前面的资源，
自然要先执行；否则，如果前面先执行，那后面函数的依赖就没有了。

采坑点
使用 defer 最容易采坑的地方是和带命名返回参数的函数一起使用时。

>defer 语句定义时，对外部变量的引用是有两种方式的，
>分别是作为函数参数和作为闭包引用。作为函数参数，则在 defer 定义
>时就把值传递给 defer，并被缓存起来；作为闭包引用的话，
>则会在 defer 函数真正调用时根据整个上下文确定当前的值。

避免掉坑的关键是要理解这条语句：
>return xxx

这条语句并不是一个原子指令，经过编译之后，变成了三条指令：
>1. 返回值 = xxx
>2. 调用 defer 函数
>3. 空的 return

1,3 步才是 return 语句真正的命令，第 2 步是 defer 定义的语句，这里就有可能会操作返回值。

相关阅读：
1.Golang之轻松化解defer的温柔陷阱 [https://segmentfault.com/a/1190000018169295#articleHeader4](https://segmentfault.com/a/1190000018169295#articleHeader4)
2.Go defer实现原理剖析  [https://my.oschina.net/renhc/blog/2870345](https://my.oschina.net/renhc/blog/2870345)
3.golang之defer简介   [https://leokongwq.github.io/2016/10/15/golang-defer.html](https://leokongwq.github.io/2016/10/15/golang-defer.html)
