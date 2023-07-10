- ### 结构体标签

  - 格式

    `` `key1:value1 key2:value2` ``
    
    放在结构体属性后面，使用反引号包裹，多个键值对使用空格分隔，value数据类型一般为string，可以使用反射读取出来
    
  - 练习
  
    ```go
    package main
    
    import (
    	"fmt"
    	"reflect"
    )
    
    type SimpleStudent struct {
    	Name string `title:"Student Name" desc:"学生姓名"`
    	Age  int    `title:"Student Age" desc:"学生年龄"`
    }
    
    func findTag(stu interface{}) {
    	elem := reflect.TypeOf(stu).Elem()
    	for i := 0; i < elem.NumField(); i++ {
    		tagTitle := elem.Field(i).Tag.Get("title")
    		tagDesc := elem.Field(i).Tag.Get("desc")
    		fmt.Println("title:", tagTitle, ",desc:"+tagDesc)
    	}
    }
    
    func main() {
    	stu := SimpleStudent{"Tom", 28}
    	findTag(&stu)
    }
    ```
  
    ![image-20230624140339771](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/24/33f04c021c8b65f96606e6ebd227edc5.png)
  
    现在有个疑惑，读取标签，是否必须使用`*interface{}`
    
    我按照下面这种写法就报错
    
    ```go
    package main
    
    import (
    	"fmt"
    	"reflect"
    )
    
    type SimpleStudent struct {
    	Name string `title:"Student Name" desc:"学生姓名"`
    	Age  int    `title:"Student Age" desc:"学生年龄"`
    }
    
    func main() {
    	stu := SimpleStudent{"Tom", 28}
    	elem := reflect.TypeOf(stu).Elem()
    	for i := 0; i < elem.NumField(); i++ {
    		tagTitle := elem.Field(i).Tag.Get("title")
    		tagDesc := elem.Field(i).Tag.Get("desc")
    		fmt.Println("title:", tagTitle, ",desc:"+tagDesc)
    	}
    }
    ```
    
    ```powershell
    panic: reflect: Elem of invalid type main.SimpleStudent
    
    goroutine 1 [running]:
    reflect.(*rtype).Elem(0xa7e300?)
            C:/Program Files/Go/src/reflect/type.go:979 +0x134
    main.main()
            D:/resource/study-resource/golangstudy/firstGolang/tag_study.go:15 +0xb3
    exit status 2
    ```
  
- ### 结构体标签在json中的应用

  - #### 步骤

    - 导入`encoding/json`库
    - 添加一个json为key的标签
    - 使用`json.Marshal`方法生成json
    - 使用`json.UnMarshal`方法将json转回为结构体

  - #### 练习

    **特别注意属性首字母大小写习惯，写成小写就无法读取了**

    ```go
    package main
    
    import (
    	"encoding/json"
    	"fmt"
    )
    
    type Computer struct {
    	Name  string  `json:"name" desc:"电脑名称"`
    	Price float32 `json:"price" desc:"电脑价格"`
    }
    
    func main() {
    	c := Computer{"拯救者Y7000", 9999.99}
    	jsonStr, err := json.Marshal(c)
    	if err != nil {
    		fmt.Println("转换错误")
    		return
    	}
    	fmt.Printf("%s\n", jsonStr)
    
    	var c2 Computer
    	json.Unmarshal(jsonStr, &c2)
    	fmt.Printf("type:%T,%v", c2, c2)
    }
    ```

    ```powershell
    {"name":"拯救者Y7000","price":9999.99}
    type:main.Computer,{拯救者Y7000 9999.99}
    ```

