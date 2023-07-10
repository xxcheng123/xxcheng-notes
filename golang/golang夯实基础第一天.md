- ### 数据类型

  - #### 值类型

    |  类型   |    名称    |                 具体类型                  |
    | :-----: | :--------: | :---------------------------------------: |
    |  bool   |   布尔型   |                  `bool`                   |
    |   int   |    整型    |   `int` `int8` `int16` `int32` `int64`    |
    |  uint   | 无符号整型 | `uint` `uint8` `uint16` `uint32` `uint64` |
    |  float  |   浮点型   |            `float32` `float64`            |
    | string  |   字符串   |                 `string`                  |
    | complex |   复合型   |         `complex64` `complex128`          |
    |  array  |    数组    |             `[长度]数据类型`              |
    | uintptr |  指针地址  |                 `uintptr`                 |
    | struct  |   结构体   |            `type xxx struct{}`            |

    ```go
    func TestValueType(t *testing.T) {
    	//bool
    	var b1, b2 bool = true, false
    
    	//int 看系统是32位的还是64位的，长度和系统相等
    	//32位系统->int32
    	var a int = 1
    
    	// int8 int16 int32 int64
    	var (
    		a1 int8  = 9
    		a2 int16 = 8
    		a3 int32 = 7
    		a4 int64 = 6
    	)
    
    	//uint 无符号整数 看系统是32位的还是64位的，长度和系统相等
    	//32位系统->uint32
    	var u uint = 11
    
    	// uint8 uint16 uint32 uint64
    	var (
    		u1 uint8  = 99
    		u2 uint16 = 98
    		u3 uint32 = 97
    		u4 uint64 = 96
    	)
    
    	// float32 float64 浮点型
    	var (
    		f1 float32 = 1.1
    		f2 float32 = 2.2
    	)
    
    	// string 字符串类型
    	// 字符串类型是所有8位字符的集合
    	// ，字符串可以为空，但是不能位nil，
    	// 字符串类型的值是不可变的，
    	// 每一次的赋值都是重新生成
    	var (
    		s1 string = "Hello Go!"
    		s2 string = string([]byte{'H', 'e', 'l', 'l', 'o', ' ', 'G', 'o', '!'})
    	)
    
    	//complex64 complex128 复数，需要相关数学知识，且开发很少使用，仅了解
    	var (
    		cp1 complex64  = 1 + 5i
    		cp2 complex64  = complex(1, 5)
    		cp3 complex128 = 8 + 5i
    		cp4 complex128 = complex(8, 5)
    	)
    
    	//array 【固定】长度数组 核心：固定
    	var (
    		arr1 [3]int    = [3]int{7, 8, 9}
    		arr2 [2]string = [2]string{"hello", "Go"}
    	)
    
    	// uintptr 这是一个特殊的无符号整数型，它足够大，确保能够存下任何指针的地址值
    	var uptr uintptr = uintptr(unsafe.Pointer(&a))
    
    	//struct 结构体类型 golang中没有class关键字来定义类，但是可以将struct变量看作一个类的实例
    	// 在每次赋值的时候，struct也属于值传递
    	type Student struct {
    		Name string
    		Age  int
    	}
    	stu := Student{"xxcheng", 18}
    	stu2 := stu
    	stu.Name = "jpc"
    
    	fmt.Println(
    		"-------------", "\n",
    		b1, b2, "\n",
    		a, a1, a2, a3, a4, "\n",
    		u, u1, u2, u3, u4, "\n",
    		f1, f2, "\n",
    		s1, s2, "\n",
    		cp1, cp2, cp3, cp4, "\n",
    		arr1, arr2, "\n",
    		uptr, "\n",
    		stu, stu2, "\n",
    		"-------------",
    	)
    }
    ```

    ![image-20230629211305250](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/30/20e61fa83203ff2fa7c1bd5ed4139a4f.png)

    ##### 各数据类型默认零值测试

    ```go
    func TestDefaultZero(t *testing.T) {
    	var (
    		a  int
    		b  float32
    		c  complex64
    		d  string
    		e1 [3]int
    		e2 [3]string
    		f  uintptr
    	)
    	fmt.Println(a, b, c, d, e1, e2, f)
    }
    ```

    ![image-20230629204053766](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/30/f912b55dfe23c72d203ef7063c47e2c5.png)

  - #### 引用类型（指针类型）

    | 类型  |  名称  |             格式              |
    | :---: | :----: | :---------------------------: |
    | slice |  切片  |          []数据类型           |
    |  map  | 键值对 | map[key数据类型]value数据类型 |
    | chan  |  通道  |         chan 数据类型         |

    ```go
    func TestReferenceType(t *testing.T) {
    	// slice 切片类型 长度不固定的数组
    	var s1 []int = []int{9, 8, 5}
    	var s2 []int = make([]int, 4)
    	s2[2] = 666
    
    	// map 键值对
    	// 最后一条键值对也要添加逗号
    	var m1 map[string]int = map[string]int{
    		"age": 18,
    		"sex": 0,
    	}
    	var m2 map[string]string = make(map[string]string, 3)
    	m2["age"] = "xxcheng"
    
    	// chan 通道 用于通信使用，分为无缓存和有缓存
    	// 无缓存
    	var c1 chan int = make(chan int)
    	// 有缓存
    	var c2 chan string = make(chan string, 2048)
    
    	go func() {
    		for i := 0; i < 3; i++ {
    			fmt.Println("【无缓存】发送端：", i*10)
    			c1 <- i * 10
    		}
    	}()
    	go func() {
    		for i := 0; i < 3; i++ {
    			fmt.Println("【有缓存】发送端：现在是：", i)
    			c2 <- fmt.Sprintf("现在是：%d", i)
    		}
    	}()
    
    	go func() {
    		for i := 0; i < 3; i++ {
    			fmt.Println("【无缓存】接收端：", <-c1)
    		}
    	}()
    	go func() {
    		for i := 0; i < 3; i++ {
    			fmt.Println("【有缓存】接收端：", <-c2)
    		}
    	}()
    
    	fmt.Println(
    		"---------------", "\n",
    		s1, s2, "\n",
    		m1, m2, "\n",
    		"---------------",
    	)
    	for {
    
    	}
    }
    ```

    ![image-20230630090655427](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/30/3e3472ba8083898a2570117462795bf0.png)

    **证明`interface`类型是引用类型**

    ```go
    func (m *Man) Say() {
    	fmt.Println("name:", m.Name, ",age:", m.age)
    }
    func TestOther(t *testing.T) {
    	var man Man = Man{"xxcheng", 18}
    	var man2 Man = man
    	var person Person = &man
    	man.Name = "jpc"
    	man2.Say()
    	person.Say()
    }
    ```

    ![image-20230630084106467](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/30/4c09fb0f45a185f1629ba0f3b6a2b5db.png)

  - #### 拓展

    - 值类型和引用类型两者之间的主要区别：**拷贝操作和函数传参**。

