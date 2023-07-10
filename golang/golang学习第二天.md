- ### 指针（入门水准）

  ```go
  package main
  
  import "fmt"
  
  func addNumberOne(p int) {
  	p += 1
  }
  
  func addNumberOnePointer(p *int) {
  	*p += 1
  }
  
  func main() {
  	var a int = 10
  	addNumberOne(a)
  	fmt.Printf("addNumberOne after,a=%d\n", a)
  	addNumberOnePointer(&a)
  	fmt.Printf("addNumberOnePointer after,a=%d\n", a)
  }
  ```

  ![image-20230623122723272](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/23/62f01e4b9abcd0d6c8f572da008e8a0c.png)

  ```go
  package main
  
  import "fmt"
  
  func swap(a, b *int) {
  	var temp int
  	temp = *a
  	*a = *b
  	*b = temp
  }
  
  func main() {
  	var a, b = 10, 20
  	fmt.Printf("swap before,a=%d,b=%d\n", a, b)
  	swap(&a, &b)
  	fmt.Printf("swap after,a=%d,b=%d", a, b)
  }
  ```

  ![image-20230623123144970](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/23/408ebf8fd16d836612917158459aa0bb.png)
  
- ### `defer`知识点

  在一个函数退出执行之前运行，在`return`之后执行，执行顺序按照栈的关系，先进后执行。

  ```go
  package main
  
  import "fmt"
  
  func fnDefer() {
  	fmt.Println("我是fnDefer")
  }
  
  func fnReturn() int {
  	fmt.Println("我是fnReturn")
  	return 0
  }
  
  func sayHello() int {
  	defer fmt.Println("我是C")
  	fmt.Println("Hello")
  
  	defer fmt.Println("我是D")
  	defer fnDefer()
  	return fnReturn()
  }
  
  func main() {
  	defer fmt.Println("我是A")
  
  	fmt.Println("哈哈哈哈")
  	sayHello()
  	fmt.Println("呜呜呜")
  	defer fmt.Println("我是B")
  }
  ```

  ![image-20230623141659521](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/23/ebe6eeb339a0d6dbe44feef8b842c03c.png)

- ### 数组与切片（slice）

  数组的定义

  ```go
  var arr1 = [10]int{1, 2, 3, 4}
  var arr2 [10]int
  arr3 := [10]int{1, 2, 3}
  ```

  切片的定义，就是相当于动态数组

  ```go
  //方法一，初始化阶段就分配值
  //当前切片长度为3，值为1、2、3
  var slice1 = []int{1, 2, 3}
  //方法二，先定义，后使用make分配空间
  var slice2 []int
  slice2 = make([]int, 5)
  //方法三，初始化阶段就使用make分配空间
  slice3 := make([]int, 6)
  ```

  - `nil`表示空切片

  - 使用`len()`可以获取切片的长度，`cap()`获取切片的实际容量

  - `append()`方法可以追加元素

    ```go
    package main
    
    import "fmt"
    
    func main() {
    	slice1 := []int{44, 55, 66, 77}
    	fmt.Printf("slice1:len=%d,cap=%d\n", len(slice1), cap(slice1))
    	slice2 := append(slice1, 99)
    	fmt.Printf("slice1:len=%d,cap=%d\n", len(slice1), cap(slice1))
    	fmt.Printf("slice2:len=%d,cap=%d\n", len(slice2), cap(slice2))
    
    	slice2[0] = 888
    	fmt.Println("---------")
    	fmt.Printf("slice1:len=%d,cap=%d\n", len(slice1), cap(slice1))
    	fmt.Printf("slice2:len=%d,cap=%d\n", len(slice2), cap(slice2))
    	fmt.Println("slice1[0]:", slice1[0])
    	fmt.Println("slice2[0]:", slice2[0])
    }
    ```

    ![image-20230623154113150](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/23/8bc4be226d7189f9c21d26462455dbd1.png)

  - `slice[start:end]`可以截取指定范围`[start,end)`的元素
    但是这种方法，还是指向原来的内存空间，对原来的切片修改，会改变现在的值

    ```go
    package main
    
    import "fmt"
    
    func main() {
    	slice1 := []int{44, 55, 66, 77}
    	slice2 := slice1[1:4]
    	for _, value := range slice2 {
    		fmt.Println(value)
    	}
    	slice1[2] = 999
    	fmt.Println("---------------")
    	for _, value := range slice2 {
    		fmt.Println(value)
    	}
    }
    ```

    ![image-20230623155049331](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/23/cdc17786f1e0a52ff3d54baad3de8b63.png)

  - `copy()`方法可以拷贝，和原来的空间没有关联

    ```go
    package main
    
    import "fmt"
    
    func main() {
    	slice1 := []int{44, 55, 66, 77}
    	var slice2 []int = make([]int, 3)
    	copy(slice2, slice1)
    	for _, value := range slice2 {
    		fmt.Println(value)
    	}
    	slice1[2] = 999
    	fmt.Println("---------------")
    	for _, value := range slice2 {
    		fmt.Println(value)
    	}
    }
    ```

    ![image-20230623155118956](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/23/2a40828b438278b500182f59e39a238b.png)

  数组是值传递，切片也是值传递，但是是指针值传递

  ```go
  package main
  
  import "fmt"
  
  func array_change(arr [5]int) {
  	arr[0] = 888
  }
  
  func slice_change(arr []int) {
  	arr[0] = 777
  }
  
  func main() {
  	var arr1 = [5]int{9, 8, 7, 6}
  	var arr2 = []int{9, 8, 7, 6}
  
  	array_change(arr1)
  	slice_change(arr2)
  
  	fmt.Println("------------")
  	for _, value := range arr1 {
  		fmt.Printf("value=%d\n", value)
  	}
  
  	fmt.Println("------------")
  	for _, value := range arr2 {
  		fmt.Printf("value=%d\n", value)
  	}
  }
  ```

  ![image-20230623144944504](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/23/3481b7e893ffbe4160a522bf169c1d5c.png)