- ### goroutine

  - 介绍

    > Goroutine 是 Go 语言中的一种轻量级线程（lightweight thread）实现，它可以在程序中并发地执行函数或方法。与传统的线程相比，Goroutine 的创建和销毁成本非常低，并且可以高效地在多个处理器上并发执行。
    >
    > 以下是 Goroutine 的一些特点和用途：
    >
    > 1. 轻量级：Goroutine 的创建和销毁开销很小，可以创建大量的 Goroutine 而不会导致系统资源的浪费。
    > 2. 并发执行：Goroutine 可以与其他 Goroutine 并发执行，它们之间通过调度器进行调度和切换。这使得在单个程序中可以同时进行多个任务的执行，提高了程序的并发性能。
    > 3. 并发通信：Goroutine 之间可以通过通道（channel）进行安全的数据传输和同步。通道提供了 Goroutine 之间进行数据交换的机制，有效地避免了资源竞争和数据访问冲突。
    > 4. 非阻塞式 IO：Goroutine 在进行 IO 操作时可以采用非阻塞式的方式，即当一个 Goroutine 遇到 IO 操作时，它可以切换到其他可执行的 Goroutine，而不需要等待 IO 操作完成。
    > 5. 并行计算：由于 Goroutine 能够高效地利用多个处理器，因此在需要进行计算密集型任务时，可以使用 Goroutine 实现并行计算，从而提高程序的性能。
    >
    > 使用 Goroutine 可以轻松实现并发和并行的程序设计，简化了并发编程的复杂性。在 Go 语言中，通过使用关键字 `go` 可以创建 Goroutine，例如：`go 函数名()`。这样创建的 Goroutine 将在后台并发执行指定的函数，而不会阻塞当前的 Goroutine。
    >
    > 需要注意的是，由于 Goroutine 的并发性质，正确地处理共享数据和避免竞态条件（race condition）非常重要，可以使用互斥锁（mutex）或通道来实现数据同步和保护共享资源的一致性。

  - 练习

    ```go
    package main
    
    import (
    	"fmt"
    	"time"
    )
    
    func simpleTask(id int) {
    	i := 0
    	for {
    		i++
    		fmt.Printf("simpleTask[%d]:%d\n", id, i)
    		time.Sleep(1 * time.Second)
    	}
    }
    
    func main() {
    	//新开一个go协程
        go simpleTask(0)
    	i := 0
    	for {
    		i++
    		fmt.Printf("main:%d\n", i)
    		time.Sleep(1 * time.Second)
    	}
    }
    ```

    ```go
    main:1
    simpleTask[0]:1
    simpleTask[0]:2
    main:2
    main:3
    simpleTask[0]:3
    simpleTask[0]:4
    main:4
    main:5
    simpleTask[0]:5
    simpleTask[0]:6
    main:6
    ....
    ```

    ```go
    package main
    
    import (
    	"fmt"
    	"time"
    )
    
    func main() {
    
    	func() {
    		fmt.Println("--------ok-------")
    
    	}()
    
    	go func() {
    		fmt.Println("--------go-------")
    	}()
    	for {
    		time.Sleep(1 * time.Second)
    	}
    }
    ```

    ![image-20230624152932256](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/24/0086a8dc29d3de40bdd9e34f1e49f034.png)

  - 匿名函数

    ```go
    //形参为空的匿名函数
    func (){
        //...
    }()
    
    func (a,b int)int{
        return a+b
    }(1,2)
    ```

  - 退出当前`goroutine`

    调用`runtime.Goexit()`方法

    ```go
    package main
    
    import (
    	"fmt"
    	"runtime"
    	"time"
    )
    
    func main() {
    
    	go func() {
    		i := 0
    		defer fmt.Println("defer执行了,其他goroutine退出了")
    		for {
    			i++
    			fmt.Println("来自其他goroutine,i=", i)
    			if i == 5 {
    				//退出当前goroutine
    				runtime.Goexit()
    			}
    		}
    	}()
    
    	fmt.Println("我是main，我执行了")
    
    	for {
    		time.Sleep(1 * time.Second)
    	}
    
    }
    ```

    ![image-20230624154208339](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/24/5e4d6df2a4e287bd1e491f0b960439df.png)

