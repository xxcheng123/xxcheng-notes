- ### 条件语句 `if`

  - #### 规范

    - 条件表达式的括号省略，如果写上了，保存之后 `Go` 会自动纠正规范，去掉括号
    - 条件表达式定义的变量，在执行体内可以访问
    - 条件表达式可以多条语句，使用分号区分，取最后一条作为判断语句

    ```go
    var a int = 0
    
    func IncOne() int {
    	a++
    	return a
    }
    
    func Test_D_0(t *testing.T) {
    	if b := IncOne(); b == 1 {
    		fmt.Println("b:", b)
    	}
    }
    ```

    ```
    b: 1
    ```

  - #### `else if`

  - #### `else`

    ```go
    func Test_D_1(t *testing.T) {
    	a := 1
    	if a == 0 {
    		fmt.Println("我是if")
    	} else if a == 1 {
    		fmt.Println("我是else if")
    	} else {
    		fmt.Println("我是else")
    	}
    	a=2
    	if a == 0 {
    		fmt.Println("我是if")
    	} else if a == 1 {
    		fmt.Println("我是else if")
    	} else {
    		fmt.Println("我是else")
    	}
    }
    ```

    ```
    我是else if
    我是else
    ```

- ### 条件语句 `switch`

  - #### 规范

    - 表达式和分支表达式可以是任何类型的数据类型，但是它们必须是同一种数据类型；
    - `Go` 不同于其他编程语言，每一条分支默认添加 `break`，如需继续往下运行添加 `fallthrough` 即可；
    - 可以当作 `if` 语句一样，省略判断表达式，分支表达式的值为布尔类型；
    - 一个条件表达式可以匹配多个值；
    - **有一种特殊的写法 `type-switch` 用来判断某个 `interface` 的具体类型**，分支表达式的值为 `type` 类型，同时支持自定义类型和类型别名，但是进不了分支语句

  - #### 基本格式

    ```go
    func Test_D_2(t *testing.T) {
    	a := 1
    	switch a {
    	case 1:
    		fmt.Println("a=1")
    	case 2:
    		fmt.Println("a=2")
    	default:
    		fmt.Println("a=3")
    	}
    }
    ```

    ```
    a=1
    ```

  - #### 使用 `fallthrough` 继续执行

    ```go
    func Test_D_4(t *testing.T) {
    	a := 1
    	switch a {
    	case 0:
    		fmt.Println("AAAA~~~")
    	case 1:
    		fmt.Println("BBBB~~~")
    		fallthrough
    	case 2:
    		fmt.Println("CCCC~~~")
    	default:
    		fmt.Println("WWWW~~~")
    	}
    }
    ```

    ```
    BBBB~~~
    CCCC~~~
    ```

  - #### 省略判断表达式

    ```go
    func Test_D_3(t *testing.T) {
    	grade := "B"
    	switch {
    	case grade == "A":
    		fmt.Println("优秀")
    	case grade == "B":
    		fmt.Println("良好")
    	default:
    		fmt.Println("不及格")
    	}
    }
    ```

    ```
    良好
    ```

  - #### 匹配多个值

    ```go
    func Test_D_5(t *testing.T) {
    	a := 1
    	switch a {
    	case 0, 1:
    		fmt.Println("AAAA~~~")
    	case 2:
    		fmt.Println("CCCC~~~")
    	default:
    		fmt.Println("WWWW~~~")
    	}
    }
    ```

    ```
    AAAA~~~
    ```

  - #### `Type Switch`

    ```go
    type myFloat float32
    type aliasFloat = float32
    
    func switchTypeGo(v interface{}) {
    	switch x := v.(type) {
    	case int:
    		fmt.Printf("int,%v,%T\n", v, v)
    		fmt.Printf("int,%v,%T\n", x, x)
    	case bool:
    		fmt.Printf("bool,%v,%T\n", v, v)
    		fmt.Printf("bool,%v,%T\n", x, x)
    	case *aliasFloat:
    		fmt.Printf("aliasFloat,%v,%T\n", v, v)
    		fmt.Printf("aliasFloat,%v,%T\n", x, x)
    	case float32:
    		fmt.Printf("float32,%v,%T\n", v, v)
    		fmt.Printf("float32,%v,%T\n", x, x)
    	case myFloat:
    		fmt.Printf("myFloat,%v,%T\n", v, v)
    		fmt.Printf("myFloat,%v,%T\n", x, x)
    	default:
    		fmt.Printf("default,%v,%T\n", v, v)
    		fmt.Printf("default,%v,%T\n", x, x)
    	}
    }
    func Test_D_6(t *testing.T) {
    	a := 111
    	b := true
    	c := "ABC"
    	var d myFloat = 1.11
    	var e aliasFloat = 2.22
    
    	switchTypeGo(a)
    	switchTypeGo(b)
    	switchTypeGo(c)
    	switchTypeGo(d)
    	switchTypeGo(e)
    }
    ```

    ```
    int,111,int
    int,111,int
    bool,true,bool
    bool,true,bool
    default,ABC,string
    default,ABC,string
    myFloat,1.11,main.myFloat
    myFloat,1.11,main.myFloat
    float32,2.22,float32
    float32,2.22,float32
    ```

