- ### 阶段一

  基础server构建

  - #### 初始化相关

    ```go
    mkdir IM_System
    cd IM_System
    go mod init my_server
    touch server.go main.go 
    ```

  - #### 创建Sever对象

    *server.go*

    ```go
    type Server struct {
    	Ip   string
    	Port int
    }
    ```

  - #### 处理server创建接口的函数

    *server.go*

    ```go
    func NewServer(ip string, port int) *Server {
    	server := &Server{
    		Ip:   ip,
    		Port: port,
    	}
    
    	return server
    }
    ```

  - #### 启动服务器接口的函数&处理请求的函数

    *server.go*

    ```go
    // 处理请求
    func (s *Server) Handler(conn net.Conn) {
    	fmt.Println("链接建立成功！！！")
    }
    
    func (s *Server) Start() {
    	listener, err := net.Listen("tcp", fmt.Sprintf("%s:%d", s.Ip, s.Port))
    	if err != nil {
    		fmt.Println("net.Listen err:", err)
    		return
    	}
    	fmt.Println("Server 运行成功~~~")
    	defer listener.Close()
    
    	for {
    		//accept
    		conn, err := listener.Accept()
    		if err != nil {
    			fmt.Println("listener accept err:", err)
    			continue
    		}
    		//do handler
    		go s.Handler(conn)
    	}
    }
    ```

  - #### 编写main函数&编辑运行

    *main.go*

    ```go
    func main() {
    	server := NewServer("127.0.0.1", 9999)
    	server.Start()
    }
    ```

    ```sh
    go build -o server
    ```

    *运行截图*

    ![image-20230626085544318](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/26/22790717844869ad34275504382d666b.png)

  - #### 全部代码

    *server.go*

    ```go
    package main
    
    import (
    	"fmt"
    	"net"
    )
    
    type Server struct {
    	Ip   string
    	Port int
    }
    
    // 创建 server 的接口
    func NewServer(ip string, port int) *Server {
    	server := &Server{
    		Ip:   ip,
    		Port: port,
    	}
    
    	return server
    }
    
    // 处理请求
    func (s *Server) Handler(conn net.Conn) {
    	fmt.Println("链接建立成功！！！")
    }
    
    // 启动服务器接口
    func (s *Server) Start() {
    	listener, err := net.Listen("tcp", fmt.Sprintf("%s:%d", s.Ip, s.Port))
    	if err != nil {
    		fmt.Println("net.Listen err:", err)
    		return
    	}
    	fmt.Println("Server 运行成功~~~")
    	defer listener.Close()
    
    	for {
    		//accept
    		conn, err := listener.Accept()
    		if err != nil {
    			fmt.Println("listener accept err:", err)
    			continue
    		}
    		//do handler
    		go s.Handler(conn)
    	}
    }
    ```

    *main.go*

    ```go
    package main
    
    func main() {
    	server := NewServer("127.0.0.1", 9999)
    	server.Start()
    }
    ```

