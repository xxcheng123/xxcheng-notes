**#如需查看过关测评代码直接点击[【测评代码】](#pass-code-title)快速查看**

- # 实验描述

  - ## 实验8-1：浏览器对象模型

    - ### 第1关：定时器

      - 任务描述

        本关任务：学习设置和取消定时器。

      - #### 相关知识

        `JavaScript`中定时器的作用是在指定的时间或者时间间隔之后执行函数，可以用来对网页上的数据做实时的更新，比如网页上的北京时间的更新：

        ![image-20230512144405551](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/67c308a972f496ffc1b338cad86e5f6a.png)

        这个可以通过每隔一秒执行一次时间更新函数来实现。

        定时器的实现有两种方式，一个是`window.setTimeout()`函数，一个是`window.setInterval()`函数，本关讲解第一种。

        - ##### 设置定时器

          `window.setTimeout(a,b)`用来指定函数`a`在延迟`b`毫秒时间后执行，即在`window.setTimeout(a,b)`这句话开始执行的`b`毫秒之后，再执行`a`函数。

          比如下面的例子中，我们点击页面上的文字，经过`4`秒的延迟后弹出警告框：

          ```html
          <body>
              <p onclick="al()">
                  单击此处4秒后弹出警告框
              </p>
              <script>
              var id;
              function al() {
                  id = window.setTimeout(showAlert,4000);
              }
              function showAlert() {
                  window.alert("警告框");
              }
          </script>
          </body>
          ```

          点击文字会触发函数`a1()`，在这个函数里面设置了一个定时任务`showAlert()`，定时的时间为`4`秒之后。即：点击文字`4`秒之后会执行`showAlert()`函数。

          效果如下所示：

          ![86c9ea57f3bf8e45367acfa6b661cc57](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/8df5951106678485a4a3df134eb54182.gif)

        - ##### 取消定时器

          上面代码里的变量`id`是用来唯一标识这个定时任务的，它的作用是，作为函数`window.clearTimeout(id)`的参数，而这个函数是用来在定时任务**发生之前**关闭定时任务，这好比，早晨我们在闹钟响之前关掉闹钟。

          对上面的例子稍微改造，在定时任务开启之前，通过调用`window.clearTimeout(id)`关闭定时任务，代码如下：

          ```html
          <body>
              <p onclick="al()">
                  单击此处4秒后弹出警告框
              </p>
              <p onclick="a2()">
                  单击此处取消警告框的弹出
              </p>
              <script>
              var id;
              function al() {
                  id = window.setTimeout(showAlert,4000);
              }
              function showAlert() {
                  window.alert("警告框");
              }
              function a2() {
                  window.clearTimeout(id);
              }
          </script>
          </body>
          ```

          效果如下：

          ![1111](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/df47b164bd58aa720b45ebfcbb8d8363.gif)

          先点击上面一行文字，触发了函数`a1()`，预定了一个`4`秒之后执行的定时任务`showAlert()`。再点击下面一行文字，执行了函数`a2()`，取消了这个定时任务，这样在`4`秒之后**不会弹出警告框**，这是它和上面的例子的区别。

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - 第一步，设置执行一次的定时任务`timerTask()`，延迟为`2000`毫秒，任务的唯一标识赋值给变量`timeId`。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`Timer.html`；
        - 执行其中的`JavaScript`代码，判断定时任务是否设置成功；
        - 执行了检测输出`设置定时任务成功`，否则输出`设置定时任务失败`。

        以下是测试样例：

        - 测试输入： `无测试输入`
        - 预期输出： `设置定时任务成功`

    - ### 第2关：循环定时器

      - #### 任务描述

        本关任务：取消指定的定时任务。

      - #### 相关知识

        上一关讲的定时器只会执行一次，好比早上的闹钟只会响一次，这个在很多情况下显然是不可接受的。本关讲的就是一种循环定时器，它会在指定的时间间隔点上**重复**执行函数。

        - ##### 定时

          `setInterval(a,b)`：每隔`b`毫秒，执行一次`a`函数。

          下面的例子，每隔一秒钟，更新一下网页上的时间：

          ```html
          <body>
              <p onclick="updateTime()">
                  开始更新时间
              </p>
              <p id="timeContainer">
              </p>
              <script>
              var id;
              function updateTime() {
                  id = window.setInterval(showTime,1000);
              }
              function showTime(){        document.getElementById("timeContainer").innerText = new Date();
              }
          </script>
          </body>
          ```

          点击“开始更新时间”，触发`updateTime()`函数，该函数开启一个循环定时任务`showTime()`，`showTime()`的作用是更新网页上显示的时间。

          效果如下所示：

          ![3333](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/e2d8d6eba839b3f687adc17d1e3601a3.gif)

        - ##### 取消定时

          与上一关类似，`window.clearInterval(id)`也是停止定时任务，其中的`id`就是上面例子里面的变量`id`，这个是为了唯一标识某一个定时任务，防止错误的取消了别的定时任务。

          修改一下上面的代码，增加停止时间更新的功能：

          ```html
          <body>
              <p onclick="updateTime()">
                  开始更新时间
              </p>
              <p onclick="stopTime()">
                  停止更新时间
              </p>
              <p id="timeContainer">
              </p>
              <script>
              var id;
              function updateTime() {
                  id = window.setInterval(showTime,1000);
              }
              function showTime() {
                  document.getElementById("timeContainer").innerText = new Date();
              }
              function stopTime() {
                  window.clearInterval(id);
              }
          </script>
          </body>
          ```

          点击“停止更新时间”，触发`stopTime()`函数，在函数里面会取消定时任务`showTime()`。

          其效果如下：

          ![5555](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/6442f3184a9bf1bdf24cfe2a747366d9.gif)

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下，请按步骤操作：

        - 取消定时任务`timerTask1()`，要求使用本关介绍的方法。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`CircleTimer.html`；
        - 执行其中的`JavaScript`代码，检测指定的定时任务是否取消成功；
        - 成功输出`取消定时任务成功`，否则输出`取消定时任务失败`。

        以下是测试样例：

        - 测试输入： `无测试输入`
        - 预期输出： `取消定时任务成功`

    - ### 第3关：location对象

      - #### 任务描述

        本关任务：读取主机名以及实现页面跳转。

      - #### 相关知识

        `location`对象就是`window.location`，记载了浏览器当前所在的窗口的`URL`（统一资源定位符）信息，它常常被用来实现网页的跳转。

        - ##### 页面的跳转

          `location.href`属性表示当前窗口所在页面的地址，比如，如果我们在本网站的首页（`https://www.educoder.net/`），打印`window.location.href`：

          ```js
          <body>
              <script>
                  console.log(window.location.href);
              </script>
          </body>
          ```

          打印出来的结果是：

          ![image-20230512145406935](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/27dbc60e7306f1d131e118e8809a92ab.png)

          这个结果应当和浏览器地址栏上内容一样。

          `window.location.href`还是可写的，如果把它设置为一个新的地址，当前窗口将立即打开这个新的地址，这是实现**页面跳转**的一种方式。比如下面的例子：

          ```html
          <body>
              <p onclick="toNew()">
                  点我调到EduCoder首页
              </p>
              <script>
                  function toNew() {
                      window.location.href = "https://www.educoder.net";
                  }
              </script> 
          </body>
          ```

          点击页面上的文字，效果如下：

          ![666](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/83fb98247f8517516e309997c3dbe716.gif)

        - ##### location对象的其它属性

          假设，当前浏览器地址栏的内容是：`https://www.educoder.net:8080/shixun.html?tab=1`。我们用一个表格看一下`location`对象的所有属性：

          | 属性名   | 解释               | 属性的值                                               |
          | -------- | ------------------ | ------------------------------------------------------ |
          | host     | 主机名和端口号     | [www.educoder.net:8080](http://www.educoder.net:8080/) |
          | hostname | 主机名             | [www.educoder.net](http://www.educoder.net/)           |
          | pathname | 路径部分           | /shixun.html                                           |
          | port     | 端口号             | 8080                                                   |
          | protocal | 协议               | https                                                  |
          | search   | 问号开始的查询部分 | ?tab=1                                                 |

          以下用一个例子打印所有这些属性，假设我们在`https://www.educoder.net:8080/shixun.html?tab=1`这个页面：

          ```html
          <body onload="printInfo()">
              location attribute
              <script>
                  function printInfo() {
                      var loc = window.location;
                      console.log("host:"+loc.host);
                      console.log("hostname:"+loc.hostname);
                      console.log("pathname:"+loc.pathname);
                      console.log("port:"+loc.port);
                      console.log("protocal:"+loc.protocal);
                      console.log("search:"+loc.search);
                  }
              </script>
          </body>
          ```

          载入页面后，打印结果如下：

          ![image-20230512145514143](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/48f036a24a2ab8c56328fa58db73dcb1.png)

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - 第一步，获取`location`对象，赋值给变量`loc`；
        - 第二步，利用`loc`打印出当前页面地址的主机名；
        - 第三步，利用`loc`实现跳转到`https://www.educoder.net/forums/categories/all?order=newest`；
        - 字符串参数放在英文双引号内。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`Location.html`；
        - 执行其中的`JavaScript`代码，检测主机名是否打印正确以及是否按照要求跳转到了指定的页面；
        - 成功输出`读取主机名及页面跳转成功`，否则输出`读取主机名及页面跳转失败`。

        以下是测试样例：

        - 测试输入： `无测试输入`
        - 预期输出： `读取主机名及页面跳转成功`

    - ### 第4关：对话框

      - #### 任务描述

        本关任务：练习输入型对话框的使用。

      - #### 相关知识

        对话框就是在浏览器页面上再弹出一个小的窗口，用于和用户直接互动，`window`对象的`alert()`、`confirm()`和`prompt()`三个方法都是用来显示对话框的。

        - ##### 警告框

          `window.alert(msg)`弹出一个警告框，表示向用户发送一条通知。前面的相关知识已经多次讲过`alert()`的用法。

          需要注意的是，`alert()`方法弹出警告框后，其后的代码会**暂停执行**，直到警告框被关闭，即：`alert()`方法是阻塞的。

          下面的例子中，关闭警告框后，控制台才会打印信息：

          ```html
          <body>
              <p onclick="showAlert()">点我弹出警告框</p>
              <script>
                  function showAlert() {
                      window.alert("先弹出一个警告框");
                      console.log("然后才能在控制台打印信息");
                  }
              </script>
          </body>
          ```

          其效果如下：

        - ##### 确认框

          `window.confirm(msg)`弹出一个确认框，`msg`是需要用户确认的信息，用户在弹出的框里面选择确认或者取消后，会返回`true`或者`false`。

          ```js
          <body>
              <p onclick="showConfirm()">点我弹出确认框</p>
              <script>
              function showConfirm() {
                  var result = window.confirm("确定今天是周五？");
                  console.log(result);
              }
              </script>
          </body>
          ```

          效果如下：

          ![888](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/b40711ae096ab4d0e445c33924b267aa.gif)

          可以看到，点击确认，返回`true`；点击取消，返回`false`。

        - ##### 输入框

          `window.prompt(a,b)`弹出一个输入框，供用户输入关键信息。其中`a`是输入框的提示语，`b`是输入框里面默认的内容。

          如果用户不输入，直接点击确认，返回`b`。如果点击取消，返回`null`。

          下面是一个例子：

          ```html
          <body>
              <p onclick="showPrompt()">点我弹出输入框</p>
              <script>
                  function showPrompt() {
                      var result = window.prompt("请输入姓名：", "Jack");
                      console.log(result);
                  }
              </script>
          </body>
          ```

          效果如下所示：

          ![8888](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/4604a50610e03f068f1dc9a1d4bedaff.gif)

          如动态图所示，不输入点确定返回默认的内容；直接点取消返回`null`。

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - 第一步：弹出一个输入型对话框（输入框），用户的输入结果赋值给变量`result`，输入框提示语为`your name`，输入框默认值为`Xiao Ming`；
        - 第二步：使用`result`判断用户的输入是否为`null`（不是字符串`null`，是表示空的`null`），如果是，在控制台打印`name cannot be null`；
        - 字符串类型的参数用`""`包含在内。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`Dialog.html`；
        - 执行其中的`JavaScript`代码，判断是否检测到了用户的空的输入；
        - 成功输出`检测用户输入框数据成功`，否则输出`检测用户输入框数据失败`。

        以下是测试样例：

        - 测试输入： `无测试输入`

          预期输出： `检测用户输入框数据成功`
      
    - ### 第5关：窗口
    
      - #### 任务描述
    
        本关任务：练习窗口的操作。
    
      - #### 相关知识
    
        浏览器的一个标签页叫做一个窗口，`window`对象可以操作浏览器窗口的打开与关闭。
    
        - ##### 打开浏览器的窗口
    
          `window.open(url,name,specs,replace)`用来打开一个浏览器的窗口，它有四个参数：
    
          `url`表示窗口里面的文档的地址；
    
          `name`有两种情况。
    
          如果`name`里面是窗口的名字。浏览器会先判断这个窗口是否已经打开。已经打开则用新的`url`里面的文档**替换**这个窗口里面原来的文档，反映到浏览器上是不会有新的标签页打开，但是一个已存在的标签页会刷新。没有打开则打开一个新的窗口，并且载入`url`里面的文档。
    
          如果`name`是`_blank`、`_self`里面中的任何一个，它的含义如下：
    
          | 值     | 含义                                              |
          | ------ | ------------------------------------------------- |
          | _blank | 打开新的窗口，载入地址为url的文档                 |
          | _self  | 不打开新的窗口，用地址为url的文档替换掉当前的文档 |
    
          `specs`是用来控制新窗口的尺寸等特征，比如值为`width=200,height=100`时，表示新窗口宽度为`200px`，高度为`100px`。
    
          `replace`用来控制新的窗口在浏览器的浏览历史里面如何显示。为`true`表示装载到窗口的`url`替换掉浏览历史中的当前条目；为`false`表示装载到窗口的`url`创建一个新的条目。
    
          下面的例子展示了这些参数的具体用法：
    
          ```html
          <body>
              <p onclick="openNewWindow()">name是一个窗口的名字，打开一个新的窗口，载入新的文档</p>
              <p onclick="openOldWindow()">name是一个窗口的名字，打开已存在的窗口，替换掉里面的文档</p>
              <p onclick="blankWindow()">name是一个target属性，值为_blank</p>
              <p onclick="selfWindow()">name是一个target属性，值为_self</p>
              <script>
                  function openNewWindow() {
                      window.open("Demo1.html", "windowName");
                  }
                  function openOldWindow() {
                      window.open("Demo2.html", "windowName");
                  }
                  function blankWindow() {
                      window.open("Demo1.html", "_blank");
                  }
                  function selfWindow() {
                      window.open("Demo1.html", "_self");
                  }
              </script>
          </body>
          ```
    
          效果如下所示：
    
          ![999](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/c7474b3ecdc564cfa14d97086a04442f.gif)
    
          如上所示，第一种情况，`name`是一个窗口的名字，因为这个窗口还没有打开，所以会打开一个新的窗口，并载入`url`文档。
    
          第二种情况，`name`是一个窗口的名字，因为这个窗口已打开，所以新的文档会替换掉原来的文档。
    
          第三种情况，`name`值为`_blank`，会直接打开一个新的窗口。
    
          第四种情况，`name`值为`_self`，会用`url`里面的文档替换掉当前窗口的文档，不会打开新的窗口。
    
        - ##### 关闭窗口
    
          上面的`window.open()`会有一个返回值`result`，`result.close()`用来关闭打开的窗口。比如下面的例子：
    
          ```js
          <body>
              <p onclick="openNewWindow()">打开新窗口</p>
              <p onclick="closeNewWindow()">关闭新窗口</p>
              <script>
                  var w;
                  function openNewWindow() {
                      w = window.open("Demo1.html", "windowName");
                  }
                  function closeNewWindow() {
                      w.close();
                  }
              </script>
          </body>
          ```
    
          效果如下：
    
          ![9999](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/51476718b5a7cf3f82dc1c2886ba2479.gif)
    
      - #### 编程要求
    
        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：
    
        - 第一步，打开一个窗口，它的文档的地址是`Demo.html`，`name`属性是窗口名字`window_name`，`specs`和`replace`不用设置。将打开的窗口赋值给变量`myWindow`；
        - 字符串参数放在英文双引号内。
    
      - #### 测试说明
    
        测试过程：
    
        - 平台将读取用户补全后的`Window.html`；
        - 执行其中的JavaScript代码，检测是否按要求打开了一个新的窗口；
        - 成功输出`打开新窗口成功`，否则输出`打开新窗口失败`。
    
        以下是测试样例：
    
        - 测试输入： `无测试输入`
    
          预期输出： `打开新窗口成功`
    
  - ## 实验8-2：HTML DOM——文档元素的操作（一）

    - ### 第1关：通过id获取文档元素

      - #### 任务描述

        本关任务：通过`id`获取指定的文档元素。

      - #### 相关知识

        - ##### 什么是DOM

          `Document Object Module`，简称`DOM`，中文名文档对象模型。在网页上，组成页面（又叫文档）的一个个对象被组织在树形结构中，用这种结构表示它们之间的层次关系，表示文档中对象的标准模型就称为`DOM`。

          `DOM`的作用是给`HTML`文档提供一个标准的树状模型，这样开发人员就能够通过`DOM`提供的接口去操作`HTML`里面的元素。

        - ##### 文档元素

          先看一段网页代码：

          ```html
          <html>
              <head>
                  <title>这里是标题</title>
              </head>
              <body>
                  <p>这是我学习JavaScript的网址：</p>
                  <a href="https://www.educoder.net/paths">JavaScript学习手册</a>
              </body>
          </html>
          ```

          执行后效果如下：

          ![image-20230512162712015](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/2486692481cd46da1faa31742a9e5aa9.png)

          文档元素：指的就是`<html>`、`<head>`等等这样一个个的**标签和里面的内容**。

          比如文档元素`<title>`就是这样：

          ```html
          <title>这里是标题</title>
          ```

          在`JavaScript`中，元素`<title>`**对应一个对象**，这个对象有一个属性的值是“这里是标题”。

          所以，用`JS`操作这些文档元素，操作的就是它们对应的`JS`对象。

        - ##### 节点树

          从代码的缩进可以知道，文档元素之间有层次关系，如下：

          ![image-20230512162749190](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/a871b178ec91452f1e65517189e2ec9f.png)

          上面的图和数据结构中树的概念类似，被称为**节点树**。`<html>`是根节点，网页的所有文档元素都在里面，`<head>`和`<body>`是两个子节点，分别存储网页**标题**有关内容和网页的**主体**部分。

          `JavaScript`要操作这些元素，第一步自然是获得这些元素对应的`JavaScript`对象，那么，怎么获取呢？

        - ##### 通过id获取文档元素

          文档元素一般都有一个`id`属性，它的值在本文档中唯一，如下：

          ```html
          <p id="myId">这是我学习JavaScript的网址：</p>
          ```

          用这个`id`获取`<p>`元素的方法如下：

          ```js
          var pElement = document.getElementById("myId");
          ```

          其中`document`表示整个文档，`getElementById()`是`document`对象的一个方法，参数是`id`属性的值`myId`。

          获取的`pElement`就代表了`<p>`标签以及里面的内容，接下来，可以通过`pElement`操作这个元素。比如可以用弹框展示一下`<p>`标签里面的内容：

          ```js
          window.alert(pElement.innerText);
          ```

          效果如下：

          ![image-20230512162857070](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/ce1454b7328c8c2bb3516c49730e7809.png)

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - 获取本文档中`id`为`a1`的文档元素，要求用`id`获取文档元素；
        - 将获取的元素赋值给变量`myElement`；
        - 具体请参见后续测试样例。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`getElementById.html`；
        - 通过执行其中的`JavaScript`代码判断用户是否正确获得了指定的文档元素；
        - 正确输出`通过id获取元素成功`，否则输出`通过id获取元素失败`。

        以下是测试样例：

        - 测试输入： `无测试输入`

          预期输出： `通过id获取元素成功`

    - ### 第2关：通过类名获取文档元素

      - #### 任务描述

        本关任务：通过类名获取指定的文档元素。

      - #### 相关知识

        除了`id`以外，文档元素另外一个常见的属性是类名。

        - **通过类名获取文档元素**

          文档元素的类名不唯一（存在多个文档元素的类名相同的情况），如下：

          ```html
          <p class="myName">段落</p>
          <a class="myName" href="https://www.educoder.net">这是一个链接</a>
          ```

          `document`的`getElementsByClassName（）`方法用来获取指定类名的文档元素数组（`NodeList`，一般叫节点列表），如下：

          ```js
          var myNodeList = document.getElementsByClassName("myName");
          ```

          这样，`myNodeList[0]`就是`<p>`元素，而`myNodeList[1]`就是`<a>`元素，通过这个方法的名字我们也可以知道获取的标签不唯一。

          我们以弹框的形式查看一下`<p>`里面的内容：

          ```js
          window.alert(myNodeList[0].innerText);
          ```

          效果如下：

          ![image-20230512163325086](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/0d80c66813405e69c08b0f1cf74f5dc8.png)

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - 通过`getElementsByClassName()`方法获取文档中唯一的`<p>`元素；
        - 将获取到的结果赋值给变量`myElement`。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`getElementsByName.html`；
        - 通过执行其中的`JavaScript`代码判断用户是否正确获得了指定的文档元素；
        - 正确输出`通过类名获取文档元素成功`，否则输出`通过类名获取文档元素失败`。

        以下是测试样例：

        - 测试输入： `无测试输入`
        - 预期输出： `通过类名获取文档元素成功`

    - ### 第3关：通过标签名获取文档元素

      - #### 任务描述

        本关任务：通过标签名获取指定的文档元素。

      - #### 相关知识

        通过前面的多个例子，我们可以看到，文档无非是由几个特定的标签组成，比如`<p>`、`<a>`、`<img>`等，由此可以想到，我们能不能通过标签的名字获取特定的文档元素呢？

        - **通过标签的名字获取文档元素**

          标签名指的是`<>`里面的字符串，`document`对象的`getElementsByTagName()`获取整个文档中指定名字的所有标签，显然，结果是一个文档元素数组（节点列表），方法的名字也暗示了这一点。

          ```html
          <div id="div1">
              <p id="p1">文本1</p>
              <p id="p2">文本2</p>
              <a name="a1">链接</a>
          </div>
          <div id="div2">
              <p id="p3" name="a1">文本3</p>
          </div>
          ```

          获取所有的`<div>`元素，如下：

          ```js
          var allDiv = document.getElementsByTagName("div");
          ```

          为了显示效果，我们以页面弹框的形式展示第一个`<div>`里面的内容：

          ```js
          window.alert(allDiv[0]);
          ```

          效果如下：

          ![image-20230512163743940](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/1b3c4c06c336e31ba1966e2718d4cbdd.png)

          这个弹框表明，我们试图弹出的内容是一个`div`元素。

        - **获取标签内部的子元素**

          我们获取到的文档元素，也有`getElementsByTagName()`方法，作用是获取该元素**内部**指定名字的所有子元素。比如，要获取第一个`<div>`里面所有的`<a>`元素，代码如下：

          ```js
          //变量allDiv上面有，这里不再重复！
          var allLink = allDiv[0].getElementsByTagName("a");
          ```

          这样就获取了第一个`<div>`里面的所有超链接元素。

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - 获取第二个`div`元素下的第二个`a`元素，要求通过使用标签名获取；
        - 将获取到的结果赋值给变量`myElement`。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`getElementsByTagName.html`；
        - 通过执行其中的`JavaScript`代码判断用户是否正确获得了指定的文档元素；
        - 正确输出`通过标签名获取文档元素成功`，否则输出`通过标签名获取文档元素失败`。

        以下是测试样例：

        - 测试输入： `无测试输入`

          预期输出： `通过标签名获取文档元素成功`

    - ### 第4关：html5中获取元素的方法一

      - #### 任务描述

        本关任务：使用`querySelector`获取指定的文本元素。在你成功的获得元素后，我们将输出该元素，效果如下：

        ![image-20230512164019632](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/61caca99ceb12631cbe1db58ffcda9b4.png)

      - #### 相关知识

        `html5`中获取文档元素还有两种方法，`querySelector`和`querySelectorAll`。

        - ##### css选择器

          `css`选择器是干什么用的？简单来说，选择你想要的元素的样式。

          这一块的内容对于没有学习过`css`的同学来说比较难，我们分三步来理解：

          第一步：先看一段`html`代码：

          ```html
          <body>
              <p>
                  CSS选择器
              </p>
          </body>
          ```

          运行的效果如下：

          ![image-20230512164055801](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/b556e01bc8d3a2282ebd2499eea40654.png)

          第二步：我们想把字体改为红色，需要使用`css`来处理，假设我们已经有了一段`css`代码：

          ```css
          .css1
          {
              color:red;
          }
          #css2
          {
              color:blue;
          }
          ```

          前四行是一个名字为`css1`的**选择器**，它是一种**类选择器**；后四行是一个名字为`css2`的**选择器**，它是一种**id选择器**。

          第三步：有了`css`，我们还要把它和`html`关联起来，比如我们想在`p`元素上使用第一个选择器，反过来说：就是第一个选择器**选择**了`p`元素（将它的样式应用在`p`元素上）。那么给`p`元素新增一个`class`属性，它的值是`css1`：

          ```html
          <body>
              <p class="css1">
                  CSS选择器
              </p>
          </body>
          ```

          再来看一下效果：

          ![image-20230512164133301](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/359575cdda435cda51f93724039e8542.png)

          这样`p`元素就**选择**了名字为`css1`的样式（反过来说也行），如果`p`元素要选择名为`css2`的样式，因为`css2`是`id`选择器，需要设置`id`属性的值为`css2`：

          ```html
          <body>
              <p id="css2">
                  CSS选择器
              </p>
          </body>
          ```

          效果如下：

          ![image-20230512164155412](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/908e1d6e9b8a8264b1ae033bcdaa0a4b.png)

        - ##### querySelector的用法

          `querySelector`返回匹配指定`css`选择器的第一个元素。

          以上面的`html`代码作为例子，比如我们想要获得`class="css1"`的元素：

          `css1`是一个**类选择器**，在`css`代码里面的完整表示为`.css1`，那么这个`.css1`直接作为`querySelector`的参数，即可获得`class="css1"`的元素：

          ```js
          var myElement =  document.querySelector(".css1");
          console.log(myElement.innerText);//输出“CSS选择器”
          ```

          总结一下就是：用`css`选择器的**完整名字**作为`querySelector`的参数，就可以获取该选择器控制的所有元素。

          需要**注意**的是，`querySelector`只返回**一个**元素，如果指定的选择器控制的元素有多个，返回**第一个**，下面是一个例子：

          先看一段`html`代码：

          ```html
          <body>
              <div class="myClass">文本1</div>
              <div class="myClass">文本2</div>
              <div class="myClass">文本3</div>
          </body>
          ```

          显然，类名为`myClass`的`div`元素有`3`个，使用`querySelector`在控制台输出类名为`myClass`的元素，看能输出几个：

          ```js
          var myClassElement = document.querySelector(".myClass");
          console.log(myClassElement);
          ```

          `F12`查看一下浏览器的控制台，效果如下：

          ![image-20230512164249034](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/089f03bc22273aa61d2a1887391780e0.png)

          很明显，`querySelector`方法**只能获得第一个**类名为`myClass`的元素。

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - 获取`html`代码里面的第一个文本元素，并赋值给变量`pElement`，要求使用`querySelector`方法。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`querySelector.html`；
        - 通过执行其中的`JavaScript`代码判断用户是否正确获得了指定的文本元素；
        - 正确输出`获取指定的文本元素成功`，否则输出`获取指定的文本元素失败`。

        以下是测试样例：

        - 测试输入： `无测试输入`

          预期输出： `获取指定的文本元素成功`

    - ### 第5关：html5中获取元素的方法二

      - #### 任务描述

        本关任务：使用`querySelectorAll()`获取`html`里面所有的文本元素。

        在你成功的获得元素后，我们将输出该元素，效果如下：

        ![image-20230512165354365](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/9b39c0954a3ece728ed5e97782d9e44f.png)

      - #### 相关知识

        `querySelector`只能获得第一个符合要求的文档元素，显然，我们需要一个能获取所有符合要求的文档元素的方法，`querySelectorAll`就是负责这项工作的。

        - ##### querySelectorAll的用法

          如果一个选择器控制的元素有多个，而且我们需要取到这些元素，就需要用`querySelectorAll`方法，该方法返回指定的选择器对应的多个元素。

          比如对于下面一段`html`代码：

          ```html
          <p class="pClass">我是第一个元素</p>
          <p class="pClass">我是第二个元素</p>
          <p class="pClass">我是第三个元素</p>
          ```

          我们分别调用`querySelector`和`querySelectorAll`方法：

          ```js
          var p1 = document.querySelector(".pClass");
          var allP = document.querySelectorAll(".pClass");
          console.log(p1);
          console.log("=====我是巨大的分割线=====");
          console.log(allP);
          ```

          打开浏览器，摁下`F12`，查看效果：

          ![image-20230512165441416](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/2ad2f87cbe5e4d066f353a70bba98634.png)

          浏览器告诉我们：`querySelectorAll`获得的是一个`NodeList`（一个有顺序的节点列表），它有三个元素，所以，很显然，`querySelectorAll`捕获了所有符合要求的元素。

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - 获取`html`代码里面的所有的`p`元素，并赋值给变量`pElement`，要求使用`querySelectorAll`方法。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`querySelector.html`；
        - 通过执行其中的`JavaScript`代码判断用户是否正确获得了指定的文本元素；
        - 正确输出`获取所有的文本元素成功`，否则输出`获取所有的文本元素失败`。

        以下是测试样例：

        - 测试输入： `无测试输入`

          预期输出： `获取所有的文本元素成功`

    - ### 第6关：节点树上的操作

      - #### 任务描述

        本关任务：练习节点树的操作。

      - #### 相关知识

        为了使接下来的`HTML DOM`内容的讲解更加易懂，我们不得不完整的认识一下节点树的有关概念。

        - ##### 节点树的有关概念

          回顾一下前面出现过的这张图：

          ![image-20230512165658091](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/af44371cd581ef8cc3bc93364d291f77.png)

          定义几个概念：

          - 父节点：逆着箭头方向的第一个节点，即为该节点的父节点；
          - 子节点：顺着箭头方向第一层的若干个结点，一般不唯一，即为该节点的的子节点；
          - 兄弟节点：父节点相同的节点互为兄弟节点；
          - 第一个子节点：子节点中最左边的节点；
          - 最后一个子节点：子节点中最右侧的节点；
          - 最后一个子节点：子节点中最右侧的节点；

        - ##### 顺着节点树获取文档元素

          先给一段`html`代码：

          ```html
          <body>
          <div id="div1">
              <div class="cl1">
                  <p>文本</p>
                  <a>超链接</a>
              </div>
              <div class="cl2">
                  <select id="se">
                      <option>红</option>
                      <option>黄</option>
                      <option>蓝</option>
                  </select>
              </div>
          </div>
          </body>
          ```

          首先，我们获取最外层的`div`节点：

          ```html
          var div1 = document.getElementById("div1");
          ```

          然后获取它的第一个子节点（`class`值为`cl1`的节点）：

          ```html
          //firstElementChild表示第一个子节点
          var cl1 = div1.firstElementChild;
          ```

          再获取`cl1`的最后一个子节点，即`a`节点：

          ```html
          //lastElementChild表示最后一个子节点
          var aElement = cl1.lastElementChild;
          ```

          在控制台打印出获取到的节点里面的文本：

          ```html
          console.log(aElement.innerText);
          ```

          效果如下（用浏览器打开这段代码，然后按下`F12`键即可）：

          ![image-20230512165814907](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/1978cfbcabf7e7d59def7c85fc6fbefd.png)

          通过这个例子可以看出，我们的思路是顺着这个节点树从根部一直往下找，即顺着箭头的方向直到目标节点。

        - ##### 其他一些属性

          篇幅有限，这里简单说明一下其它属性，用法和上面的相同。

          - **前一个兄弟节点**

            `previousElementSibling`表示前一个兄弟节点。比如获取上面的超链接的前一个节点`p`，然后在控制台打印`p`的内容，代码以及效果如下：

            ```js
            var left = aElement.previousElementSibling;
            console.log(left.innerText);
            ```

            ![image-20230512165848353](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/893285137d2a0637617ade9d5eb04f7f.png)

          - **后一个兄弟节点**

            `nextElementSibling`表示后一个兄弟节点。显然，上面的`p`元素的后一个兄弟节点是`a`元素，打印一下`a`的内容：

            ```js
            var right = left.nextElementSibling;
            console.log(right.innerText);
            ```

            ![image-20230512165908574](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/d614974aaeda9ae3852610876d8df988.png)

          - **子节点列表： children**

            `children[0]`表示第一个子节点，以此类推。比如依次在控制台打印出上面的`select`标签的三个子节点：

            ```js
            var selectTag = document.getElementById("se");
            console.log(selectTag.children[0].innerText);
            console.log(selectTag.children[1].innerText);
            console.log(selectTag.children[2].innerText);
            ```

            效果如下：

            ![image-20230512165929124](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/9f38b56e09d34c728f5ceb365443c423.png)

            - `children.length`：子节点的个数；

              ```js
              console.log(selectTag.children.length);//输出3
              ```

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - 已知变量`cl2`为获取到的类为`c12`的节点；
        - 要求获取该`div`内文本为`黄`的`option`标签，并赋值给变量`myElement`；
        - 要求使用本关相关知识中介绍的方法。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`NodeTree.html`；
        - 通过执行其中的`JavaScript`代码判断用户是否正确获得了指定的文档元素；
        - 正确输出`获取指定标签成功`，否则输出`获取指定标签失败`。

        以下是测试样例：

        - 测试输入： `无测试输入`

          预期输出： `获取指定标签成功`

    - ### 第7关：属性值的获取

      - #### 任务描述

        本关任务：获取指定的属性。

      - #### 相关知识

        获取了文档元素之后，接下来我们要做的就是获取和设置文档元素的属性的值。

        - ##### 获取文档元素的属性

          通过前面的学习，我们可以发现，文档元素后面都会跟着相应的属性，比如`<a>`后面都会有一个`href`属性，用来表示超链接的地址，即点击这个超链接后跳往的目标页的地址。

          怎么获取属性值呢？先看一段`html`代码：

          ```html
          <a href="https://www.educoder.net" target="_blank">EduCoder</a>
          ```

          先获取文档元素：

          ```js
          var aElement = document.getElementsByTagName("a").[0];
          ```

          然后通过获取到的元素的属性名来得到属性值：

          ```js
          var hrefValue = aElement.href;
          console.log(hrefValue);//输出https://www.educoder.net
          ```

          从上面可以看出，`文档元素.属性名`即获得了该属性的值。

        - ##### getAttribute()

          `getAttribute(a)`用来获取属性值，`a`表示属性的名字。比如上面获取超链接的`href`属性值，也可以这样写：

          ```js
          console.log(aElement.getAttribute("href"));//输出https://www.educoder.net
          ```

          区别是：`getAttribute()`返回的值统一为字符串，而第一种方法会返回属性实际的值，比如`<input>`的`maxLength`属性的值应该是数值，第一种方法就会返回数值，`getAttribute()`返回了字符串：

          ```html
          <input type="name" maxLength=16 id="inputId"/>
          ```

          ```js
          //typeof检测变量的类型
          var result1 = document.getElementById("inputId").maxLength;//返回16
          var result2 = document.getElementById("inputId").getAttribute("maxLength");//返回"16"
          console.log(typeof(result1));//输出number
          console.log(typeof(result2));//输出string
          ```

        - ##### 特别提醒

          `class`等文档元素的属性，不能直接用`文档元素对象.class`来获取，因为`class`是`JavaScript`中的关键字，需要用`className`来代替。

          但是，如果用的是`getAttribute()`，直接用`class`作为参数就行。

          ```html
          <a class="aClass" id="aId">超链接</a>
          ```

          ```js
          document.getElementById("aId").className;//返回"aClass"
          document.getElementById("aId").getAttribute("class");//返回"aClass"
          ```

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - 获取`html`代码里面`<img>`的`class`属性的值；
        - 结果赋值给变量`srcValue`；
        - 获取文档元素通过**类名**来获取。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`getAttribute.html`；
        - 通过执行其中的`JavaScript`代码输出结果；
        - 将结果与预期输出对比后，判断补充的代码是否正确。

        以下是测试样例：

        - 测试输入： `无测试输入`
        - 预期输出： `imgClass`

    - ### 第8关：属性值的设置

      - #### 任务描述

        本关任务：设置表单的提交方法。

      - #### 相关知识

        文档元素属性的值，除了可以获取外，当然也可以设置。设置属性的值也有两种方法。

        - ##### 直接设置

          用`=`连接，左边是你要设置的属性变量，右边是你要赋予这个属性的具体的值。比如：

          ```html
          <a id="a1" href="https://www.google.com">EduCoder</a>
          ```

          设置超链接的`href`属性的值的表达式如下：

          ```js
          document.getElementById("a1").href="https://www.educoder.net";
          ```

          这样，`a`标签的`href`属性的值就变成了`https://www.educoder.net`。

          需要注意：标签之间的文本用`innerText`属性表示，要修改上面超链接里面的文本，需要这样：

          ```js
          document.getElementById("a1").innerText="educoder";
          ```

        - ##### setAttribute()

          没错，`setAttribute(a,b)`就是一个与`getAttribute()`对应的方法，参数`a`是你要设置的属性的名字，参数`b`是你要设置的属性的值。

          所以上面的操作（设置`href`）也可以这样写：

          ```js
          document.getElementById("a1").setAttribute("href","https://www.educoder.net");
          ```

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - 通过`id`来获取`form1`元素（只有一个），赋值给变量`myElement`；
        - 通过操作`myElement`将`form1`的`method`属性改为`post`；
        - 要求必须分两步进行。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`setAttribute.html`；
        - 执行其中的JavaScript代码，输出`form`设置后的的`method`属性；
        - 通过将输出结果与预期输出对比，判断您填写的代码是否正确。

        以下是测试样例：

        - 测试输入： `无测试输入`

          预期输出： `post`

  - ## 实验8-3：HTML DOM——文档元素的操作（二

    - ### 第1关：创建节点

      - #### 任务描述

        本关任务：创建一个表单`<form>`节点。

      - #### 相关知识

        有的时候，我们需要往页面动态的添加文档元素（节点），比如根据省份的不同展现不同的城市列表，在添加节点之前需要先创建该节点。

        - ##### 创建节点

          `document.createElement("tagName")`用来创建一个新的`Element`节点（即文档元素），返回值是新创建的节点，`tagName`是标签的名字，比如`a`、`p`、`img`等，需要注意的是它不区分大小写。

          比如创建一个新的超链接节点：

          ```js
          var newA = document.createElement("a");//创建超链接节点
          newA.src = "https://www.educoder.net";//设置超链接的目的地址
          ```

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - 创建一个`form`节点，然后赋值给变量`newNode`，设置节点的`id`值为`myForm`，`method`值为`post`，如下所示：

          ```html
          <form id="myForm" method="post"></form>
          ```

        - 字符串类型参数用`""`包含在内。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`CreateElement.html`；
        - 执行其中的`JavaScript`代码，输出新节点的`method`值；
        - 输出结果与预期输出相同，则表示您填写的代码正确，否则错误。

        以下是测试样例：

        - 测试输入： `无测试输入`

          预期输出： `post`

    - ### 第2关：插入节点

      - #### 任务描述

        `ul`标签在`html`中表示无序列表，`li`标签标识其中的列表项。

        本关任务：在无序列表中插入新的列表项。

      - #### 相关知识

        插入节点：插入一个新的文档元素。

        最常见的应用就是在`<select>`标签里插入一个新的`<option>`标签。

        - ##### 插入节点

          两种可选的方法：

          - **方法1appendChild()**

            `node1.appendChild(node2)`表示将`node2`插入到`node1`的最后一个子节点中。

            比如原来的选择框是这样：

            ```html
            <select id="s1">
                <option id="cs">长沙</option>
                <option id="zz">株洲</option>
            </select>
            ```

            要把它变成这样：

            ```html
            <select id="s1">
                <option id="cs">长沙</option>
                <option id="zz">株洲</option>
                <option >湘潭</option>
            </select>
            ```

            实现代码如下：

            ```js
            var node1 = document.getElementById("s1");
            var node2 = document.createElement("option");
            node2.innerText = "湘潭";
            node1.appendChild(node2);
            ```

          - **方法2insertBefore()**

            `pNode.insertBefore(node1,node2)`：将`node1`插入作为`pNode`的子节点，并且`node1`排在已存在的子节点`node2`的**前面**。

            比如要把最开始的复选框变成这样：

            ```html
            <select id="s1">
                <option id="cs">长沙</option>
                <option>湘潭</option>
                <option id="zz">株洲</option>
            </select>
            ```

            我们需要像下面这样操作节点:

            ```js
            var pNode = document.getElementById("s1");
            var node1 = document.createElement("option");
            node1.innerText = "湘潭";
            var node2 = document.getElementById("zz");
            //将内容为"湘潭"的节点插入到内容为"株洲"的节点前面
            pNode.insertBefore(node1,node2);
            ```

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - 使用`createElement`方法创建一个`li`类型的新节点`newNode`；
        - 通过`innerText`方法设置该节点内部文本的内容为`Canada`；
        - 通过`getElementById`和`appendChild`方法将`newNode`节点添加到`ul`标签下面，作为它的最后一个子节点；
        - 所有的字符串类型参数请用`""`包含在内；
        - 请按照以上的步骤操作。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`AppendChild.html`；
        - 执行其中的`JavaScript`代码，输出新增列表项的文本内容；
        - 通过将输出结果与预期输出对比，判断您填写的代码是否正确。

        以下是测试样例：

        - 测试输入： `无测试输入`

          预期输出： `Canada`

    - ### 第3关：删除节点

      - #### 任务描述

        `ol`标签在`html`中表示有序列表，`li`标签表示其中的列表项。

        本关任务：在有序列表中删除指定的列表项。

      - #### 相关知识

        这里的删除节点指的是：父元素删除自己的子元素。

        - ##### 删除节点

          删除节点的方法是`removeChild()`，调用者是父节点，参数是子节点，作用是删除这个子节点。

          下面是一个无序列表的代码：

          ```html
          <ul id="parent">
            <li>提子</li>
            <li>车厘子</li>
            <li id="child3">荔枝</li>
          </ul>
          ```

          用浏览器打开这个文件，效果如下：

          ![image-20230512172201799](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/cf428c0565f9983f4fe6823c62046641.png)

          如果我们要删除第三行，可以这样操作：

          - 第一步：获取父节点，即`ul`节点：

            ```js
            var parentNode = document.getElementById("parent");
            ```

          - 第二步：获取待删除的子结点：

            ```js
            var childNode = document.getElementById("child3");
            ```

          - 第三步：父节点调用`removeChild()`方法删除子节点：

            ```js
            parentNode.removeChild(childNode);
            ```

          执行完这三个`js`语句后，再次用浏览器打开，结果为：

          ![image-20230512172258418](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/7c4920a19504ace8ba866d48cedb8cf4.png)

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，要求按照步骤进行：

        - 获取`id`为`browser`的节点，赋值给变量`parent`;
        - 获取`id`为`opera`的节点，赋值给变量`child`；
        - 通过`removeChild`方法删除`child`节点；
        - 获取节点使用`getElementById`方法；
        - 字符串类型参数用`""`包含在内。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`deleteNode.html`；
        - 执行其中的`JavaScript`代码，查看指定列表项是否已经删除；
        - 删除成功输出`删除节点成功`，否则输出`删除节点失败`。

        以下是测试样例：

        - 测试输入： `无测试输入`

          预期输出： `删除节点成功`

    - ### 第4关：替换节点

      - #### 任务描述

        本关任务：替换指定的节点。

      - #### 相关知识

        一般来说，替换节点`=`删除节点`+`新增节点，可以用前面介绍的知识结合起来实现，当然，`js`提供了`replaceChild()`方法一次完成替换。

        - ##### 替换节点

          `replaceChild(a,b)`的调用者是要被替换的节点的父节点，`a`是新的节点，`b`是被替换的节点。

          以无序列表为例：

          ```html
          <ul id="parent">
            <li id="child1">黄山</li>
            <li id="child2">庐山</li>
            <li id="child3">泰山</li>
          </ul>
          ```

          要替换掉第三个子节点，过程如下：

          - 第一步，获取父节点：

            ```js
            var parNode = document.getElementById("parent");
            ```

          - 第二步，获取要被替换的子节点：

            ```js
            var oldNode = document.getElementById("child3");
            ```

          - 第三步，创建新节点：

            ```js
            var newChild = document.createElement("li");
            newChild.innerText = "武夷山";
            ```

          - 第四步，替换：

            ```js
            parNode.replaceChild(newChild,oldNode);
            ```

          替换前后的效果对比：

          ![image-20230512172749288](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/c0a5ca1e63f592143587b00501fb2ea9.png)

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下，请按步骤操作：

        - 获取`id`为`parent`的节点（父节点），赋给变量`parent`；
        - 获取`id`为`old`的节点（子节点），赋给变量`old`；
        - `newChild`已知，用`newChild`替换子节点`old`；
        - 使用`getElementById`方法获取节点；
        - 字符串参数用`""`包含在内。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`replaceNode.html`；
        - 执行其中的`JavaScript`代码，检测节点是否替换成功；
        - 成功输出`替换节点成功`，否则输出`替换节点失败`。

        以下是测试样例：

        - 测试输入： `无测试输入`

          预期输出： `替换节点成功`

    - ### 第5关：综合练习

      - #### 任务描述

        本关任务：练习节点的操作。

      - #### 相关知识

        综合前面学习过的节点的各种操作，可以实现很复杂的功能。

        - ##### 下拉列表的级联

          相信大家都见过这样的下拉框：

          ![image-20230512173036895](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/8e348ef9665fcb87a5f8cd80f039c961.png)

          它有三列，每一列都会根据前一列的结果动态的改变自己的可选项，称为下拉框的级联，那么如何实现呢？

          - 第一步：静态的HTML的代码如下（简单起见，只考虑前两列）：

            ```html
            <select id="province" onChange="changeCity()">
                <option value="BeiJing" id="bj">北京</option>
                <option value="AnHui" id="ah">安徽</option>
            </select>
            <select id="city">
                <option value="BeiJingCity">北京市</option>
            </select>
            ```

            `select`表示选择框，`option`表示选择框里面的每一个选项，`onChange="changeCity()"`表示一旦改变当前的选项，就会触发`JavaScript`函数`changeCity()`的执行。

            对于这个静态的`HTML`页面，不论你在第一列选择的是`北京`还是`安徽`，第二列的选项都只有`北京市`一项。

          - 第二步：获取两个选择框对应的节点元素

            （以下的所有代码都是`changeCity()`函数里面的内容）：

            ```js
            var province = document.getElementById("province");
            var city = document.getElementById("city");
            ```

            `province`变量代表第一列的选择框，`city`变量代表第二列的选择框。

          - 第三步：清空第二列中的所有内容；

            根据第一列的选择结果改变第二列的内容，就是根据父节点的不同替换不同的子节点，我们采用先删除后新增的方法实现替换：

            ```js
            var length = city.children.length;
            for(var i = length-1;i >= 0;i--) {
                city.removeChild(city.children[i]);
            }
            ```

            在`for`循环里面，依次删除`city`节点下面的所有子节点。

            需要注意的是，每删除一个子节点后，表示子节点个数的属性`city.children.length`都会自动减`1`，所以我们需要在`for`循环开始之前索取子节点的长度。

          - 第四步：根据父节点的不同新增不同的子节点：

            ```js
            if(province.value == "BeiJing") {
                var child1 = document.createElement("option");
                child1.value ="BeiJingCity";
                child1.innerText="北京市"
                city.appendChild(child1);
            } else {
                var child1 = document.createElement("option");
                child1.value="HuangShanCity";
                child1.innerText="黄山市";
                city.appendChild(child1);
            }
            ```

            `province.value`表示第一列选中的选项的值，即选中的`option`标签的`value`的值。

            如果第一列选择的是`北京`，我们需要增加一个选项为`北京市`的`option`节点，这个节点将作为`city`的子节点。如果第一列选择的是`安徽`，我们需要增加一个选项为`黄山市`的`option`节点，这个节点将作为`city`的子节点。

            `value`属性表示`option`标签的值，`innerText`表示`option`标签呈现出来的值。

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - 继续拓展相关知识中介绍的功能，要求第一列选择安徽省后，第二列下拉的选项中出现黄山市和合肥市两个选项，按照以下步骤进行；
        - 创建一个`option`类型节点`child2`；
        - 设置`child2`的`value`属性的值为`HeFeiCity`；
        - 设置`child2`的显示的文本为`合肥市`;
        - 将`child2`设置为`id`为`city`的节点下面的第二个子节点；
        - 使用`getElementById`方法获取节点；
        - 字符串类型的参数用`""`包含在内。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`NodeOperate.html`；

        - 执行其中的`JavaScript`代码，检测当第一列选择安徽省后，第二列是否出现黄山市和合肥市两个选项；
        - 成功输出`插入节点成功`，否则输出`插入节点失败`。

        以下是测试样例：

        - 测试输入： `无测试输入`

          预期输出： `插入节点成功`

- # <a id="pass-code-title" style="color:unset;border-bottom: unset;">测评代码</a>

  - ## 实验8-1：浏览器对象模型

    - ### 第1关：定时器

      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <title>Title</title>
      </head>
      <body>
          <p onclick="handleTimer()">set timer then undo</p>
          <script>
              var timeId;
              function timerTask() {
                  console.log("this is timer task");
              }
              function handleTimer() {
      			//请在此处编写代码
      			/********** Begin **********/
                  setTimeout(timerTask,2000);
      			/********** End **********/
              }
          </script>
      </body>
      </html>
      ```

    - ### 第2关：循环定时器

      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <title>Title</title>
      </head>
      <body>
          <p onclick="task1()">this is task onea</p>
          <p onclick="task2()">this is task two</p>
          <p onclick="removeTask1()">try to remove task one</p>
          <script>
              var timeId1;
              var timeId2;
              function timerTask1() {
                  console.log("timerTask1!");
              }
              function timerTask2() {
                  console.log("timerTask2!");
              }
              function task1() {
                  timeId1 = window.setInterval(timerTask1,2000);
              }
              function task2() {
                  timeId2 = window.setInterval(timerTask2,1500);
              }
              function removeTask1() {
      			//请在此处编写代码
      			/********** Begin **********/
                  clearInterval(timeId1);
      			/********** End **********/
              }
          </script>
      </body>
      </html>
      ```

    - ### 第3关：location对象

      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <title>Title</title>
      </head>
      <body>
          <p onclick="openNew()">Click me to new page!</p>
          <script>
              function openNew() {
      		//请在此处编写代码
      		/********** Begin **********/
              var loc=location;
              console.log(loc.hostname);
              loc.href='https://www.educoder.net/forums/categories/all?order=newest';
      		/********** End **********/
              }
          </script>
      </body>
      </html>
      ```

    - ### 第4关：对话框

      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <title>Title</title>
      </head>
      <body>
          <p onclick="inputName()">Click to input name!</p>
          <script>
              function inputName() {
                  var result;
      			//请在此处编写代码
      			/********** Begin **********/
                  result = window.prompt("your name", "Xiao Ming");
                  if( result===null ){
                      console.log('name cannot be null')
                  }
                  result result;
      			/********** End **********/
              }
          </script>
      </body>
      </html>
      ```

    - ### 第5关：窗口

      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <title>Title</title>
      </head>
      <body>
          <p onclick="openNewWindow()">open new window</p>
          <script>
              var myWindow;
              function openNewWindow() {
      		//请在此处编写代码
      		/********** Begin **********/
                  myWindow = window.open("Demo.html", "window_name");
      		/********** End **********/
              }
          </script>
      </body>
      </html>
      ```
    
  - ## 实验8-2：HTML DOM——文档元素的操作（一）

    - ### 第1关：通过id获取文档元素

      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <title>get element by id</title>
      </head>
      <body>
          <a id="a1" src="https://www.google.com">Google</a>
          <p id="p1">this is a text</p>
          <script>
          	<!-- 请在此处编写代码 -->
              <!---------Begin--------->
              var myElement=document.getElementById("a1");
              <!---------End--------->
              myElement.href="https://www.educoder.net";
          </script>
      </body>
      </html>
      ```

    - ### 第2关：通过类名获取文档元素

      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <title>get element by name</title>
      </head>
      <body>
          <img src="" class="myName"/>
          <form class="myName" id="myForm"></form>
          <div class="myName">This is quote</div>
          <p class="myName">This is what you should get</p>
          <script>
          	<!-- 请在此处编写代码 -->
              <!---------Begin--------->
              var myElement=document.getElementsByClassName('myName')[3];
              <!---------End--------->
              myElement.innerText="I changed the text";
          </script>
      </body>
      </html>
      ```

    - ### 第3关：通过标签名获取文档元素

      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <title>get element by id</title>
      
      </head>
      <body>
          <div class="diva">
              <a href="https://www.educoder.net">EduCoder</a>
              <a href="https://www.facebook.com">FaceBook</a>
          </div>
          <div class="divb">
              <a href="https://www.twitter.com">Twitter</a>
              <form name="myForm"></form>
              <a href="https://www.nudt.edu.cn">NUDT</a>
          </div>
          <p id="pp">this is a text</p>
      <script>
      	<!-- 请在此处编写代码 -->
          <!---------Begin--------->
           var myElement=document.getElementsByTagName("div")[1].getElementsByTagName('a')[1];
          <!---------End--------->
          myElement.innerText="nudt";
      </script>
      </body>
      </html>
      ```

    - ### 第4关：html5中获取元素的方法一

      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
      </head>
      <body>
          <p>你需要获得的元素是我</p>
          <p>是楼上</p>
          <p>是楼上的楼上</p>
          <script>
          	<!-- 请在此处编写代码 -->
              <!---------Begin--------->
              var pElement=document.querySelector('p');
              <!---------End--------->
              console.log(pElement);
          </script>
      </body>
      </html>
      ```

    - ### 第5关：html5中获取元素的方法二

      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
      </head>
      <body>
          <p>你需要获得的元素是我</p>
          <p>包括我</p>
          <p>还有我</p>
          <script>
          	<!-- 请在此处编写代码 -->
              <!---------Begin--------->
              var pElement= document.querySelectorAll("p");
              <!---------End--------->
              console.log(pElement);
          </script>
      </body>
      </html>
      ```

    - ### 第6关：节点树上的操作

      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
      </head>
      <body>
      <div id="div1">
      	<div class="cl1">
      		<p>文本</p>
      		<a>超链接</a>
      	</div>
      	<div class="cl2">
      		<select>
      			<option>红</option>
      			<option>黄</option>
      			<option>蓝</option>
      		</select>
      	</div>
      </div>
        <script>
            var cl2 = document.getElementById("div1").lastElementChild;
            <!-- 请在此处编写代码 -->
            <!---------Begin--------->
            var myElement=cl2.children[0].children[1];
            <!---------End--------->
            myElement.innerText = "绿";
          </script>
      </body>
      </html>
      ```

    - ### 第7关：属性值的获取

      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
      </head>
      <body>
          <p id="p"></p>
          <img class="imgClass"/>
          <a id="a"></a>
          <script>
          	<!-- 请在此处编写代码 -->
              <!---------Begin--------->
              var srcValue=document.getElementsByTagName("img")[0].className;
              <!---------End--------->
              console.log(srcValue);
          </script>
      </body>
      </html>
      ```

    - ### 第8关：属性值的设置

      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
      </head>
      <body>
          <p id="p"></p>
          <form id="form1" method="get" target="https://abc.xyz/def/ghi">This is form</form>
          <a id="a"></a>
          <script>
          	<!-- 请在此处编写代码 -->
              <!---------Begin--------->
              var myElement=document.getElementById("form1");
              myElement.setAttribute('method','post');
              <!---------End--------->
              console.log(document.getElementById("form1").method);
          </script>
      </body>
      </html>
      ```

  - ## 实验8-3：HTML DOM——文档元素的操作（二

    - ### 第1关：创建节点

      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
      </head>
      <body>
          <p></p>
          <script>
          	<!-- 请在此处编写代码 -->
              <!---------Begin--------->
              var newNode=document.createElement('form');
              newNode.id='myForm';
              newNode.method='post';
              <!---------End--------->
      		document.body.appendChild(newNode);
              console.log(newNode.method);
          </script>
      </body>
      </html>
      ```

    - ### 第2关：插入节点

      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
      </head>
      <body>
          <ul id="ul1">
              <li>America</li>
              <li>Mexio</li>
          </ul>
          <script>
          	<!-- 请在此处编写代码 -->
              <!---------Begin--------->
              var newNode=document.createElement('li');
              newNode.innerText='Canada';
              document.getElementById('ul1').appendChild(newNode);
              <!---------End--------->
              var out = document.getElementsByTagName("li")[2];
              console.log(out.innerText);
          </script>
      </body>
      </html>
      ```

    - ### 第3关：删除节点

      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
      </head>
      <body>
          <ol id="browser">
              <li id="chrome">Chrome</li>
              <li id="firefox">Firefox</li>
              <li id="opera">Opera</li>
              <li id="safari">Safari</li>
          </ol>
          <script>
          	<!-- 请在此处编写代码 -->
              <!---------Begin--------->
              var parent=document.getElementById("browser");
              var child=document.getElementById("opera");
              parent.removeChild(child);
              <!---------End--------->
          </script>
      </body>
      </html>
      ```

    - ### 第4关：替换节点

      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
      </head>
      <body>
          <ol id="parent">
              <a id="old" href="https://www.google.com">Google</a>
          </ol>
          <script>
              var newChild = document.createElement("a");
              newChild.innerText = "eduCoder";
              newChild.href = "https://www.educoder.net";
              <!-- 请在此处编写代码 -->
              <!---------Begin--------->
              var parent=document.getElementById("parent");
              var old=document.getElementById("old");
              parent.replaceChild(newChild,old);
              <!---------End--------->
          </script>
      </body>
      </html>
      ```

    - ### 第5关：综合练习

      ```html
      <html>
      <head>
      <title>myTitle</title>
      <meta charset="utf-8" />
      </head>
      <body>
      <select id="province" onChange="changeCity()">
      	<option value="BeiJing" id="bj">北京</option>
          <option value="AnHui" id="ah">安徽</option>
      </select>
      <select id="city">
          <option value="BeiJingCity">北京市</option>
      </select>
      <script>
      function changeCity() {
          var province = document.getElementById("province");
          var city = document.getElementById("city");
          var length = city.children.length;
          for(var i = length-1;i >= 0;i--) {
              city.removeChild(city.children[i]);
          }
          if(province.value == "BeiJing") {
              var child1 = document.createElement("option");
              child1.value="BeiJingCity";
              child1.innerText="北京市"
              city.appendChild(child1);
          } else {
              var child1 = document.createElement("option");
              child1.value="HuangShanCity";
              child1.innerText="黄山市";
              city.appendChild(child1);
              //请在此处编写代码
              /*********Begin*********/
              var child2 = document.createElement("option");
              child2.value="HeFeiCity";
              child2.innerText="合肥市";
              city.appendChild(child2);
              /*********End*********/
          }
      }
      </script>
      </body>
      </html>
      ```