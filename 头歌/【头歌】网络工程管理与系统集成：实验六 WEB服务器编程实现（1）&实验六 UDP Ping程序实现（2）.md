- # 实验六 WEB服务器编程实现（1）

  执行程序总是会出现如下问题

  ```python
  Traceback (most recent call last):
    File "webserver-5.py", line 6, in <module>
      serverSocket.bind(("127.0.0.1",6789))
  OSError: [Errno 98] Address already in use
  open error!! 
  ```

  可以尝试右上角**重置代码仓库**按钮或者多试几次

  ![image-20230515164100668](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/15/632c79295afb6abb625e921e0079a015.png)

  - ## 第1关 创建流式套接字

    ```python
    #import socket module
    from socket import *
    serverSocket = socket(AF_INET, SOCK_STREAM) 
    #Prepare a sever socket 
    ########## Begin ##########
    serverSocket.bind(('127.0.0.1',6789))
    serverSocket.listen(1)
    ########## End ##########
    print(serverSocket)
    serverSocket.close()
    ```

  - ## 第2关 服务端获取连接请求

    ```python
    #import socket module
    from socket import *
    serverSocket = socket(AF_INET, SOCK_STREAM) 
    #Prepare a sever socket 
    serverSocket.bind(("127.0.0.1",6789))
    serverSocket.listen(1)
     
    #while True:
    #Establish the connection
    print('开始WEB服务...')
     
    try:
        ########## Begin ##########
        connectionSocket,addr = serverSocket.accept()
        message = connectionSocket.recv(1024)
        ########## End ##########
        print(addr[0])
        print(message)
        connectionSocket.close()
    except IOError:
            connectionSocket.close()
    serverSocket.close()
    ```

  - ## 第3关 服务端读取请求文件内容

    ```python
    #import socket module
    from socket import *
     
    serverSocket = socket(AF_INET, SOCK_STREAM) 
    #Prepare a sever socket
    serverSocket.close()
    serverSocket = socket(AF_INET, SOCK_STREAM) 
    serverSocket.bind(("127.0.0.1",6789))
    serverSocket.listen(1)
     
    #while True:
    print('开始WEB服务...')
     
    try:
        connectionSocket, addr = serverSocket.accept()
        message = connectionSocket.recv(1024) # 获取客户发送的报文
            
        #读取文件内容
        ######### Begin #########
        fi = message.split()[1]
        outputdata = open(fi[1:]).read()
        ######### End #########
        print(outputdata)
        connectionSocket.close()
    except IOError:
        connectionSocket.close()
    serverSocket.close()
    ```

  - ## 第4关 服务端响应请求头部信息

    ```python
    #import socket module
    from socket import *
     
    serverSocket = socket(AF_INET, SOCK_STREAM) 
    #Prepare a sever socket 
    serverSocket.bind(("127.0.0.1",6789))
    serverSocket.listen(1)
     
    #while True:
    print('开始WEB服务...')
     
    try:
            connectionSocket, addr = serverSocket.accept()
            message = connectionSocket.recv(1024) # 获取客户发送的报文
            
            #读取文件内容
            filename = message.split()[1]       
            f = open(filename[1:])
            outputdata = f.read();
            
            #发送响应的头部信息
            header = ' HTTP/1.1 200 OK\nConnection: close\nContent-Type: text/html\nContent-Length: %d\n\n' % (len(outputdata))
             #########Begin#########
            connectionSocket.send(header.encode())
            #########End#########
            connectionSocket.close()
    except IOError:
            connectionSocket.close()
    serverSocket.close()
    ```

  - ## 第5关 服务端响应请求的正文

    ```python
    #import socket module
    from socket import *
    
    serverSocket = socket(AF_INET, SOCK_STREAM) 
    #Prepare a sever socket 
    serverSocket.bind(("127.0.0.1",6789))
    serverSocket.listen(1)
    
    #while True:
    print('开始WEB服务...')
    try:
            connectionSocket, addr = serverSocket.accept()
            message = connectionSocket.recv(1024) # 获取客户发送的报文
            
            #读取文件内容
            filename = message.split()[1]       
            f = open(filename[1:])
            outputdata = f.read();
            
            #向套接字发送头部信息
            header = ' HTTP/1.1 200 OK\nConnection: close\nContent-Type: text/html\nContent-Length: %d\n\n' % (len(outputdata))
            connectionSocket.send(header.encode())
            
            #发送文件内容
            #########Begin#########
            for i in outputdata:
                    connectionSocket.send(i.encode())
        #########End#########
            
            #关闭连接
            connectionSocket.close()
    except IOError:             #异常处理
            #发送文件未找到的消息
            
            #关闭连接
            connectionSocket.close()
    #关闭套接字
    serverSocket.close()
    ```

  - ## 第6关 服务端异常（文件不存在异常）处理

    ```python
     #import socket module
    from socket import *
     
    serverSocket = socket(AF_INET, SOCK_STREAM) 
    #Prepare a sever socket 
    serverSocket.bind(("127.0.0.1",6789))
    serverSocket.listen(1)
     
    #while True:
    print('开始WEB服务...')
    try:
            connectionSocket, addr = serverSocket.accept()
            message = connectionSocket.recv(1024) # 获取客户发送的报文
            
            #读取文件内容
            filename = message.split()[1]       
            f = open(filename[1:])
            outputdata = f.read();
            
            #向套接字发送头部信息
            header = ' HTTP/1.1 200 OK\nConnection: close\nContent-Type: text/html\nContent-Length: %d\n\n' % (len(outputdata))
            connectionSocket.send(header.encode())
     
            #S发送请求文件的内容
            for i in range(0, len(outputdata)):
                connectionSocket.send(outputdata[i].encode())
            
            #关闭连接
            connectionSocket.close()
    except IOError:             #异常处理
            #发送文件未找到的消息
            header = ' HTTP/1.1 404 not Found\nContent-Type: text/html\n\n '
            #########Begin#########
            response = "<html><head></head><body><h1>404 not Found</h1></body></html>"
            connectionSocket.send(header.encode() + response.encode())
            #########End#########
            #关闭连接
            connectionSocket.close()
    #关闭套接字
    serverSocket.close()
    ```