- ### channel

  - 介绍

    > 
    > 在 Go 语言中，通道（channel）是一种用于 Goroutine 之间进行通信和同步的机制。通道可以看作是 Goroutine 之间传递数据的管道，用于安全地传输数据和进行同步操作。
    >
    > 以下是通道的一些特点和用途：
    >
    > 1. 数据传输：通道提供了在 Goroutine 之间传递数据的机制。一个 Goroutine 可以将数据发送到通道，而另一个 Goroutine 可以从通道接收该数据。通道保证了数据的顺序传输，确保发送和接收操作按照发送的顺序进行。
    > 2. 同步操作：通道还可以用于 Goroutine 之间的同步。通过在通道上发送和接收数据，可以实现 Goroutine 的同步等待，以确保在需要的时候 Goroutine 可以正确地进行交互和协调。
    > 3. 阻塞特性：通道具有阻塞特性，即在发送和接收数据时，如果通道的缓冲区已满（发送阻塞）或为空（接收阻塞），相关的 Goroutine 将被阻塞，直到有可用的空间或数据。
    > 4. 安全性：通道提供了对并发访问共享数据的安全性。多个 Goroutine 可以同时访问同一个通道，但只能有一个 Goroutine 进行发送操作，另一个 Goroutine 进行接收操作，从而避免了数据竞争和访问冲突。
    > 5. 类型限制：通道可以指定特定的数据类型进行传输。这意味着在通道的声明时需要指定通道的元素类型，只能发送和接收该类型的数据。
    >
    > 在 Go 语言中，通过使用 `make()` 函数创建通道，例如：`ch := make(chan int)`，创建了一个用于传输整数类型数据的通道。发送数据到通道使用 `<-` 运算符，例如：`ch <- data`，接收数据使用 `<-` 运算符，例如：`data := <- ch`。
    >
    > 通道是 Go 语言中并发编程的重要组件，可以用于解决并发问题、实现协程间的协作和数据交换。

  - 格式

    ```go
    //创建
    //无缓冲
    c:=make(chan dataType)
    //有缓冲
    c2:=make(chan dataType,3)
    //发送
    c<-666
    //接收
    num:=<-c
    num,ok:=<-c
    ```

  - 第一次练习(无缓冲的channel)

    会有阻塞

    ![image-20230624160935706](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/24/c877b5e4df3b8ff32a4d76710c5d0b3f.png)

    ```go
    package main
    
    import (
    	"fmt"
    	"time"
    )
    
    func main() {
    	c := make(chan int)
    	go func() {
    		defer fmt.Println("goroutine结束")
    		fmt.Println("goroutine执行")
    		time.Sleep(1 * time.Second)
    		fmt.Println("goroutine，延迟1s")
    		c <- 888
    	}()
    
    	num := <-c
    	fmt.Println("收到channel:", num)
    	fmt.Println("main结束")
    }
    ```

    ![image-20230624160626228](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/24/e2162797aaf1beba6b33f8e0ce0e169e.png)

  - 第二次练习(有缓冲的channel)

    ![image-20230624161007534](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/24/298e06d845eea768b348e9203597a235.png)

    ```go 
    package main
    
    import (
    	"fmt"
    	"time"
    )
    
    func main() {
    	c := make(chan int, 3)
    
    	go func() {
    		defer fmt.Println("子进程结束")
    		for i := 0; i < 3; i++ {
    			c <- i * 100
    			fmt.Println("子进程正在运行,i=", i)
    		}
    
    	}()
    
    	time.Sleep(2 * time.Second)
    
    	for i := 0; i < 3; i++ {
    		num := <-c
    		fmt.Printf("num:%d\n", num)
    	}
    	time.Sleep(2 * time.Second)
    	fmt.Println("mian进程结束")
    }
    ```

    ![image-20230624162539360](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/24/5ba70f308d2055ae82d4ef5368ee6600.png)

    channel满出的情况

    ```go
    package main
    
    import (
    	"fmt"
    	"time"
    )
    
    func main() {
    	c := make(chan int, 3)
    
    	go func() {
    		defer fmt.Println("子进程结束")
    		for i := 0; i < 4; i++ {
    			c <- i * 100
    			fmt.Println("子进程正在运行,i=", i)
    		}
    
    	}()
    
    	time.Sleep(2 * time.Second)
    
    	for i := 0; i < 4; i++ {
    		num := <-c
    		fmt.Printf("num:%d\n", num)
    	}
    	time.Sleep(2 * time.Second)
    	fmt.Println("mian进程结束")
    }
    ```

    ![image-20230624162512026](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/24/d37c21cb715b27b9ad4722e1a92a84e5.png)

  - 关闭channel

    调用`close(channelElem)`方法关闭

    ![image-20230624163259693](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/24/ae24c29e474f60d46c8477d178689126.png)

    - 关闭channel，并且判断退出循环

      ```go
      package main
      
      import (
      	"fmt"
      	"time"
      )
      
      func main() {
      	c := make(chan int)
      
      	go func() {
      		defer fmt.Println("子进程结束")
      		for i := 0; i < 3; i++ {
      			c <- i * 100
      			fmt.Println("子进程正在运行,i=", i)
      		}
      		close(c)
      	}()
      
      	time.Sleep(2 * time.Second)
      
      	for {
      		num, ok := <-c
      		fmt.Println("ok:", ok)
      		if ok {
      			fmt.Printf("num:%d\n", num)
      		} else {
      			break
      		}
      	}
      
      	time.Sleep(2 * time.Second)
      	fmt.Println("mian进程结束")
      }
      ```

      ![image-20230624182555520](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/24/04b0c737938754d1335f836b6faa1ee3.png)

    - 使用range简写

      ```go
      package main
      
      import (
      	"fmt"
      	"time"
      )
      
      func main() {
      	c := make(chan int)
      
      	go func() {
      		defer fmt.Println("子进程结束")
      		for i := 0; i < 3; i++ {
      			c <- i * 100
      			fmt.Println("子进程正在运行,i=", i)
      		}
      		close(c)
      	}()
      
      	time.Sleep(2 * time.Second)
      
      	for num := range c {
      		fmt.Printf("num:%d\n", num)
      	}
      
      	time.Sleep(2 * time.Second)
      	fmt.Println("mian进程结束")
      }
      ```

      ![image-20230624182841044](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/24/30947baecb455868965ee390f3448326.png)

      ```go
      for num := range c {
          fmt.Printf("num:%d\n", num)
      }
      ```

      相当于

      ```go
      for {
          if num, ok := <-c; ok {
              fmt.Printf("num:%d\n", num)
          } else {
              break
          }
      }
      ```