- ### 阶段二

  用户上线功能

  - #### 原理图

    ![image-20230626100332175](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/26/4f33328508f7b4888b745fff6c92a372.png)

  - #### 重构`Server`对象和创建方法

    *server.go*

    ```go
    type Server struct {
    	Ip   string
    	Port int
    
    	OnlineMap map[string]*User
    	mapLock   sync.RWMutex
    
    	Message chan string
    }
    
    func NewServer(ip string, port int) *Server {
    	server := &Server{
    		Ip:        ip,
    		Port:      port,
    		OnlineMap: make(map[string]*User),
    		Message:   make(chan string),
    	}
    
    	return server
    }
    ```

  - #### 广播消息函数

    *server.go*

    ```go
    func (s *Server) BroadCast(user *User, msg string) {
    	sendMsg := "[" + user.Addr + "]" + user.Name + ":" + msg
    	s.Message <- sendMsg
    }
    ```

  - #### 监听到广播消息去处理函数

    *server.go*

    ```go
    func (s *Server) ListenMessager() {
       for {
          msg := <-s.Message
          s.mapLock.Lock()
          for _, cli := range s.OnlineMap {
             cli.C <- msg
          }
          s.mapLock.Unlock()
       }
    }
    ```

  - #### `User`对象和新建方法

    *user.go*

    ```go
    type User struct {
    	Name string
    	Addr string
    	C    chan string
    	conn net.Conn
    }
    
    func NerUser(conn net.Conn) *User {
    	userAddr := conn.RemoteAddr().String()
    	user := &User{
    		Name: userAddr,
    		Addr: userAddr,
    		C:    make(chan string),
    		conn: conn,
    	}
    
    	//启动当前user channel的goroutine
    	go user.ListenMessage()
    
    	return user
    }
    ```

  - ### 将user监听到消息发送给客户端

    *user.go*

    ```go
    func (u *User) ListenMessage() {
    	for {
    		msg := <-u.C
    		u.conn.Write([]byte(msg + "\n"))
    	}
    }
    ```

  - ### 重写`server`的`Start`、`Handler`方法

    `server.go`

    ```go
    func (s *Server) Handler(conn net.Conn) {
       fmt.Println("链接建立成功！！！")
       user := NerUser(conn)
       //将当前用户插入map中
       s.mapLock.Lock()
       s.OnlineMap[user.Name] = user
       s.mapLock.Unlock()
       //广播当前用户上线消息
       s.BroadCast(user, "已上线")
    }
    
    func (s *Server) Start() {
       listener, err := net.Listen("tcp", fmt.Sprintf("%s:%d", s.Ip, s.Port))
       if err != nil {
          fmt.Println("net.Listen err:", err)
          return
       }
       fmt.Println("Server 运行成功~~~")
       defer listener.Close()
    
       go s.ListenMessager()
    
       for {
          //accept
          conn, err := listener.Accept()
          if err != nil {
             fmt.Println("listener accept err:", err)
             continue
          }
          //do handler
          go s.Handler(conn)
       }
    }
    ```

  - #### 测试运行

    ![image-20230626100800067](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/26/4de133f0615361ef422c0e1c472900c3.png)

    ![image-20230626100806827](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/26/e98751faa5b266a9678de21846e349e8.png)

    ![image-20230626100812695](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/26/d575df299c09c3621517c6d499b8ab60.png)

  - #### 全部代码

    *server.go*

    ```go
    package main
    
    import (
    	"fmt"
    	"net"
    	"sync"
    )
    
    type Server struct {
    	Ip   string
    	Port int
    
    	OnlineMap map[string]*User
    	mapLock   sync.RWMutex
    
    	Message chan string
    }
    
    // 创建 server 的接口
    func NewServer(ip string, port int) *Server {
    	server := &Server{
    		Ip:        ip,
    		Port:      port,
    		OnlineMap: make(map[string]*User),
    		Message:   make(chan string),
    	}
    
    	return server
    }
    
    // 当监听到message广播消息,就将消息发送给全部的在线User
    func (s *Server) ListenMessager() {
    	for {
    		msg := <-s.Message
    		s.mapLock.Lock()
    		for _, cli := range s.OnlineMap {
    			cli.C <- msg
    		}
    		s.mapLock.Unlock()
    	}
    }
    
    // 广播消息的方法
    func (s *Server) BroadCast(user *User, msg string) {
    	sendMsg := "[" + user.Addr + "]" + user.Name + ":" + msg
    	s.Message <- sendMsg
    }
    
    // 处理请求
    func (s *Server) Handler(conn net.Conn) {
    	fmt.Println("链接建立成功！！！")
    	user := NerUser(conn)
    	//将当前用户插入map中
    	s.mapLock.Lock()
    	s.OnlineMap[user.Name] = user
    	s.mapLock.Unlock()
    	//广播当前用户上线消息
    	s.BroadCast(user, "已上线")
    }
    
    // 启动服务器接口
    func (s *Server) Start() {
    	listener, err := net.Listen("tcp", fmt.Sprintf("%s:%d", s.Ip, s.Port))
    	if err != nil {
    		fmt.Println("net.Listen err:", err)
    		return
    	}
    	fmt.Println("Server 运行成功~~~")
    	defer listener.Close()
    
    	go s.ListenMessager()
    
    	for {
    		//accept
    		conn, err := listener.Accept()
    		if err != nil {
    			fmt.Println("listener accept err:", err)
    			continue
    		}
    		//do handler
    		go s.Handler(conn)
    	}
    }
    ```

    *user.go*

    ```go
    package main
    
    import "net"
    
    type User struct {
    	Name string
    	Addr string
    	C    chan string
    	conn net.Conn
    }
    
    // 创建用户的接口
    func NerUser(conn net.Conn) *User {
    	userAddr := conn.RemoteAddr().String()
    	user := &User{
    		Name: userAddr,
    		Addr: userAddr,
    		C:    make(chan string),
    		conn: conn,
    	}
    
    	//启动当前user channel的goroutine
    	go user.ListenMessage()
    
    	return user
    }
    
    // 监听消息
    func (u *User) ListenMessage() {
    	for {
    		msg := <-u.C
    		u.conn.Write([]byte(msg + "\n"))
    	}
    }
    ```

    *main.go*

    ```go
    package main
    
    func main() {
    	server := NewServer("127.0.0.1", 9999)
    	server.Start()
    }
    ```

