### 1.下面这段代码能否通过编译，不能的话原因是什么；如果能，输出什么。
```go
func main(){
	list := new([]int)
	list = append(list,1)
	fmt.Println(list)
}
```
参考答案及解析：不能通过编译，new([]int) 之后的 list 是一个 *[]int 类型的指针，
不能对指针执行 append 操作。可以使用 make() 初始化之后再用。
同样的，map 和 channel 建议使用 make() 或字面量的方式初始化，不要用 new() 。

### 2.下面这段代码能否通过编译，如果可以，输出什么？
```go
func main(){
	s1 := []int{1,2,3}
	s2 := []int{4,5}
	s1 = append(s1, s2)
	fmt.Println(s1)
}
//运行结果为：
/*
# command-line-arguments
.\main.go:8:13: cannot use s2 (type []int) as type int in append
*/
```
参考答案及解析：不能通过编译。
append() 的第二个参数不能直接使用 slice，需使用 … 操作符，将一个切片追加到另一个切片上：append(s1,s2…)。
或者直接跟上元素，形如：append(s1,1,2,3)。

### 3.下面这段代码能否通过编译，如果可以，输出什么？

```go
var (
	size := 1024
	max_size = size*2
)
func main(){
	fmt.Println(size,max_size)
}
//运行结果
/**
# command-line-arguments
.\main.go:6:7: syntax error: unexpected :=, expecting type
 */
```
解析： var 和 := 冲突了    有一个就可以
参考答案及解析：不能通过编译。
这道题的主要知识点是变量声明的简短模式，形如：x := 100。但这种声明方式有限制：
1. 必须使用显示初始化；
2. 不能提供数据类型，编译器会自动推导；
3.只能在函数内部使用简短模式；


## 4下面这段代码能否通过编译？不能的话，原因是什么？如果通过，输出什么？
```go
func main() {
	sn1 := struct {
		age  int
		name string
	}{age:11,name:"qq"}
	sn2 := struct{
		age int
		name string
	}{age:11,name:"qq"}
	if sn1 == sn2{
		fmt.Println("sn1==sn2")
	}
	sm1 := struct{
		age int
		m map[string]string
	}{age:11,m:map[string]string{"a":"1"}}
	sm2 := struct{
		age int
		m map[string]string
	}{age:11,m:map[string]string{"a":"1"}}
	if sm1 == sm2 {
		fmt.Println("sm1==sm2")
	}
}
/**
第一个是相等的 

第二个报错结果为
# command-line-arguments
.\main.go:25:9: invalid operation: sm1 == sm2 (struct containing map[string]string cannot be compared)
 */
```
参考答案及解析：编译不通过 invalid operation: sm1 == sm2
这道题目考的是结构体的比较，有几个需要注意的地方：

1. 结构体只能比较是否相等，但是不能比较大小。
2. 相同类型的结构体才能够进行比较，结构体是否相同不但与属性类型有关，还与属性顺序相关，sn3 与 sn1 就是不同的结构体；
```go
 sn3:= struct {
        name string
        age  int
    }{age:11,name:"qq"}
```
3. 如果 struct 的所有成员都可以比较，则该 struct 就可以通过 == 或 != 进行比较是否相等，比较时逐个项进行比较，如果每一项都相等，则两个结构体才相等，否则不相等；

那什么是可比较的呢，
<font color="red">常见的有 bool、数值型、字符、指针、数组等，</font>
<font color="red">像切片、map、函数等是不能比较的。 具体可以参考 Go 说明文档。http://docs.studygolang.com/ref/spec#Comparison_operators</font>
