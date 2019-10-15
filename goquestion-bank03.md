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
## 6下面赋值正确的是()
A. var x = nil
B. var x interface{} = nil
C. var x string = nil
D. var x error = nil

B D


