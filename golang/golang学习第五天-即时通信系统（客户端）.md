- ### 阶段一

  与服务器成功连接

  - #### 实现代码

    *client.go*

    ```go
    package main
    
    import (
    	"fmt"
    	"net"
    )
    
    type Client struct {
    	ServerIp   string
    	ServerPort int
    	Name       string
    	conn       net.Conn
    }
    
    func NewClient(serverIp string, serverPort int) *Client {
    	client := &Client{
    		ServerIp:   serverIp,
    		ServerPort: serverPort,
    	}
    
    	conn, err := net.Dial("tcp", fmt.Sprintf("%s:%d", serverIp, serverPort))
    	if err != nil {
    		fmt.Println("net Dial error:", err)
    		return nil
    	}
    	client.conn = conn
    	return client
    }
    
    func main() {
    	client := NewClient("127.0.0.1", 9999)
    	if client == nil {
    		fmt.Println("服务器连接失败>>>")
    		return
    	}
    	fmt.Println("服务器连接成功~~~")
    	select {}
    }
    ```

  - #### 测试运行

    ```sh
    #先编译
    go build -o client client.go
    ```

    ![image-20230627135938358](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/27/23540e82f0e928840c222fb799387496.png)

- ### 阶段二

  解析命令行，实现自定义服务器IP和端口

  - #### 实现代码

    利用`init`函数初始化使用`flag`库读取命令行的参数（不一定非要在`init`函数中）

    *client.go*

    ```go
    var serverIp string
    var serverPort int
    
    func init() {
    	//变量,参数名,默认值,说明
    	flag.StringVar(&serverIp, "ip", "127.0.0.1", "设置服务端的IP地址（默认：127.0.0.1）")
    	flag.IntVar(&serverPort, "port", 9999, "设置服务端的端口（默认：9999）")
    }
    
    func main() {
    	flag.Parse()
    	client := NewClient(serverIp, serverPort)
    	if client == nil {
    		fmt.Println("服务器连接失败>>>")
    		return
    	}
    	fmt.Println("服务器连接成功~~~")
    	select {}
    }
    ```

  - #### 测试运行

    ![image-20230627141052002](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/27/d32b0309e1b2f7f4e6a03de4c81c2a44.png)

- ### 阶段三

  菜单显示

  - #### 实现代码

    *client.go*

    ```go
    func (c *Client) menu() bool {
    	var flag int
    	fmt.Println("请选择菜单模式：")
    	fmt.Println("1.公聊模式")
    	fmt.Println("2.私聊模式")
    	fmt.Println("3.更新用户名")
    	fmt.Println("0.退出")
    	fmt.Scanln(&flag)
    	if flag >= 0 && flag <= 3 {
    		c.flag = flag
    		return true
    	} else {
    		return false
    	}
    }
    
    func (c *Client) Run() {
    	defer fmt.Println("bye~~~")
    	for c.flag != 0 {
    		if c.menu() != true {
    			fmt.Println("输入不合法，请重新输入")
    		}
    		switch c.flag {
    		case 1:
    			fmt.Println("你选择你了[1]:公聊模式")
    			break
    		case 2:
    			fmt.Println("你选择你了[2]:私聊模式")
    			break
    		case 3:
    			fmt.Println("你选择你了[3]:修改用户名")
    			break
    		}
    	}
    }
    
    //....
    
    
    func main() {
    	flag.Parse()
    	client := NewClient(serverIp, serverPort)
    	if client == nil {
    		fmt.Println("服务器连接失败>>>")
    		return
    	}
    	fmt.Println("服务器连接成功~~~")
    	client.Run()
    }
    ```

  - #### 测试运行

    ![image-20230627144012664](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/27/6762aea589bfb5ff8c4da843ae3a037b.png)

- ### 阶段四

  实现用户名修改和接受服务端消息

  - #### 实现代码

    *client.go*

    ```go
    func (c *Client) UpdateName() bool {
    	fmt.Println("请输入用户名:")
    	fmt.Scanln(&c.Name)
    	sendMsg := "rename|" + c.Name + "\n"
    	_, err := c.conn.Write([]byte(sendMsg))
    	if err != nil {
    		fmt.Println("update Name err:", err)
    		return false
    	}
    	return true
    }
    
    // 接收服务端消息输出到屏幕
    func (c *Client) HandleServerMsg() {
    	io.Copy(os.Stdout, c.conn)
    	//相当于
    	/*
    		for {
    			buf := make([]byte, 4096)
    			c.conn.Read(buf)
    			fmt.Println(buf)
    		}
    	*/
    }
    
    func main() {
    	//...
    	go client.HandleServerMsg()
    	//...
    }
    ```

  - #### 测试运行

    ![image-20230627145945042](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/27/8cdbb54c538bd2cbd6daf83628ba58f4.png)

- ### 阶段五

  实现公聊模式

  - #### 实现代码

    *client.go*

    ```go
    func (c *Client) SendPublicMsg() {
    	fmt.Println("进入公聊模式（输入exit退出）")
    	for {
    		var chatMsg string
    		fmt.Scanln(&chatMsg)
    		if chatMsg == "exit" {
    			break
    		}
    		c.conn.Write([]byte(chatMsg))
    	}
    }
    ```

  - #### 测试运行

    ![image-20230627151156340](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/27/f666641cbaf66878c9dd8cd339a49400.png)

- ### 阶段六

  实现私聊模式

  - #### 流程

    1. 查询哪些用户在线
    2. 提示用户选择一个用户进入私聊

  - #### 实现代码

    *client.go*

    ```go
    func (c *Client) QueryAllOnlineUsers() {
    	c.conn.Write([]byte("who\n"))
    }
    func (c *Client) SendPrivateMsg() {
    	fmt.Println("进入私聊模式（输入exit退出）")
    	c.QueryAllOnlineUsers()
    	var targetUserName string
    	fmt.Println("请输入聊天对象")
    	fmt.Scanln(&targetUserName)
    	for {
    		fmt.Println("请输入聊天内容：")
    		var chatMsg string
    
    		fmt.Scanln(&chatMsg)
    		if chatMsg == "exit" {
    			break
    		}
    		c.conn.Write([]byte("to|" + targetUserName + "|" + chatMsg + "\n"))
    	}
    }
    ```

  - #### 测试运行

    ![image-20230627153126326](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/27/835a33c7f343d6a7f1fdc66b715a3762.png)

- ### 代码

  - [点击下载](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/27/5d87ab9a71de8438ca58517ce9138955.zip)
  - 服务端编译 `go build -o server server.go main.go user.go`
  - 客户端编译 `go build -o client client.go`

- ### 展望

  ![image-20230627153653375](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/27/84f5615f1a040175dc179e2e12c5c248.png)