- ### map知识点

  - 声明方式

    ```go
    //方式一：
    var map1 map[string]string
    //在使用前，需要给map分配空间
    map1=make(map[string]string,10)
    //方式二：
    map2 :=make(map[string]string)
    //方式三：
    //这种方法最后一行必须也保留逗号
    map2 :=map[string]string{
        "one":"xxcheng",
        "name":"jpc",
    }
    ```

  - 添加/修改

    ```go
    map1["name"]="xxcheng"
    ```

  - 遍历

    ```go
    for key,value:=range map1{
        //do something
    }
    ```

  - 删除

    ```go
    delete(map1,"name")
    ```

  - 练习

    ```go
    package main
    
    import "fmt"
    
    func main() {
    	map1 := make(map[string]string)
    
    	//添加
    	map1["name"] = "xxcheng"
    	map1["sex"] = "男"
    	map1["age"] = "23"
    
    	//遍历
    	for key, value := range map1 {
    		fmt.Printf("key:%s,value:%s\n", key, value)
    	}
    	//删除
    	delete(map1, "sex")
    	//修改
    	map1["age"] = "22"
    	fmt.Println("--------")
    	for key, value := range map1 {
    		fmt.Printf("key:%s,value:%s\n", key, value)
    	}
    
    	fmt.Printf("len:%d", len(map1))
    }
    ```

    ![image-20230623163801192](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/23/6033d881c84cf09a1ed50d9cdd6ed4d9.png)

