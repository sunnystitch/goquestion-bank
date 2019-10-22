## 1通过指针变量p访问其成员变量name,有哪几种方式？
A. p.name
B. (&p).name
C. (*p).name
D. p->name
解析：AC  &取址运算符， *指针解引用

## 2下面这段代码是否通过编译？如果通过，输出什么
```go
type MyInt1 int
type MyInt2 = int

func main(){
	var i int = 0
	var i1 MyInt1 = i
	var i2 MyInt2 = i
	fmt.Println(i2)
}
```
运行结果：
```# command-line-arguments
     .\main.go:10:6: cannot use i (type int) as type MyInt1 in assignment
```  
解析：编译不通过，
考察点：类型别名与类型定义的区别。
基于类型int创建了新类型MyInt1, 
下一行是创建了int的类型别名MyInt2,
注意类型别名的定义时 = 。 所以
var i1 MyInt1 相当于是将int类型赋值给
MyInt1 类型的变量，Go是强类型语言，编译不通过；
而MyInt2只是int的别名，本质上还是int,可以赋值。


## 3 以下代码输出什么?
```go
func main(){
	a := []int{7,8,9}
	fmt.Printf("%+v\n",a)
	ap(a)
	fmt.Printf("%+v\n",a)
	app(a)
	fmt.Printf("%+v\n",a)
}
func ap(a []int){
	a = append(a,10)
}
func app(a []int){
	a[0] = 1
}

[7 8 9]
[7 8 9]
[1 8 9]

```
解析：因为 append 导致底层数组重新分配内存了，
ap 中的 a 这个slice 的底层数组和外面的不是一个，
并没有改变外面的。 这个地方看下官方手册append的解释

## 4.关于字符串连接，下面语法正确的是？
A. str := 'abc' + '123'
B. str := "abc" + "123"
C. str := '123' + "abc"
D. fmt.Sprintf("abc%d", 123)
解析：
B D 
知识点：字符串连接。除了以上两种连接方式，还有 strings.Join()、buffer.WriteString()等。


## 5.下面这段代码能否编译通过？ 如果可以，输出什么？

```go

const (
	x = iota
	_
	y
	z = "zz"
	k
	p = iota
)
func main(){
	fmt.Println(x,y,z,k,p)
}

 // 结果0 2 zz zz 5

```
解析: 
 参考博客
 https://www.cnblogs.com/zsy/p/5370052.html

## 6下面赋值正确的是()
A. var x = nil
B. var x interface{} = nil
C. var x string = nil
D. var x error = nil

B D
解析:
  nil 值。nil 只能赋值给指针、chan、func、interface、map 或 slice 类型的变量。
  强调下 D 选项的 error 类型，它是一种内置接口类型，
```go
type error interface {
    Error() string
}
```

## 7 关于init函数，下面说法正确的是()
    A. 一个包中，可以包含多个 init 函数；
    B. 程序编译时，先执行依赖包的 init 函数，再执行 main 包内的 init 函数；
    C. main 包中，不能有 init 函数；
    D. init 函数可以被其他函数调用；
    
解析：AB  
关于 init() 函数有几个需要注意的地方：
- init() 函数是用于程序执行前做包的初始化的函数，比如初始化包里的变量等;
- 一个包可以出线多个 init() 函数,一个源文件也可以包含多个 init() 函数；
- 同一个包中多个 init() 函数的执行顺序没有明确定义，但是不同包的init函数是根据包导入的依赖关系决定的（看下图）;
- init() 函数在代码中不能被显示调用、不能被引用（赋值给函数变量），否则出现编译错误;
- 一个包被引用多次，如 A import B,C import B,A import C，B 被引用多次，但 B 包只会初始化一次；
- 引入包，不可出现死循坏。即 A import B,B import A，这种情况编译失败；
  
## 8 下面这段代码输出什么以及原因？
```go
func hello() []string {
    return nil
}
func main() {
    h := hello
    if h == nil {
        fmt.Println("nil")
    }else{
        fmt.Println("not nil")
    }
}


```
A. nil
B. not nil
C. compilation error  

解析:// not nil
这道题目里面，是将 hello() 赋值给变量 h，
而不是函数的返回值，所以输出 not nil。


## 下面这段代码能否编译通过？如果可以，输出什么？
```go
func GetValue() int {
    return 1
}
func main() {
    i := GetValue()
    switch i.(type) {
    case int:
        println("int")
    case string:
        println("string")
    case interface{}:
        println("interface")
    default:
        println("unknown")
    }
}

/**
# command-line-arguments
.\hello.go:8:5: cannot type switch on non-interface value i (type int)
*/
```
解析：类型选择，类型选择的语法形如：i.(type)，其中 i 是接口，type 是固定关键字，
需要注意的是，只有接口类型才可以使用类型选择
