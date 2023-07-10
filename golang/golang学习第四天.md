- ### GOPATH

  - #### 弊端

    - 无版本控制概念
    - 无法同步一致第三方版本号
    - 无法指定当前项目引用的第三方版本库

- ### Go Modules

  - #### 命令

    | 命令            | 作用                             |
    | --------------- | -------------------------------- |
    | go mod init     | 生成 go.mod 文件                 |
    | go mod download | 下载 go.mod 文件中指明的所有依赖 |
    | go mod tidy     | 整理现有的依赖                   |
    | go mod graph    | 查看现有的依赖结构               |
    | go mod edit     | 编辑 go.mod 文件                 |
    | go mod vendor   | 导出项目所有的依赖到vendor目录   |
    | go mod verify   | 校验一个模块是否被篡改过         |
    | go mod why      | 查看为什么需要依赖某模块         |

  - #### 开启`module`

    - 方法一
      `go env -w GO111MODULE=on`

      ![image-20230625193127151](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/25/53a5ef8fb0bd013c5df6ce2eddfbb0ad.png)

    - 方法二

      在环境变量添加`GO111MODULE`变量，值设置为`on`

      ```sh
      vim /etc/profile
      
      #最后一行添加
      export GO111MODULE=on
      
      source /etc/profile
      ```

  - #### 新建一个项目初始化

    - 初始化

      `go mod  init moduleName`

      此时，目录下会生成一个`go.mod`文件

      ![image-20230625193450211](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/25/ce72a2318dde511a87fb7469a341a985.png)

    - 下载依赖

      `go get -u modulePath`

      `-u`表示去网络中下载依赖，而不在本地寻找

      ![image-20230625193617506](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/25/a45d8bc780e61ccd7756e6e1e19798b9.png)

      `go.mod`文件会保存依赖关系

      ![image-20230625194924633](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/25/78f42effc6254bd99249d8e308e11646.png)

      如果是第一次下载依赖，会生成一个`go.sum`文件，主要用于记录依赖的hash值，防止被篡改

      ![image-20230625193741760](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/25/2e7a71c60dfb3b48e2ea40ded817eb12.png)

      所有安装的依赖的包，都被安装在`GOPATH`目录的`pkg`目录下

      ![image-20230625193935390](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/25/e09ef41a4fda42927d4f887119649a3c.png)

  ![image-20230625152330618](https://cdn-static.xxcheng.cn/static/blog/images/2023/06/25/5e71d182d7f8f8136ad230f9726d2cf9.png)

- ### 参考链接

  - [Go Modules](https://www.yuque.com/aceld/mo95lb/ovib08)