- ### 内置函数

  - #### `append`

    添加n个元素到切片的末尾

    ```go
    func append(slice []Type, elems ...Type) []Type
    ```

    ```go
    func TestBuildinFuncAppend(t *testing.T) {
    	a := []int{7, 8, 9}
    	b := append(a, 999)
    	c := []int{11, 22, 33}
    	d := append(a, c...)
    	fmt.Println(b, d)
    }
    ```

    ![image-20230630091945790](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/30/241d9473fd60e7d1a7eee9ba5faf2224.png)

  - #### `cap`

    返回变量占用的实际容量

    ```go
    func cap(v Type) int
    ```

    > ```
    > Array: the number of elements in v (same as len(v)).
    > Pointer to array: the number of elements in *v (same as len(v)).
    > Slice: the maximum length the slice can reach when resliced;
    > if v is nil, cap(v) is zero.
    > Channel: the channel buffer capacity, in units of elements;
    > if v is nil, cap(v) is zero.
    > ```

    ```go
    func TestBuildinFuncCap(t *testing.T) {
    	arr := [3]int{7, 8, 9}
    	arr[0] = 111
    	slice := make([]int, 4)
    	slice = []int{1, 2, 3, 4}
    	slice = append(slice, 88)
    	slice2 := []int{}
    	fmt.Println(
    		"----"+"\n",
    		"arr:", cap(arr), "\n",
    		"&arr:", cap(&arr), "\n",
    		"slice:", cap(slice), "\n",
    		"slice2:", cap(slice2),
    	)
    }
    ```

    ![image-20230630092828218](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/30/a708ef6d9b7d6b43e4cfb0d07ddb61cf.png)

  - #### `close`

    关闭通道

    ```go
    func TestBuildinFuncClose(t *testing.T) {
    	defer fmt.Println("bye~~~")
    	c := make(chan int, 4)
    	go func() {
    		defer fmt.Println("exit send")
    		for i := 111; i < 9999; i++ {
    			c <- i
    			if i == 119 {
    				close(c)
    				break
    			}
    		}
    	}()
    	go func() {
    		defer fmt.Println("exit receive")
    		for {
    			v, ok := <-c
    			if !ok {
    				break
    			}
    			fmt.Println("receive:", v)
    		}
    	}()
    	time.Sleep(time.Second * 5)
    }
    ```

    ![image-20230630094945136](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/30/b777811ed21cdc1fea9b871ab16b2a0f.png)

  - #### `complex`

    返回一个复数

    ```go
    func complex(r, i FloatType) ComplexType
    ```

    ```go
    func TestBuildinFuncComplex(t *testing.T) {
    	c := complex(2.2, 3.3)
    	fmt.Println(c)
    }
    ```

    

    ![image-20230630095306713](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/30/aab153ac6913ba8d78145e54d36db339.png)

  - #### `copy`

    复制一个切片的数据到另外一个切片

    *特殊情况，可以将一个字符串复制到一个byte切片*

    返回一个int类型，表示成功复制了多少个

    ```go
    func copy(dst, src []Type) int
    ```

    ```go
    func TestBuildinFuncCopy(t *testing.T) {
    	a := []int{1, 2, 3}
    	// 这里特别注意，切片需要初始化分配空间，否则copy不进去数据的
    	var b []int = make([]int, 3)
    	c := "ABC"
    	var d []byte = make([]byte, 3)
    	var e []byte = make([]byte, 2)
    	// 这里不初始化分配空间，进行测试
    	var f []byte
    	l1 := copy(b, a)
    	l2 := copy(d, c)
    	l3 := copy(e, c)
    	l4 := copy(f, c)
    	fmt.Println(a, c)
    	fmt.Println(b, d, e, f)
    	fmt.Println(l1, l2, l3, l4)
    }
    ```

    ![image-20230630101342582](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/30/5bfda8940c02d277ef3db8d5705de081.png)

  - #### `delete`

    删除指定键值对的key元素

    ```go
    func delete(m map[Type]Type1, key Type)
    ```

    ```go
    func TestBuildinFuncDelete(t *testing.T) {
    	m := map[string]string{
    		"name": "xxcheng",
    		"sex":  "男",
    	}
    	fmt.Println(m)
    	delete(m, "name")
    	delete(m, "age")
    	fmt.Println("----------")
    	fmt.Println(m)
    }
    ```

    ![image-20230630101725315](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/30/16f7390e1adb1a860b796286a8e19462.png)

  - #### `imag`

    给定一个复数，返回其虚部

    ```go
    func imag(c ComplexType) FloatType
    ```

    ```go
    func TestBuildinFuncImag(t *testing.T) {
    	c := 3.3 + 5.5i
    	i := imag(c)
    	fmt.Println(i)
    }
    ```

    ![image-20230630102134977](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/30/a5815f57698886cfe739fc70c7eb44aa.png)

  - #### `len`

    根据数据类型返回对应规则下的长度，并非实际容量长度，一般与`cap`配合使用

    ```go
    func len(v Type) int
    ```

    > ```
    > Array: the number of elements in v.
    > Pointer to array: the number of elements in *v (even if v is nil).
    > Slice, or map: the number of elements in v; if v is nil, len(v) is zero.
    > String: the number of bytes in v.
    > Channel: the number of elements queued (unread) in the channel buffer;
    >    if v is nil, len(v) is zero.
    > ```
    
    ```go
    func TestBuildinFuncLen(t *testing.T) {
    	arr := [3]int{7, 8}
    	arr2 := [0]int{}
    	slice := make([]int, 8)
    	str := "ABC"
    	c := make(chan int)
    	c2 := make(chan int, 4)
    	go func() {
    		for {
    			c <- 333
    		}
    	}()
    	go func() {
    		for {
    			c2 <- 333
    		}
    	}()
    	time.Sleep(time.Second * 3)
    	fmt.Println(
    		"-------", "\n",
    		"arr:", len(arr), "\n",
    		"&arr:", len(&arr), "\n",
    		"arr2:", len(arr2), "\n",
    		"&arr2:", len(&arr2), "\n",
    		"&slice:", len(slice), "\n",
    		"&str:", len(str), "\n",
    		"&c:", len(c), "\n",
    		"&c2:", len(c2), "\n",
    		"-------",
    	)
    }
    ```
    
    ![image-20230630103201366](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/30/37ef07ac1d86cc970589f2114198a9f5.png)
    
  - #### `make`

    初始化和分配内存，只能用于`slice`、`map`和`chan`类型

    ```go
    func make(t Type, size ...IntegerType) Type
    ```

    > ```
    > Slice: The size specifies the length. The capacity of the slice is
    > equal to its length. A second integer argument may be provided to
    > specify a different capacity; it must be no smaller than the
    > length. For example, make([]int, 0, 10) allocates an underlying array
    > of size 10 and returns a slice of length 0 and capacity 10 that is
    > backed by this underlying array.
    > Map: An empty map is allocated with enough space to hold the
    > specified number of elements. The size may be omitted, in which case
    > a small starting size is allocated.
    > Channel: The channel's buffer is initialized with the specified
    > buffer capacity. If zero, or the size is omitted, the channel is
    > unbuffered.
    > ```

    ```go
    func TestBuildinFuncMake(t *testing.T) {
    	slice := make([]int, 3)
    	slice2 := make([]int, 0, 3)
    	fmt.Println(len(slice), len(slice2))
    	fmt.Println(slice, slice2)
    	fmt.Println("------")
    	m1 := make(map[string]int)
    	m2 := make(map[string]int, 3)
    	fmt.Println(len(m1), m1)
    	fmt.Println(len(m2), m2)
    	fmt.Println("------")
    	c1 := make(chan int)
    	c2 := make(chan int, 3)
    	fmt.Println(len(c1), c1)
    	fmt.Println(len(c2), c2)
    }
    ```

    ![image-20230630104559715](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/30/575519fd077d1b22b459dbeb39123d43.png)

  - #### `new`

    返回一个初始化一个指定类型的函数并且分配好内存的变量的**指针**

    ```go
    func new(Type) *Type
    ```

    ```go
    func TestBuildinFuncNew(t *testing.T) {
    	a := new(int)
    	fmt.Printf("%T,%v\n", a, *a)
    	arr := new([3]int)
    	fmt.Printf("%T,%v\n", arr, arr)
    	slice := new([]int)
    	fmt.Printf("%T,%v", slice, slice)
    }
    ```

    ![image-20230630105538631](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/30/d00ad88874e63aaf1d8cb100d52af8f0.png)

    **`make`与`new`的异同点**

    - 相同点：
      1. 都是在堆上分配内存；
    - 不同点
      1. `make`返回的是对应的传入的数据类型，`new`返回的是对应的传入的数据类型的指针；
      2. `make`只能使用在`slice`、`map`和`chan`的创建

  - #### `panic`

    抛出一个异常

    在此之后代码不会执行，但是会先调用在此之前声明了的`defer`，然后返回调用当前函数的函数，调用当前函数的函数之后的代码也不会执行，但是会先调用在此之前声明了的`defer`，依次执行最后结果执行，除非当异常被捕获，才能正常执行

    **官方原文**

    > The panic built-in function stops normal execution of the current goroutine. When a function F calls panic, normal execution of F stops immediately. Any functions whose execution was deferred by F are run in the usual way, and then F returns to its caller. To the caller G, the invocation of F then behaves like a call to panic, terminating G's execution and running any deferred functions. This continues until all functions in the executing goroutine have stopped, in reverse order. At that point, the program is terminated with a non-zero exit code. This termination sequence is called panicking and can be controlled by the built-in function recover.

    ```go
    func panic(v any)
    ```

    ```go
    func HandlePanic_A() {
    	defer fmt.Println("HandlePanic_A defer")
    	fmt.Println("HandlePanic_A func")
    	HandlePanic_B()
    	fmt.Println("HandlePanic_A2 func")
    }
    func HandlePanic_B() {
    	defer fmt.Println("HandlePanic_B defer")
    	fmt.Println("HandlePanic_B func")
    	HandlePanic_IN()
    	fmt.Println("HandlePanic_B2 func")
    }
    func HandlePanic_IN() {
    	defer fmt.Println("HandlePanic_IN defer")
    	fmt.Println("HandlePanic_IN func")
    	panic("呃呃呃呃")
    	fmt.Println("HandlePanic_IN_2 func")
    }
    func TestBuildinFuncPanic(t *testing.T) {
    	HandlePanic_A()
    }
    ```

    ![image-20230630110849955](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/30/19baecf70e21c419589e72d299999222.png)

  - #### `print`

    见`println`

    ```go
    func print(args ...Type)
    ```

  - #### `println`

    将内容按照指定格式的形式输出到标准错误流，而`fmt`包是将内容输出到标准输出流，一般是屏幕输出，根据官方的说明，不是很推荐使用，并且有可能会在之后的版本被抛弃，并且输出内容和格式也和`fmt`很不一样

    ```go
    func println(args ...Type)
    ```

    ```go
    func TestBuildinFuncPrint(t *testing.T) {
    	a := 5
    	b := []int{4, 5, 6}
    	println(a, b)
    	print(a, b)
    	fmt.Println()
    	fmt.Println(a, b)
    	fmt.Print(a, b)
    }
    ```

    ![image-20230630112031025](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/30/f2126d97441c9c42928a744bbaff3453.png)

  - #### `real`

    给定一个复数，返回其实部

    ```go
    func real(c ComplexType) FloatType
    ```

    ```go
    func TestBuildinFuncReal(t *testing.T) {
    	c := 3.3 + 5.5i
    	i := real(c)
    	fmt.Println(i)
    }
    ```

    ![image-20230630121155350](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/30/474fbb41f207cec1fd76ead715ba83e9.png)

  - #### `recover`

    捕获`panic`，使程序恢复正常，`recover`只有在defer的函数里面才能发挥真正的作用

    ```go
    func recover() any
    ```

    ```go
    func makeError() {
    	fmt.Println("我是makeError Top")
    	panic("我是makeError 我丢一个错误")
    	fmt.Println("我是makeError Bottom")
    }
    func catchError() {
    	defer func() {
    		if r := recover(); r != nil {
    			fmt.Println("我是catchError 捕获到一个Error ", r)
    		}
    	}()
    	fmt.Println("catchError Top")
    	makeError()
    	fmt.Println("catchError Bottom")
    }
    
    func TestBuildinFuncRecover(t *testing.T) {
    	fmt.Println("TestBuildinFuncRecover Top")
    	catchError()
    	fmt.Println("TestBuildinFuncRecover Bottom")
    }
    ```

    ![image-20230630134029224](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/30/994d1780fad8b6f3d9555e74f25bd812.png)

- ### 参考链接

  - [Go基础](https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/)
  - [builtin package - builtin - Go Packages-Types](https://pkg.go.dev/builtin#pkg-types)
  - [builtin package - builtin - Go Packages-Functions](https://pkg.go.dev/builtin#pkg-functions)
  - [What is type complex64 in Golang?](https://www.educative.io/answers/what-is-type-complex64-in-golang)
  - [CSDN-golang中的值类型和引用类型](https://blog.csdn.net/love666666shen/article/details/99882528)
  - [博客园-【go】golang中置new()函数和make()函数的区别](https://www.cnblogs.com/chenpingzhao/p/9918062.html)