- ### 阶段三

  用户消息广播机制

  - #### 修改`Handler`方法

    *server.go*

    ```go
    func (s *Server) Handler(conn net.Conn) {
    	fmt.Println("链接建立成功！！！")
    	user := NerUser(conn)
    	//将当前用户插入map中
    	s.mapLock.Lock()
    	s.OnlineMap[user.Name] = user
    	s.mapLock.Unlock()
    	//广播当前用户上线消息
    	s.BroadCast(user, "已上线")
    
    	//处理客户端发送的消息，由于是堵塞状态，新开一个goroutine
    	go func() {
    		buf := make([]byte, 4096)
    		for {
    			n, err := conn.Read(buf)
    			if n == 0 {
    				s.BroadCast(user, "下线")
    				return
    			}
    			if err != nil && err != io.EOF {
    				fmt.Println("Conn Error:", err)
    				return
    			}
    			msg := string(buf[:n-1])
    			s.BroadCast(user, msg)
    		}
    	}()
    
    }
    ```

  - #### 测试运行

    ![image-20230626103136877](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/26/2577a421068bd9f33c46dcfdf53b243a.png)

- ### 阶段四

  用户业务封装，将server处理业务封装成用户对象的方法

  添加`Online`、`Offline`、`DoMessage`

  - #### 新增`Online`、`Offline`、`DoMessage`方法

    *user.go*

    ```go
    func (u *User) Online() {
    	//将当前用户插入map中
    	u.server.mapLock.Lock()
    	u.server.OnlineMap[u.Name] = u
    	u.server.mapLock.Unlock()
    	//广播当前用户上线消息
    	u.server.BroadCast(u, "已上线")
    }
    
    func (u *User) Offline() {
    	//将当前用户插入map中
    	u.server.mapLock.Lock()
    	delete(u.server.OnlineMap, u.Name)
    	u.server.mapLock.Unlock()
    	//广播当前用户上线消息
    	u.server.BroadCast(u, "下线")
    }
    
    func (u *User) DoMessage(msg string) {
    	u.server.BroadCast(u, msg)
    }
    ```

  - #### 修改`Handler`

    *server.go*

    ```go
    func (s *Server) Handler(conn net.Conn) {
    	fmt.Println("链接建立成功！！！")
    	user := NerUser(conn, s)
    	////将当前用户插入map中
    	//s.mapLock.Lock()
    	//s.OnlineMap[user.Name] = user
    	//s.mapLock.Unlock()
    	////广播当前用户上线消息
    	//s.BroadCast(user, "已上线")
    	user.Online()
    
    	//处理客户端发送的消息，由于是堵塞状态，新开一个goroutine
    	go func() {
    		buf := make([]byte, 4096)
    		for {
    			n, err := conn.Read(buf)
    			if n == 0 {
    				user.Offline()
    				//s.BroadCast(user, "下线")
    				return
    			}
    			if err != nil && err != io.EOF {
    				fmt.Println("Conn Error:", err)
    				return
    			}
    			msg := string(buf[:n-1])
    			//s.BroadCast(user, msg)
    			user.DoMessage(msg)
    		}
    	}()
    }
    ```

  - #### 测试运行

    ![image-20230626105804258](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/26/b64be91495b824bf81ce064e72e3e448.png)

