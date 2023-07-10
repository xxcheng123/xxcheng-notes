- ### 循环语句 `for`

  - #### 语法

    - `Go` 语言 `for` 循环有三种形式 

      1. ```go
         for init; condition ; post {}
         ```

      2. ```go
         for condition {}
         ```

      3. ```go
         for {}
         ```

    - 第一种形式和其他语言一样

    - 第二种形式相当于其他语言的 `while` 循环

    - 第三者形式相当于第二种形式为关系表达式为 `true` 的结果，无限循环

  - #### 示例

    - ##### 第一种

      ```go
      func Test_D6_0(t *testing.T) {
      	for i := 0; i < 5; i++ {
      		fmt.Println("i:", i)
      	}
      }
      ```

      ```
      i: 0
      i: 1
      i: 2
      i: 3
      i: 4
      ```

    - ##### 第二种

      ```go
      func Test_D6_1(t *testing.T) {
      	i := 0
      	for i < 5 {
      		fmt.Println("i:", i)
      		i++
      	}
      }
      ```

      ```
      i: 0
      i: 1
      i: 2
      i: 3
      i: 4
      ```

    - ##### 第三种

      ```go
      func Test_D6_2(t *testing.T) {
      	i := 0
      	for {
      		fmt.Println("i:", i)
      		i++
      	}
      }
      ```

      ```
      i: 0
      i: 1
      i: 2
      i: 3
      i: 4
      .... //无限循环下去，当遇到break后退出循环
      ```

- ### 循环语句 `for range`

  - #### 语法

    - 类似于迭代器的一种操作，返回索引、值或者键、值或者其他一些格式；
    - 可以对数组、切片、字符串、键值对等进行循环迭代；
    - `for range` 相当于 `for` 循环，它可以实现对字符串、管道（`channel`） 进行遍历；
    - 它遍历之前会把变量复制一份然后遍历

  - #### 遍历数组

    ```go
    func Test_D6_3(t *testing.T) {
    	a := [3]int{7, 8, 9}
    	for index, value := range a {
    		fmt.Println(index, value)
    	}
    }
    ```

    ```
    0 7
    1 8
    2 9
    ```

  - #### 遍历切片

    ```go
    func Test_D6_4(t *testing.T) {
    	a := []int{7, 8, 9}
    	for index, value := range a {
    		fmt.Println(index, value)
    	}
    }
    ```

    ```
    0 7
    1 8
    2 9
    ```

  - #### 遍历字符串

    ```go
    func Test_D6_5(t *testing.T) {
    	a := "China中国"
    	for index, value := range a {
    		fmt.Printf("%v,%v,%T\n", index, string(value), value)
    	}
    }
    ```

    ```
    0,C,int32
    1,h,int32
    2,i,int32
    3,n,int32
    4,a,int32
    5,中,int32
    8,国,int32
    ```

  - #### 遍历键值对

    *特别需要注意的是，当遍历 `Map` 键值对时，`for range` 遍历的顺序不是赋值的顺序，而是随机从一个位置开始的*

    ```go
    func Test_D6_6(t *testing.T) {
    	a := map[string]int{
    		"AA": 111,
    		"BB": 222,
    		"CC": 333,
    		"DD": 444,
    	}
    	for key, value := range a {
    		fmt.Printf("%v,%v\n", key, value)
    	}
    }
    ```

    *第一次运行*

    ```
    CC,333
    DD,444
    AA,111
    BB,222
    ```

    *第二次运行*

    ```
    AA,111
    BB,222
    CC,333
    DD,444
    ```

  - #### 遍历管道

    `for range` 在没有数据的情况下，会一直处于阻塞状态

    ```go
    func Test_D6_7(t *testing.T) {
    	a := make(chan int, 3)
    	a <- 111
    	a <- 222
    	a <- 333
    	go func() {
    		time.Sleep(time.Second * 3)
    		fmt.Println("现在是3s之后...")
    		a <- 666
    	}()
    	for value := range a {
    		fmt.Printf("%v\n", value)
    	}
    }
    ```

    ```
    111
    222
    333
    现在是3s之后...
    666
    //之后继续处于阻塞状态
    ```

  - #### 循环会把变量拷贝一遍后在遍历

    ```go
    func Test_D6_8(t *testing.T) {
    	a := [3]int{111, 222, 333}
    	for index, value := range a {
    		if index == 0 {
    			a[1] = 777
    			a[2] = 888
    		}
    		fmt.Printf("%v,%v\n", index, value)
    	}
    }
    ```

    ```
    0,111
    1,222
    2,333
    ```

