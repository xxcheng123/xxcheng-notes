# 面向对象

## 匿名字段

见[匿名嵌套结构体](https://www.xxcheng.cn/754.html#cl-32)

## 接口 `interface`

定义对象方法的规范，只定义规范，具体实现由对应的对象实现，在 `Go1.18` 之前，接口只定义方法，只有方法集（`method set`），在 `Go1.18` 版本出现了泛型，接口同时也转变为了类型集（`type Set`），可以用于定义类型。

在这里先按照 `Go1.18` 之前的版本进行学习归纳，在后面再对泛型进行学习之后，再学习归纳接口对类型的定义规范。

### 定义

- 接口是一个或多个方法定义的集合；
- 任何类型只要实现了接口定义的全部的方法集，那么它就实现了该接口；
- 实现接口的方法指的是方法名相同，接收参数、数据类型相同，返回值参数、数据类型相同；
- 任何数据类型都实现了 `interface{}` 这个 `空接口`；
- `Go` 的接口在命名时，一般会在单词后面添加 `er`，比如 `Writer`；
- 一个类型可以实现一个或多个接口，多个类型可以实现同一个接口；
- 接口和接口之间可以实现嵌套；

### 基本格式

```go
 type 接口类型名 interface{
     方法名1( 参数列表1 ) 返回值列表1
     方法名2( 参数列表2 ) 返回值列表2
     // …
 }
```

```go
type Person interface {
	Walk()
	Say()
}
```

### 基本示例

*a.go*

```go
package day08

import "fmt"

type Person interface {
	Walk()
	Say()
}
type Student struct {
	Name string
	Age  int
}

func (s Student) Walk() {
	fmt.Println("学生", s.Name, "正在走路...")
}
func (s Student) Say() {
	fmt.Println("学生", s.Name, "正在说话...")
}

type Man struct {
	Name string
	Age  int
}

func (m Man) Walk() {
	fmt.Println("男人", m.Name, "正在走路...")
}
```

运行 `Test_01` 单元测试

```go
func Test_01(t *testing.T) {
	s := Student{"xxcheng", 18}
	var a Person = s
	a.Walk()
}
```

```shell
[root@CentOS day08]# go test -run Test_01
学生 xxcheng 正在走路...
PASS
ok      day08   0.002s
```

`Student` 实现了 `Person` 接口定义的两个方法，所以实现了 `Person` 接口，所以可以将定义好的变量 `s` 赋值给类型为 `Person` 的变量 `a` ，可以运行成功

运行 `Test_02` 单元测试

```go
func Test_02(t *testing.T) {
	m := Man{"xxcheng", 18}
	var a Person = m
	a.Walk()
}
```

```shell
[root@CentOS day08]# go test -run Test_02
# day08 [day08.test]
./a_test.go:12:17: cannot use m (variable of type Man) as Person value in variable declaration: Man does not implement Person (missing method Say)
FAIL    day08 [build failed]
```

`Man` 只实现了 `Person` 接口的 `Walk` 方法，没有 `Person` 接口，所以无法将定义好的变量 `m` 赋值给类型为 `Person` 的变量 `a` ，不能运行成功

### 接收者类型不同对接口实现的区别

先定义一个 `Run` 接口和一个 `Dog` 结构体

```go
type Run interface {
	Run()
}
type Dog struct {
}
```

#### 值类型接收者

```go
func (d Dog) Run() {
	fmt.Println("狗跑...")
}
```

```go
func Test_03(t *testing.T) {
	var r Run
	d := Dog{}
	d2 := &Dog{}
	fmt.Printf("%T,%T\n", d, d2)
	r = d
	fmt.Printf("%T\n", r)
	r = d2
	fmt.Printf("%T\n", r)
}
```

```shell
[root@CentOS day08]# go test -run Test_03
day08.Dog,*day08.Dog
day08.Dog
*day08.Dog
PASS
ok      day08   0.002s
```

接收者为值类型时，实现 `Run` 接口的是 `Dog` 类型和 `*Dog` 类型

#### 指针类型接收者

```go
func (d *Dog) Run() {
	fmt.Println("狗跑...")
}
```

```shell
[root@CentOS day08]# go test -run Test_03
# day08 [day08.test]
./a_test.go:34:6: cannot use d (variable of type Dog) as Run value in assignment: Dog does not implement Run (method Run has pointer receiver)
FAIL    day08 [build failed]
```

![image-20230708151405160](https://cdn-static.xxcheng.cn/static/blog/images/2023/07/08/0896ab730bd6d84c6f1c405982453c88.png)

接收者为指针类型时，实现 `Run` 接口的只有 `*Dog` 类型

可以参考[方法-方法集](https://www.xxcheng.cn/757.html#cl-47)

### 一个类型对应多个接口

```go
type A interface {
	a()
}
type B interface {
	b()
}

type C struct {
}

func (c C) a() {
	fmt.Println("aaa...")
}
func (c C) b() {
	fmt.Println("bbb...")
}

func Test_04(t *testing.T) {
	c := C{}
	var x A = c
	var y B = c
	x.a()
	y.b()
}
```

```shell
[root@CentOS day08]# go test -run Test_04
aaa...
bbb...
PASS
ok      day08   0.003s
```

### 多个类型对应一个接口

```go
type Q interface {
	q()
}
type X struct{}
type Y struct{}

func (x X) q() {
	fmt.Println("我是X的q")
}
func (y Y) q() {
	fmt.Println("我是Y的q")
}
func Test_05(t *testing.T) {
	var x Q = X{}
	var y Q = Y{}
	x.q()
	y.q()
}
```

```shell
[root@CentOS day08]# go test -run Test_05
我是X的q
我是Y的q
PASS
ok      day08   0.002s
```

### 接口嵌套

```go
type AA interface {
	aa()
}
type BB interface {
	AA
	bb()
}
type CC struct{}

func (c CC) aa() {
	fmt.Println("aa")
}
func (c CC) bb() {
	fmt.Println("bb")
}

func Test_06(t *testing.T) {
	c := CC{}
	var a AA = c
	var b BB = c
	a.aa()
	b.aa()
	b.bb()
}
```

```shell
[root@CentOS day08]# go test -run Test_06
aa
aa
bb
PASS
ok      day08   0.002s
```

### 空接口

没有定义任何的方法，所以任何类型都实现了空接口，所以任何类型都可以定义为空接口

```go
func Test_07(t *testing.T) {
	var a interface{} = 1
	var b interface{} = "abc"
	fmt.Printf("%T,%v\n", a, a)
	fmt.Printf("%T,%v\n", b, b)
}
```

```shell
[root@CentOS day08]# go test -run Test_07
int,1
string,abc
PASS
ok      day08   0.002s
```

#### 应用场景

##### 接收参数

```go
func Fn(v interface{}) {
	fmt.Printf("%T,%v\n", v, v)
}
func Test_08(t *testing.T) {
	Fn("ABC")
	Fn(123)
}
```

```shell
[root@CentOS day08]# go test -run Test_08
string,ABC
int,123
PASS
ok      day08   0.002s
```

##### 值为任意类型的键值对 `map` 

```go
func Test_09(t *testing.T) {
	m := map[string]interface{}{
		"name":   "xxcheng",
		"sex":    "男",
		"age":    18,
		"weight": 59.9,
	}
	for key, value := range m {
		fmt.Printf("key:%s\t value:%#v\t value-type:%T\n", key, value, value)
	}
}
```

```shell
[root@CentOS day08]# go test -run Test_09
key:age  value:18        value-type:int
key:weight       value:59.9      value-type:float64
key:name         value:"xxcheng"         value-type:string
key:sex  value:"男"      value-type:string
PASS
ok      day08   0.002s
```