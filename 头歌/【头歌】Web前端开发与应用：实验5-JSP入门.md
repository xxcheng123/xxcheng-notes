**如需查看过关测评代码直接点击[【测评代码】](#pass-code-title)快速查看**

# 实验描述

- ## 实验5-JSP入门

  - ### 第1关：搭建你的第一个Web服务器

    - #### 任务描述

      认识和使用服务器是学习`JavaWeb`必须要掌握的知识，本关需要你根据文中步骤，**搭建你的第一个Tomcat服务器**。

    - #### 相关知识

      - ##### 什么是服务器（Server）

        服务器一般是由一台或者多台计算机组成的设备，关于服务器你可以简单理解为：**接收请求，做出响应**的设备。

        ![image-20230505144214803](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/7b2452d3ba11e92ac5d610056ac5ffa4.png)

        根据服务器提供的服务类型不同，分为`文件服务器`，`数据库服务器`，`应用程序服务器`，`WEB服务器`等。在这里我们主要学习`WEB服务器`。

      - ##### 服务端应用

        我们都知道。如果一台计算机没有操作系统那么这台计算机就只能算是一个铁盒子，所以服务器同样也需要有操作系统，服务器的操作系统使用最多的是`windows`、`Linux`、`unix`这三种操作系统，而`Linux`由于性能优越，价格便宜，安全性高等优点已经成为了服务器操作系统的首选。 那我们有了操作系统了之后需要什么呢？相信你已经想到了，需要装软件，也就是服务器应用程序。常见的`WEB`服务器程序有很多例如：`IIS`，`Kangle`，`nginx`，`Tomcat`，`apache`，`WebLogic`等等，这里就不一一展开介绍了，在后面的课程中，我们主要学习的是`Tomcat`。

        ![image-20230505144331710](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/3011da73ce07b87b77d1bf242ad7bad3.png)

    - #### 搭建你的第一个服务器

      接下来我们在自己的计算机上搭建一个`Tomcat`服务器。

      1. 首先我们需要下载和解压`Tomcat`，下载地址：https://tomcat.apache.org/download-70.cgi

         ![image-20230505144421156](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/0bd4d5b779c8eaeae07eb5cddd7ac8c0.png)

         ![image-20230505144432397](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/cc75c9c18a5fb5baacf382da091a719f.png)

      2. 进入`Tomcat`文件夹下的`bin`目录，点击`startup.bat`启动`Tomcat`。

         ![image-20230505144454035](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/650fd2864848a05a7c66dccab6fc4a6a.png)

         ![image-20230505144509968](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/137910e755d3ab723918e2bbad746b63.png)

      3. 在浏览器中输入 [http://localhost:8080](http://localhost:8080/) 看到如下界面就说明`Tomcat`服务器已经搭建好啦。

         ![image-20230505144524092](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/770ed49c38d67666c2d9c53e8f60018d.png)

      4. 搭建好之后，通过本机的`IP`地址也可以访问，在同一个局域网下的其他人也可以通过`IP`访问你的网站哦。如下：

         ![image-20230505144538056](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/b981afc74ea31859874bd8dbf34efe0a.png)

      5. 如果以上步骤你都能完成，那恭喜你已经搭建了一个服务器啦，好了，接下来一起我们来写一个网页然后访问它。

         ![image-20230505144551173](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/d23bd9cc65673e4133dc25a3f79e1146.png)

         输入如下代码：

         ```html
         <!DOCTYPE html>
         <html lang="en">
         <head>
             <meta charset="UTF-8">
             <title>Title</title>
         </head>
         <body>
             hello tomcat
         </body>
         </html>
         ```

      6. 按照之前步骤启动`Tomcat`服务器，输入地址：http://localhost:8080/helloTomcat.html 看到如下界面即可。

         ![image-20230505144621406](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/4ee106b6e7fde23cc49ad71be1463436.png)

    - #### 编程要求

      每次你进入实训我们会在后台帮你搭建好服务器运行所需的环境，请在右侧编辑器中输入`hello educoder`，点击评测-->查看效果，就可以看到你刚刚编写代码实现的效果哦。

    - #### 测试说明

      平台会对你的代码进行运行测试，如果实际输出结果与预期结果相同，则通关；反之，则 `GameOver`。

  - ### 第2关：JSP基础（一）

    - #### 任务描述

      本小节需要完成：**创建你的第一个动态网页**，效果图如下：

      ![image-20230505144856374](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/54f3bb73a3ba3dd578a92eccc7ce4fe9.png)

    - #### 相关知识

      - ##### JSP是什么

        `JSP`全名为`Java Server Pages`，中文名叫`java`服务器页面，和我们之前学习的`HTML`静态网页相比，`JSP`是一个动态网页，什么是动态网页呢？我们之前学习的静态网页，在代码编写完成之后，你如果想要改变他原有的效果和数据就只能重新修改它的源代码了，而动态网页就是能在运行的时候根据一些条件来修改网页的效果和数据，动态网页和用户是有**交互**的。 关于`JSP`，你现在可以这样理解：**能嵌入JAVA代码的网页**，当然这个解释不是很准确，不过不用担心，随着你学习的深入这些问题都会迎刃而解的。

      - ##### 为什么学习JSP

        `JSP`程序与`CGI`程序有着相似的功能，但和`CGI`程序相比，`JSP`程序有如下优势：

        1. 性能更加优越，因为`JSP`可以直接在`HTML`网页中动态嵌入元素而不需要单独引用`CGI`文件；
        2. 服务器调用的是已经编译好的`JSP`文件，而不像`CGI/Perl`那样必须先载入解释器和目标脚本；
        3. `JSP` 基于`Java Servlet API`，因此，JSP拥有各种强大的企业级`Java API`，包括`JDBC`，`JNDI`，`EJB`，`JAXP`等等；
        4. `JSP`页面可以与处理业务逻辑的 `Servlet` 一起使用，这种模式被`Java servlet` 模板引擎所支持。

        `JSP`是`Java EE`不可或缺的一部分，是一个完整的企业级应用平台。这意味着`JSP`可以用最简单的方式来实现最复杂的应用。

    - #### 创建你的第一个动态网页

      1. ##### 创建Web项目

         - 打开`Eclipse`创建`Web`项目

           `Eclipse`下载地址：https://pan.baidu.com/s/1o5GrtcndJf208drJ06ZMKA

           ![image-20230505145222890](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/2df769e1d182ea410b8c46dbe9181a9d.png)

           ![image-20230505145232171](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/dd6285464103c37a28db3596395dcb79.png)

           - 配置`Tomcat`服务器

             ![image-20230505145249839](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/6d5b1a6a60b387b13fc6cbcd519ad09d.png)

             ![image-20230505145300139](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/537ddd92d51304df4f61d220f41f000f.png)

             ![image-20230505145307409](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/f7b092a747bdd027f77f77ae2f3d80f2.png)

      2. ##### 创建JSP页面

         - 在`WebContent`目录下新建`index.JSP`页面

           ![image-20230505145339703](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/45121603e2fa35705b90653a8ab87164.png)

           ![image-20230505145348976](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/e1bac3170d33c5010798865497a906d6.png)

           ![image-20230505145403715](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/787540d25881804caaaf741fb8164049.png)

      3. ##### 在JSP中编写JAVA代码

         - 首先我们需要解决编码问题

           因为在网页中我们一般使用中文，如果使用`JSP`默认编码格式会导致乱码，解决办法如下：

           ![image-20230505145454384](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/b471c6c64ac7c5e3814e996a39c631f7.png)

           你可能会想到如果每次新建一个`JSP`文件都需要这样设置，那该有多麻烦呀！一劳永逸的解决办法如下：

           ![image-20230505145505792](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/cd16ec0bca03bcc4795b40b481027ddf.png)

           好了现在我们就可以开始愉快的编程了。

         - 在`JSP`中循环输出信息

           接下来我们在网页中输出`100`行`我要学JSP`

           在你的`JSP`页面中输入代码如图：

           ![image-20230505145529955](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/12f302e20b487a17367f302af8e5dd5f.png)

           运行项目：

           ![image-20230505145546568](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/362a288cb9e0f46b22688e30dd7ccf6b.png)

           查看效果：

           ![image-20230505145601967](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/679bf22307c52e6ce3999c5d045656db.png)

      好了开始编写你的第一个`JSP`网站吧。

    - #### 编程要求

      通过上面的例子，你应该可以发现在`JSP`页面中如果我们想要添加`java`代码，只需要在`<%`和`%>`这对标签中间填充代码即可。

      在右侧编辑器中的`begin-end`之间添加代码，请使用刚刚所学知识和`for`循环在网页中显示九九乘法表，界面效果：

      ![image-20230505145727244](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/6a7faa77225ff0678a8b3d95c28f75f1.png)

    - #### 测试说明

      平台会对你的代码进行运行测试，如果实际输出结果与预期结果相同，则通关；反之，则 `GameOver`。

  - ### 第3关：JSP基础测试题（一）

    完成选择题，巩固已学知识

    1. 关于动态网页的特点，以下说法正确的是（  ）

       A、交互性

       B、自动更新

       C、随机性

       D、以上说法都正确

    2. 如果做动态网站开发,以下（  ）可以作为服务器端脚本语言

       A、java

       B、jsp

       C、JavaScript

       D、html
    
  - ### 第4关：JSP基础（二）
  
    - #### 任务描述
  
      本关需要你：**使用三种JSP脚本元素创建动态网页**，效果图如下：
  
      ![image-20230505150612958](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/d75675b18d3d0d93a7d65497a2cc9071.png)
  
    - #### 相关知识
  
      通过上一节我们知道，`JSP`页面主要由`HTML`和`JSP`代码构成，`JSP`代码是通过`<%`和`%>`符号加入到`HTML`代码中间的，这个就是`JSP`的页面结构，学完上一节你可能会有一些疑问：我们创建的`Web`项目那些文件夹的作用是什么呢？JSP中有哪些是我们需要重点掌握的呢？接下来我们就来解答这些问题。
  
      - ##### Web项目结构
  
        ![image-20230505150640026](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/79f31359ace529155d8e0de34bf283f2.png)
  
      - ##### JSP基本语法
  
        `JSP`程序中的绝大部分标签是以`<%`开始，以`%>`结束的，被标签包围的部分称为`JSP`元素的内容。开始标签、结束标签和元素内容组成`JSP`元素。关于只需要你了解三种即可：`脚本元素`、`指令元素`和`动作元素`。
  
        1. 脚本元素：是嵌入到`JSP`页面中的`Java`代码，包括`JSP`注释、声明、表达式和脚本段。
        2. 指令元素：是针对`JSP`引擎设计的，它控制`JSP`引擎如何处理代码。包括`include`指令，`page`指令和`taglib`指令。
        3. 动作元素：用于连接所要使用的组件，另外还可控制`JSP`引擎的动作。主要有`include`动作和`forward`动作。
  
      - ##### JSP脚本元素
  
        相信对于基本语法你已经有个大概的印象了，不过可能还有点模糊，没关系，本小节只需要掌握脚本元素的使用即可。
  
        `JSP`脚本元素是可以在`JSP`中使用的动态编程语言，即可以在`JSP`中嵌入类似于`Java`的程序。`JSP`脚本元素主要包括注释、声明、表达式和脚本程序。
  
        1. ###### 声明
  
           语法格式如下：
  
           ```jsp
           <%!  声明;[ 声明;]….%>    //声明的变量和方法都是全局属性
           ```
  
        2. ###### 表达式 `JSP`表达式是由变量、常量组成的算式，`Web`服务器会把`Java`表达式计算得到的结果转换成字符串，然后插入到页面中。
  
           其语法格式如下：
        
           ```jsp
           <%=表达式%>
        如：<%=2* count ＋１%> //输出  5
           ```
  
           ![image-20230505150827827](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/7fee9f4765be0ae00aab75609bbd6c3c.png)
  
           输出效果：
  
           ![image-20230505150838857](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/ab9c317e7f9328c491d997d13eb9e08c.png)
  
        3. ###### 脚本程序
  
           脚本程序是`JSP`的主要组成部分，它里面一般是一段`Java`代码，且必须符合`Java`语言要求。当Web服务器收到浏览器端请求时，这段`Java`代码(程序)会被编译执行，执行结果重新嵌入HTML后一起发送到浏览器端。其语法格式如下：
        
           ```jsp
        <% Java代码; %>
           ```
  
           前面的章节中我们其实已经接触过脚本程序了，我们一起来回顾一下。
        
           ![image-20230505150947814](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/d5657eb1e45246825f1f132db586f6c5.png)
  
    - #### 编程要求
  
      是时候检验一下了，请仔细查看右侧编辑器中提示在`start-end`之间输入相应代码完成测试，祝你成功。 
  
      表格效果如下：
  
      ![image-20230505151013105](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/05/90061f5c1879d21150131de953fd2bd9.png)
  
    - #### 测试说明
  
      平台会对你的代码进行运行测试，如果实际输出结果与预期结果相同，则通关；反之，则 `GameOver`。
  
  - ### 第5关：JSP基础测试题（二）
  
    1. 在jsp中，要定义一个方法，需要用到以下哪个元素？（ ）
  
       A、`<%= %>`
  
       B、`<% %>`
  
       C、`<%! %>`
  
       D、`<%@ %>`
  
    2. 在J2EE中，一个test.jsp文件如下，试图运行时，将发生什么情况：（ ）
  
       ```jsp
       <% String str=null;%>
       str is <%=”str”%>
       ```
  
       A、转译期错误
  
       B、编译期错误
  
       C、运行后，浏览器上显示：`str is null`
  
       D、运行后，浏览器上显示：`str is str`

# <a id="pass-code-title" style="color:unset;border-bottom: unset;">测评代码</a>

- ## 实验5-JSP入门

  - ### 第1关：搭建你的第一个Web服务器

    ```jsp
    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Insert title here</title>
    </head>
    <body>
    	<!-- 请在此 添加代码 -->
    	<!-- begin -->
    	hello tomcat
    	<!-- end -->
    </body>
    </html>
    ```
    
  - ### 第2关：JSP基础（一）

    ```jsp
    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <title>Insert title here</title>
    </head>
    <body>
    	<!-- 请在此处添加代码 -->
    	<!-- begin -->
    	for( var i=1;i<=9;i++ ){
            for( var j=1;j<=i;j++ ){
                <% out.println(i+"*"+j+"="+i*j); %>
            }
        }
    	<!-- end -->
    </body>
    </html>
    ```

  - ### 第3关：JSP基础测试题（一）

    1. 关于动态网页的特点，以下说法正确的是（  ）

       A、交互性

       B、自动更新

       C、随机性

       D、以上说法都正确

       **答案：A**

    2. 如果做动态网站开发,以下（  ）可以作为服务器端脚本语言

       A、java

       B、jsp

       C、JavaScript

       D、html

       **答案：B**

  - ### 第4关：JSP基础（二）

    ```jsp
    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>JSP脚本元素测试</title>
    </head>
    <body>
    	<!-- 创建一个公有的整形全局变量count 初始值为0-->
    	<!-- start -->
    	
        <%! int count=0; %>
    	
    	<!-- end -->
    	
    	<!-- 使用JSP脚本程序将count变量+1之后输出 -->
    	<!-- start -->
    		
        <%=count+1; %>
    		
    	<!-- end -->
    	
    	<!-- 使用JSP表达式将count的值输出 -->
    	<!-- start -->
    		使用表达式输出的count值为：<%=count;>
    	<!-- end -->
    
    	<table width="800" cellpadding="0"  border = 1>
    	<tr><td>i</td><td>i的平方</sup></td></tr>
    	<!-- 在这里使用JSP脚本程序输出表格的行和列，循环的变量请使用 "i" 效果图请看编程要求 -->
    	<!-- start -->
    	<%
            for( int i=0;i<=5;i++ ){
                out.println("<tr><td>"i+"</td><td>"+i*i+"</sup></td></tr>");
            }
        %>
    	<!-- end -->
    	</table>
    	
    
    </body>
    </html>
    ```

  - ### 第5关：JSP基础测试题（二）

    1. 在jsp中，要定义一个方法，需要用到以下哪个元素？（ ）

       A、`<%= %>`

       B、`<% %>`

       C、`<%! %>`

       D、`<%@ %>`

       **答案：C**

    2. 在J2EE中，一个test.jsp文件如下，试图运行时，将发生什么情况：（ ）

       ```jsp
       <% String str=null;%>
       str is <%=”str”%>
       ```
    
       A、转译期错误
      
       B、编译期错误
      
       C、运行后，浏览器上显示：`str is null`
      
       D、运行后，浏览器上显示：`str is str`
      
       **答案：D**

# 参考链接

- ### [CSDN-JSP基础（一）](https://blog.csdn.net/qq_45823118/article/details/116724777)