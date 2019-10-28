
## 1.下面这段代码正确的输出是什么？
```go
func f() {
    defer fmt.Println("D")
    fmt.Println("F")
}

func main() {
    f()
    fmt.Println("M")
}
```
    A. F M D
    B. D F M
    C. F D M
    
解析: C  
被调用函数里的 defer 语句在返回之前就会被执行，所以输出顺序是 F D M。


##   2.下面代码输出什么？
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
 
     person = &Person{29}
 }
```
// 29
   28
   28
   
解析:  
参考答案及解析：29 28 28。这道题在第 19 天题目的基础上做了一点点小改动，前一题最后一行代码 person.age = 29 是修改引用对象的成员 age，这题最后一行代码 person = &Person{29} 是修改引用对象本身，来看看有什么区别。

1处.person.age 这一行代码跟之前含义是一样的，此时是将 28 当做 defer 函数的参数，会把 28 缓存在栈中，等到最后执行该 defer 语句的时候取出，即输出 28；

2处.defer 缓存的是结构体 Person{28} 的地址，这个地址指向的结构体没有被改变，最后 defer 语句后面的函数执行的时候取出仍是 28；

3处.闭包引用，person 的值已经被改变，指向结构体 Person{29}，所以输出 29.

由于 defer 的执行顺序为先进后出，即 3 2 1，所以输出 29 28 28。
 
## 3.下面的两个切片声明中有什么区别？哪个更可取？
   A. var a []int
   B. a := []int{}

解析:
A 声明的是 nil 切片；B 声明的是长度和容量都为 0 的空切片。第一种切片声明不会分配内存，优先选择。

## 4.  A、B、C、D 哪些选项有语法错误？
```go
type S struct {
}

func f(x interface{}) {
}

func g(x *interface{}) {
}

func main() {
    s := S{}
    p := &s
    f(s) //A
    g(s) //B
    f(p) //C
    g(p) //D
}
```
解析:  B D
函数参数为 interface{} 时可以接收任何类型的参数，包括用户自定义类型等，
即使是接收指针类型也用 interface{}，而不是使用 *interface{}。

> 永远不要使用一个指针指向一个接口类型，因为它已经是一个指针。


## 5.下面 A、B 两处应该填入什么代码，才能确保顺利打印出结果？
```go

type S struct {
    m string
}

func f() *S {
    return __  //A
}

func main() {
    p := __    //B
    fmt.Println(p.m) //print "foo"
}   
```
解析:
    A. &S{"foo"} 
    B. *f() 或者 f() 
f() 函数返回参数是指针类型，所以可以用 & 取结构体的指针；
B 处，如果填 *f()，则 p 是 S 类型；
如果填 f()，则 p 是 *S 类型，不过都可以使用 p.m 取得结构体的成员。 