- ### 阶段五

  在线用户查询

  输入`who`查询当前在线用户

  - #### 实现代码

    *user.go*

    ```go
    func (u *User) DoMessage(msg string) {
    	if msg == "who" {
    		u.server.mapLock.Lock()
    		for _, user := range u.server.OnlineMap {
    			onlineMsg := "[" + user.Name + "] is online\n"
    			u.sendMsg(onlineMsg)
    		}
    		u.server.mapLock.Unlock()
    	} else {
    		u.server.BroadCast(u, msg)
    	}
    }
    
    func (u *User) sendMsg(msg string) {
    	u.conn.Write([]byte(msg))
    }
    ```

  - #### 测试运行

    ![image-20230626141847522](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/26/20fc5e1d9f441484b82196a36559c864.png)

- ### 阶段六

  用户名修改

  输入`rename|newName`格式修改用户名

  - #### 实现代码

    *user.go*

    ```go
    func (u *User) DoMessage(msg string) {
    	if msg == "who" {
    		u.server.mapLock.Lock()
    		for _, user := range u.server.OnlineMap {
    			onlineMsg := "[" + user.Name + "] is online\n"
    			u.sendMsg(onlineMsg)
    		}
    		u.server.mapLock.Unlock()
    	} else if len(msg) > 7 && msg[:7] == "rename|" {
    		newName := strings.Split(msg, "|")[1]
    		_, isExist := u.server.OnlineMap[newName]
    		if isExist {
    			u.sendMsg("用户名已存在，修改失败")
    		} else {
    			u.server.mapLock.Lock()
    			oldName := u.Name
    			u.server.OnlineMap[newName] = u
    			delete(u.server.OnlineMap, oldName)
    			u.Name = newName
    			u.sendMsg("用户名修改成功,[" + oldName + "]->[" + newName + "]")
    			u.server.mapLock.Unlock()
    		}
    	} else {
    		u.server.BroadCast(u, msg)
    	}
    }
    ```

  - #### 测试运行

    ![image-20230626143608610](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/26/5f7c229af39449902dde77124df73093.png)
  
- ### 阶段七

  超时强踢

  - #### 实现代码

    *server.go*

    ```go
    func (s *Server) Handler(conn net.Conn) {
    	fmt.Println("链接建立成功！！！")
    	user := NerUser(conn, s)
    	////将当前用户插入map中
    	//s.mapLock.Lock()
    	//s.OnlineMap[user.Name] = user
    	//s.mapLock.Unlock()
    	////广播当前用户上线消息
    	//s.BroadCast(user, "已上线")
    	user.Online()
    
    	isLive := make(chan bool)
    
    	//处理客户端发送的消息，由于是堵塞状态，新开一个goroutine
    	go func() {
    		buf := make([]byte, 4096)
    		for {
    			n, err := conn.Read(buf)
    			if n == 0 {
    				user.Offline()
    				//s.BroadCast(user, "下线")
    				return
    			}
    			if err != nil && err != io.EOF {
    				fmt.Println("Conn Error:", err)
    				return
    			}
    			msg := string(buf[:n-1])
    			//s.BroadCast(user, msg)
    			user.DoMessage(msg)
    
    			isLive <- true
    		}
    	}()
    
    	for {
    		select {
    		case <-isLive:
    		case <-time.After(time.Second * 10):
    			user.sendMsg("你已超时，将被踢出下线")
    			//关闭channel
    			close(user.C)
    			//关闭链接
    			conn.Close()
    			delete(s.OnlineMap, user.Name)
    			return
    		}
    	}
    }
    ```

  - #### 测试运行

    ![image-20230626145615043](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/26/c835cdfee9a887ee6e52bdbc3a7f3cd2.png)

