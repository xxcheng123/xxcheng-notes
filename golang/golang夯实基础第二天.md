- ### `	init`函数

  - #### 功能

    在程序执行之前（调用`main`函数之前，做一些初始化的操作）

  - #### 特点

    - 每个包可以可以有多个`init`函数；

      *a.go*

      ```go
      package main
      
      func init() {
      	fmt.Println("我是来自a.go的init函数")
      }
      ```

      *b.go*

      ```go
      package main
      
      func init() {
      	fmt.Println("我是来自b.go的init函数")
      }
      ```

    - 每个源文件可以有多个`init`函数；

      ```go
      package main
      
      func init() {
      	fmt.Println("我是init函数A")
      }
      
      func init() {
      	fmt.Println("我是init函数B")
      }
      
      func init() {
      	fmt.Println("我是init函数C")
      }
      ```

    - 同一个包，`init`函数的执行顺序是官方没有说明；

    - 总体执行顺序：**变量初始化->init()->main()**

    - 不能被其他函数调用

      *会显示函数undefined*

  - #### 执行顺序

    - 对同一个go文件的`init()`调用顺序是从上到下的。
    - 对同一个package中不同文件是按文件名字符串比较“从小到大”顺序调用各文件中的`init()`函数。
    - 对于不同的`package`，如果不相互依赖的话，按照main包中"先`import`的后调用"的顺序调用其包中的`init()`，如果`package`存在依赖，则先调用最早被依赖的`package`中的`init()`，最后调用`main`函数。
    - 如果`init`函数中使用了`println()`或者`print()`你会发现在执行过程中这两个不会按照你想象中的顺序执行。这两个函数官方只推荐在测试环境中使用，对于正式环境不要使用。