- ### 条件语句 `select`

  类似与 `switch` 语句，但是又不完全相同。`select` 会随机执行一条  `case`，如果没有可执行的 `case`，它会阻塞，直到有可执行的 `case` 为止，但是如果有默认子句 `default`，那么它将是永远可行的。

  - #### 规范

    - 每一个 `case` 都必须是 `chan`，可以是发送也可以是接收 ；
    - 没有阻塞的情况下，执行哪一条语句是随机的；
    - 如果在阻塞且有 `default` 子句的情况下，直接执行 `default` 语句

  - #### 基本格式

    ```go
    func Test_D_8(t *testing.T) {
    	c1, c2 := make(chan int, 3), make(chan int, 3)
    	go func() {
    		time.Sleep(time.Second * 1)
    		select {
    		case c := <-c1:
    			fmt.Println("c1:", c)
    		case c := <-c2:
    			fmt.Println("c2:", c)
    		}
    	}()
    	go func() {
    		c1 <- 111
    		fmt.Println("发送完成")
    	}()
    
    	time.Sleep(time.Second * 10)
    }
    ```

    ```
    发送完成
    c1 111
    ```

  - #### 没有阻塞的情况，随机执行

    ```go
    func Test_D_7(t *testing.T) {
    	c1, c2 := make(chan int, 3), make(chan int, 3)
    	go func() {
    		c2 <- 222
    		c1 <- 111
    		fmt.Println("发送完成")
    	}()
    	go func() {
    		time.Sleep(time.Second * 1)
    		select {
    		case c := <-c1:
    			fmt.Println("c1", c)
    		case c := <-c2:
    			fmt.Println("c2", c)
    		}
    	}()
    
    	time.Sleep(time.Second * 10)
    }
    ```

    *第一次执行*

    ```
    发送完成
    c1 11
    ```

    *第二次执行*

    ```
    发送完成
    c2 222
    ```

  - #### 阻塞且有 `default` 的情况

    ```go
    func Test_D_9(t *testing.T) {
    	c1, c2 := make(chan int, 3), make(chan int, 3)
    	go func() {
    		time.Sleep(time.Second * 1)
    		select {
    		case c := <-c1:
    			fmt.Println("c1:", c)
    		case c := <-c2:
    			fmt.Println("c2:", c)
    		default:
    			fmt.Println("我是default")
    		}
    	}()
    	go func() {
            // 不发送数据，则一直处于阻塞状态
    		// c1 <- 111
    		// c2 <- 222
    		fmt.Println("发送完成")
    	}()
    
    	time.Sleep(time.Second * 10)
    }
    
    ```

    ```
    发送完成
    我是default
    ```

- ### 三元表达式

  `Go` 不支持三元表达式

  官方说明：[Why does Go not have the `?:` operator?](https://go.dev/doc/faq#Does_Go_have_a_ternary_form)

  *`JavaScript` 的三元表达式*

  ```js
  var a=10,b=20
  var res=a>b?"a大":"b大"
  ```