- ### 阶段八

  私聊功能

  格式：`to|username|msg`

  - #### 实现代码

    ```go
    func (u *User) DoMessage(msg string) {
    	if msg == "who" {
    		u.server.mapLock.Lock()
    		for _, user := range u.server.OnlineMap {
    			onlineMsg := "[" + user.Name + "] is online\n"
    			u.sendMsg(onlineMsg)
    		}
    		u.server.mapLock.Unlock()
    	} else if len(msg) > 7 && msg[:7] == "rename|" {
    		newName := strings.Split(msg, "|")[1]
    		_, isExist := u.server.OnlineMap[newName]
    		if isExist {
    			u.sendMsg("用户名已存在，修改失败")
    		} else {
    			u.server.mapLock.Lock()
    			oldName := u.Name
    			u.server.OnlineMap[newName] = u
    			delete(u.server.OnlineMap, oldName)
    			u.Name = newName
    			u.sendMsg("用户名修改成功,[" + oldName + "]->[" + newName + "]")
    			u.server.mapLock.Unlock()
    		}
    	} else if len(msg) > 3 && msg[:3] == "to|" {
    		targetUserName := strings.Split(msg, "|")[1]
    		if targetUserName == "" {
    			u.sendMsg("发送私聊消息格式错误,正确格式:to|张三|你好呀")
    			return
    		}
    		targetUser, isExist := u.server.OnlineMap[targetUserName]
    		if !isExist {
    			u.sendMsg("目标对象不存在，请仔细检查")
    			return
    		}
    		content := strings.Split(msg, "|")[2]
    		if content == "" {
    			u.sendMsg("消息内容格式错误")
    			return
    		}
    		targetUser.sendMsg("来自:[" + u.Name + "]的消息，内容是:" + content)
    	} else {
    		u.server.BroadCast(u, msg)
    	}
    }
    ```

  - #### 测试运行

    ![image-20230626151130404](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/26/4d7c6967a858cc92c787892b61bae31e.png)

- ### 代码

  [点击下载](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/26/67c25709f199fbcccf12934dae4686cd.zip)

- ### 说明

  使用`nc`工具进行测试

  > nc是一个网络工具，全称为"Netcat"。它是一个功能强大的命令行工具，可用于在计算机网络上进行数据传输和网络连接的创建。nc可以在不同的操作系统上使用，包括Linux、Windows和macOS。
  >
  > nc的主要功能包括：
  >
  > 1. 端口扫描：通过指定目标IP地址和端口范围，nc可以扫描主机上开放的网络端口。
  > 2. 网络监听：nc可以作为一个网络服务器，监听指定的端口并接受传入的连接请求。
  > 3. 文件传输：通过nc，可以在网络上传输文件。你可以将文件从一个计算机发送到另一个计算机，或者将输出导向到文件。
  > 4. 网络调试和测试：nc可以用于测试网络服务和应用程序的连接和响应。你可以使用nc发送自定义的网络请求并查看服务器的响应。
  > 5. 网络代理：使用nc可以创建简单的网络代理，将数据从一个端口转发到另一个端口，从而实现端口转发和代理功能。
  >
  > 请注意，由于nc是一个强大的工具，也被一些恶意用户用于攻击或非法活动。因此，在使用nc时要遵守法律法规，并确保只在合法授权的网络环境中使用它。