- ### 变量初始化

  `golang`的变量初始化不是从上到下的初始化的，而是先从没有依赖其他变量的变量开始初始化的。

  - #### 案例一

    ```go
    package main
    
    import "fmt"
    
    var (
    	a = c + 1
    	b = a + 666
    	c = getC()
    )
    
    func getC() int {
    	return 9
    }
    
    func main() {
    	fmt.Println("a:", a, ",b:", b, ",c:", c)
    }
    ```

    ![image-20230702082413731](https://cdn-static.xxcheng.cn/static/blog/images/2023/07/02/fbd2f3ee8ada0250cf3b35f862bb8713.png)

  - #### 案例二

    *a.go*

    ```go
    package deep
    
    import "fmt"
    
    var (
    	X int = Y + 9
    	Y int = Z + 66
    )
    
    func init() {
    	fmt.Println("我是a.go的init函数")
    }
    ```

    *b.go*

    ```go
    package deep
    
    import "fmt"
    
    var (
    	Z int = GetZ()
    )
    
    func init() {
    	fmt.Println("我是b.go的init函数")
    }
    ```

    *c.go*

    ```go
    package deep
    
    import "fmt"
    
    func GetZ() int {
    	fmt.Println("我是c.go的GetZ函数")
    	return 3
    }
    
    func init() {
    	fmt.Println("我是c.go的init函数")
    }
    ```

    *d.go*

    ```go
    package deep
    
    import "fmt"
    
    var (
    	W int = 666
    )
    
    func init() {
    	fmt.Println("我是d.go的init函数")
    }
    ```

    *print.go*

    ```go
    package deep
    
    import "fmt"
    
    func init() {
    	fmt.Println("我是print.go的init函数")
    }
    
    func MyPrint() {
    	fmt.Println("x:", X, ",y:", Y, ",z:", Z, ",w:", W)
    }
    ```

    *deep_test.go*

    ```go
    package deep
    
    import "testing"
    
    func TestRun(t *testing.T) {
    	MyPrint()
    }
    ```

    ![image-20230702093924798](https://cdn-static.xxcheng.cn/static/blog/images/2023/07/02/e437f18aaaed99a3d91c04567c86a5fb.png)

  - #### 案例三（失败案例）

    *x.go*

    ```go
    package deep
    
    import "fmt"
    
    var (
    	A = B + 1
    	B = C + 66
    )
    
    func init() {
    	fmt.Println("我是x.go的init函数")
    }
    ```

    *y.go*

    ```go
    package deep
    
    import "fmt"
    
    var (
    	C = GetC()
    )
    
    func GetC() int {
    	return A
    }
    func init() {
    	fmt.Println("我是y.go的init函数")
    }
    
    ```

    *deep_test.go*

    ```go
    package deep
    
    import (
    	"fmt"
    	"testing"
    )
    
    func TestRun(t *testing.T) {
    	// MyPrint()
    	fmt.Println("A:", A, ",B:", B, ",C:", C)
    }
    ```

    ![image-20230702094418245](https://cdn-static.xxcheng.cn/static/blog/images/2023/07/02/fb079fb4043ab0e62581f2c7dcad066d.png)

- ### `main`函数

  Go程序的默认入口函数

- ### 命令

  ```sh
  # go
  Go is a tool for managing Go source code.
  
  Usage:
  
  	go <command> [arguments]
  
  The commands are:
  
  	bug         start a bug report
  	build       compile packages and dependencies
  	clean       remove object files and cached files
  	doc         show documentation for package or symbol
  	env         print Go environment information
  	fix         update packages to use new APIs
  	fmt         gofmt (reformat) package sources
  	generate    generate Go files by processing source
  	get         add dependencies to current module and install them
  	install     compile and install packages and dependencies
  	list        list packages or modules
  	mod         module maintenance
  	work        workspace maintenance
  	run         compile and run Go program
  	test        test packages
  	tool        run specified go tool
  	version     print Go version
  	vet         report likely mistakes in packages
  
  Use "go help <command>" for more information about a command.
  
  Additional help topics:
  
  	buildconstraint build constraints
  	buildmode       build modes
  	c               calling between Go and C
  	cache           build and test caching
  	environment     environment variables
  	filetype        file types
  	go.mod          the go.mod file
  	gopath          GOPATH environment variable
  	gopath-get      legacy GOPATH go get
  	goproxy         module proxy protocol
  	importpath      import path syntax
  	modules         modules, module versions, and more
  	module-get      module-aware go get
  	module-auth     module authentication using go.sum
  	packages        package lists and patterns
  	private         configuration for downloading non-public code
  	testflag        testing flags
  	testfunc        testing functions
  	vcs             controlling version control with GOVCS
  
  Use "go help <topic>" for more information about that topic.
  ```

  - `go env`

    查看当前环境信息

    `go env -w Name=Value`修改配置信息

  - `go run`

    编译并且运行程序

    `go run main.go`

  - `go build`

    编译

    `go build -o server server.go main.go`

  - `go get`

    下载或者更新依赖

  - `go install`

    下载并且安装依赖

  #### `go get`和`go install`的区别

  |   **命令**   |              **功能**              |                  **下载位置**                  |    **生成文件位置**    |                         **编译效果**                         |                           应用场景                           |
  | :----------: | :--------------------------------: | :--------------------------------------------: | :--------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
  |   `go get`   | 从远程代码库中下载或更新 Go 代码包 |                  $GOPATH/src                   |      $GOPATH/bin       | 可能由于版本或编译环境的不同，生成的可执行文件无法运行或出现其他问题 | **需要快速下载和安装指定版本的 Go 代码包**，如果生成的可执行文件无法运行，则需要使用 go install 命令重新编译 |
  | `go install` |        编译并安装 Go 代码包        | 无需下载自身代码，$GOPATH/src 用于依赖包的查找 | GOPATH/bin或GOPATH/pkg | 在当前环境下重新编译源代码并生成可执行文件或库，能够更加稳定地运行代码包 |       生成可执行文件或库，能够**更加稳定地运行代码包**       |

  可以认为 `go get` 命令包含了 `go install` 命令的部分功能，但是在一些特殊情况下，还是需要使用 `go install` 命令来进行更灵活的编译和安装操作。

  使用`go install`之前需要先`go get`

  ![image-20230702102157336](https://cdn-static.xxcheng.cn/static/blog/images/2023/07/02/36ebc809ead56cf53edb76d9a08874db.png)

- ### 下划线

  `_`匿名变量，用于忽略结果，无法访问，不占用命名空间，不会分配内存

  比如当函数返回值有两个，而其中一个没有作用，就可以使用这个

  ```go
  func AllAddOne(a, b int) (x, y int) {
  	x = a + 1
  	y = b + 1
  	return
  }
  func Test_(t *testing.T) {
  	_, res := AllAddOne(88, 99)
  	fmt.Println(res)
  }
  ```

  ![image-20230702103013573](https://cdn-static.xxcheng.cn/static/blog/images/2023/07/02/eb41827242232204de73076b9b4d4234.png)

  `_`还可以用于一种特殊用途，当只想让某个包的`init`运行，而不想让整个包都导入，就可以使用匿名变量，`import _ "hello"`

- ### 变量

  - #### 基本格式

    ```go
    var 变量名 变量类型=表达式
    ```

  - #### 批量声明

    ```go
    var (
    	a int
        b string
        c bool
    )
    ```

    ```go
    var a ,b int =456,123
    var c,d="abc",123
    ```

  - #### 类型推导

    ```go
    var a=123
    var c,d="abc",123
    ```

  - #### 短变量声明

    使用`:=`这样的格式进行声明，省略`var`，自带类型推导

    ```go
    a:="abc"
    b,c:="ddd",123
    ```

    **`:=`**只能用于函数内部

    ![image-20230702110652208](https://cdn-static.xxcheng.cn/static/blog/images/2023/07/02/3c298a1f8af26d2217b8bbc0305a3796.png)

  - #### 匿名变量

    就是上面提到的下划线

- ### 常量

  - #### 特点

    - 无法修改，固定不变的值
    - 使用`const`关键字定义
    - 在初始化定义时就必须赋值

  - #### 批量声明

    ```go
    const (
    	A = 100
    	B = 101
    	C = 102
    )
    const (
    	X = 200
    	Y
    	Z
    )
    ```

    ![image-20230702111107563](https://cdn-static.xxcheng.cn/static/blog/images/2023/07/02/8e90b22675947a032f67702d4bb3e8f5.png)

    这样子有规律的常量一个一个声明影响效率，Go为多常量声明提供了一个计数器`iota`（只能用于`const`），让`iota`关键字相当于一个变量，然后使用表达式进行赋值，第一个值为0

    ```go
    const (
    	A = 100 + iota
    	B
    	C
    )
    const (
    	X = iota*2 + 10
    	Y
    	Z = 111//这里被打断了，后面的M也是111
    	M
    	N = iota
    	O
    	_
    	_
    	P
    )
    
    func TestConst(t *testing.T) {
    	fmt.Println("A:", A, ",B:", B, ",C:", C, ",\nX:", X, ",Y:", Y, ",Z:", Z, ",\nM:", M, ",N:", N, ",O:", O, ",P:", P)
    }
    ```

    ![image-20230702111729122](https://cdn-static.xxcheng.cn/static/blog/images/2023/07/02/3ebd9eb66ec46f92971fae979d1f2c4e.png)

    使用单行多变量定义时，相同一行`iota`相同

    ```go
    const (
    	A1, A2 = iota*2 + 1, iota*3 + 2
    	B1, B2
    	C1, C2
    )
    
    func TestConst(t *testing.T) {
    	fmt.Println("A1:", A1, "A2:", A2, "\n", "B1:", B1, "B2:", B2, "\n", "C1:", C1, "C2:", C2)
    }
    ```

    ![image-20230702112252640](https://cdn-static.xxcheng.cn/static/blog/images/2023/07/02/2f7653b3dc008017ae498dacd6b2d758.png)

- ### 学习中遇到的问题及解决

  - `VScode`提示`gopls was not able to find modules in your workspace.`错误

    - ##### 解决方法

      1. 将workspace与项目路径保持一致

      2. 使用go work指明纳入工作区的的module

         在项目路径根路径下新建一个`go.work`文件，将项目路径写入`use`

         ![image-20230702083408970](https://cdn-static.xxcheng.cn/static/blog/images/2023/07/02/aa1a82c5bca2439c31edcaf3fb335279.png)

    - ##### 参考

      - [golang vscode环境报错gopls was not able to find modules in your workspace的解决方式](https://blog.csdn.net/u011285281/article/details/131261615)

- ### 参考链接

  - [golang变量的初始化](https://mp.weixin.qq.com/s/PGDzMaYznZVuDiO6V-zYDw)
  - [Go 模块--开始使用 Go Modules](https://juejin.cn/post/6844904038127894536)
  - [Init函数和main函数](https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/Init%E5%87%BD%E6%95%B0%E5%92%8Cmain%E5%87%BD%E6%95%B0.html)