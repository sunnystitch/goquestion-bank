## 1.定义一个包内全局字符串变量，下面语法正确的是（）
    A. var str string
    B. str := ""
    C. str = ""
    D. var str = ""
解析：
    A D  B 只支持局部变量声明；C 是赋值，str 必须在这之前已经声明；
##  2.下面这段代码输出什么?
```go
func hello(i int) {  
    fmt.Println(i)
}
func main() {  
    i := 5
    defer hello(i)
    i = i + 10
}

```
//5
解析：  延迟加载 defer 是关闭后执行，但是值早就赋值了
这个例子中，hello() 函数的参数在执行 defer 语句的时候会保存一份副本，在实际调用 hello() 函数时用，所以是 5.


##   3.下面这段代码输出什么？
```go
type People struct{}

func (p *People) ShowA() {
    fmt.Println("showA")
    p.ShowB()
}
func (p *People) ShowB() {
    fmt.Println("showB")
}

type Teacher struct {
    People
}

func (t *Teacher) ShowB() {
    fmt.Println("teacher showB")
}

func main() {
    t := Teacher{}
    t.ShowA()
}
//showA
  showB
```
解析：结构体嵌套。

## 4 下面代码运行什么结果？
```go
func main() {
	str := "hello"
	str[0] = 'x'
	fmt.Println(str)
}
```
    A. hello
    B. xello
    C. compilation error
解析：知识点：C。 常量，Go 语言中的字符串是只读的。

## 5.下面代码输出什么？
```go
func incr(p *int) int {
    *p++
    return *p
}

func main() {
    p :=1
    incr(&p)
    fmt.Println(p)
}
```    
    A. 1
    B. 2
    C. 3
// B.2 
解析 ：   ：指针，incr() 函数里的 p 是 *int 类型的指针，指向的是 main() 函数的变量 p 的地址。
第 2 行代码是将该地址的值执行一个自增操作，incr() 返回自增后的结果。

## 6.对 add() 函数调用正确的是（）
```go
func add(args ...int) int {

    sum := 0
    for _, arg := range args {
        sum += arg
    }
    return sum
}
```
    A. add(1, 2)
    B. add(1, 3, 7)
    C. add([]int{1, 2})
    D. add([]int{1, 3, 7}…)
解析： ABD。知识点：可变函数  

## 7.下面代码下划线处可以填入哪个选项？
```go
func main() {
	var s1 []int
	var s2 = []int{}
	if s2 == nil {
		fmt.Println("yes nil")
	}else{
		fmt.Println("no nil")
	}
}
```
  A. s1
  B. s2
  C. s1、s2 都可以
解析：C     A。知识点：nil 切片和空切片。
nil 切片和 nil 相等，一般用来表示一个不存在的切片；
空切片和 nil 不相等，表示一个空的集合。
s1 --->  yes nil 
s2 ---> no nil

## 8 
```go
func main() {
	i := 65
	fmt.Println(string(i))
}
// A
```
    A. A
    B. 65
    C. compilation error
解析：  UTF-8 编码中，十进制数字 65 对应的符号是 A。

## 9.下面这段代码输出什么？
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
	fmt.Println(a.ShowA())
	fmt.Println(b.ShowB())
}
// 13
//   23
```
解析：接口。一种类型实现多个接口，结构体 Work 分别实现了接口 A、B，
所以接口变量 a、b 调用各自的方法 ShowA() 和 ShowB()，输出 13、23。