- ### 循环控制关键字

  - #### `break`

    跳出当前循环体，配合标签可跳出多层循环体

    ```go
    func Test_D6_9(t *testing.T) {
    	a := [3]int{111, 222, 333}
    	for index, value := range a {
    		if index == 1 {
    			break
    		}
    		fmt.Printf("%v,%v\n", index, value)
    	}
    }
    ```

    ```
    0,111
    ```

    配合标签

    ```go
    func Test_D6_10(t *testing.T) {
    	a := [3]int{111, 222, 333}
    out:
    	for index, value := range a {
    		for j := 0; j < 3; j++ {
    			if index == 1 && j == 1 {
    				break out
    			}
    			fmt.Printf("%v,%v,%v\n", index, value, j)
    		}
    	}
    }
    ```

    ```
    0,111,0
    0,111,1
    0,111,2
    1,222,0
    ```

  - #### `continue`

    跳过本次循环，配合标签可跳过多层循环

    ```go
    func Test_D6_11(t *testing.T) {
    	a := [3]int{111, 222, 333}
    	for index, value := range a {
    		if index == 1 {
    			continue
    		}
    		fmt.Printf("%v,%v\n", index, value)
    	}
    }
    ```

    ```
    0,111
    2,333
    ```

    配合标签

    ```go
    func Test_D6_12(t *testing.T) {
    	a := [3]int{111, 222, 333}
    out:
    	for index, value := range a {
    		for j := 0; j < 3; j++ {
    			if index == 1 && j == 1 {
    				continue out
    			}
    			fmt.Printf("%v,%v,%v\n", index, value, j)
    		}
    	}
    }
    ```

    ```
    0,111,0
    0,111,1
    0,111,2
    1,222,0
    2,333,0
    2,333,1
    2,333,2
    ```

  - #### `goto`

    移动到指定位置执行，会操作程序混乱，不建议使用。