- ### select与channel结合

  - 介绍

    >在 Go 语言中，select 是用于在多个通道操作中进行选择的控制结构。它可以用于监视多个通道的状态，并选择第一个准备好进行通信的通道进行操作。
    >
    >以下是 select 的一些特点和用途：
    >
    >1. 多路复用：select 允许在一个控制结构中同时监视多个通道的状态。它可以等待多个通道中的任何一个变为可用状态，然后执行相应的操作。
    >2. 非阻塞操作：通过 select，可以进行非阻塞的通道操作。如果所有的通道都没有准备好进行通信，select 语句将立即返回，而不会阻塞当前 Goroutine 的执行。
    >3. 随机选择：当多个通道都准备好进行通信时，select 语句会随机选择一个可用的通道执行操作。这样可以避免对通道操作的偏好，并增加程序的灵活性。
    >4. 默认情况：select 语句可以包含一个默认分支（default case），用于处理所有通道都没有准备好的情况。默认分支是可选的，并且在其他分支都没有准备好时会立即执行。
    >
    >使用 select 可以实现多个通道的并发操作和同步。以下是 select 的基本语法：
    >
    >```go
    >goCopy codeselect {
    >case <-ch1:
    >    // 从通道 ch1 接收数据
    >case data := <-ch2:
    >    // 从通道 ch2 接收数据，并赋值给变量 data
    >case ch3 <- value:
    >    // 将数据 value 发送到通道 ch3
    >default:
    >    // 默认情况，当所有通道都没有准备好时执行
    >}
    >```
    >
    >通过在 select 语句中包含不同的 case 分支，可以根据通道的状态进行接收或发送操作。select 会等待其中一个 case 准备好进行通信，然后执行相应的操作。
    >
    >需要注意的是，select 语句只能用于通道的操作，而不能用于其他类型的选择或条件判断。select 在并发编程中经常与 Goroutine 和通道配合使用，用于实现高效的并发控制和数据交换。

  - 练习

    ```go
    package main
    
    import "fmt"
    
    func tan(a, quit chan int) {
    	i, total := 0, 0
    	for {
    		i++
    		select {
    		case <-a:
    			total += i
    		case <-quit:
    			fmt.Println("quit,total:", total)
    			return
    		}
    	}
    }
    
    func main() {
    	a := make(chan int)
    	quit := make(chan int)
    
    	go func() {
    		for i := 0; i < 5; i++ {
    			fmt.Println("向a写入:", i)
    			a <- i
    		}
    
    		quit <- 0
    	}()
    
    	tan(a, quit)
    }
    ```

    ![image-20230624184952187](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/24/8698279007bd8a79e6b0096cc0254ebb.png)

