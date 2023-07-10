- # 异常处理

  `Go` 没有结构化的异常，只能使用 `panic` 内置函数抛出异常，`recover` 内置函数在 `defer` 中捕获异常，然后程序正常运行。

  - ## `panic`

    ```go
    func panic(v any)
    ```

    - 抛出异常的内置函数；
    - 可以在任何地方调用；

  - ## `recover`

    ```go
    func recover() any
    ```

    - 捕获 `panicking` 行为的函数；
    - 可以在任何地方定义，但是只有在 `defer` 定义的函数或者调用的函数执行，才能起到实际作用，其他地方返回值恒为 `nil`；

  - ## 有效的 `recover`

    ```go
    func Test_07_1(t *testing.T) {
    	fmt.Println("Test_07_1")
    	defer func() {
    		err := recover() // 有效
    		fmt.Println("func recover", err)
    	}()
    	defer recover() // 无效
    	defer fmt.Println(recover()) // 无效
    	defer func() {
    		func() {
    			err := recover() // 无效
    			fmt.Println("inner recover", err)
    		}()
    	}()
    	recover() // 无效
    	panic("故意的错误")
    }
    ```

    ```
    Test_07_1
    inner recover <nil>
    <nil>
    func recover 故意的错误
    ```
  
  - ## 自定义 `error`

    `error` 是一个内置接口

    ```go
    type error interface {
    	Error() string
    }
    ```

    所以主要实现了 `Error` 方法，就是一个 `error`

    ```go
    type MyError int
    
    func (m MyError) Error() string {
    	str := fmt.Sprintf("这是我自定义的一个Error，错误代码：%d", int(m))
    	return str
    
    }
    func getError(i int) error {
    	var e MyError = MyError(i)
    	return e
    }
    func Test_07_5(t *testing.T) {
    	defer func() {
    		if err := recover(); err != nil {
    			fmt.Println(err)
    		}
    	}()
    	err := getError(999)
    	if err != nil {
    		panic(err)
    	}
    }
    ```

    ```shell
    [root@CentOS single]# go test -v  day07_test.go -run Test_07_5
    === RUN   Test_07_5
    这是我自定义的一个Error，错误代码：999
    --- PASS: Test_07_5 (0.00s)
    PASS
    ok      command-line-arguments  0.003
    ```
  
  如果程序被 `panic` 抛出异常，那么之后所有的程序都不会再运行了，因此定义在 `panic` 之后的 `defer` 因为还没有被定义，所以也不会执行。
  
