- ### 指针

  指针是存储变量地址值的一种指针变量

  指针的概念并非 `Go` 语言所独有的，`C/C++` 也有指针，但是 `Go` 语言的指针，在正常情况下，是不允许编译和运算的。

  使用 `&` 取变量的地址值，使用 `*` 取指针指向的地址值所存储的值，`&` 和 `*` 是一对互补的操作符

  ```go
  func Test_C_0(t *testing.T) {
  	a := 100
  	b := &a
  	c := *b
  	fmt.Printf("%v,%p,%T\n", a, &a, a)
  	fmt.Printf("%v,%p,%T\n", b, &b, b)
  	fmt.Printf("%v,%p,%T\n", c, &c, c)
  }
  ```

  ```
  100,0xc000018178,int
  0xc000018178,0xc000012068,*int
  100,0xc000018180,int
  ```

  根据上面演示，他们每一个变量是类似这样子的：

  ![image-20230704092722009](https://cdn-static.xxcheng.cn/static/blog/images/2023/07/04/83094a25b09844e60c36b21e001d341b.png)

  - #### 作用

    `Go` 函数的参数是值拷贝，也就是说，给函数传入一个值引用类型的变量 `a`，函数内部进行修改，执行完后，重新打印 `a`，`a` 的值不变

    ```go
    func updateArr(arr [3]int) {
    	arr[1] = 666
    }
    func Test_C_1(t *testing.T) {
    	a := [3]int{7, 8, 9}
    	updateArr(a)
    	fmt.Println(a)
    }
    ```

    ```
    [7 8 9]
    ```

    把参数修改为对应的指针类型就可以进行修改了

    ```go
    func updateArr2(arr *[3]int) {
    	arr[1] = 666
    }
    func Test_C_2(t *testing.T) {
    	a := [3]int{7, 8, 9}
    	updateArr2(&a)
    	fmt.Println(a)
    }
    ```

    ```
    [7 666 9]
    ```

    **特别需要注意的是，函数虽然是只能值拷贝，但是 `Go` 还有一种引用类型的变量，它们赋值时也是值拷贝（`GO` 语言中，只有值拷贝），只是拷贝是指针，通过下面的示例可以很清晰的理解和明白**

    ```go
    func Test_C_3(t *testing.T) {
    	a := []int{7, 8, 9}
    	b := a
    	fmt.Printf("%v,%p,%p,%T\n", a, a, &a, a)
    	fmt.Printf("%v,%p,%p,%T\n", b, b, &b, b)
    }
    ```

    ```
    [7 8 9],0xc00001a120,0xc0000100c0,[]int
    [7 8 9],0xc00001a120,0xc0000100d8,[]int
    ```

    ![image-20230704094800578](https://cdn-static.xxcheng.cn/static/blog/images/2023/07/04/c9dd6ed03551ca28fe778458319e1eec.png)

  - #### 空指针

    一个指针没有被初始化分配变量，它的值就是空指针，就是 `nil`。

    ```go
    func Test_C_4(t *testing.T) {
    	var a *int
    	fmt.Println(a == nil)
    }
    ```

    ```
    true
    ```

- ### `new` 和 `make`

  `new` 和 `make` 都是 `Go` 的内置函数，之前有[学习归纳](https://www.xxcheng.cn/751.html)过

  - #### `new`

    接受一个类型 `T`，返回一个初始化好了的指针 `*T`，并且已经赋好对应的零值了

    ```go
    func new(Type) *Type
    ```

    ```go
    func Test_C_5(t *testing.T) {
    	var a *int = new(int)
    	b := *a
    	fmt.Println(a == nil)
    	fmt.Printf("%v,%T\n", a, a)
    	fmt.Printf("%v,%T\n", b, b)
    }
    ```

    ```
    false
    0xc000018178,*int
    0,int
    ```

  - #### `make`

    - 第一个参数接受一个类型 `T`，且只能为 `slice` 、 `map` 、`chan` 这三种引用值类型的内存创建
    - 第二个参数可以缺省，用于指定长度
      - 对于 `slice` 来说，用于定义初始化时的赋的零值的个数
      - 对于 `map` 来说，用于设置键值对个数
      - 对于 `chan` 来说，用于设置缓存区大小，为 `0` 或者缺省表示没有缓冲区
    - 第三个参数只有 `slice` 类型才有，表示实际容量大小，每一次开辟的新的空间的刻度
    - 返回值类型为 `T`

    ```go
    func make(t Type, size ...IntegerType) Type
    
    // slice
    func make(t Type, len int [, cap int]) Type
    // map
    func make(t Type [, cap int]) Type
    // chan
    func make(t Type [, cap int]) Type
    ```

    ```go
    func Test_C_6(t *testing.T) {
    	a := make([]int, 4, 8)
    	b := make(map[int]int)
    	c := make(chan int, 4)
    	fmt.Printf("%v,%T\n", a, a)
    	fmt.Printf("%v,%T\n", b, b)
    	fmt.Printf("%v,%T\n", c, c)
    }
    ```

    ```
    [0 0 0 0],[]int
    map[],map[int]int
    0xc000024200,chan int
    ```

  - #### 区别

    1. 都是用于内存分配，但是 `new` 所有数据类型都可以使用，而 `make` 只能 `slice` 、 `map` 、`chan`  三种类型使用；
    2. 返回值类型不同，`new` 返回的是对应类型的指针，而 `make` 是对应的数据类型

- ### `Map`

  - #### 定义

    一种无序的 `key:value` 格式的数据结构，因为是引用类型，所以必须初始化

    数据格式：

    ```go
    map[keyType]ValueType
    ```

  - #### 初始化

    - ##### 使用 `make` 初始化

      ```go
      func Test_C_7(t *testing.T) {
      	user := make(map[string]string, 4)
      	user["name"] = "xxcheng"
      	user["sex"] = "男"
      	fmt.Printf("%v,%T,%d", user, user, len(user))
      }
      ```

      ```
      map[name:xxcheng sex:男],map[string]string,2
      ```

    - ##### 直接初始化

      ```go
      func Test_C_8(t *testing.T) {
      	user := map[string]string{
      		"name": "xxcheng",
      		"sex":  "男",
      	}
      	fmt.Printf("%v,%T,%d", user, user, len(user))
      }
      ```

      ```
      map[name:xxcheng sex:男],map[string]string,2
      ```

  - #### 直接取值

    格式：`value,ok=map[key]`

    第一个返回值为取到的值，第二个返回值类型为 `bool`，表示所取的 `key` 是否存在

    ```go
    func Test_C_9(t *testing.T) {
    	user := map[string]string{
    		"name": "xxcheng",
    		"sex":  "男",
    	}
    	value, isExist := user["name"]
    	value2, isExist2 := user["age"]
    	fmt.Println(value, isExist)
    	fmt.Println(value2, isExist2)
    }
    ```

    ```
    xxcheng true
    true false
    ```

  - #### 遍历取值

    第一个返回值为取到的值，第二个返回值为键

    ```go
    func Test_C_10(t *testing.T) {
    	user := map[string]string{
    		"name":  "xxcheng",
    		"sex":   "男",
    		"book":  "海底两万里",
    		"music": "如愿",
    	}
    	for value, key := range user {
    		fmt.Println(key, value)
    	}
    }
    ```

    ```
    海底两万里 book
    如愿 music
    xxcheng name
    男 sex
    ```

    可以看到遍历顺序和赋值顺序是不一样的，如果重新执行一遍，顺序再次不一样，这是`Go` 故意这么设计的，随机从一个位置开始遍历

  - #### 删除键值对

    使用 `delete` 内置方法

    ```go
    func Test_C_11(t *testing.T) {
    	user := map[string]string{
    		"name":  "xxcheng",
    		"sex":   "男",
    		"book":  "海底两万里",
    		"music": "如愿",
    	}
    	for value, key := range user {
    		fmt.Println(key, value)
    	}
    	fmt.Println("-------")
    	delete(user, "book")
    	delete(user, "music")
    	for value, key := range user {
    		fmt.Println(key, value)
    	}
    }
    ```

    ```
    如愿 music
    xxcheng name
    男 sex
    海底两万里 book
    -------
    xxcheng name
    男 sex
    ```

- ### 结构体

  在 `Go` 语言中没有类的概念，没有提供 `class` 这样的关键字来创建，但是我们可以通过结构体配合接口来满足类的需求，同时实现 `继承`、`封装`、`多态` 三大特性。

  - #### 前置知识

    - ##### 自定义类型

      使用 `type` 关键字自定义类型

      ```go
      type MyInt int
      ```

    - ##### 类型别名

      `Go1.9` 版本新增的功能，它只是一个别名，数据类型还是原来的数据类型

      ```go
      type TypeAlias = Type
      ```

      之前见过的两种字符类型就是类型别名

      ```go
      type byte = uint8
      type rune = int32
      ```

    - ##### 区别

      感觉使用上面没有什么区别，那他们之间有什么差异吗？

      自定义类型它是一种新的数据类型，拥有原来的数据类型的特性，而类型别名只是一个别名，数据类型还是原来的数据类型

      ```go
      func Test_C_12(t *testing.T) {
      	type myInt int
      	type intAlias = int
      	var a myInt = 1
      	var b intAlias = 2
      	fmt.Printf("%v,%T\n", a, a)
      	fmt.Printf("%v,%T\n", b, b)
      }
      ```

      ```
      1,main.myInt
      2,int
      ```

  - #### 格式

    相同类型的字段可以写在一行

    字段名首字母大写表示可公开访问，包外可访问，小写表示私有，仅包内可访问。

    ```go
    type 类型名 struct{
        字段名 字段类型 `标签`
    }
    ```

  - #### 实例化

    有两种实例化方式，一种是键值对的方式，一种是值列表的方式

    ```go
    func Test_C_13(t *testing.T) {
    	p := Person{
    		"xxcheng",
    		"男",
    		18,
    	}
    	p2 := Person{
    		name: "jpc",
    		sex:  "男",
    		age:  18,
    	}
    	p2.name = "www"
    	fmt.Println(p, p2)
    }
    ```

    ```
    {xxcheng 男 18} {www 男 18}
    ```

  - #### 匿名结构体

    在一些临时的场景只实例化一次时使用

    ```go
    func Test_C_14(t *testing.T) {
    	var user struct {
    		name string
    		sex  string
    		age  int
    	}
    	user.name = "xxcheng"
    	user.sex = "男"
    	user.age = 18
    	fmt.Printf("%v,%T", user, user)
    }
    ```

    ```
    {xxcheng 男 18},struct { name string; sex string; age int }
    ```

  - #### 结构体指针

    ```go
    func Test_C_15(t *testing.T) {
    	var user struct {
    		name string
    		sex  string
    		age  int
    	}
    	user2 := &user
    	user2.name = "xxcheng"
    	user2.sex = "男"
    	(*user2).age = 18
    	fmt.Printf("%#v,%T", user2, user2)
    }
    ```

    ```
    &struct { name string; sex string; age int }{name:"xxcheng", sex:"男", age:18},*struct { name string; sex string; age int }
    ```

    通过示例可以发现，使用 `user2.sex = "男"` 和 使用 `(*user2).sex = "男"` 效果是一样的，这是 `Go` 语言帮我们实现的一种语法糖。

  - #### 结构体内存布局

    这一块知识点浅学了一下，感觉知识点很有难度，设计到底层计组相关知识，先暂缓放着，否则很浪费时间

    ```go
    func Test_C_16(t *testing.T) {
    	type User struct {
    		age  int8
    		age2 int16
    		age3 int32
    	}
    	user := User{}
    	fmt.Printf("user     :%p,%T,%d,%d\n", &user, user, unsafe.Sizeof(user), unsafe.Alignof(user))
    	fmt.Printf("user.age :%p,%T,%d,%d\n", &user.age, user.age, unsafe.Sizeof(user.age), unsafe.Alignof(user.age))
    	fmt.Printf("user.age2:%p,%T,%d,%d\n", &user.age2, user.age2, unsafe.Sizeof(user.age2), unsafe.Alignof(user.age2))
    	fmt.Printf("user.age3:%p,%T,%d,%d\n", &user.age3, user.age3, unsafe.Sizeof(user.age3), unsafe.Alignof(user.age3))
    }
    ```

    ```
    user     :0xc0000181a8,main.User,8,4
    user.age :0xc0000181a8,int8,1,1
    user.age2:0xc0000181aa,int16,2,2
    user.age3:0xc0000181ac,int32,4,4
    ```

  - #### 自定义实现构造函数

    `Go` 语言没有类的概念，所以也没有构造函数的概念，但是我们可以通过函数内使用结构体创建一个，并且以返回值的形式返回，同时，因为结构体是值类型，所以 `return` 返回时会重新创建一个新的结构体，重复开销，所以返回值使用对应类型的指针类型

    ```go
    type Student struct {
    	name string
    	sex  string
    	age  int
    }
    
    func newStudent(name, sex string, age int) *Student {
    	return &Student{name, sex, age}
    }
    func Test_C_17(t *testing.T) {
    	s := newStudent("xxcheng", "男", 18)
    	fmt.Printf("%#v,%T\n", s, s)
    	fmt.Println("name:", s.name, ",sex:", s.sex, ",age:", s.age)
    }
    ```

    ```
    &main.Student{name:"xxcheng", sex:"男", age:18},*main.Student
    name: xxcheng ,sex: 男 ,age: 18
    ```

  - #### 方法（接收者）

    一种特殊类型的函数，相当于其他语言的 `this` 和 `self`

    官方建议接收者变量为接收者类型**第一个小写字母**

    ```go
    func (接收者变量 接收者类型) 方法名(参数列表) (返回参数) {
        函数体
    }
    ```

    接收者类型有两种，一种是指针类型的接收者，一种是值类型的接收者

    一般情况下，都最好使用指针类型的接收者，当需要对接收者的值进行修改时，只能使用指针类型的接收者

    - ##### 值类型

      ```go
      type Student struct {
      	name string
      	sex  string
      	age  int
      }
      
      func (s Student) SayHello() {
      	fmt.Println("你好，我叫", s.name, "，性别是", s.sex, "，年龄是", s.age)
      }
      func Test_C_18(t *testing.T) {
      	s := Student{"xxcheng", "男", 18}
      	s.SayHello()
      }
      ```

      ```
      你好，我叫 xxcheng ，性别是 男 ，年龄是 18
      ```

    - ##### 指针类型

      ```go
      type Student struct {
      	name string
      	sex  string
      	age  int
      }
      
      func (s Student) SayHello() {
      	fmt.Println("你好，我叫", s.name, "，性别是", s.sex, "，年龄是", s.age)
      }
      
      func (s *Student) UpdateName(name string) {
      	s.name = name
      }
      
      func Test_C_19(t *testing.T) {
      	s := Student{"xxcheng", "男", 18}
      	s.UpdateName("jpc")
      	s.SayHello()
      }
      ```

      ```
      你好，我叫 jpc ，性别是 男 ，年龄是 18
      ```

    同时，方法（接收者）并不是只能给结构体使用，可以给任何数据类型都可以使用

    ```go
    type myInt int
    
    func (m myInt) SayCurrentInt() {
    	fmt.Println("当前的值为：", m)
    }
    func Test_C_20(t *testing.T) {
    	var a myInt = 1
    	a.SayCurrentInt()
    }
    ```

    ```
    当前的值为： 1
    ```

    上面示例我们可以给我们自定义类型 `myInt` 定义一个方法，但是不能直接给 `int` 定义一个方法，因为方法（接收者）只能在本地定义，也就是只能在当前包定义

  - #### 匿名字段

    结构体中还有一种特殊的字段，`匿名字段` 只写数据类型，而不写变量

    ```go
    type Human struct {
    	string
    	int
    	sex string
    }
    
    func Test_C_21(t *testing.T) {
    	h := Human{"xxcheng", 18, "男"}
    	fmt.Printf("%#v,%T\n", h, h)
    	fmt.Println(h.string)
    	fmt.Println(h.int)
    	fmt.Println(h.sex)
    }
    ```

    ```
    main.Human{string:"xxcheng", int:18, sex:"男"},main.Human
    xxcheng
    18
    男
    ```

    和普通的字段没什么差别，只不过是字段名称是对应的 `数据类型` 的名称

  - #### 嵌套结构体

    ```go
    type Book struct {
    	name  string
    	price float32
    }
    type TextBook struct {
    	subject string
    	Book    Book
    }
    
    func Test_C_22(t *testing.T) {
    	a := TextBook{"数学", Book{"数学书", 10.99}}
    	fmt.Println(a)
    }
    ```

    ```
    {数学 {数学书 10.99}}
    ```

  - #### 匿名嵌套结构体

    匿名嵌套结构体在自身的字段找不到字段的情况下，可以直接访问被嵌套的结构体的字段

    ```go
    type A struct {
    	name string
    	age  int
    }
    type B struct {
    	name string
    	A
    }
    
    func Test_C_23(t *testing.T) {
    	b := B{"xxcheng", A{"jpc", 18}}
    	b.name = "www"
    	b.age = 999
    	fmt.Printf("%#v\n", b)
    }
    ```

    ```
    main.B{name:"www", A:main.A{name:"jpc", age:999}}
    ```

    - ##### 匿名嵌套结构体嵌套的嵌套

      当前字段名找不到，就会顺着匿名结构体一层一层往下找

      ```go
    type F struct {
      	name string
      	age  int
      }
      type G struct {
      	name2 string
      	F
      }
      type H struct {
      	name3 string
      	G
      }
      
      func Test_C_25(t *testing.T) {
      	a := H{"hhh", G{"ggg", F{"fff", 18}}}
      	a.name = "www"
      	a.age = 999
      	fmt.Printf("%#v\n", a)
      }
      ```
    
      ```
      main.H{name3:"hhh", G:main.G{name2:"ggg", F:main.F{name:"www", age:999}}}
    ```
    
    - ##### 匿名嵌套结构体发生冲突
    
      当两个匿名嵌套结构体字段相同时，就无法直接访问了，会造成歧义，就需要指定字段名访问了
    
      ```go
    type C struct {
      	name string
    	age  int
      }
    type D struct {
      	name string
      	age  int
      }
      type E struct {
      	name string
      	C
      	D
      }
    
      func Test_C_24(t *testing.T) {
      	b := E{"xxcheng", C{"ccc", 11}, D{"ddd", 22}}
      	b.name = "www"
    	b.age = 999
      	fmt.Printf("%#v\n", b)
      }
      ```
    
      ```
      # command-line-arguments [command-line-arguments.test]
      ./c_test.go:292:4: ambiguous selector b.age
      FAIL    command-line-arguments [build failed]
      FAIL
      ```
    
      ![image-20230704135029496](https://cdn-static.xxcheng.cn/static/blog/images/2023/07/04/5e5238be2fb461ea495459f17c2fc89b.png)
    
      修改为
    
      ```go
      func Test_C_24(t *testing.T) {
      	b := E{"xxcheng", C{"ccc", 11}, D{"ddd", 22}}
      	b.name = "www"
      	b.C.age = 999
      	fmt.Printf("%#v\n", b)
      }
      ```
    
      ```
      main.E{name:"www", C:main.C{name:"ccc", age:999}, D:main.D{name:"ddd", age:22}}
      ```
    
  - #### 结构体的 `继承`

    基于匿名结构体会自动寻找对应的字段名这一特点，可以实现其 `继承` 的特点

    ```go
    type Animal struct {
    	name string
    }
    
    type Cat struct {
    	speed int
    	Animal
    }
    
    func (c Cat) run() {
    	fmt.Println(c.name, "正在奔跑，速度为：", c.speed)
    }
    
    type Dog struct {
    	color string
    	Animal
    }
    
    func (d Dog) dark() {
    	fmt.Println(d.name, "正在叫，它的颜色是：", d.color)
    }
    
    func Test_C_26(t *testing.T) {
    	a := Cat{20, Animal{"咪咪"}}
    	b := Dog{"白色", Animal{"小白"}}
    	a.run()
    	b.dark()
    }
    ```

    ```
    咪咪 正在奔跑，速度为： 20
    小白 正在叫，它的颜色是： 白色
    ```

  - #### 结构体的序列化与反序列化

    *摘自百度百科*

    > JSON（JavaScript Object Notation, JS对象简谱）是一种轻量级的数据交换格式。它基于 ECMAScript（European Computer Manufacturers Association, 欧洲计算机协会制定的js规范）的一个子集，采用完全独立于编程语言的文本格式来存储和表示数据。简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。

    `Go` 语言可以使用  `encoding/json` 这个库提供的 `Marshal` 函数序列化，`Unmarshal` 函数反序列化。

    ```go
    func Marshal(v any) ([]byte, error)
    ```

    ```go
    func Unmarshal(data []byte, v any) error
    ```

    ```go
    func Test_C_27(t *testing.T) {
    	m := Man{"Tom", 18}
    	jsonByte, ok := json.Marshal(m)
    	var str string = string(jsonByte)
    	fmt.Println(str, ok)
    	var myJsonStr string = "{\"Name\":\"xxcheng\",\"Age\":22}"
    	var m2 Man
    	err := json.Unmarshal([]byte(myJsonStr), &m2)
    	fmt.Printf("%v,%#v", err, m2)
    }
    ```

    ```
    {"Name":"Tom","Age":18} <nil>
    <nil>,main.Man{Name:"xxcheng", Age:22}
    ```

  - #### 结构体标签

    `Tag` 是结构体的元信息，可以在运行的时候通过反射的机制读取出来。

    在结构体字段的后面使用反引号（`` ` ``）括起来，多对使用空格分割

    ```go
    `key1:"value1" key2:"value2"`
    ```

    - ##### 标签与序列化相结合

      当我们把一个结构体确认之后，如果想修改对应生成的 `json` 的字段，这是在生成环境来说是不可能的，使用我们可以利用标签来自定义 `json` 的字段，我们添加一个字段名为 `json` 的键值对即可

      ```go
      type Woman struct {
      	Name string `json:"name" desc:"姓名"`
      	Age  int    `json:"aaa"`
      }
      
      func (w Woman) Walk() {
      	fmt.Println("女人正在散步...")
      }
      func Test_C_28(t *testing.T) {
      	w := Woman{"Jerry", 18}
      	jsonByte, _ := json.Marshal(w)
      	fmt.Printf("%s\n", jsonByte)
      }
      ```

      ```
      {"name":"Jerry","aaa":18}
      ```

- ### 参考链接

  - [Go结构体的内存布局](https://www.liwenzhou.com/posts/Go/struct_memory_layout/)
  - [JSON](https://baike.baidu.com/item/JSON/2462549)
  - [json package - encoding/json - Go Packages](https://pkg.go.dev/encoding/json)