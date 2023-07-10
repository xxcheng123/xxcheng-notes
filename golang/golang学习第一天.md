- ### 四种变量声明方式

  - 声明一个变量，不给初始值，则默认为0

  - 声明一个变量，初始化一个值

  - 声明一个变量，在初始化时省略数据类型，通知值自动匹配数据类型

  - 声明一个命令，省略`var`关键字和数据类型，使用冒号代替，自动匹配数据类型

    **`:=`只能在函数体内声明变量，不支持全局**

  ```go
  func main() {
  	//方法一
  	var a int
  	fmt.Printf("a=%d,type is [%T]\n", a, a)
  
  	//方法二
  	var b int = 99
  	fmt.Printf("b=%d,type is [%T]\n", b, b)
  
  	//方法三
  	var c = 88
  	fmt.Printf("c=%d,type is [%T]\n", c, c)
  	var c2 = "Today is first day."
  	fmt.Printf("c2=%s,type is [%T]\n", c2, c2)
  
  	//方法四
  	d := 77
  	d2 := "今天是一个好天气"
  	fmt.Printf("d=%d,type is [%T]\n", d, d)
  	fmt.Printf("d2=%s,type is [%T]\n", d2, d2)
  }
  ```

  ![image-20230622154309381](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/22/ec54a5394cb62c0dc4181bb4a5e5a6f0.png)

  - 多变量写法

    ```go
    //多变量写法
    //单行写法
    var x1, x2 int = 11, 22
    var x3, x4 = 33, true
    fmt.Printf("x1=%d,x2=%d,x3=%d,x4=%t", x1, x2, x3, x4)
    //多行写法
    var (
        y1 int    = 44
        y2 string = "China"
        y3        = true
    )
    fmt.Printf("y1=%d,y2=%s,y3=%t", y1, y2, y3)
    ```

    ![image-20230622155339742](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/22/faa6e73dccc1d70674b6909badcf0b42.png)

- ### `const`和`iota`

  用于定义常量、`iota`用于在枚举时代替公式，初始值为0，依次+1

  `iota`只能和`const`配合使用

  ```go
  func main() {
  	const (
  		AA = 11
  		BB
  		CC
  		DD
  	)
  	const (
  		XX = iota*2 + 5
  		YY
  		ZZ
  		WW = 100
  		QQ = iota + 99
  	)
  	fmt.Printf("AA=%d,BB=%d,CC=%d,DD=%d\n", AA, BB, CC, DD)
  	fmt.Printf("XX=%d,YY=%d,ZZ=%d,WW=%d,QQ=%d", XX, YY, ZZ, WW, QQ)
  }
  ```

  ![image-20230622160654355](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/22/0bed31522ea2a464c3707e410c691697.png)

- ### 函数的返回值的4种方式

  - 返回单个返回值
  - 返回多个返回值，匿名的方式
  - 返回多个返回值，有形参的方法
  - 返回多个返回值，返回值类型的相同的，可以省略类型定义，仅保留最后一个

  ```go
  package main
  
  import "fmt"
  
  // 情况一：单个返回值
  func fn1(a int, b int) int {
  	return a + b
  }
  
  // 情况二：多个返回值，匿名的
  func fn2(a int, b int) (int, int) {
  	a += 10
  	b += 20
  	return a, b
  }
  
  // 情况三：多个返回值，有形参
  func fn3(a int, b int) (x int, y int) {
  	x = a + 20
  	y = b + 30
  
  	return
  }
  
  // 情况四：多个返回值，有形参，且形参类型相同
  func fn4(a int, b int) (x, y int, z string) {
  	x = a + 20
  	y = b + 30
  	z = "China"
  	return
  }
  
  func main() {
  	var a, b = 8, 9
  	var ret1, ret2 int
  	var ret3 string
  	ret1 = fn1(a, b)
  	fmt.Printf("fn1,ret1=%d\n", ret1)
  	ret1, ret2 = fn2(a, b)
  	fmt.Printf("fn2,ret1=%d,ret2=%d\n", ret1, ret2)
  	ret1, ret2 = fn3(a, b)
  	fmt.Printf("fn3,ret1=%d,ret2=%d\n", ret1, ret2)
  	ret1, ret2, ret3 = fn4(a, b)
  	fmt.Printf("fn3,ret1=%d,ret2=%d,ret3=%s", ret1, ret2, ret3)
  }
  ```

  ![image-20230622162853200](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/22/f06331cb5e3517d339e6eb63fccb7845.png)

- ### `import`和`init`导包

  本地被导包的模块，必须在`GOPATH/src`路径下的，`init`函数在导包时运行，用于加载配置文件等等。函数首字母大写表示可被外部运行调用。

  *test.go*

  ```go
  package main
  
  import (
  	"my_package/lib1"
  	"my_package/lib2"
  )
  
  func main() {
  	lib1.SayMsg("哈哈哈")
  	lib2.SayMsg2("哈哈哈")
  }
  ```

  */lib1/lib1.go*

  ```go
  package lib1
  
  import "fmt"
  
  func SayMsg(msg string) {
  	fmt.Printf("lib:%s\n", msg)
  }
  
  func init() {
  	fmt.Println("lib1 init is loading...")
  }
  
  func main() {
  	fmt.Println("lib1 main is loading...")
  }
  ```

  */lib2/main.go*

  ```go
  package lib2
  
  import "fmt"
  
  func SayMsg2(msg string) {
  	fmt.Printf("lib2:%s\n", msg)
  }
  
  func init() {
  	fmt.Println("lib2 init is loading...")
  }
  
  func main() {
  	fmt.Println("lib1 main main is loading...")
  }
  ```

  ![image-20230622174123431](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/22/c6d5f181167de21aebd048dc8054433c.png)

  导包的时候可以给包取别名，或者使用匿名别名，或者使用`.`把包内的所有函数导入到当前。

  ```go
  package main
  
  import (
  	. "my_package/lib1"
  	_ "my_package/lib1"
  	mylib2 "my_package/lib2"
  )
  
  func main() {
  	SayMsg("www哈哈哈")
  	mylib2.SayMsg2("哈哈哈")
  }
  ```

  ![image-20230622174750498](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/22/993eb4c4d3e5f2f70c9df1b55ab7a569.png)