- # 实验六 UDP Ping程序实现（2）

  - ## 第1关 Ping服务端创建UDP套接字

    ```python
    # UDPPingerServer.py 
    from socket import * 
    
    ########## Begin ##########
    
    serverSocket = socket(AF_INET, SOCK_DGRAM)
    serverSocket.bind(('0.0.0.0', 12000))
    
    ########## End ##########
    
    print( serverSocket)
    ```

  - ## 第2关 接收并转发消息

    ```python
    from socket import *
     
    # 创建UDP套接字
    serverSocket = socket(AF_INET, SOCK_DGRAM)
    # 绑定本机IP地址和端口号
    serverSocket.bind(('', 12000))
     
    ########## Begin ##########
    # 接收客户端消息
    message, address = serverSocket.recvfrom(2048)
    # 将数据包消息转换为大写
    message = message.upper()
    #将消息传回给客户端
    serverSocket.sendto(message, address)
    ########## End ##########
    ```

  - ## 第3关 服务端模拟丢包事件

    ```python
    from socket import *
    import random
     
    # 创建UDP套接字
    serverSocket = socket(AF_INET, SOCK_DGRAM)
    # 绑定本机IP地址和端口号
    serverSocket.bind(('', 12000))
     
    num=0
    while True:
               
        # 接收客户端消息
        message, address = serverSocket.recvfrom(1024)
        # 将数据包消息转换为大写
        message = message.upper()
            
        num=num+1
        if num>=8:
            break
        ########## Begin #########
     
        if num%3==1:
            continue;
     
        ########## End ##########    
        #将消息传回给客户端
        serverSocket.sendto(message, address)
    ```

  - ## 第4关 客户端创建UDP套接字

    ```python
    from socket import *
     
    ########## Begin ##########
    clientSocket = socket(AF_INET, SOCK_DGRAM) # 创建UDP套接字，使用IPv4协议
    # 设置套接字超时值1秒
    clientSocket.settimeout(1)
     
    ########## End ##########
    print(clientSocket)
    print(clientSocket.gettimeout())
    ```

  - ## 第5关 客户端向服务器发送消息并接收消息

    ```python
    from socket import *
    import time
     
    serverName = '127.0.0.1' # 服务器地址，本例中使用本机地址
    serverPort = 12000 # 服务器指定的端口
    clientSocket = socket(AF_INET, SOCK_DGRAM) # 创建UDP套接字，使用IPv4协议
    clientSocket.settimeout(1) # 设置套接字超时值1秒
    for i in range(0, 9):
        sendTime = time.time()
        message = ('Ping %d %s' % (i+1, sendTime)).encode()     # 生成数据报，编码为bytes以便发送    
        try:        
            ########## Begin ##########
            # 将信息发送到服务器
            #clientSocket.sendto(modifiedMessage, serverAddress)
            clientSocket.sendto(message, (serverName, serverPort))
            # 从服务器接收信息，同时也能得到服务器地址
            modifiedMessage, serverAddress = clientSocket.recvfrom(1024)
     
            ########## End ##########   
            rtt = time.time() - sendTime # 计算往返时间
            print('Sequence %d: Reply from %s    RTT = %.3fs' % (i+1, serverName, rtt)) # 显示信息
        except Exception as e:
            print('Sequence %d: Request timed out.' % (i+1))
            
    clientSocket.close() # 关闭套接字
    ```