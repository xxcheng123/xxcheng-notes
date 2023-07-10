**如需查看过关测评代码直接点击[【测评代码】](#pass-code-title)快速查看**

- # 实验描述

  - ## 实验五 标准 ACL 配置（1）

    - ### 第1关：配置 ACL

      - #### 任务描述

        本关任务：学会标准 ACL 的配置方法。

      - #### 相关知识

        为了完成本关任务，你需要掌握：

        1. 学习 ACL 原理；
        2. 配置标准 ACL 。

        - ##### ACL 原理

          访问控制列表 (ACL) 是一种基于包过滤的访问控制技术，它可以根据设定的条件**对接口上的数据包进行过滤**，允许其**通过或丢弃**。访问控制列表被广泛地应用于路由器和三层交换机，借助于访问控制列表，可以有效地控制用户对网络的访问，从而最大程度地保障网络安全。

          ###### 匹配方法：

          ACL 会根据数据包中的源 IP 地址，目的 IP 地址，TCP、UDP 端口号对比每条匹配规则，匹配则允许包继续传递，不匹配则丢弃包。

          访问控制列表的**功能**：

          1. 限制网络流量、提高网络性能；
          2. 通信流量的控制，例如 ACL 可以限定或简化路由更新信息的长度，从而限制通过路由器某一网段的通信流量；
          3. 提供网络安全访问的基本手段；
          4. 在路由器端口处决定哪种类型的通信流量被转发或被阻塞，例如，用户可以允许 E-mail 通信流量被路由，拒绝所有的 Telnet 通信流量等。

          ###### **进入：** 

          > 1. 当一个数据包进入一个端口，路由器检查这个数据包是否可路由；
          > 2. 如果是可以路由的，路由器检查这个端口是否有 ACL 规则在控制进入的数据包；
          > 3. 如果有，根据 ACL 中的条件指令，检查这个数据报； 
          > 4. 如果数据报是被允许的，就查询路由表，决定数据报的目标端口。 

          ###### 发出：

          > 1. 数据包发出的时候路由器检查发出端口是否存在 ACL 规则控制流出的数据包； 
          > 2. 若不存在，这个数据报就直接发送到目标端口；
          > 3. 若存在，就再根据 ACL 进行取舍，禁止发出的就将数据包丢弃，允许发出的就正常发出。 

          ###### 过滤基本原则：
          
          按照访问控制列表进行筛选，从头到尾逐条匹配，当遇到规则匹配之后就直接根据规则对数据包进行处理，不再匹配后面的规则。
          
          在访问控制列表的末尾处有一条隐藏的 deny 语句，会直接丢弃所有信息，所以如果列表中所有的规则都不匹配会直接被隐藏的 deny 语句丢弃。
          
          ###### 标准访问控制列表配置命令：
          
          列表号从 1 到 99 属于标准访问列表规则，根据数据包的源 IP 地址来选择允许或拒绝通过的数据包。 一般情况下一条完整的规则应该包含两个部分，分别是允许通过的和禁止通过的，这样规则才完整，否则规则的配置就有漏洞，无法达到效果。
          
          **配置命令：**`access-list 列表号 permit或deny IP地址 反掩码`，在命令最前方加 no 可以删除该规则，也可以使用`no access-list 列表号`指定删除某条规则。
          
          在配置规则时通过使用 **permit** 来允许数据通过，通过 **deny** 来拒绝数据通过，通过这两个选项的配合完成对数据发送与接收的控制，从而达到网络安全的目的。
          
          使用配置命令可以创建新的规则，允许或拒绝指定的 IP 地址的数据的传输，配置完毕需要将规则应用在指定的地方才能生效。
          
          **反掩码**是用于路由器使用的通配符掩码与源或目标地址一起来分辨匹配的地址范围，在配置的时候有两种特殊的反掩码，分别是 255.255.255.255 ( 等价于 any )和 0.0.0.0 ( 等价于 host )。
          
          any 可以代替 0.0.0.0 255.255.255.255 ，例如：
          
          ```shell
          R1(config)#access-list 1 permit 0.0.0.0 255.255.255.255
          R1(config)#access-list 1 permit any
          //这两个命令是同一个意思，都是允许所有包通过
          ```
          
          host 表示检查 IP 地址的每一位，即指定到一个确定的 IP 地址，而不是一个网段，例如：
          
          ```shell
          R1(config)#access-list 1 permit 192.168.1.1 0.0.0.0
          R1(config)#access-list 1 permit host 192.168.1.1
          //这两个命令都是只允许 192.168.1.1 访问
          ```
          
          **应用规则：**`ip access-group 列表号 in 或 out` 必须将规则应用在指定的地方，并指明是 in 进入还是 out 发出，否则规则无法生效。
          
          在接口应用规则，例：
          
          ```shell
          R1(config)#int f0/0
          R1(config-if)#ip access-group 1 in
          //将规则 1 应用在接口 f0/0 的进入方向
          ```
          
          在远程访问线路上应用访问控制策略，例：
          
          ```shell
          R1(config)#access-list 1 permit host 192.168.1.1   
          //定义 ACL 访问列表 1 ，只允许指定主机访问
          R1(config)#access-list 1 deny any
          //禁止其他所有设备访问
          R1(config-if)#line vty 0 4                
          //设置同时允许 0~4 五个用户访问
          R1(config-line)#access-class 1 in        
          //在 vty 线路上应用 ACL 1 号访问列表
          R1(config-line)#password gns3      
          //设置密码为 gns3
          R1(config-line)#login
          //启用登录
          ```
          
        - ##### 标准 ACL 配置方法
        
          1. 搭建拓扑图，根据拓扑图添加设备；
        
             ![image-20230508160714558](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/607de13ef3628617bcefac5434523d97.png)
        
          2. 根据下表给出的 IP 地址搭建网络拓扑图；
        
             <table>
                 <tbody>
                     <tr>
                         <th>网络设备</th>
                         <th>接口</th>
                         <th>IP地址</th>
                         <th>默认网关</th>
                     </tr>
                     <tr>
                         <td rowspan="3">R1</td>
                         <td>f0/0</td>
                         <td>10.1.1.1</td>
                         <td>\</td>
                     </tr>
                     <tr>
                         <td>f0/1</td>
                         <td>20.1.1.1</td>
                         <td>\</td>
                     </tr>
                      <tr>
                         <td>f1/0</td>
                         <td>192.168.12.1</td>
                         <td>\</td>
                     </tr>
                     <tr>
                         <td rowspan="2">R2</td>
                         <td>f0/0</td>
                         <td>192.168.12.2</td>
                         <td>\</td>
                     </tr>
                     <tr>
                         <td>f0/1</td>
                         <td>172.16.1.1</td>
                         <td>\</td>
                     </tr>
                     <tr>
                         <td colspan="1">PC1</td>
                          <td>e0</td>
                         <td>10.1.1.10</td>
                         <td>10.1.1.1</td>
                     </tr>
                     <tr>
                         <td colspan="1">PC2</td>
                          <td>e0</td>
                         <td>20.1.1.10</td>
                         <td>20.1.1.1</td>
                     </tr>
                     <tr>
                         <td colspan="1">PC3</td>
                          <td>e0</td>
                         <td>172.16.1.10</td>
                         <td>172.16.1.1</td>
                     </tr>
                 </tbody>
             </table>
        
          3. 配置 R2 的访问控制策略。
        
      - #### 实训要求

        1. 配置 RIP 协议连通所有设备；
        2. 在路由器 R2 上配置访问规则 1 ，禁止 PC2 所在的网段访问 PC3 所在的网段，允许其他网段的设备访问 PC3 所在网段；
        3. 在路由器 R2 上配置访问规则 2 ，只允许 R1 Telnet R2 ，不允许其他设备 Telnet 路由器 R2 ，并设置 Telnet 登录密码为 gns3 ，同时允许 0~4 共 5 个用户登录，最后启用登录。

      - #### 操作要求

        打开模拟器之后按照要求搭建拓扑图并完成任务。具体要求如下：

        1. 打开 gns3 ，创建新的工程，将其命名为 **`acl`** ，如果工程名不对将无法通过测评，按照实训要求完成实训任务；
        2. 添加两台路由器三台 PC 并启动电源；
        3. 根据上方作业的拓扑图将设备连接起来；
        4. 根据 IP 地址表为设备配置 IP 地址，并保存设置，路由器特权模式输入`write`保存设置，PC 输入`save`保存设置；
        5. 按照上方作业的要求完成配置；
        6. 按照保存配置文件的方法更新工程目录的配置文件；
        7. 保存之后点击测评，如果不通过可以再保存一遍配置文件，可能会因为延迟问题配置文件来不及更新。

        **配置文件保存方法：**

        1. 在路由器上特权模式下输入`write`等待保存完毕。

        2. 在 GNS3 主页面点击 Tools 并选择第二个选项。

           ![image-20230508161240119](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/b19c692366ac4c4a98cb6c0f81a464db.png)

        3. 在弹出的窗口中选择 Export 选项然后确定。

           ![image-20230508161251165](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/d6d74b01d3bf1fad446dca20b60820ff.png)

        4. 根据图片找到文件保存路径，点击 Open 更新配置文件。

           ![image-20230508161302985](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/f8a234702b8e947d83243616c907d123.png)

        5. 配置上传完毕，可以开始测评。

      - #### 测试说明

        平台会对你编写的代码进行测试，如果与预期结果一致，则顺利通关。

  - ## 实验五 扩展 ACL 配置（2）

    - ### 第1关：扩展ACL

      - #### 任务描述

        本关任务：掌握扩展 ACL 的配置方法。

      - #### 相关知识

        1. 为了完成本关任务，你需要掌握
        2. 配置扩展 ACL 。

        - ##### 扩展 ACL

          标准访问控制列表只能过滤源地址，无法针对某个协议，所以在有些时候无法达到目的，于是就出现了扩展访问列表控制。

          扩展访问控制列表是根据源地址、目的地址、端口号进行过滤的，而且扩展 ACL 可以针对特定的协议。

          标准访问列表控制的列表号范围是 1~99 ，扩展访问控制列表的列表号范围是 100~199 。

          标准访问控制列表与扩展访问控制列表都有一条隐藏的 deny 规则，如果配置了 permit ，有时候不配置 deny ，规则也可以正常使用。但是如果配置的是 deny ，没有配置 permit 的话，所有的规则都会被丢弃。

          扩展 ACL 配置方法：

          ```shell
          access-list 列表号 deny 或 permit 条件
          ```

          注意事项：

          - 扩展访问控制列表的**列表号的范围**是 100~199 ；
          - 选择使用 deny 拒绝或 permit 允许通过；
          - 条件包括**协议种类、源地址、目的地址、运算符、端口号**。

          其中运算符和端口号是用于匹配 TCP、UDP 数据包中的端口号，可以指定端口号，也可以直接指定具体的协议。根据需要，也可以配置**指定某个网段**，配置整个网段所有的设备的访问控制，也可以**单独指定某一个主机**的访问控制，需要根据不同的环境是具体选择。

          下表为常见的协议所使用的端口号以及该协议使用的传输方式：

          | 端口号 | 协议名称                        | TCP/UDP |
          | ------ | ------------------------------- | ------- |
          | 20/21  | FTP (文件传输协议)              | TCP     |
          | 23     | Telnet (终端连接)               | TCP     |
          | 25     | SMTP (简单邮件传输协议)         | TCP     |
          | 42     | NAMESERVER (主机名字服务器)     | UDP     |
          | 53     | DOMAIN (域名服务器（DNS）)      | TCP/UDP |
          | 69     | TFTP (普通文件传输协议（TFTP）) | UDP     |
          | 80     | WWW (万维网)                    | TCP     |

          上表是一些常用的协议以及这些协议所使用的端口号、传输方式，但是还有一个常用的 ping 命令没有被放进去，因为 ping 命令使用的 ICMP 协议，这个协议将数据包含在 IP 中，没有特定的端口，所以配置的方式也与其他协议的配置方式不同，具体的配置方式为：

          ```shell
          access-list 列表号 permit或deny icmp 源地址 源地址反掩码 目的地址 目的地址反掩码
          ```

          通过端口实现扩展访问控制列表配置**示例**：

          R1

          ```shell
          R1(config)#access-list 100 permit tcp 192.168.1.0 0.0.0.255 192.168.2.0 0.0.0.255 eq 23
          //允许来自 192.168.1.0 网段传输方式为 tcp 的数据通过 23 号端口进入 192.168.2.0 网段
          R1(config)#access-list 100 deny ip any any
          //拒绝其他所有流量
          R1(config)#int f0/0
          R1(config-if)#ip access-group 100 in
          //将规则应用在端口的进方向
          ```

          通过协议实现扩展访问控制列表配置**示例**：

          R1

          ```shell
          R1(config)#access-list 100 permit tcp host 192.168.23.2 host 192.168.23.3 eq telnet     
          //允许 R2 访问 R3 的 telnet 服务
          R1(config)#int f0/0    
          //进入路由器的 f0/0 接口
          R1(config-if)#ip access-group 100 in      
          //将规则应用在入方向
          ```

        - ##### 配置扩展 ACL
        
          1. 根据给出的网络拓扑图添加设备，搭建拓扑图时，需要为路由器 R1 添加 NM-1FE-TX 模块；
        
             ![image-20230508164030138](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/8720988c072a55d36b022f4766135993.png)
        
          2. 根据下表给出的 IP 地址搭建网络拓扑图；
        
             <table>
                 <tbody>
                     <tr>
                         <th>网络设备</th>
                         <th>接口</th>
                         <th>IP地址</th>
                         <th>默认网关</th>
                     </tr>
                     <tr>
                         <td rowspan="3">R1</td>
                         <td>f0/0</td>
                         <td>10.1.1.1/24</td>
                         <td>\</td>
                     </tr>
                     <tr>
                         <td>f0/1</td>
                         <td>20.1.1.1/24</td>
                         <td>\</td>
                     </tr>
                      <tr>
                         <td>f1/0</td>
                         <td>192.168.12.1/24</td>
                         <td>\</td>
                     </tr>
                     <tr>
                         <td rowspan="2">R2</td>
                         <td>f0/0</td>
                         <td>192.168.12.2/24</td>
                         <td>\</td>
                     </tr>
                     <tr>
                         <td>f0/1</td>
                         <td>192.168.23.2/24</td>
                         <td>\</td>
                     </tr>
                           <tr>
                         <td colspan="1">R3</td>
                          <td>f0/0</td>
                         <td>192.168.23.3/24</td>
                         <td>\</td>
                     </tr>
                     <tr>
                         <td colspan="1">PC1</td>
                          <td>e0</td>
                         <td>10.1.1.10/24</td>
                         <td>10.1.1.1</td>
                     </tr>
                     <tr>
                         <td colspan="1">PC2</td>
                          <td>e0</td>
                         <td>20.1.1.10/24</td>
                         <td>20.1.1.1</td>
                     </tr>
                 </tbody>
             </table>
        
          3. 配置 RIPv2 使全网连通，并且测试连通性，开启 R3 的 telnet 设置密码为 123 ；
        
             - R1 配置 RIPv2
        
               ```shell
               R1(config)#router rip
               R1(config-router)#version 2
               R1(config-router)#no auto-summary
               R1(config-router)#network 10.1.1.0
               R1(config-router)#network 20.1.1.0
               R1(config-router)#network 192.168.12.0
               R1(config-router)#exit
               ```
        
             - R2 配置 RIPv2
        
               ```shell
               R2(config)#router rip
               R2(config-router)#version 2
               R2(config-router)#no auto-summary
               R2(config-router)#network 192.168.23.0
               R2(config-router)#network 192.168.12.0
               ```
        
             - R3 配置 RIPv2
        
               ```shell
               R3(config)#router rip
               R3(config-router)#version 2
               R3(config-router)#no auto-summary
               R3(config-router)#network 192.168.23.0
               ```
        
             - 开启 R3 的 telnet 功能
        
               ```shell
               R3(config)#line vty 0 4
               R3(config-line)#password 123
               ```
        
          4. 配置扩展 ACL。
        
      - #### 实训要求

        1. 在 R3 上配置 telnet ，同时允许 0~4 共 5 个设备连接，设置登录密码为 123 ；

        2. 在 R3 上配置扩展 ACL ，列表号设置为 111 ，只配置一条规则，使用 host 指定只允许 R2 telnet R3 ，不配置禁止其他路由器访问 R3 ，配置规则时一个路由器如果有多个端口处于开启状态，选择离目标路由器近的端口作为源地址；

        3. 在 R3 上配置扩展 ACL ，列表号设置为 112 ，同样使用 host 禁止 PC1 ping R3 ，允许所有其他设备 ping R3 ，将规则应用在 R3 的 f0/0 接口生效。

           **注：** 如果在 GNS3 环境中配置正确，使用 PC1 ping R3 将会出现如下图提示：目的网络被强制禁止。

           ![image-20230508164234340](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/da6314eded046d9d5e22f2bf3893fd76.png)

      - #### 操作要求

        打开模拟器之后按照要求搭建拓扑图并完成任务。具体要求如下：

        1. 打开 gns3 ，创建新的工程，将其命名为 **`acl`** ，如果工程名不对将无法通过测评，按照实训要求完成实训任务；
        2. 添加两台路由器三台 PC 并启动电源；
        3. 根据上方作业的拓扑图将设备连接起来；
        4. 根据 IP 地址表为设备配置 IP 地址，并保存设置，路由器特权模式输入`write`保存设置，PC 输入`save`保存设置；
        5. 按照上方作业的要求完成配置；
        6. 按照保存配置文件的方法更新工程目录的配置文件；
        7. 保存之后点击测评，如果不通过可以再保存一遍配置文件，可能会因为延迟问题配置文件来不及更新。

        **配置文件保存方法：**

        1. 在路由器上特权模式下输入`write`等待保存完毕。

        2. 在 GNS3 主页面点击 Tools 并选择第二个选项。

           ![image-20230508161240119](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/b19c692366ac4c4a98cb6c0f81a464db.png)

        3. 在弹出的窗口中选择 Export 选项然后确定。

           ![image-20230508161251165](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/d6d74b01d3bf1fad446dca20b60820ff.png)

        4. 根据图片找到文件保存路径，点击 Open 更新配置文件。

           ![image-20230508161302985](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/f8a234702b8e947d83243616c907d123.png)

        5. 配置上传完毕，可以开始测评。

      - #### 测试说明

        平台会对你创建的工程拓扑图进行测试。

        预期输出为工程拓扑文件中有关节点和节点间连接的关键信息，如实际输出与预期输出不匹配，请检查：

        1. 保存工程文件的文件名及路径是否正确；
        2. 拓扑图中设备的配置及其连接，需按照要求的顺序添加设备以及设备模块和连线。

  - ## 实验五 静态 NAT 配置（3）

    - ### 第1关：静态 NAT 配置

      - #### 任务描述

        本关任务：配置静态 NAT 使内网中的 PC 能正常访问外网 。

      - #### 相关知识

        为了完成本关任务，你需要掌握：静态 NAT 的基本配置方法。

        #### NAT

        ##### 出现的背景：

        联网的设备越来越多，为了安全企业都开始构建自己的专用网，专用网只有一台路由器连接公网，其他所有设备都只在内网工作，这样缓解了 IP 地址资源日益缺少的问题。但是当内部的主机需要访问外网的时候，因为专用网中的所有设备分配的都是专用网中的 IP 地址，而不是公网地址，所以根本无法访问外部网络，但是总有必须访问外网的时候。为了解决这个问题，而又不浪费 IP 地址，于是出现了 NAT （网络地址转换），为需要访问外网的设备分配一个公网的 IP 地址，于是这个设备就可以访问公网了。
        
        网络地址转换可以隐藏专用网中的主机，有一定的安全作用。
        
        ##### NAT 原理：
        
        NAT 是将网络地址从一个内部私有 IP 地址转换为合法 IP 地址的行为。NAT 会将数据包中的 IP 包头改变，将其中的目的地址、源地址或者两个地址都替换成公网地址。NAT 协议将网络分为内部网络 inside 和外部网络 outside 。当内部的数据包发往公网，或外网的数据包发向内网时，就将数据包中的包头更改，达到内网与公网正常通信的效果。
        
        ![image-20230508171900264](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/83e8503455773ca92f1fbe1e5c185f79.png)
        
        NAT 在使用过程种出现一些不可避免的问题：
        
        1. 最初 IP 地址被设计出来就是为每一个设备都分配一个 IP 地址，标识了一个网络的连接，然而 NAT 的出现刚好违反这最初的想法。NAT 使得很多主机可能在使用相同的 IP 地址。
        2. NAT 使得 IP 协议从面向无连接变成了面向连接。NAT 必须维护换用 IP 地址与公用 IP 地址以及端口号之间的映射关系。在 TCP/IP 协议体系中，如果一个路由器出现故障，不会影响 TCP 协议的执行，因为只要几秒收不到应答，发送进程就会进入超时重传处理。而当存在 NAT 时，最初设计的 TCP/IP 协议的处理过程将会发生变化，Internet 可能会变得非常脆弱。
        3. NAT 违反了基本的网络分层结构模型的设计原则。因为在传统的网络分层结构模型中，第 N 层是不能修改第 N+1 层的报头内容的。子网内的主机需要和外网进行通信时，路由器将 IP 首部中的 IP 地址进行替换（替换成 WAN 口 IP ），这样逐级替换，最终数据包中的 IP 地址将变成为一个公网 IP 。NAT 破坏了这种各层之间的独立性。
        4. NAT 存在对高层协议和安全性的影响问题。NAT 只是临时性缓解了 IP 地址短缺的暂时方案，并不是解决问题的根本办法，而根本办法是向 IPv6 迁移。
        
        ##### 静态 NAT ：
        
        如果一个内部主机唯一占用一个公网 IP ，这种方式被称为一对一模型。此种方式下，转换上层协议就是不必要的，因为一个公网 IP 就能唯一对应一个内部主机。显然，这种方式对节约公网 IP 没有太大意义，主要是为了实现一些特殊的组网需求。比如**用户希望隐藏内部主机的真实 IP ，或者实现两个 IP 地址重叠网络的通信**。
        
        ##### 限制：
        
        1. 影响网络速度，NAT 的应用会使 NAT 设备变成网络的瓶颈；
        2. 跟某些应用不兼容，如果一些应用在有效载荷中协商下次会话的 IP 地址和端口号，NAT 将无法对内嵌 IP 地址进行地址转换，造成这些程序不能正常运行；
        3. 地址转换不能处理 IP 报头加密的报文；
        4. 无法实现对 IP 端到端的路径跟踪，经过 NAT 地址转换后，对数据包的路径跟踪变得非常困难。
        
        ##### 静态 NAT 配置方法
        
        NAT 在边界路由器上进行配置，连接内部网络的端口是内部接口，连接外部网络的部分是外部接口，在配置的时候**需要进入接口配置指明内部接口与外部接口**。
        
        ```shell
        R1(config)#int f0/0
        R1(config-if)#ip nat inside
        //将接口标记为内部接口
        R1(config-if)#int s0/0
        R1(config-if)#ip nat outside
        //将接口标记为外部接口
        ```
        
        配置静态 NAT 的时候，内部专用网在外网必须有自己的合法公网地址，才能配置静态 NAT 地址转换，而且静态 NAT 地址转换是一个内网地址对应一个外网地址。
        
        NAT 配置命令为：
        
        ```shell
        ip nat inside(内部） source static（静态） 192.168.1.1（源地址） 100.1.1.1（外部接口IP地址） 
        ```
        
        例：在 R1 上配置静态 NAT ，将 PC1 的 IP 地址 192.168.1.1 映射成公网地址 100.1.1.21 
        
        ![image-20230508172152342](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/08cad1c1ec055e1cca5fe065f0182b1a.png)
        
        R1
        
        ```shell
        R1(config)#int f0/0
        R1(config-if)#ip nat inside
        //将接口设置为内部接口
        R1(config-if)#int s1/0
        R1(config-if)#ip nat outside
        //将接口设置为外部接口
        R1(config-if)#exit
        R1(config)#ip nat inside source static 192.168.1.1 100.1.1.21
        //配置静态 NAT ，指明内部需要连接外网设备的 IP 地址与外部接口 IP 地址
        ```
        
        静态 NAT 配置完毕之后，是无法使用的，因为内部网络的路由器不知道外部网络的路由表，**当内网中有多个路由器的时候需要在内部路由器上配置前往边界路由器 ASBR 的默认路由**，而且由于外网的设备并不知道内部网络的 IP 地址，所以外网设备只能将数据发送给映射之后的外部地址。
        
        查看 NAT 配置：使用`show ip nat translations`可以查看 NAT 的映射关系。
        
      - #### 实训要求

        1. 根据下图搭建网络拓扑图（添加两台路由器，两台 PC ，一台 Switch ，根据要求为路由器添加接口并启动电源）：

           ![image-20230508172402399](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/3d6c97983103ea051a5762ea7e5d38c1.png)

        2. 根据下表，为设备配置 IP 地址：

           <table>
               <tbody>
                   <tr>
                       <th>网络设备</th>
                       <th>接口</th>
                       <th>IP地址</th>
                       <th>默认网关</th>
                   </tr>
                   <tr>
                       <td rowspan="3">Switch1</td>
                       <td>e0</td>
                       <td>\</td>
                       <td>\</td>
                   </tr>
                   <tr>
                       <td>e1</td>
                       <td>\</td>
                       <td>\</td>
                   </tr>
                   <tr>
                       <td>e2</td>
                       <td>\</td>
                       <td>\</td>
                   </tr>
                   <tr>
                       <td rowspan="2">R1</td>
                       <td>f0/0</td>
                       <td>192.168.1.1/24</td>
                       <td>\</td>
                   </tr>
                   <tr>
                       <td>s1/0</td>
                       <td>100.99.12.1/24</td>
                       <td>\</td>
                   </tr>
                   <tr>
                       <td>R2</td>
                       <td>s1/0</td>
                       <td>100.99.12.2/24</td>
                       <td>\</td>
                   </tr>
                   <tr>
                       <td colspan="1">PC1</td>
                        <td>e0</td>
                       <td>192.168.1.10/24</td>
                       <td>192.168.1.1/24</td>
                   </tr>
                   <tr>
                       <td colspan="1">PC2</td>
                        <td>e0</td>
                       <td>192.168.1.20/24</td>
                       <td>192.168.1.1/24</td>
                   </tr>
               </tbody>
           </table>

        3. 配置静态 NAT 。

           以 R1 为边界路由器为 PC1 和 PC2 都配置静态 NAT ，为 PC1 分配的外部地址为 100.99.12.10 ，为 PC2 分配的外部地址为 100.99.12.20 。

      - #### 操作要求

        打开模拟器之后按照要求搭建拓扑图并完成任务。具体要求如下：

        1. 打开 gns3 ，创建新的工程，将其命名为 **`acl`** ，如果工程名不对将无法通过测评，按照实训要求完成实训任务；
        2. 添加两台路由器三台 PC 并启动电源；
        3. 根据上方作业的拓扑图将设备连接起来；
        4. 根据 IP 地址表为设备配置 IP 地址，并保存设置，路由器特权模式输入`write`保存设置，PC 输入`save`保存设置；
        5. 按照上方作业的要求完成配置；
        6. 按照保存配置文件的方法更新工程目录的配置文件；
        7. 保存之后点击测评，如果不通过可以再保存一遍配置文件，可能会因为延迟问题配置文件来不及更新。

        **配置文件保存方法：**

        1. 在路由器上特权模式下输入`write`等待保存完毕。

        2. 在 GNS3 主页面点击 Tools 并选择第二个选项。

           ![image-20230508161240119](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/b19c692366ac4c4a98cb6c0f81a464db.png)

        3. 在弹出的窗口中选择 Export 选项然后确定。

           ![image-20230508161251165](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/d6d74b01d3bf1fad446dca20b60820ff.png)

        4. 根据图片找到文件保存路径，点击 Open 更新配置文件。

           ![image-20230508161302985](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/f8a234702b8e947d83243616c907d123.png)

        5. 配置上传完毕，可以开始测评。

      - #### 测试说明

        平台会对你创建的工程拓扑图进行测试。

        预期输出为工程拓扑文件中有关节点和节点间连接的关键信息，如实际输出与预期输出不匹配，请检查：

        1. 保存工程文件的文件名及路径是否正确；
        2. 拓扑图中设备的配置及其连接，需按照要求的顺序添加设备以及设备模块和连线。

  - ## 实验五 动态 NAT 配置（4）

    - ### 第1关：动态 NAT 配置

      - #### 任务描述

        本关任务：配置动态 NAT 资源池 。

      - #### 相关知识

        为了完成本关任务，你需要掌握：动态 NAT 的基本配置方法。

        - ##### 动态 NAT

          动态 NAT 与静态 NAT 一样都是将内部本地地址转换成公网地址，但是动态 NAT 是从一个公网地址池中选择未使用的地址进行转换，选择的方式是从地址池配置时最前面的开始排起，数据传输完毕之后，使用的公网地址又会被放回地址池给其他需要的设备使用。但是一个地址在被使用的时候，是不能再次进行转换的。

          与静态 NAT 一样，内部网络需要配置前往边界路由器上的默认路由，否则内部网络的设备不知道如何将数据发往公网。

          动态 NAT **配置步骤**：

          1. 创建资源池，命令为：`ip nat pool 资源池名字 起始IP 结束IP netmask 子网掩码`，如果资源池中只有一个公网地址，可以将起始 IP 与结束 IP 都设置为同一个地址，这样资源池中就只有一个公网地址了；
          2. 定义访问控制列表，防止所有设备都能访问外网；
          3. 指定网络地址转换映射，命令为：`ip nat inside source list 列表号 pool 资源池名字`；
          4. 在内部端口和外部端口启用 NAT 。

      - #### 实训要求

        1. 根据下图搭建网络拓扑图

           ![image-20230508174251788](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/a4f64fa7d9f8d67a8ec5dc2b89fab432.png)

        2. 根据下表，为设备配置 IP 地址；

           <table>
               <tbody>
                   <tr>
                       <th>网络设备</th>
                       <th>接口</th>
                       <th>IP地址</th>
                       <th>默认网关</th>
                   </tr>
                   <tr>
                       <td rowspan="3">Switch1</td>
                       <td>e0</td>
                       <td>\</td>
                       <td>\</td>
                   </tr>
                   <tr>
                       <td>e1</td>
                       <td>\</td>
                       <td>\</td>
                   </tr>
                   <tr>
                       <td>e2</td>
                       <td>\</td>
                       <td>\</td>
                   </tr>
                   <tr>
                       <td rowspan="2">R1</td>
                       <td>f0/0</td>
                       <td>192.168.1.1/24</td>
                       <td>\</td>
                   </tr>
                   <tr>
                       <td>s1/0</td>
                       <td>100.99.12.1/24</td>
                       <td>\</td>
                   </tr>
                   <tr>
                       <td>R2</td>
                       <td>s1/0</td>
                       <td>100.99.12.2/24</td>
                       <td>\</td>
                   </tr>
                   <tr>
                       <td colspan="1">PC1</td>
                        <td>e0</td>
                       <td>192.168.1.10/24</td>
                       <td>192.168.1.1/24</td>
                   </tr>
                   <tr>
                       <td colspan="1">PC2</td>
                        <td>e0</td>
                       <td>192.168.1.20/24</td>
                       <td>192.168.1.1/24</td>
                   </tr>
                 <tr>
                       <td colspan="1">PC3</td>
                        <td>e0</td>
                       <td>192.168.1.30/24</td>
                       <td>192.168.1.1/24</td>
                   </tr>
               </tbody>
           </table>

        3. 配置动态 NAT；

           **NAT要求：**

           1. 动态资源池名字为：nat ；
           2. 动态资源池范围为：100.99.12.100~100.99.12.200 ，子网掩码设置为 24 位；
           3. 配置标准访问控制列表：列表号设置为 1 ，只配置允许内部设备 PC1 、PC2 访问公网，不配置禁止访问的规则。

           **查看：** 完成所有配置之后，在 R2 特权模式下输入`debug ip nat`，然后使用 PC1 ping R2，可以看到 R2 收到的信息来自哪个 IP 地址。

      - #### 操作要求

        打开模拟器之后按照要求搭建拓扑图并完成任务。具体要求如下：

        1. 打开 gns3 ，创建新的工程，将其命名为 **`acl`** ，如果工程名不对将无法通过测评，按照实训要求完成实训任务；
        2. 添加两台路由器三台 PC 并启动电源；
        3. 根据上方作业的拓扑图将设备连接起来；
        4. 根据 IP 地址表为设备配置 IP 地址，并保存设置，路由器特权模式输入`write`保存设置，PC 输入`save`保存设置；
        5. 按照上方作业的要求完成配置；
        6. 按照保存配置文件的方法更新工程目录的配置文件；
        7. 保存之后点击测评，如果不通过可以再保存一遍配置文件，可能会因为延迟问题配置文件来不及更新。

        **配置文件保存方法：**

        1. 在路由器上特权模式下输入`write`等待保存完毕。

        2. 在 GNS3 主页面点击 Tools 并选择第二个选项。

           ![image-20230508161240119](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/b19c692366ac4c4a98cb6c0f81a464db.png)

        3. 在弹出的窗口中选择 Export 选项然后确定。

           ![image-20230508161251165](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/d6d74b01d3bf1fad446dca20b60820ff.png)

        4. 根据图片找到文件保存路径，点击 Open 更新配置文件。

           ![image-20230508161302985](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/f8a234702b8e947d83243616c907d123.png)

        5. 配置上传完毕，可以开始测评。

      - #### 测试说明

        平台会对你创建的工程拓扑图进行测试。

        预期输出为工程拓扑文件中有关节点和节点间连接的关键信息，如实际输出与预期输出不匹配，请检查：

        1. 保存工程文件的文件名及路径是否正确；
        2. 拓扑图中设备的配置及其连接，需按照要求的顺序添加设备以及设备模块和连线。

  - ## 实验五 PAT 配置（5）

    - ### 第1关：PAT 配置

      - #### 任务描述

        本关任务：掌握 PAT 的基本原理与配置方式。

      - #### 相关知识

        为了完成本关任务，你需要掌握：

        1. PAT 的原理；
        2. PAT 的配置方式。

        ##### PAT

        PAT 也是属于 NAT 中的一种，只不过 PAT 比普通的 NAT 转换多了一个端口的转换，内网中所有的主机都享用同一个 IP 地址或者少量 IP 地址访问公网，可以最大限度地节省公网 IP 资源，同时又可隐藏网络内部的所有主机，有效避免来自公网的攻击，也是 IPv4 能够维持到今天的最重要的原因之一。

        然而使用同一个 IP 地址，数据如何区分呢？ 

        PAT 给出的答案是通过不同的端口去区分，内网设备的数据发出时，通过不同端口号进行区分；接收数据时，也根据对应的端口号区分数据是发送给内网中哪一台设备的。

        PAT 使用端口对设备进行区分，同时 PAT 也支持静态和动态两种方式。

        - **动态**相对简单，只需要在配置动态 NAT 最后启用资源池的时候，在命令最末尾添加一个`overload`，就可以实现动态 PAT 了。

          例：

          ```shell
          R1(config)#ip nat pool nat 192.168.1.1 192.168.1.1 netmask 255.255.255.0
          //创建资源池
          R1(config)#ip nat inside source list 1 pool nat overload
          //使用端口地址转换
          ```

        - **静态** NAT 在配置的时候，需要在命令中指定数据的传输方式、数据进入的端口、转换之后的端口号。

          例：

          ```shell
          R1(config)#ip nat inside source static tcp或udp 源地址 源端口号 外部接口地址 转换后端口号
          //静态 PAT 需要在静态 NAT 的基础上指明传输的方式，以及端口号
          ```

        **调试：**

        配置过程中，可能因为失误导致配置出错，也可能导致无法达到目的地，所以需要配合查看命令对配置进行调整。

        ```shell
        show ip nat translations
        //查看地址转换表
        show ip statistics
        //查看 IP 统计信息
        debug ip nat
        //查看 nat 的详细转换情况
        no debug ip nat
        //关闭详细情况显示
        ```

        **排错：**

        在确认配置出错之后，需要对 NAT 配置进行修改。**修改的方法为：清除错误的配置，重新进行配置。**

        ```shell
        clear ip nat translation *
        //清除 NAT 所有条目
        clear ip nat translation inside local-ip global-ip
        //清除包含内部转换条目
        ```

      - #### 作业：

        1. 根据拓扑图，添加设备；

           ![image-20230508175005301](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/dc22a6ebc2430719ef5a8e3589a60f4c.png)

        2. 根据 IP 地址表，为设备配置 IP 地址;

           <table>
               <tbody>
                   <tr>
                       <th>网络设备</th>
                       <th>接口</th>
                       <th>IP地址</th>
                       <th>默认网关</th>
                   </tr>
                   <tr>
                       <td rowspan="3">Switch1</td>
                       <td>e0</td>
                       <td>\</td>
                       <td>\</td>
                   </tr>
                   <tr>
                       <td>e1</td>
                       <td>\</td>
                       <td>\</td>
                   </tr>
                   <tr>
                       <td>e2</td>
                       <td>\</td>
                       <td>\</td>
                   </tr>
                   <tr>
                       <td rowspan="2">R1</td>
                       <td>f0/0</td>
                       <td>192.168.1.1/24</td>
                       <td>\</td>
                   </tr>
                   <tr>
                       <td>s1/0</td>
                       <td>100.99.12.1/24</td>
                       <td>\</td>
                   </tr>
                   <tr>
                       <td>R2</td>
                       <td>s1/0</td>
                       <td>100.99.12.2/24</td>
                       <td>\</td>
                   </tr>
                   <tr>
                       <td colspan="1">PC1</td>
                        <td>e0</td>
                       <td>192.168.1.10/24</td>
                       <td>192.168.1.1/24</td>
                   </tr>
                   <tr>
                       <td colspan="1">PC2</td>
                        <td>e0</td>
                       <td>192.168.1.20/24</td>
                       <td>192.168.1.1/24</td>
                   </tr>
                 <tr>
                       <td colspan="1">PC3</td>
                        <td>e0</td>
                       <td>192.168.1.30/24</td>
                       <td>192.168.1.1/24</td>
                   </tr>
               </tbody>
           </table>

        3. 在 R1 上配置动态 PAT ，资源池中只有一个 IP 地址 100.99.12.100 ，启用端口地址转换。

           **要求：**

           1. 配置标准访问控制列表，列表号为 1 ,只配置允许 PC3 访问外网，不配置禁止其他设备访问；
           2. 资源池名字设置为 pat 。

           **配置文件保存方法：**

           1. 在路由器上特权模式下输入`write`等待保存完毕。

           2. 在 GNS3 主页面点击 Tools 并选择第二个选项。

              ![image-20230508161240119](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/b19c692366ac4c4a98cb6c0f81a464db.png)

           3. 在弹出的窗口中选择 Export 选项然后确定。

              ![image-20230508161251165](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/d6d74b01d3bf1fad446dca20b60820ff.png)

           4. 根据图片找到文件保存路径，点击 Open 更新配置文件。

              ![image-20230508161302985](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/f8a234702b8e947d83243616c907d123.png)

           5. 配置上传完毕，可以开始测评。

      - #### 操作要求

        **说明：**

        打开模拟器之后，按照要求搭建拓扑图并完成任务。具体要求如下：

        1. 打开 gns3 ，创建新的工程，将其命名为 **`pat`** ，如果工程名不对将无法通过测评，按照实训要求完成实训任务。配置完成之后，继续按照下面的要求进行测试。根据任务描述搭建网络拓扑图，并完成相应配置；
        2. 添加两台路由器，三台 PC ，一台 Switch ，根据要求为路由器添加接口并启动电源；
        3. 根据上方要求完成作业；
        4. 按照保存配置文件的方法更新工程目录的配置文件；
        5. 保存之后点击测评，如果不通过可以再保存一遍配置文件，可能会因为延迟问题配置文件来不及更新。

      - #### 测试说明

        平台会对你创建的工程拓扑图进行测试。

        预期输出为工程拓扑文件中有关节点和节点间连接的关键信息，如实际输出与预期输出不匹配，请检查：

        1. 保存工程文件的文件名及路径是否正确；
        2. 拓扑图中设备的配置及其连接，需按照要求的顺序添加设备以及设备模块和连线。

- # <a id="pass-code-title" style="color:unset;border-bottom: unset;">测评代码</a>

  **使用说明**

  > 将下面代码直接在命令行复制粘贴回车运行命令
> 图示：
  > ![image-20230424233348985](https://cdn-static.xxcheng.cn/static/blog/images/2023/04/24/21d8753392acfe2dfb19f17608bf933d.png)
  > ![image-20230424233458683](https://cdn-static.xxcheng.cn/static/blog/images/2023/04/24/d169a4f6ea47e539763350a765773576.png)
  > ![image-20230424233927369](https://cdn-static.xxcheng.cn/static/blog/images/2023/04/24/aac858a61d32b5a9c90b75416fd6d4d0.png)

  - ## 实验五 标准 ACL 配置（1）

    - ### 第1关：配置 ACL

      ```sh
      wget "https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/294de2e8c8df4b5b64ce09ac795d49e7.txt" -O "/home/headless/Desktop/workspace/myshixun/test.txt" && mkdir -p "/home/headless/Desktop/workspace/myshixun/acl" && wget "https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/02f9b50137ed62d636223a7151810358.gns3" -O "/home/headless/Desktop/workspace/myshixun/acl/acl.gns3"
      ```

  - ## 实验五 扩展 ACL 配置（2）

    - ### 第1关：扩展ACL
  
      ```sh
      wget "https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/b1b3c4302b484f13fe1838a0f079df79.tar.gz" -O "acl.tar.gz" && tar -xzvf "acl.tar.gz" -C "/home/headless/Desktop/workspace/myshixun/" && rm -f "acl.tar.gz"
      ```
  
  - ## 实验五 静态 NAT 配置（3）

    - ### 第1关：静态 NAT 配置
  
      ```sh
      wget "https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/43c655731a943e971d241b61cc61fb3c.tar.gz" -O "nat.tar.gz" && tar -xzvf "nat.tar.gz" -C "/home/headless/Desktop/workspace/myshixun/" && rm -f "nat.tar.gz"
      ```
  
  - ## 实验五 动态 NAT 配置（4）

    - ### 第1关：动态 NAT 配置

      ```sh
      wget "https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/4bfe8060a3c493c63c9af093a23aafc8.tar.gz" -O "nat.tar.gz" && tar -xzvf "nat.tar.gz" -C "/home/headless/Desktop/workspace/myshixun/" && rm -f "nat.tar.gz"
      ```
  
  - ## 实验五 PAT 配置（5）
  
    - ### 第1关：PAT 配置
  
      ```sh
      wget "https://cdn-static.xxcheng.cn/static/blog/images/2023/05/08/9ebacf7c8bc47e8f3964a77ff9ec728f.tar.gz" -O "pat.tar.gz" && tar -xzvf "pat.tar.gz" -C "/home/headless/Desktop/workspace/myshixun/" && rm -f "pat.tar.gz"
      ```