- # 单元测试

  使用 `go test` 命令，对包内以 `_test.go` 后缀执行测试，并且不会被 `go build` 编译到可执行文件中。

  - ## 分类

    |   类型   |        格式         |          作用          |
    | :------: | :-----------------: | :--------------------: |
    | 测试函数 | 函数前缀名为 `Test` | 测试函数的逻辑是否正确 |
    | 基准函数 | 函数前缀名为 `Benchmark` | 测试函数的性能 |
    | 示例函数 | 函数前缀名为 `Example` | 为文档提供测试用例 |

  - ## 文件名 & 方法名规范

    - 文件名必须以 `_test.go` 结尾，比如 `day07_test.go`；
    - 测试函数的函数名必须以 `Test` 开头，基准函数的函数名必须以 `Benchmark` 开头，示例函数的函数名必须以 `Example` 开头，且它们之后的第一个字母必须大写；
    - 测试函数的参数类型必须是 `* testing.T`，基准函数的参数类型必须是 `*testing.B`，示例函数的没有参数；

  - ## 参数（常用）

  - ## 测试函数

    - ### 基本格式

      以 `Test` 为前缀，接收一个 `*testing.T`类型的参数

      ```go
      func TestName(t *testing.T){
          // ...
      }
      ```
    
      `testing.T` 拥有的方法：
    
      ```go
      func (c *T) Error(args ...interface{})
      func (c *T) Errorf(format string, args ...interface{})
      func (c *T) Fail()
      func (c *T) FailNow()
      func (c *T) Failed() bool
      func (c *T) Fatal(args ...interface{})
      func (c *T) Fatalf(format string, args ...interface{})
      func (c *T) Log(args ...interface{})
      func (c *T) Logf(format string, args ...interface{})
      func (c *T) Name() string
      func (t *T) Parallel()
      func (t *T) Run(name string, f func(t *T)) bool
      func (c *T) Skip(args ...interface{})
      func (c *T) SkipNow()
      func (c *T) Skipf(format string, args ...interface{})
      func (c *T) Skipped() bool
      ```
    
      调用其中的 `Error`、`Errorf`、`FailNow`、`FatalFatalIf` 方法，说明测试不通过
    
    - ### 常用方法
    
      - #### `Error` 、`Errorf`
    
        抛出错误
    
      - #### `Log`、`Logf`
    
        记录信息
    
    - ### 基本测试
    
      目录结构：
    
      ```shell
      .
      ├── go.mod
      ├── join.go
      └── join_test.go
      
      0 directories, 3 files
      ```
    
      *join.go*
    
      ```go
      package join
      
      func Join(strSlice []string, sep string) (s string) {
      	if len(strSlice) == 0 {
      		return
      	}
      	for _, value := range strSlice {
      		s += value + "-"
      	}
      	length := len(s)
      	s = s[:length-1]
      	return
      }
      ```
    
      - #### 单函数
    
        *join_test.go*
    
        ```go
        package join
        
        import (
        	"fmt"
            "reflect"
        	"testing"
        )
        
        func TestJoin(t *testing.T) {
        	slice := []string{"ABC", "XYZ", "WWW"}
        	got := Join(slice, "-")
        	want := "ABC-XYZ-WWW"
        	if !reflect.DeepEqual(got, want) {
        		t.Errorf("excepted:%v, got:%v", want, got)
        	}
        }
        ```
    
        ```shell
        [root@CentOS join]# go test
        PASS
        ok      join    0.002s
        ```
    
      - #### 多函数
    
        *join_test.go*
    
        ```go
        package join
        
        import (
        	"reflect"
        	"testing"
        )
        
        func TestJoin(t *testing.T) {
        	slice := []string{"ABC", "XYZ", "WWW"}
        	got := Join(slice, "-")
        	want := "ABC-XYZ-WWW"
        	if !reflect.DeepEqual(got, want) {
        		t.Errorf("excepted:%v, got:%v", want, got)
        	}
        }
        func TestJoinTwo(t *testing.T) {
        	slice := []string{"中国", "湖北", "黄冈", "黄冈师范学院", "HGNU"}
        	got := Join(slice, "-")
        	want := "中国-湖北-黄冈-黄冈师范学院-HGNU"
        	if !reflect.DeepEqual(got, want) {
        		t.Errorf("excepted:%v, got:%v", want, got)
        	}
        }
        ```
    
        ```shell
        [root@CentOS join]# go test
        PASS
        ok      join    0.002s
        ```
    
      - #### 测试不通过示例
    
        *join_test.go*
    
        ```go
        package join
        
        import (
        	"reflect"
        	"testing"
        )
        
        func TestJoin(t *testing.T) {
        	slice := []string{"ABC", "XYZ", "WWW"}
        	got := Join(slice, "-")
        	want := "ABC-XYZ-WWW"
        	if !reflect.DeepEqual(got, want) {
        		t.Errorf("excepted:%v, got:%v", want, got)
        	}
        }
        func TestJoinTwo(t *testing.T) {
        	slice := []string{"中国", "湖北", "黄冈", "黄冈师范学院", "HGNU"}
        	got := Join(slice, "-")
        	want := "中国-湖北-黄冈-黄冈师范学院-HGNU"
        	if !reflect.DeepEqual(got, want) {
        		t.Errorf("excepted:%v, got:%v", want, got)
        	}
        }
        
        func TestJoinThree(t *testing.T) {
        	slice := []string{}
        	got := Join(slice, "-")
        	want := "null"
        	if !reflect.DeepEqual(got, want) {
        		t.Errorf("excepted:%v, got:%v", want, got)
        	}
        }
        ```
    
        ```shell
        [root@CentOS join]# go test
        --- FAIL: TestJoinThree (0.00s)
            join_test.go:30: excepted:null, got:
        FAIL
        exit status 1
        FAIL    join    0.002s
        ```
    
      - #### 查看每个参数函数具体信息 `-v` 参数
    
        直接使用 `go test` 在这么多函数执行时不能查看每个函数的运行时间，这里可以添加 `-v` 参数查看每一个测试函数的具体信息
    
        - ##### 执行 `多函数` 示例 `join_test.go` 文件
    
          ```shell
          [root@CentOS join]# go test -v
          === RUN   TestJoin
          --- PASS: TestJoin (0.00s)
          === RUN   TestJoinTwo
          --- PASS: TestJoinTwo (0.00s)
          PASS
          ok      join    0.002s
          ```
    
        - ##### 执行 `测试不通过示例` 示例 `join_test.go` 文件
    
          ```shell
          [root@CentOS join]# go test -v
          === RUN   TestJoin
          --- PASS: TestJoin (0.00s)
          === RUN   TestJoinTwo
          --- PASS: TestJoinTwo (0.00s)
          === RUN   TestJoinThree
              join_test.go:30: excepted:null, got:
          --- FAIL: TestJoinThree (0.00s)
          FAIL
          exit status 1
          FAIL    join    0.002s
          ```
    
      - #### 指定执行某个测试函数 `-run` 参数
    
        `-run` 参数接收一个正则表达式，函数名匹配上的测试函数会被执行
    
        *join_test.go*
    
        ```go
        package join
        
        import (
        	"reflect"
        	"testing"
        )
        
        func TestJoin(t *testing.T) {
        	slice := []string{"ABC", "XYZ", "WWW"}
        	got := Join(slice, "-")
        	want := "ABC-XYZ-WWW"
        	if !reflect.DeepEqual(got, want) {
        		t.Errorf("excepted:%v, got:%v", want, got)
        	}
        }
        func TestJoinTwo(t *testing.T) {
        	slice := []string{"中国", "湖北", "黄冈", "黄冈师范学院", "HGNU"}
        	got := Join(slice, "-")
        	want := "中国-湖北-黄冈-黄冈师范学院-HGNU"
        	if !reflect.DeepEqual(got, want) {
        		t.Errorf("excepted:%v, got:%v", want, got)
        	}
        }
        
        func TestJoinTwo2(t *testing.T) {
        	slice := []string{"中国", "浙江", "衢州", "xxcheng"}
        	got := Join(slice, "-")
        	want := "中国-浙江-衢州-xxcheng"
        	if !reflect.DeepEqual(got, want) {
        		t.Errorf("excepted:%v, got:%v", want, got)
        	}
        }
        ```
    
        ```shell
        [root@CentOS join]# go test -v -run JoinTwo
        === RUN   TestJoinTwo
        --- PASS: TestJoinTwo (0.00s)
        === RUN   TestJoinTwo2
        --- PASS: TestJoinTwo2 (0.00s)
        PASS
        ok      join    0.001s
        ```
    
        `TestJoin` 函数未匹配，没有被执行
    
    - ### 测试组
    
      每个测试都要写一个单独的函数没有必要，可以将多个测试用例写成一个数组或者切片，遍历运行。
    
      *join_test.go*
    
      ```go
      package join
      
      import (
      	"reflect"
      	"testing"
      )
      
      func TestJoin(t *testing.T) {
      	type test struct {
      		input []string
      		sep   string
      		want  string
      	}
      	tests := []test{
      		{[]string{"ABC", "XYZ", "WWW"}, "-", "ABC-XYZ-WWW"},
      		{[]string{"中国", "湖北", "黄冈", "黄冈师范学院", "HGNU"}, "-", "中国-湖北-黄冈-黄冈师范学院-HGNU"},
      	}
      	for _, tc := range tests {
      		got := Join(tc.input, tc.sep)
      		if !reflect.DeepEqual(got, tc.want) {
      			t.Errorf("excepted:%v, got:%v", tc.want, got)
      		}
      	}
      }
      
      ```
    
      ```sh
      [root@CentOS join]# go test -v
      === RUN   TestJoin
      --- PASS: TestJoin (0.00s)
      PASS
      ok      join    0.002s
      ```
    
      *join_test.go*
    
      ```go
      package join
      
      import (
      	"reflect"
      	"testing"
      )
      
      func TestJoin(t *testing.T) {
      	type test struct {
      		input []string
      		sep   string
      		want  string
      	}
      	tests := []test{
      		{[]string{"ABC", "XYZ", "WWW"}, "-", "ABC-XYZ-WWW"},
      		{[]string{"中国", "湖北", "黄冈", "黄冈师范学院", "HGNU"}, "-", "中国-湖北-黄冈-黄冈师范学院-HGNU"},
      		{[]string{}, "-", "null"},
      	}
      	for _, tc := range tests {
      		got := Join(tc.input, tc.sep)
      		if !reflect.DeepEqual(got, tc.want) {
      			t.Errorf("excepted:%v, got:%v", tc.want, got)
      		}
      	}
      }
      ```
    
      ```shell
      [root@CentOS join]# go test -v
      === RUN   TestJoin
          join_test.go:22: excepted:null, got:
      --- FAIL: TestJoin (0.00s)
      FAIL
      exit status 1
      FAIL    join    0.042s
      ```
    
    - ### 子测试
    
      使用测试组虽然可以测试多组数据，但是如果在测试用例较多的情况下，出现了失败的测试用例，无法很快定位是哪个测试用例，所以我们可以给每一个测试用例设置一个名称，失败的时候将名称也打印输出，`Go1.7` 版本之后提供了子测试的函数 `T.Run`
    
      ```go 
      func (t *T) Run(name string, f func(t *T)) bool
      ```
    
      *join_test.go*
    
      ```go
      package join
      
      import (
      	"reflect"
      	"testing"
      )
      
      func TestJoin(t *testing.T) {
      	type test struct {
      		input []string
      		sep   string
      		want  string
      	}
      	tests := map[string]test{
      		"simple": {[]string{"ABC", "XYZ", "WWW"}, "-", "ABC-XYZ-WWW"},
      		"hgnu":   {[]string{"中国", "湖北", "黄冈", "黄冈师范学院", "HGNU"}, "-", "中国-湖北-黄冈-黄冈师范学院-HGNU"},
      		"abc":    {[]string{}, "-", "null"},
      	}
      	for name, tc := range tests {
      		t.Run(name, func(t *testing.T) {
      			got := Join(tc.input, tc.sep)
      			if !reflect.DeepEqual(got, tc.want) {
      				t.Errorf("excepted:%v, got:%v", tc.want, got)
      			}
      		})
      	}
      }
      ```
    
      ```shell
      [root@CentOS join]# go test -v
      === RUN   TestJoin
      === RUN   TestJoin/simple
      === RUN   TestJoin/hgnu
      === RUN   TestJoin/abc
          join_test.go:23: excepted:null, got:
      --- FAIL: TestJoin (0.00s)
          --- PASS: TestJoin/simple (0.00s)
          --- PASS: TestJoin/hgnu (0.00s)
          --- FAIL: TestJoin/abc (0.00s)
      FAIL
      exit status 1
      FAIL    join    0.002s
      ```
    
      可以很直观的看从来是 `abc` 这个测试用例出现了问题
    
      同时，它也可以使用 `-run` 正则指定运行
    
      ```shell
      [root@CentOS join]# go test -v -run TestJoin/hgnu
      === RUN   TestJoin
      === RUN   TestJoin/hgnu
      --- PASS: TestJoin (0.00s)
          --- PASS: TestJoin/hgnu (0.00s)
      PASS
      ok      join    0.002s
      ```
    
    - ### 测试覆盖率
    
      检查我们写的测试函数是否覆盖了我们写的代码，检查我们的覆盖率
    
      使用 `go test -cover` 检查覆盖率
    
      ```shell
      [root@CentOS join]# go test -cover
      --- FAIL: TestJoin (0.00s)
          --- FAIL: TestJoin/abc (0.00s)
              join_test.go:23: excepted:null, got:
      FAIL
              join    coverage: 100.0% of statements
      exit status 1
      FAIL    join    0.002s
      ```
    
      ![image-20230707112119260](https://cdn-static.xxcheng.cn/static/blog/images/2023/07/07/8ce1a1cc1a9fc2cd9ae27944f8f054e2.png)
    
      使用 `-coverprofile` 参数可以将检查结果输出到一个文件
    
      ```shell
      [root@CentOS join]# go test -cover -coverprofile=c.out
      --- FAIL: TestJoin (0.00s)
          --- FAIL: TestJoin/abc (0.00s)
              join_test.go:23: excepted:null, got:
      FAIL
              join    coverage: 100.0% of statements
      exit status 1
      FAIL    join    0.004s
      [root@CentOS join]# tree
      .
      ├── c.out
      ├── go.mod
      ├── join.go
      └── join_test.go
      
      0 directories, 4 files
      [root@CentOS join]# cat c.out
      mode: set
      join/join.go:3.53,4.24 1 1
      join/join.go:4.24,6.3 1 1
      join/join.go:7.2,7.33 1 1
      join/join.go:7.33,9.3 1 1
      join/join.go:10.2,12.8 3 1
      ```
    
      如果在 `join.go` 文件添加一个 `SayHello` 函数再测试
    
      ```go
      func SayHello() {
      	fmt.Println("Hello World!")
      }
      ```
    
      ```shell
      [root@CentOS join]# go test -cover
      --- FAIL: TestJoin (0.00s)
          --- FAIL: TestJoin/abc (0.00s)
              join_test.go:23: excepted:null, got:
      FAIL
              join    coverage: 87.5% of statements
      exit status 1
      FAIL    join    0.003s
      ```
    
      覆盖率降为 `87.5%`
    
  - ## 基准测试

    - ### 基本格式

      以 `Benchmark` 为前缀，接收一个 `*testing.B` 类型的参数，函数执行 `b.N` 次，来测试性能，`N`  的值根据系统实时自动调整的，**不是固定不变的**

      使用 `-test.bench` 参数，可省略为 `-bench`
      
      ```go
      func BenchmarkName(b *testing.B){
          // ...
    }
      ```

      `testing.B` 拥有的方法：
      
      ```go
      func (c *B) Error(args ...interface{})
      func (c *B) Errorf(format string, args ...interface{})
      func (c *B) Fail()
      func (c *B) FailNow()
      func (c *B) Failed() bool
      func (c *B) Fatal(args ...interface{})
      func (c *B) Fatalf(format string, args ...interface{})
      func (c *B) Log(args ...interface{})
      func (c *B) Logf(format string, args ...interface{})
      func (c *B) Name() string
      func (b *B) ReportAllocs()
      func (b *B) ResetTimer()
      func (b *B) Run(name string, f func(b *B)) bool
      func (b *B) RunParallel(body func(*PB))
      func (b *B) SetBytes(n int64)
      func (b *B) SetParallelism(p int)
      func (c *B) Skip(args ...interface{})
      func (c *B) SkipNow()
      func (c *B) Skipf(format string, args ...interface{})
      func (c *B) Skipped() bool
      func (b *B) StartTimer()
      func (b *B) StopTimer()
      ```
      
    - ### 基本测试

      *join_test.go*

      ```go
      package join
      
      import (
      	"reflect"
      	"testing"
      )
      
      func TestJoin(t *testing.T) {
      	type test struct {
      		input []string
      		sep   string
      		want  string
      	}
      	tests := map[string]test{
      		"simple": {[]string{"ABC", "XYZ", "WWW"}, "-", "ABC-XYZ-WWW"},
      		"hgnu":   {[]string{"中国", "湖北", "黄冈", "黄冈师范学院", "HGNU"}, "-", "中国-湖北-黄冈-黄冈师范学院-HGNU"},
      		// "abc":    {[]string{}, "-", "null"},
      	}
      	for name, tc := range tests {
      		t.Run(name, func(t *testing.T) {
      			got := Join(tc.input, tc.sep)
      			if !reflect.DeepEqual(got, tc.want) {
      				t.Errorf("excepted:%v, got:%v", tc.want, got)
      			}
      		})
      	}
      }
      func BenchmarkJoin(b *testing.B) {
      	slice := []string{"ABC", "XYZ", "WWW"}
      	for i := 0; i < b.N; i++ {
      		Join(slice, "-")
      	}
      }
      ```

      ```shell
      [root@CentOS join]# go test -v -bench=Join
      === RUN   TestJoin
      === RUN   TestJoin/simple
      === RUN   TestJoin/hgnu
      --- PASS: TestJoin (0.00s)
          --- PASS: TestJoin/simple (0.00s)
          --- PASS: TestJoin/hgnu (0.00s)
      goos: linux
      goarch: amd64
      pkg: join
      cpu: AMD EPYC Processor
      BenchmarkJoin
      BenchmarkJoin-8          7051003               171.1 ns/op
      PASS
      ok      join    1.519s
      ```

      ##### 相关结果说明：

      - `BenchmarkJoin-8`，`8` 表示 `GOMAXPROCS`
      - `7051003` 调用次数
      - `171.1 ns/op` 调用7051003次平均执行时间

      可以再添加一个 `-benchmem` 参数，统计内存相关数据

      ```go
      [root@CentOS join]# go test -v -bench=Join -benchmem
      === RUN   TestJoin
      === RUN   TestJoin/simple
      === RUN   TestJoin/hgnu
      --- PASS: TestJoin (0.00s)
          --- PASS: TestJoin/simple (0.00s)
          --- PASS: TestJoin/hgnu (0.00s)
      goos: linux
      goarch: amd64
      pkg: join
      cpu: AMD EPYC Processor
      BenchmarkJoin
      BenchmarkJoin-8          6850162               181.6 ns/op            32 B/op          3 allocs/op
      PASS
      ok      join    1.569s
      ```

      ##### 相关结果说明：

      - `32 B/op` 表示平均每次分配了32个字节内存
      - `3 allocs/op` 表示平均每次进行了3次内存分配

      下面对 `join.go` 的 `Join` 函数优化一下，再测试

      *join.go*

      ```go
      package join
      
      import (
      	"fmt"
      )
      
      func Join(strSlice []string, sep string) (s string) {
      	if len(strSlice) == 0 {
      		return
      	}
      	length := 0
      	for _, value := range strSlice {
      		length += len(value)
      	}
      	b := make([]byte, 0, length+len(strSlice)-1)
      	//使用strings.Count!@!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
      	for index, value := range strSlice {
      		b = append(b, []byte(value)...)
      		if index != len(strSlice)-1 {
      			b = append(b, '-')
      		}
      	}
      	return string(b)
      }
      ```

      ```she
      [root@CentOS join]# go test -v -bench=Join -benchmem
      === RUN   TestJoin
      === RUN   TestJoin/simple
      === RUN   TestJoin/hgnu
      --- PASS: TestJoin (0.00s)
          --- PASS: TestJoin/simple (0.00s)
          --- PASS: TestJoin/hgnu (0.00s)
      goos: linux
      goarch: amd64
      pkg: join
      cpu: AMD EPYC Processor
      BenchmarkJoin
      BenchmarkJoin-8         17493249                75.17 ns/op           16 B/op          1 allocs/op
      PASS
      ok      join    1.634s
      ```

      ##### 相关结果说明：

      - 程序执行耗时从之前的 `181.6 ns/op`  下降为 `75.17 ns/op`；
      - 内存分配字节从 `32 B/op` 下降为 `16 B/op`；
      - 内存分配次数从 `3 allocs/op` 下降为 `1 allocs/op`；

      有力的证明了，优化能够对程序带来的提升

    - ### 性能比较函数

      上面的基础的基准测试只能得到相对的测试结果，不能计算相对的耗时，比如同一个函数，运行100次和10000次的耗时差别，或者执行一个任务，使用哪种算法最优？

      这时候就可以让他们同时执行一个任务，然后固定执行多少次，这里以斐波那契数列为例：

      ```shell
      [root@CentOS fib]# tree
      .
      ├── fib.go
      ├── fib_test.go
      └── go.mod
      
      0 directories, 3 files
      ```

      *fib.go*

      ```go
      package fib
      
      func Fib(n int) int {
      	if n < 2 {
      		return n
      	}
      	return Fib(n-1) + Fib(n-2)
      }
      ```

      *fib_test.go*

      ```go
      package fib
      
      import "testing"
      
      func do_task(b *testing.B, n int) {
      	for i := 0; i < b.N; i++ {
      		Fib(n)
      	}
      }
      
      func BenchmarkFib1(b *testing.B) {
      	do_task(b, 1)
      }
      func BenchmarkFib2(b *testing.B) {
      	do_task(b, 2)
      }
      func BenchmarkFib3(b *testing.B) {
      	do_task(b, 3)
      }
      func BenchmarkFib5(b *testing.B) {
      	do_task(b, 5)
      }
      func BenchmarkFib10(b *testing.B) {
      	do_task(b, 10)
      }
      func BenchmarkFib50(b *testing.B) {
      	do_task(b, 50)
      }
      ```

      ```shell
      [root@CentOS fib]# go test -bench .
      goos: linux
      goarch: amd64
      pkg: fib
      cpu: AMD EPYC Processor
      BenchmarkFib1-8         476197117                2.549 ns/op
      BenchmarkFib2-8         216786968                6.100 ns/op
      BenchmarkFib3-8         128765414                9.580 ns/op
      BenchmarkFib5-8         45925664                31.64 ns/op
      BenchmarkFib10-8         3360859               328.3 ns/op
      BenchmarkFib50-8               1        81771779865 ns/op
      PASS
      ok      fib     90.914s
      ```

      `BenchmarkFib50` 只运行了一次，可能就会有误差，可以设置最小基准时间 `-benchtime`，增加运行次数

    - ### 重置运行时间

      有些时候会有有些无关紧要的操作比如 `time.Sleep` 影响测试结果，我们可以执行 `b.ResetTimer` 重置运行时间

      *join_test.go*

      ```go
      package join
      
      import (
      	"reflect"
      	"testing"
      	"time"
      )
      
      func TestJoin(t *testing.T) {
      	type test struct {
      		input []string
      		sep   string
      		want  string
      	}
      	tests := map[string]test{
      		"simple": {[]string{"ABC", "XYZ", "WWW"}, "-", "ABC-XYZ-WWW"},
      		"hgnu":   {[]string{"中国", "湖北", "黄冈", "黄冈师范学院", "HGNU"}, "-", "中国-湖北-黄冈-黄冈师范学院-HGNU"},
      		// "abc":    {[]string{}, "-", "null"},
      	}
      	for name, tc := range tests {
      		t.Run(name, func(t *testing.T) {
      			got := Join(tc.input, tc.sep)
      			if !reflect.DeepEqual(got, tc.want) {
      				t.Errorf("excepted:%v, got:%v", tc.want, got)
      			}
      		})
      	}
      }
      func BenchmarkJoin(b *testing.B) {
      	slice := []string{"ABC", "XYZ", "WWW"}
      	for i := 0; i < b.N; i++ {
      		Join(slice, "-")
      	}
      }
      
      func BenchmarkJoinSleep(b *testing.B) {
      	slice := []string{"ABC", "XYZ", "WWW"}
      	time.Sleep(time.Second * 1)
      	for i := 0; i < b.N; i++ {
      		Join(slice, "-")
      	}
      }
      func BenchmarkJoinSleepResetTimer(b *testing.B) {
      	slice := []string{"ABC", "XYZ", "WWW"}
      	time.Sleep(time.Second * 1)
      	b.ResetTimer()
      	for i := 0; i < b.N; i++ {
      		Join(slice, "-")
      	}
      }
      ```

      ```shell
      [root@CentOS join]# go test -bench .
      goos: linux
      goarch: amd64
      pkg: join
      cpu: AMD EPYC Processor
      BenchmarkJoin-8                         18337059                70.53 ns/op
      BenchmarkJoinSleep-8                           1        1000581868 ns/op
      BenchmarkJoinSleepResetTimer-8          17905347                66.42 ns/op
      PASS
      ok      join    10.953s
      ```

    - ### 并行测试

      `Go` 有个最大的优势就是并发，所以并行测试也必不可少，`*testing.B` 提供了 `*RunParallel` 函数进行并发测试

      由于虚拟机只有一个 `vCPU`，使用我自己电脑测试

      *join_test.go*

      ```go
      package join
      
      import (
      	"reflect"
      	"testing"
      )
      
      func TestJoin(t *testing.T) {
      	type test struct {
      		input []string
      		sep   string
      		want  string
      	}
      	tests := map[string]test{
      		"simple": {[]string{"ABC", "XYZ", "WWW"}, "-", "ABC-XYZ-WWW"},
      		"hgnu":   {[]string{"中国", "湖北", "黄冈", "黄冈师范学院", "HGNU"}, "-", "中国-湖北-黄冈-黄冈师范学院-HGNU"},
      		// "abc":    {[]string{}, "-", "null"},
      	}
      	for name, tc := range tests {
      		t.Run(name, func(t *testing.T) {
      			got := Join(tc.input, tc.sep)
      			if !reflect.DeepEqual(got, tc.want) {
      				t.Errorf("excepted:%v, got:%v", tc.want, got)
      			}
      		})
      	}
      }
      func BenchmarkJoin(b *testing.B) {
      	slice := []string{"ABC", "XYZ", "WWW"}
      	for i := 0; i < b.N; i++ {
      		Join(slice, "-")
      	}
      }
      func BenchmarkJoinParallel(b *testing.B) {
      	slice := []string{"ABC", "XYZ", "WWW"}
      	b.SetParallelism(8)
      	b.RunParallel(func(p *testing.PB) {
      		for p.Next() {
      			Join(slice, "-")
      		}
      	})
      }
      ```

      ```powershell
      D:\Desktop\join>go test -bench .
      goos: windows
      goarch: amd64
      pkg: join
      cpu: Intel(R) Core(TM) i5-9300H CPU @ 2.40GHz
      BenchmarkJoin-8                 22852267                48.23 ns/op
      BenchmarkJoinParallel-8         51907125                23.58 ns/op
      PASS
      ok      join    3.980s
      ```

  - ## 示例测试

    - ## 基本格式

      ```go
      func ExampleName() {
          // ...
          // Output
          // ...
      }
      ```

      需要注释一个 `Output`

      并且将预期输出的内容也注释出来

    - ### 基本示例

      *join_test.go*

      ```go
      package join
      
      import (
      	"fmt"
      )
      
      func ExampleJoin() {
      	fmt.Println(Join([]string{"ABC", "WWW", "CCTV"}, "-"))
      	// Output:
      	// ABC-WWW-CCTV
      }
      
      ```

      ```shell
      [root@CentOS join]# go test -run Example
      PASS
      ok      join    0.006s
      ```

      如果不加预期输出

      *join_test.go*

      ```go
      package join
      
      import (
      	"fmt"
      )
      
      func ExampleJoin() {
      	fmt.Println(Join([]string{"ABC", "WWW", "CCTV"}, "-"))
      }
      ```

      ```go
      [root@CentOS join]# go test -run Example
      testing: warning: no tests to run
      PASS
      ok      join    0.006s
      ```

      提示：`testing: warning: no tests to run`

  - ## `TestMain`

    用于测试程序在测试前额外的设置（`setup`）和测试之后额外的拆卸（`teardown`）

    如果我们没有自定义 `TestMain` 函数，默认的 `TestMain` 函数相当于下面这个：

    ```go
    func TestMain(m *testing.M) {
        // 测试之前的一些操作
        retCode := m.Run() 
    	// 测试之后的一些操作
  	os.Exit(retCode) //退出
    }
    ```
  ```
    
    `m.Run()` 就是执行测试，如果不调用，测试就直接退出了。
    
    - ### 读取命令行参数的示例
    
      *join_test.go*
    
      ```go
      package join
      
      import (
      	"flag"
      	"fmt"
      	"os"
      	"strings"
      	"testing"
      )
      
      func TestJoin(t *testing.T) {
      	str := Join(slice, "-")
      	fmt.Println(str)
      }
      
      var sliceStr string
      var slice []string
      
      func init() {
      	//变量,参数名,默认值,说明
      	flag.StringVar(&sliceStr, "str", "ABC,XYZ,WWW", "要合并的字符串数组，半角逗号分割（默认：ABC,XYZ,WWW）")
      }
      func TestMain(m *testing.M) {
      	fmt.Println("我执行了~~~")
      	flag.Parse()
      	slice = strings.Split(sliceStr, ",")
      	retCode := m.Run()
      	fmt.Println("执行之后的一些操作")
      	os.Exit(retCode)
      }
  ```

      ```shell
      [root@CentOS join]# go test -str 123,ABC,WWW
      我执行了~~~
      123-ABC-WWW
      PASS
      执行之后的一些操作
      ok      join    0.002s
      ```

- # 方法

  个人理解：一种包含了接收者（`recover`）的特殊函数，接收者可以为任意类型，而不是只能是”类“，如何通过 `T.Xxx()` 的方式调用

  *[函数相关学习笔记](https://www.xxcheng.cn/756.html#cl-19)*

  - ## 语法

    - 只能与接收者处于一个包内；
    - 接收者可以是任意数据类型；
    - 不支持重载；
    - 接收者支持对应类型的指针 `*T`，基础类型除外；
    - 调用时不会区分是普通类型的接收者还是指针类型的接收者，所有方法都可以调用，编译器会自动识别转换；

  - ## 定义

    ```go
    func (recevier type) methodName(参数列表)(返回值列表){}
    
    func (recevier *type) methodName(参数列表)(返回值列表){}
    ```

  - ## 示例

    先预定义一个 `Cup` 结构体

    ```go
    type Cup struct {
    	id    int
    	color string
    	weight float32
    }
    ```

    - ### 只能在一个包内

      执行会报错，因为 `int` 不在一个包内，而不是不支持基础类型，解决办法是使用别名

      ```go
      func (i int)PrintInt() {
      	fmt.Println("我是：", i)
      }
      ```

      ```
      cannot define new methods on non-local type int
      ```

      ![image-20230707172823229](https://cdn-static.xxcheng.cn/static/blog/images/2023/07/07/fa8ced732d8f6de751e62595976e833d.png)

    - ### 基础类型的接收者

      使用别名实现 `int` 类型的接收者

      ```go
      type my2Int int
      
      func (i my2Int) PrintInt() {
      	fmt.Println("我是：", i)
      }
      func Test_07_2(t *testing.T) {
      	var a my2Int = 1
      	a.PrintInt()
      }
      ```

      ```shell
      [root@CentOS single]# go test -v  day07_test.go -run Test_07_2
      === RUN   Test_07_2
      我是： 1
      --- PASS: Test_07_2 (0.00s)
      PASS
      ok      command-line-arguments  0.002s
      ```

    - ### 不区分普通类型还是指针类型的接收者

      ```go
      func (c Cup) Say() {
      	fmt.Println("我的ID:", c.id, "我的颜色:", c.color)
      }
      func (c *Cup) UpdateColor(color string) {
      	c.color = color
      }
      func Test_07_3(t *testing.T) {
      	c := Cup{0, "白色", 2.23}
      	c.Say()
      	c.UpdateColor("黑色")
      	c.Say()
      }
      ```

      ```shell
      [root@CentOS single]# go test -v  day07_test.go -run Test_07_3
      === RUN   Test_07_3
      我的ID: 0 我的颜色: 白色
      我的ID: 0 我的颜色: 黑色
      --- PASS: Test_07_3 (0.00s)
      PASS
      ok      command-line-arguments  0.002s
      ```

  - ## 匿名字段

    见[匿名嵌套结构体](https://www.xxcheng.cn/754.html#cl-32)

  - ## 方法集

    - 类型 `T` 方法集包含所有接收者为 `T` 的方法；
    - 类型 `*T` 方法集包含所有接收者为 `T` 和 `*T` 的方法；
    - 类型 `S` 如果保护匿名字段 `T`，则 `S` 和 `*S` 的方法集包含 `T` 的方法集；
    - 类型 `S` 如果保护匿名字段 `*T`，则 `S` 和 `*S` 的方法集包含 `T` 和 `*T` 的方法集；

    上面定义在实际调用中不受约束，编译器会自动识别运行的

  - ## 方法表达式

    将方法赋值给一个变量

    ***方法表达式受到方法集的约束***

    - ### 实现方式

      - #### `method value` 方法值

        隐式调用，通过 `struct` 实例获取到方法对象

      - #### `method expression` 方法表达式

        显示调用，通过 `struct` 类型获取到方法对应，调用时第一个参数输入对应的一个实例

    - ### 示例

      ```go
      func Test_07_4(t *testing.T) {
      	c := Cup{1, "红色", 3.55}
      	c2 := Cup{2, "白色", 6.66}
      	say := c.Say
      	updateColor := c.UpdateColor
      
      	say2 := Cup.Say
      	updateColor2 := (*Cup).UpdateColor
      
      	say()
      	say2(c2)
      
      	updateColor("黑色")
      	updateColor2(&c2, "绿色")
      
      	say()
      	say2(c2)
      }
      ```

- # 参考链接

  - [testing package - testing - Go Packages : Examples](https://pkg.go.dev/testing?utm_source=godoc#hdr-Examples)
  - [方法-方法集](https://www.topgoer.com/%E6%96%B9%E6%B3%95/%E6%96%B9%E6%B3%95%E9%9B%86.html)
  - [GO的方法值和方法表达式用法](https://www.cnblogs.com/phpper/p/12370086.html)