- ### 函数

  - #### 特点

    1. 支持多返回值；

    2. 支持匿名和闭包；

    3. 不支持默认参数；

    4. 函数也是一种类型，可以赋值给变量；

    5. 可以定义函数类型

       ```go
       type myFunc func(a int)string
       ```

    6. 支持不定参数

  - #### 定义

    - ##### 基本格式

      ```go
      func 函数名(参数列表)(返回值列表){// 左大括号{不能换行写
          //函数体
      }
      
      func swap(a int, b int)(x int , y int){
          return b,a
      }
      ```

  - #### 参数

    - ##### 语法

      - 传递参数时，是值传递，即使是引用类型，传递的是地址值的值；
      - 连续的参数类型相同可以省略，保留最后一个；
      - 函数的参数可以不固定，最后使用一个切片（`slice`）接收

    - ##### 示例

      - 省略连续的参数类型

        ```go
        func add(a, b int, c string) int {
        	fmt.Println(c)
        	return a + b
        }
        func Test_D6_13(t *testing.T) {
        	a := add(11, 22, "www~~~")
        	fmt.Println(a)
        }
        ```

        ```
        www~~~
        33
        ```

      - 不定参数

        ```go
        func add2(a string, b ...int) int {
        	fmt.Println(a)
        	fmt.Printf("%v,%T\n", b, b)
        	total := 0
        	for _, v := range b {
        		total += v
        	}
        	return total
        }
        func Test_D6_14(t *testing.T) {
        	a := add2("www~~~", 11, 22, 33, 44)
        	fmt.Println(a)
        }
        ```

        ```
        www~~~
        [11 22 33 44],[]int
        110
        ```

  - #### 返回值

    - ##### 语法

      - 可以有多个返回值；
      - 返回值可以命名；
      - 如果返回值有命名，可以只写一个 `return` 程序会自动寻找对应的变量返回；
      - 返回值类型连续相同，也可以省略类型

    - ##### 示例

      - 多个返回值

        ```go
        func swap(a, b int) (int, int) {
        	return b, a
        }
        func Test_D6_15(t *testing.T) {
        	a, b := swap(11, 22)
        	fmt.Println(a, b)
        }
        ```

        ```
        22 11
        ```

      - 返回值命名

        ```go
        func swap2(a, b int) (x int, y int) {
        	x = b
        	y = a
        	return x, y
        }
        func Test_D6_16(t *testing.T) {
        	a, b := swap2(11, 22)
        	fmt.Println(a, b)
        }
        ```

        ```
        22 11
        ```

      - 返回时省略写变量

        ```go
        func swap3(a, b int) (x int, y int) {
        	x = b
        	y = a
        	return
        }
        func Test_D6_17(t *testing.T) {
        	a, b := swap3(11, 22)
        	fmt.Println(a, b)
        }
        ```

        ```
        22 11
        ```

      - 连续相同类型省略

        ```go
        func swap4(a, b int) (x, y int) {
        	x = b
        	y = a
        	return
        }
        func Test_D6_18(t *testing.T) {
        	a, b := swap4(11, 22)
        	fmt.Println(a, b)
        }
        ```

        ```
        22 11
        ```

  - #### 匿名函数

    - 语法

      - 由一个不带函数名的函数声明和函数体组成；
      - 可以像一个普通变量一样被传递和引用；
      - 可以在任意地方定义，普通函数不能再函数内部嵌套定义；

    - ##### 示例

      - 基本

        ```go
        func Test_D6_19(t *testing.T) {
        	sayHello := func() {
        		fmt.Println("Hello Wordld")
        	}
        	fmt.Printf("%T\n", sayHello)
        	sayHello()
        }
        ```

        ```
        func()
        Hello Wordld
        ```

      - 向 `chan` 传递匿名函数

        ```go
        func Test_D6_20(t *testing.T) {
        	sayHello := func(str string) {
        		fmt.Println("Hello ", str)
        	}
        	c := make(chan func(string), 3)
        	go func() {
        		(<-c)("Channel")
        	}()
        	c <- sayHello
        	for {
        	}
        }
        ```

        ```
        Hello  Channel
        ```

  - #### 闭包

    - ##### 定义

      指的是一个拥有许多变量和绑定了这些变量的环境的表达式（通常是一个函数），因而这些变量也是该表达式的一部分。
    
      个人理解：能够读取其他函数内部变量的函数
    
      这不是 `Go` 语言的独有的特性
    
    - ##### 示例
    
      ```go
      func counter() func() int {
      	i := 0
      	return func() int {
      		i++
      		return i
      	}
      }
      func Test_D6_21(t *testing.T) {
      	c := counter()
      	fmt.Println(c())
      	fmt.Println(c())
      	fmt.Println(c())
      }
      ```
    
      ```
      1
      2
      3
      ```
    
  - #### 延迟调用 `defer`

    - ##### 定义

      - 在 `return` 执行之后再执行；
      - 多条 `defer` 语句，按照先进后出的顺序执行；
      - `defer` 的变量在声明时就确定了，但是如果是一个函数，内部调用外部的变量不会保存
      - 就算执行出错，也会继续执行下去

    - ##### 用途

      - 关闭文件句柄
      - 锁资源释放
      - 数据库连接释放

    - ##### 示例

      - 基本格式

        ```go
        func D6_23_fn_0() (int, error) {
        	a := 666
        	defer fmt.Println("defer:1", a)
        	a++
        	defer fmt.Println("defer:2", a)
        	a++
        	return fmt.Println("return")
        }
        func Test_D6_23(t *testing.T) {
        	D6_23_fn_0()
        }
        ```

        ```
        return
        defer:2 667
        defer:1 666
        ```

      - 函数内部读取的变量不会被保存

        ```go
        func D6_24_fn_0() (int, error) {
        	a := 666
        	defer fmt.Println("defer:1", a)
        	defer func() {
        		fmt.Println("defer-func:2", a)
        	}()
        	a++
        	return fmt.Println("return")
        }
        func Test_D6_24(t *testing.T) {
        	D6_24_fn_0()
        }
        ```

        ```
        return
        defer-func:2 667
        defer:1 666
        ```

        ```go
        func Test_D6_25(t *testing.T) {
        	a := []int{111, 222, 333}
        	for index, value := range a {
        		fmt.Println(index, value)
        		defer func() {
        			fmt.Println("defer", index, value)
        		}()
        	}
        	fmt.Println("done")
        }
        ```

        ```
        0 111
        1 222
        2 333
        done
        defer 2 333
        defer 2 333
        defer 2 333
        ```

      - 已被定义的 `defer` 遇到 `panic` 仍可继续运行

        所以，可以与 `recover` 配合，用于捕捉 `panic`

        ```go
        func Test_D6_26(t *testing.T) {
        	defer fmt.Println("defer-1")
        	defer func() {
        		r := recover()
        		fmt.Println("捕捉到错误了：", r)
        	}()
        	defer fmt.Println("defer-2")
        	panic("故意抛的")
        	defer fmt.Println("defer-3")
        	defer fmt.Println("defer-4")
        }
        ```

        ```
        defer-2
        捕捉到错误了 故意抛的
        defer-1
        ```

        上面示例中，因为有两条 `defer` 是在 `panic` 后定义的，所以还没声明到，就无法执行输出了。

      - 利用传参，让函数内部变量也保持定义时状态

        ```go
        func D6_27_fn_0() (int, error) {
        	a := 111
        	b := 222
        	defer fmt.Println("defer:1", a, b)
        	defer func(a int) {
        		fmt.Println("defer-func:2", a, b)
        	}(a)
        	a++
        	b++
        	return fmt.Println("return")
        }
        func Test_D6_27(t *testing.T) {
        	D6_27_fn_0()
        }
        ```

        ```
        return
        defer-func:2 111 223
        defer:1 111 222
        ```

    - ##### 与 `return` 的执行顺序

      `return` -> 运行 `defer` -> 最终返回值

      ```go
      func D6_28_fn_0() (a int) {
      	defer func() {
      		a++
      	}()
      	return 111
      }
      func Test_D6_28(t *testing.T) {
      	a := D6_28_fn_0()
      	fmt.Println("a:", a)
      }
      ```

      ```
      a: 112
      ```

- ### 参考链接

  - [wiki-闭包 (计算机科学)](https://zh.wikipedia.org/zh-hans/%E9%97%AD%E5%8C%85_(%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6))