- ### 结构体`struct`

  `type myint int` 声明一种新的数据类型myint，是int的别名

  - 定义

    **类型、属性名、方法名首字母大写表示对外可以访问，否则只能在本包内访问**

    如果涉及到修改属性的方法，需要使用指针的方式表示`this`，否则修改无效

    ```go
    type Book struct{
        name string
        author string
        price float32
    }
    ```

  - 使用

    ```go
    package main
    
    import "fmt"
    
    type Book struct {
    	title  string
    	author string
    	price  float32
    }
    
    func (this Book) SayName() {
    	fmt.Println("Book name is ", this.title)
    }
    func (this Book) SetAuthor(author string) {
    	this.author = author
    }
    func (this *Book) SetPrice(price float32) {
    	this.price = price
    }
    
    func main() {
    	book := Book{
    		title:  "我的天",
    		author: "无敌",
    		price:  3.99,
    	}
    	book.SayName()
    	book.SetAuthor("www")
    	book.SetPrice(9.99)
    	fmt.Println("author:", book.author, ",price", book.price)
    }
    ```

    ![image-20230623171053137](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/23/e8555059cab23c0eefd24199238979b0.png)

  - 封装

  - 继承

    - 格式

      ```go
      type Person struct {
      	name string
      	age  int
      }
      
      type Man struct {
      	Person
      	like string
      }
      ```

    - 添加新方法

      ```go
      func (this Man) printLike() {
      	fmt.Println("[", this.name, "]like ", this.like)
      }
      ```

    - 重写父类方法

      ```go
      func (this Person) printAge() {
      	fmt.Println("[", this.name, "]age is ", this.age)
      }
      
      func (this Man) printAge() {
      	fmt.Println("[", this.name, "] is man ,his age is ", this.age)
      }
      ```

    - 合并运行

      ```go
      package main
      
      import "fmt"
      
      type Person struct {
      	name string
      	age  int
      }
      
      func (this Person) walk() {
      	fmt.Println("[", this.name, "]正在walk...")
      }
      func (this Person) printAge() {
      	fmt.Println("[", this.name, "]age is ", this.age)
      }
      
      type Man struct {
      	Person
      	like string
      }
      
      func (this Man) printLike() {
      	fmt.Println("[", this.name, "]like ", this.like)
      }
      func (this Man) printAge() {
      	fmt.Println("[", this.name, "] is man ,his age is ", this.age)
      }
      
      func main() {
      	person := Person{
      		name: "xxcheng",
      		age:  18,
      	}
      	person.walk()
      	person.printAge()
      
      	man := Man{
      		Person{
      			name: "jpc",
      			age:  21,
      		},
      		"swimming",
      	}
      
      	man.walk()
      	man.printAge()
      	man.printLike()
      }
      ```

      ![image-20230623173627948](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/23/56874e8bb206017698d63c4ddd8c4fbe.png)

  - 动态

    多态是指在父类中定义的属性和方法被子类继承之后，可以具有不同的数据类型或表现出不同的行为，这使得同一个属性或方法在父类及其各个子类中具有不同的含义。

    在Go语言中，通过对接口的实现，来实现动态

    ```go
    package main
    
    import "fmt"
    
    type Animal interface {
    	Eat()
    	Say()
    	SetName(name string)
    }
    
    type Panda struct {
    	typeName string
    	name     string
    }
    
    func (p Panda) Eat() {
    	fmt.Println(p.name, "正在吃笋")
    }
    func (p Panda) Say() {
    	fmt.Println(p.name, "说，我是", p.typeName)
    }
    
    func (p *Panda) SetName(name string) {
    	p.name = name
    }
    
    type Bird struct {
    	typeName string
    	name     string
    }
    
    func (p Bird) Eat() {
    	fmt.Println(p.name, "正在吃小米")
    }
    func (p Bird) Say() {
    	fmt.Println(p.name, "说，我是", p.typeName)
    }
    
    func (p *Bird) SetName(name string) {
    	p.name = name
    }
    
    func main() {
    	var animal Animal
    	animal = &Panda{"大熊猫", "哇哇"}
    	animal.Say()
    	animal.Eat()
    	animal.SetName("威威")
    
    	animal = &Panda{"鸟", "嗯嗯"}
    	animal.Say()
    	animal.Eat()
    }
    ```

    ![image-20230623180400573](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/23/56342dd8a443305cf7a6d63b5ba52d2b.png)

- ### 接口`interface`

  `interface{}`是万能数据类型

  - 断言机制(只能用于`interface{}`类型中)

    ```go
    //值,布尔  :=变量.(断言的数据类型)
    value,ok :=arg.(string)
    if ok {
        fmt.Println("arg is string type")
    }else{
     	fmt.Println("arg is not string type")   
    }
    ```

    测试

    ```go
    package main
    
    import "fmt"
    
    func main() {
    	var arg interface{} = "abc"
    
    	value, ok := arg.(string)
    	if ok {
    		fmt.Println("arg is string type")
    	} else {
    		fmt.Println("arg is not string type")
    	}
    	fmt.Println(value)
    }
    ```

    ![image-20230623202351320](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/23/a43d66eecd4a2fe91dcae0e74b68f858.png)

- ### 变量内置的`pair`

  ![关系图](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/23/0ec44818051bbe82adf767586739235c.png)

- ### 反射

  ```go
  package main
  
  import (
  	"fmt"
  	"reflect"
  )
  
  type Student struct {
  	name string
  	age  int
  	sex  int
  }
  
  func (s Student) SayHello() {
  	fmt.Println("你好，我叫", s.name, "，年龄：", s.age, "，性别：", s.sex)
  }
  
  func main() {
  	s := Student{"xxcheng", 18, 1}
  	s.SayHello()
  	//获取type
  	sType := reflect.TypeOf(s)
  	fmt.Println("type:", sType.Name())
  	//获取value
  	sValue := reflect.ValueOf(s)
  	fmt.Println("value:", sValue)
  	//获取字段
  	for i := 0; i < sType.NumField(); i++ {
  		field := sType.Field(i)
  		value := sValue.Field(i)
  		fmt.Println("type:", field.Type, ",name:", field.Name, ",value:", value)
  	}
  
  	fmt.Println("NumMethod:", sType.NumMethod())
  	//获取方法
  	for i := 0; i < sType.NumMethod(); i++ {
  		field := sType.Method(i)
  		fmt.Println("type:", field.Type, ",name:", field.Name)
  	}
  }
  ```

  ![image-20230623212854546](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/23/6708400e984ef799155e7f82db29703e.png)