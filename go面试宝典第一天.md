### 1<font color="red">下面代码输出内容为</font>
考察点  延迟调用 defer的使用 
```go 

package main

import "fmt"

func main() {
   defer_call()
}

func defer_call() {
   defer func() { fmt.Println("打印前") }()
   defer func() { fmt.Println("打印中") }()
   defer func() { fmt.Println("打印后") }()
   panic("出发异常")
}


```
运行结果为
![](images/screenshot_1569728754250.png)

解析  ：
1.  defer语句只能出现在函数或方法的内部。
2. 多个defer语句会以LIFO(后进先出)有点类似栈的方式执行。
参考解析：defer 的执行顺序是后进先出。当出现 panic 语句的时候，会先按照 defer 的后进先出的顺序执行，最后才会执行panic。

### 2 下面这段代码输出什么，说明原因。
```go 
package main

import "fmt"

func main(){
   slice := []int{0,1,2,3}
   m := make(map[int]*int)

   for key,val := range slice {
      m[key] = &val
   }

   for k,v := range m {
      fmt.Println(k,"--->",*v)
   }
}

```
![下面这段代码输出什么，说明原因。](images/screenshot_1569733500750.png)
参考解析：这是新手常会犯的错误写法，for range 循环的时候会**创建每个元素的副本，而不是元素的引用**，所以 m\[key\] = &val 取的都是变量 val 的地址，所以最后 map 中的所有元素的值都是变量 val 的地址，因为最后 val 被赋值为3，所有输出都是3.
正常写法
```go 
~~~
package main

import "fmt"

func main(){
   slice := []int{0,1,2,3}
   m := make(map[int]*int)

   for key,val := range slice {
      value := val
      m[key] = &value
   }
~~~
```

### 3  下面两段代码输出什么。
```go
//1.
func main() {
   s := make([]int, 5)
   s = append(s, 1, 2, 3)
   fmt.Println(s)
}

// 2
func main() {
   s := make([]int, 0)
   s = append(s, 1, 2, 3, 4)
   fmt.Println(s)
}

```
>   [00000123\]  
> [1234\]


参考解析：这道题考的是使用 append 向 slice 添加元素，第一段代码常见的错误是 \[1 2 3\]，需要注意。

### 4下面这段代码有什么缺陷
```go

func funcMui(x,y int)(sum int,error){  
     return x+y,nil  
     }

 ```
参考答案：第二个返回值没有命名。
参考解析：
在函数有多个返回值时，只要有一个返回值有命名，其他的也必须命名。如果有多个返回值必须加上括号()；如果只有一个返回值且命名也必须加上括号()。
这里的第一个返回值有命名 sum，第二个没有命名，所以错误。  

### 5 new() 与make()的区别
new(T) 和 make(T,args) 是 Go 语言内建函数，用来分配内存，但适用的类型不同。
new(T) 会为 T 类型的新值分配已置零的内存空间，并返回地址（指针），即类型为 *T 的值。换句话说就是，返回一个指针，该指针指向新分配的、类型为 T 的零值。适用于值类型，如数组、结构体等。
make(T,args) 返回初始化之后的 T 类型的值，这个值并不是 T 类型的零值，也不是指针 *T，是经过初始化之后的 T 的引用。make() 只适用于 slice、map 和 channel.
补充：make只能创建slice、map和channel，并且返回一个有初始值(非零)。
