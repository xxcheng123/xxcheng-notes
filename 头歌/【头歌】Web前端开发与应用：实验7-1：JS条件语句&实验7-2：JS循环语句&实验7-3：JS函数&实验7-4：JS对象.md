**如需查看过关测评代码直接点击[【测评代码】](#pass-code-title)快速查看**

- # 实验描述

  - ## 实验7-1：JS条件语句

    - ### 第1关：if-else类型

      - #### 任务描述

        本关任务：根据成绩判断考试结果。

      - #### 相关知识

        在编程中，我们常常根据变量是否满足某个条件来执行不同的语句。

        `JavaScript`中利用以`if`关键字开头的条件语句达到以上目的，根据`if`后面括号内表达式的计算结果来进行分支控制。

        - ##### if语句

          一段完整的`JavaScript`语句相当于一条主干路，从第一句开始执行直到最后一句。而`if`语句是一条连接在干路上的支路，满足某个条件时程序进入支路中执行，执行完后回到干路。如下所示：

          ![image-20230511195645745](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/7b37a48931c836be5fb804245b398e78.png)

          条件成立指`if`后面的括号内的表达式的计算结果为`true`。

          `if`语句的结构为：

          ```js
          if(表达式)
          {
              //上面的表达式成立则执行本语句
              语句;
          }
          ```

          比如下面的例子会根据`a`的正负输出相应的结果：

          ```js
          //求一个数的绝对值
          function abs(a) {
              if(a < 0) {//如果a是负数
                  a = -a;//取反
              }
              return a;
          }
          ```

        - ##### if-else语句

          `if-else`相当于干路分成了两条支路，程序执行遇到分支的时候，必须且只能选择其中一条继续执行，结束后回到干路。如下：

          ![image-20230511195717201](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/502994663dd20ab116001c36ea370993.png)

          条件成立时执行语句`1`，这里的语句`1`是条件成立时你希望执行的语句，条件不成立时执行语句`2`。

          语句结构为：

          ```js
          if(条件表达式)
          {
            //条件成立执行语句1
            语句1;
          }
          else
          {
            //条件不成立执行语句2
            语句2;
          }
          ```

          下面是一个具体的例子：

          ```js
          //a为正数或0返回1，a为负数返回0
          function num(a) {
              if(a >= 0) {
                  return 1;
              }
              else {
                  return 0;
              }
          }
          ```

        - ##### 匹配问题

          多个`if-else`连接起来的时候会出现匹配问题，如下面的例子：

          ```js
          function abs(a) {
              if(a >= 0)
                  if(a > 0)
                      a = 1;
              else
                  a = -1;
              return a;
          }
          ```

          从代码的缩进角度看来，程序中的`else`和第一个`if`实现了匹配。但是，实际上`else`匹配的是第二个`if`，因为`JavaScript`中的`else`遵循的是就近匹配，即`else`会和最近的`if`组合成一个完整的`if-else`结构。

          建议：`if`语句执行部分加`{}`，防止出现 `if-else`不匹配问题。

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - 根据分数`a`（百分制）返回考试结果；
        - `a`小于`60`分返回`unpass`，否则返回`pass`；
        - 本题考察非嵌套的`if-else`语句；
        - 具体请参见后续测试样例。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`IfElse.js`；

        - 调用其中的`mainJs()`方法，并输入若干组测试数据；

        - 接着根据测试结果判断程序是否正确。

        以下是测试样例：

        - 测试输入：`59`

          预期输出：`unpass`

        - 测试输入：`90`

          预期输出：`pass`

    - ### 第2关：switch类型

      - #### 任务描述

        北美五大湖的名称和面积如下：

        | 名称     | 面积(平方公里) |
        | -------- | -------------- |
        | Superior | 82414          |
        | Huron    | 59600          |
        | Michigan | 58016          |
        | Erie     | 25744          |
        | Ontario  | 19554          |

        本关任务：根据面积判断湖泊的名字。

      - #### 相关知识

        上一关讲解的是拥有少数分支的`if-else`结构，实际开发的过程中，还会遇到多分支的情况，比如根据电话号码判断运营商，如果用`if-else`型条件语句，代码会很长而且难以理解。所幸的是，`JavaScript`提供了另外一种选择结构：`switch`语句。

        - ##### 严格相等

          在了解`switch`语句之前，先要知道严格相等的概念，严格相等的符号为`===`。

          对于`JavaScript`中的内置数据类型，如数字，字符串，布尔型等。严格相等要求比较双方的数据类型和值都相等，而相等`==`只要求比较双方的值相等，因为可以进行数据类型转换。例子如下：

          ```js
          var string = "1";
          var number1 = 1;
          var number2 = 1;
          console.log(string === number1);
          console.log(number1 === number2);
          ```

          输出结果：

          ```js
          false
          true
          ```

          对于`JavaScript`中的对象类型，严格相等要求双方的引用相同，即必须是同一个对象。如果不是同一个对象，即使双方的属性、值都相同，也被认为不等，比如下面的例子：

          ```js
          var class1 = {
          id:251,
          name:"class"
          }
          var class2 = {
          id:251,
          name:"class"
          }
          var class3 = class1;
          console.log(class2 === class1);
          console.log(class3 === class1);
          ```

          输出结果：

          ```js
          false
          true
          ```

          虽然`class1`和`class2`的属性名、属性值都相等，但是不满足严格相等，因为它们是不同的对象，指向内存的不同地方。而`class3`和`class1`严格相等，因为它们指向内存的同一个地方。

        - ##### switch语句

          `switch`是一种多分支的选择结构，采用等值判断，如下是结构图，其中`T`表示条件成立，`F`表示条件不成立，箭头表示语句的执行方向。

          ![image-20230511200124137](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/d8ede05baa1acae56ffb9182ef884eef.png)

          `switch`语句的语法如下：

          ```js
          switch(表达式)
          {
              case 值1:语句1;
              break;
              case 值2:语句2;
              break;
              case 值3:语句3;
              break;
              default:语句;
              break;
          }
          ```

          括号中的表达式计算后会得到一个值，该值会**从上到下**依次与`case`关键字后面的值比较，如果满足**严格相等**，则执行相应`case`后面的语句；如果与所有的值都不满足严格相等，则执行`default`关键字后的语句。

          当和`case`后面的某一个值完成匹配并执行完语句后，需要用`break`结束整个的`switch`选择，否则会与后面的继续匹配。

          ```js
          //函数（方法）：根据身份证号前两位判断所在省份
          function judgeProvince(idCard) {
              switch(idCard) {
                  case 31:console.log("上海");
                  break;
                  case 32:console.log("江苏");
                  break;
                  case 33:console.log("浙江");
                  break;
                  case 34:console.log("安徽");
                  break;
                  case 35:console.log("福建");
                  break;
                  case 36:console.log("江西");
                  break;
                  case 37:console.log("山东");
                  break;
                  default:console.log("未知");
                  break;
              }
          }
          //调用上面的函数
          judgeProvince(36);//输出“江西”
          ```

          如果不加`break`，代码会从满足`switch`条件的地方开始执行，一直执行到最后，不符合的`case`后面的语句也会被执行。

          下面的例子根据输入的分数计算绩点`（GPA）`：

          ```js
          //函数（方法）：根据百分制的成绩计算GPA
          function calGrade(grade) {
              grade = parseInt(grade/10);//除以10后取整数
              var gpa;
              switch(grade) {
                  case 10://注意这后面没有break
                  case 9: gpa = 4;break;//90到100均为4
                  case 8: gpa = 3;break;
                  case 7: gpa = 2;break;
                  case 6: gpa = 1;break;
                  default: gpa = 0;break;
              }
              return gpa;
          }
          //调用上面的函数
          console.log(calGrage(100));//输出4
          ```

          当`grade`为`10`的时候，没有`break`，会往下一直执行；执行到`grade`为`9`的时候，有`break`，会终止`switch`语句块，此时 `gpa`被赋值`4`，所以`90`到`100`分的返回值都是`4`。

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - 根据面积参数`a`返回湖泊的名字，湖泊的名称和面积的对照表在最上面的任务描述里面，这里不再赘述；
        - 没有对应的湖泊返回`error`；
        - 具体请参见后续测试样例。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`SwitchSeten.js`；
        - 调用其中的`mainJs()`方法，并生成若干组测试数据；
        - 接着根据测试结果判断程序是否正确。

        以下是测试样例：

        - 测试输入：`82414`

          预期输出：`Superior`

    - ### 第3关：综合练习

      - #### 编程要求

        本关的任务分为三个小题，你需要完成全部三个小题才能通关：

        - ##### 第一题

          完成函数`judgeLeapYear(year)`，功能是判断某一年是否为闰年，是则返回“`xxx`年是闰年”，否则返回“`xxx`年不是闰年”。参数`year`是表示年份的四位数整数。

          判断闰年的过程如下：

          ![image-20230511200608376](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/c5ca5d455e10630639c31efb1d874054.png)

          效果如下：

          当`year`等于`2000`，该函数返回“`2000`年是闰年”。

        - ##### 第二题

          完成函数`normalizeInput(input)`，功能是对输入的字符串进行规范化，参数`input`是输入的字符串，返回规范化后的字符串，规范化的标准如下：

          |    input     |     输出     |
          | :----------: | :----------: |
          |   中共党员   |   中共党员   |
          |     党员     |   中共党员   |
          |   共产党员   |   中共党员   |
          | 中共预备党员 | 中共预备党员 |
          |   预备党员   | 中共预备党员 |
          |     团员     |   共青团员   |
          |   共青团员   |   共青团员   |
          |     大众     |     群众     |
          |     群众     |     群众     |
          |     市民     |     群众     |
          |     人民     |     群众     |
          | （其他数据） |   错误数据   |

          注：篇幅有限，这里不单独列出民主党派和无党派人士。

          效果如下：

          当`input`等于“市民”，返回“群众”。

        - ##### 第三题

          完成函数`evaluateApple(weight,water)`，功能是判断苹果是否为优质品，是则返回“是优质品”，否则返回“不是优质品”。参数`weight`表示重量（克），为整数；参数`water`表示含水量，为小数。

          判断标准如下：

          - `weight`大于等于`200`，为优质品；
          - `weight`小于`200`，但`water`大于等于`0.7`为优质品；
          - 其余不是优质品。

          效果如下：

          `weight`为`220`，`water`为`0.6`，返回“是优质品”。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`ConditionSetenceCase.js`；
        - 根据测试输入调用相应的方法，并输入测试数据；
        - 接着根据测试结果判断程序是否正确。

        测试样例：（每个测试用例仅测试其中一个小题！已经用函数名标出来了！相应的输出也只是该题的输出！）

        - 测试输入：`judgeLeapYear:2006`

          预期输出：`judgeLeapYear:2006年不是闰年`

        - 测试输入：`normalizeInput:党员`

          预期输出：`normalizeInput:中共党员`

        - 测试输入：`evaluateApple:200,0.5`

          预期输出：`evaluateApple:是优质品`

  - ## 实验7-2：JS循环语句

    - ### 第1关：while类型

      - #### 任务描述

        质数的定义如下：大于`1`的自然数，且除了`1`和本身外没有别的因数。如`2`、`3`、`5`、`7`。

        本关任务：利用循环结构求质数的和。

      - #### 相关知识

        在选择结构中，条件会被测试一次，成立则执行`if`内语句，结束后回到主线执行下一条语句。循环结构在执行结束后会**再次**判断条件是否成立，这样一直重复下去**直到条件不成立**。

        本关将介绍`while`型循环结构。

        - ##### while类型

          `while`类型的循环结构如下：

          ```js
          while(条件表达式)
          {
          //条件成立执行里面的语句
          }
          ```

          和条件语句一样，循环语句先判断条件表达式是否成立，如果成立，执行大括号内部的语句块；如果不成立则直接跳过循环体。如下：

          ![image-20230511201615028](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/dd4e5f14e445f69d00309c4090e20f3e.png)

          语句块执行结束后，再次**回到**条件表达式，判断表达式是否成立，成立则执行大括号内语句块，不成立执行循环体外下一句。这个过程会一直**重复进行**，直到条件表达式不再成立为止，也就是说，大括号内的语句块有可能被执行多次。

          在执行大括号内的语句块的过程中，条件表达式内的某些变量的值会被改变，等到下一次执行的时候条件表达式有可能不再成立。这样循环会在有限的次数内结束。

          输出`100`以内的偶数的例子：

          ```js
          var i = 0;
          while(i <= 100) {
              console.log(i);
              i = i+2;//条件表达式里面变量i的值改变了
          }
          ```

          在上面的例子中，条件表达式中的`i`变量会在循环体内被加上`2`，这样总会在某个循环时，条件表达式不再成立，循环结束。

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - 求出小于等于整数`a`的所有质数；
        - 计算并返回所有这些质数的和；
        - 具体请参见后续测试样例。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`DoWhile.js`；
        - 调用其中的`mainJs()`方法，并生成若干组测试数据；
        - 接着根据测试结果判断程序是否正确。

        以下是测试样例：

        - 测试输入：`20`

          预期输出：`77`

    - ### 第2关：do while类型

      - #### 任务描述

        本关任务：完成一个函数，用于计算两个参数之间的所有整数的和。

      - #### 相关知识

        上一关介绍的`while`型循环结构又被成为“当”型循环结构，本关将介绍“直到”型循环结构：`do while`。

        - ##### do while类型

          `do whle`类型会在循环体的**末尾**判断条件表达式是否成立，也就是说，循环体内的语句**至少会执行一次**。结构如下：

          ```js
          do
          {
          //循环体内的语句
          }
          while(条件表达式);
          ```

          第一遍循环体内的语句执行结束后，检测条件表达式的返回值，如果返回`true`将会第二次执行循环体，返回`false`结束循环，如下：

          ![image-20230511202431039](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/5d4f971c4c37c99eff52485772461036.png)

          还是第一关中的例子，输出`100`以内的偶数，我们换一种解决方案：

          ```js
          var i = 0;
          do{
              console.log(i);
              i = i+2;
          } while(i <= 100);
          ```

          这种`do while`循环适合用在循环体至少会被执行一次的场景。

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - 求出并返回参数`a`和`b`之间的所有整数的和，不包括这两个端点；
        - 具体请参见后续测试样例。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`DoWhileFunction.js`；

        - 调用其中的`mainJs()`方法，并生成若干组测试数据；
        - 接着根据测试结果判断程序是否正确。

        以下是测试样例：

        - 测试输入：`1,5`

          预期输出：`9`

    - ### 第3关：for类型

      - #### 任务描述

        定义“倒数”如下：把一个数的各位的顺序颠倒，如`1234`的“倒数”是`4321`。

        本关任务：求一个数的“倒数”。

      - #### 相关知识

        `while`和`do while`的一个缺点是循环的次数不够直观，需要通过计算表达式何时返回`false`确定。`JavaScript`提供了新的循环结构：`for`型，这种结构把条件表达式和循环次数**并列**书写，便于控制循环次数。

        - ##### for型

          `for`型循环的结构如下：

          ```js
          for(初始化;条件表达式;修改值)
          {
          //条件表达式成立执行的语句块
          }
          ```

          初始化、条件表达式、修改值都操作控制循环次数的变量，初始化对该变量赋一个**初值**，紧接着**执行**条件表达式，如果返回`true`则进入循环体内，否则直接跳过循环体。如下：

          ![image-20230511202947522](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/376a50b7d60cc0b794e1f723453e5289.png)

          循环体内执行结束后，修改值操作会修改变量的值，紧接着再次执行条件表达式，根据返回值决定是否进入循环体，这个步骤会一直重复进行下去，**直到**条件表达式返回`false`，循环结束。

          初始化只执行一次，条件表达式在每一次进入循环体之前执行，修改值在每次执行完循环体之后进行。这三个式子都可以没有，但是整个括号内必须有**两个分号**。

          还是以上一关中的输出小于等于`100`的偶数为例子：

          ```js
          for(var i = 0;i <= 100;i+=2) {
              console.log(i);
          }
          ```

          与上一关不同的是，这里变量`i`的初始化和增加都是在括号内，循环体内只有一句。

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - 计算并返回整数`a`的“倒数”；
        - 具体请参见后续测试样例。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`ForIn.js`；
        - 调用其中的`mainJs()`方法，并生成若干组测试数据；
        - 接着根据测试结果判断程序是否正确。

        以下是测试样例：

        - 测试输入：`1234`

          预期输出：`4321`

        - 测试输入：`78`

          预期输出：`87`

    - ### 第4关：for in类型

      - #### 任务描述

        苹果`apple`有多个属性表示它的产地，比如`locationProvince`表示省份，这些属性都以`location`开头，和产地无关的属性都不以`location`开头。

        本关任务：完成一个计算苹果产地的函数。

      - #### 相关知识

        - ##### for in型

          `JavaScript`的`for in`循环主要用于**枚举**对象的**可枚举属性名**，可枚举属性的定义请参考《`JavaScript`学习手册四：`JS`对象》。

          对象类型是键值对的集合，键指的是属性的名字，值指的是属性的值。

          `for in`除了枚举对象自己拥有的可枚举属性外，还会枚举**继承**的可枚举属性。

          ```js
          var orange = {
              color:"orange",
              weight:200,
              location:"GanZhou",
              date:"October"
          };
          for(var att in orange) {
              console.log(att);//依次输出color,weight,location,date
          }
          ```

          > 注意：上述代码中，att 是临时变量，可以是其他名称。

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - 求出`apple`对象所有表示产地的属性的值（这些值都是字符串），然后拼接这些值，并返回；
        - 注意我们有可能通过参数`a`和`b`给`apple`添加新的表示产地的属性，也有可能修改已有的属性的值，所以不要投机取巧哦；
        - 提示：`a.indexOf("location")`的结果如果为`0`，表示字符串`a`以`location`开头；
        - 具体请参见后续测试样例。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`ForInFunction.js`；
        - 调用其中的`mainJs()`方法，并生成若干组测试数据；
        - 接着根据测试结果判断程序是否正确。

        以下是测试样例：

        - 测试输入：`locationCounty,米脂县`
        - 预期输出：`陕西省榆林市米脂县`

    - ### 第5关：break和continue的区别——break

      - #### 任务描述

        本关任务：求数组中的质数。

      - #### 相关知识

        前四关介绍了四种循环结构，这些结构都是建立在**已知循环次数**的基础上。在很多情况下，不能确定循环次数，即循环会在进行到满足某个特定的条件时结束。比如，当数组中出现第二个整数`0`时，结束元素的输出。这些情况下就需要使用`break`和`continue`关键字帮助**结束循环**。本关介绍关键字`break`。

        - ##### break

          `break`的作用是**跳出循环**，跳过循环体内`break`下面的所有语句以及剩余的所有循环，而直接执行循环体外下面的第一句。`break`通常和`if`条件语句一起使用，表示满足该条件时结束循环。

          ```js
          for(;;)
          {
          if(条件语句) break;
          }
          //满足条件时直接跳到这里执行
          ```

          下面的例子输出一个数组，当遇到数组中第一个负数时，结束输出。

          ```js
          //下面的整个程序将依次输出12,23,满足条件直接执行我这里！
          var arr = [12,23,-1,45,2,0,-1];
          for(var i = 0;i < arr.length;i++) {
              if(arr[i] < 0) break;
              console.log(arr[i]);
          }
          console.log("满足条件直接执行我这里！");
          ```

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - 返回数组`arr`中第一个质数；
        - 具体请参见后续测试样例。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`BreakContinue`；
        - 调用其中的`mainJs()`方法，并生成若干组测试数据；
        - 接着根据测试结果判断程序是否正确。

        以下是测试样例：

        - 测试输入：`9,8,4,1,2`

          预期输出：`2`

    - ### 第6关：break和continue的区别——continue

      - #### 任务描述

        本关任务：计算数组中所有正数或者所有负数的和。

      - #### 相关知识

        上一关介绍了关键字`break`的使用，`continue`是一个和`break`含义十分接近的关键字，本关将详细剖析`continue`的用法。

        - ##### continue语句

          `continue`的作用是结束**本次**循环，即循环体内`continue`下面的语句不再执行，直接进入**下一个循环周期**。

          比如上一关的例子中，原要求是遇到第一个负数时结束输出。现在把要求改成：输出数组中的所有正数。这个时候就需要用到`continue`语句。

          ```js
          //只输出所有的正数，程序将依次输出12,23,45,2
          var arr = [12,23,-1,45,2,0,-1];
          for(var i = 0;i < arr.length;i++) {
              if(arr[i] <= 0) continue;
              console.log(arr[i]);
          }
          ```

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - `a`是一个数字数组，`b`是非零整数；
        - 如果`b`为正数，计算`a`中所有正数的和；如果`b`是负数，计算`a`中所有负数的和；
        - 具体请参见后续测试样例。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`BreakContinueFunction.js`；
        - 调用其中的`mainJs()`方法，并生成若干组测试数据；
        - 接着根据测试结果判断程序是否正确。

        以下是测试样例（**分号前面是数组`a`，分号后面是`b`**）：

        - 测试输入：`-2,1,4,6,-1;1`

          预期输出：`11`

  - ## 实验7-3：JS函数

    - ### 第1关：用函数语句定义函数

      - #### 任务描述

        本关任务：用函数语句定义一个函数。

      - #### 相关知识

        函数的定义是指用一段代码实现函数的功能，通常的定义方式以关键字`function`开头。

        - ##### 用函数语句定义

          先给一个例子，该函数的功能是返回数组元素的和；

          ```js
          function sumArray(arr) {
              var sum = 0;
              for(var i = 0,aLength = arr.length;i < aLength;i++) {
                  sum += arr[i];
              }
              return sum;
          }
          ```

          关键字`function`后面空一格，`sumArray`是函数的名字，其命名规范与变量名的命名规范相同：只能有字母、数字、下划线和美元符号，不能以数字开头，不能是关键字。

          括号中是参数，又叫**形式参数**，只需要参数名就可以。参数可以是`0`个、`1`个或者多个，相互之间用`,`隔开，`{}`中间包含的是**函数体**。含有一条或者多条语句。函数体用来实现函数的功能。

          关键字`return`后面是函数的**返回值**，函数也可以没有返回值。函数运行完`return`这句话这里就会退出运行，`return`下面的语句**不再运行**。返回值即函数的输出。

          用这种方式定义的函数，在函数定义的前面和后面都可以调用该函数，只要函数和调用函数的语句在一个源文件里面就可以了。

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - 定义一个名字为`mainJs()`的函数；
        - 该函数有两个参数，均为字符串类型；
        - 函数的功能是返回这两个参数的拼接结果；
        - 具体请参见后续测试样例。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`FunctionCreate.js`；
        - 调用其中的`mainJs()`方法，并输入若干组测试数据；
        - 接着根据测试结果判断程序是否正确。

        以下是测试样例：

        - 测试输入：`child,hood`

          预期输出：`childhood`

    - ### 第2关：用表达式定义函数

      - #### 任务描述

        本关任务：定义一个匿名函数。

      - #### 相关知识

        除了最直观的用函数语句定义函数之外，还可以用表达式定义函数。

        - ##### 用表达式定义

          用表达式的方式定义函数，就是用赋值表达式**把函数赋值给一个变量**，这其实就是把函数看成一个变量。这个时候函数可以有名字，也可以没有名字，没有名字的函数叫做**匿名函数**。

          - 带名字的；

            ```js
            var funct = function getMax(a,b) {
                return a>b?a:b;
            };//注意这后面的分号不能少，因为我们定义的是一个变量!
            ```

            和前一关不同的是，只能在**函数定义语句之后**调用该函数，且调用的时候只能用**变量名**`funct`，不能用函数名`getMax`，如：

            ```js
            var funct = function getMax(a,b) {
                return a>b?a:b;
            };
            console.log(funct(1,2));//输出2
            ```

          - 匿名函数

            所谓匿名函数就是关键字`function`之后直接是参数列表：

            ```js
            var funct = function(a,b) {
                return a>b?a:b;
            };
            ```

            这个函数没有名字，它被赋值给了变量`funct`，所以叫匿名函数。同样，也只能在这一语句之后调用该函数。

            ```js
            var funct = function(a,b) {
                return a>b?a:b;
            };
            console.log(funct(1,2));//输出2
            ```

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - 定义一个匿名函数，将它赋值给变量`myFunc`；
        - 该函数实现求一个三位数的各个位上的数字之和，比如`123`各个位上的数字分别为`1`、`2`和`3`，和是`6`；
        - 具体请参见后续测试样例。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`AnonymousFunctionCreate.js`；
        - 调用其中的`mainJs()`方法，并输入若干组测试数据；
        - 接着根据测试结果判断程序是否正确。

        以下是测试样例：

        - 测试输入： `123`

          预期输出： `6`

    - ### 第3关：函数的调用

      - #### 任务描述

        本关任务：根据要求调用不同的函数。

      - #### 相关知识

        - ##### 函数的调用

          在实训四中，我们曾经介绍过对象可以有自己的方法，当然这也是函数。这种函数的调用和前面两关定义的函数有细小的区别。

          ```js
          //函数的定义：求三个数的最大值
          function max(a,b,c) {
              if(a > b) {
                  if(a > c)
                      return a;
                  else 
                      return c;
              }
              else {
                  if(b > c)
                      return b;
                  else 
                      return c;
              }
          }
          //调用该函数
          var result = max(1,2,3);//result为3
          console.log(result);//输出3
          ```

          调用函数的时候，需要传入和形参相同个数的的具体值，上面的函数有`3`个参数，所以下面调用的时候传入`3`个具体的值，`1`传给参数`a`，`2`传给参数`b`，`3`传给参数`c`。函数的返回值通过赋值符号`=`传给了变量`result`。如果函数体内没有`return`关键字，将返回`undefined`。

          再来看一下对象里定义的函数的调用：

          ```js
          var ob = {
              id:1,
              getMax:function(a,b) {
                  return a>b?a:b;
              }
          };
          var result = ob.getMax(2,1);//result值为2
          var result1 = ob["getMax"](2,1);//result1的值也是2
          ```

          与上面的区别是，这里要定位到函数，需要使用`对象名.函数名`或者`对象名["函数名"]`,其它相同。

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - `mainJs()`代码区上面定义了三个函数，从上到下分别编号为`1`、`2`和`3`；
        - `mainJs()`有三个参数`a`、`b`和`c`，根据`a`的值（函数的编号，可取的值是`1`、`2`和`3`）调用相应的函数（可选的函数分别是`getMax()`、`getMin()`和`getSum()`，具体请参考代码！），并传入参数`b`和`c`，返回得到的结果；
        - 比如`a`为`1`表示你需要调用函数`getMax()`；
        - 具体请参见后续测试样例。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`FunctionCall.js`；
        - 调用其中的`mainJs()`方法，并输入若干组测试数据；
        - 接着根据测试结果输出判断程序是否正确。

        以下是测试样例：

        - 测试输入：`1,22,33`

          预期输出：`33`

    - ### 第4关：未定义的实参

      - #### 任务描述

        `JavaScript`的实际参数的个数有时候是不确定的。

        本关任务：学习处理未定义的实参。

      - #### 相关知识

        函数的基本功能是对函数内的参数进行操作，其中，函数定义时的参数被称为形式参数，函数被调用时传入的参数被称为**实际参数**。

        - ##### 未定义的实参

          在大部分的编程语言里面，都会对调用函数时传入的实参个数和类型进行检查，而`JavaScript`既**不检查**实参的类型，也不检查实参的个数。

          `JavaScript`中的实参会按照顺序**从左到右**依次匹配上形参，例如：

          ```js
          function myFunction(a,b,c) {
              console.log(a);
              console.log(b);
              console.log(c);
          }
          myFunction(1,2,3);
          ```

          实参`1`传入形参`a`，实参`2`传入形参`b`，实参`3`传入形参`c`。 当实参个数少于形参时，靠右的形参会被传入值`undefined`。如：

          ```js
          function myFunction(a,b,c) {
              console.log(a);
              console.log(b);
              console.log(c);
          }
          myFunction(1,2);
          ```

          实参`1`传入形参`a`，实参`2`传入形参`b`，`undefined`传入形参`c`。 如果只想给右侧的参数传入数据，可以给前几个实参传入`undefined`。如：

          ```js
          function myFunction(a,b,c){
          console.log(a);
          console.log(b);
          console.log(c);
          }
          myFunction(undefined,1,2);
          ```

          上面这两种做法不够严谨，最佳实践是给可能被传入`undefined`值的形参设定一个**默认值**。如：

          ```js
          function getSum(a,b,c) {
              if(c === undefined) 
                  c = 0;
              console.log(a+b+c);
          }
          myFunction(1,2);
          ```

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：

        - 路口有四个方向的红绿灯，其默认值分别是`green`、`green` 、`red`和`yellow`；
        - 对于函数`mainJs(a,b,c,d)`的四个参数，要求在没有传入实参或者传入`undefined`时，其分别设置为上述默认值；
        - 最后返回四个字符串型参数的拼接结果，字符串中间用`-`符号隔开，如分别传入`red`、`red`、`yellow`和`undefined`时，返回`red-red-yellow-yellow`；
        - 具体请参见后续测试样例。

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`UndefinedArguments.js`；
        - 调用其中的`mainJs()`方法，并输入若干组测试数据；
        - 接着根据测试结果判断程序是否正确。

        以下是测试样例：

        - 测试输入： `red,red,red`
        - 预期输出： `red-red-red-yellow`
      
    - ### 第5关：实参对象
    
      - #### 任务描述
    
        本关任务：编写一个计算若干个数的最大值的程序。
    
      - #### 相关知识
    
        - ##### 实参对象
    
          `JavaScript`一切都是对象，实参也是一个**对象**，有一个专门的名字`arguments`，这个对象可以看成一个数组（类数组，不是真的数组），实参从左到右分别是`arguments[0]、arguments[1]...`，`arguments.length`表示实参的个数。
    
          实参对象一个最重要的应用是**可变长参数列表**，想象一下求一组数的和，如果这组数不在一个数组里面，使用函数来求则无法定义函数体，因为不知道形参的个数。这个时候就可以用`arguments`来解决问题。如:
    
          ```js
          //求参数的和
          function getSum() {
              var aLength = arguments.length;
              var sum = 0;
              for(var i = 0;i < aLength;i++) {
                  sum += arguments[i];
              }
              return sum;
          }
          console.log(getSum(1,2,3,4,5))//输出15
          ```
    
          这里的形参直接省略，使用`arguments[i]`表示。
    
      - #### 编程要求
    
        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：
    
        - 定义函数`getMax()`；
        - 该函数计算并返回一组整数的最大值；
        - 整数的个数不确定；
        - 如果整数个数为`0`，直接返回`0`；
        - 具体请参见后续测试样例。
    
      - #### 测试说明
    
        测试过程：
    
        - 平台将读取用户补全后的`FunctionArguments.js`；
        - 调用其中的`mainJs()`方法，并生成若干组测试数据；
        - 接着根据测试结果判断程序是否正确。
    
        以下是测试样例：
    
        - 测试输入：`1`
    
          预期输出：`123`
    
    - ### 第6关：对象作为参数
    
      - #### 任务描述
    
        本关任务：编写一个以对象作为参数的函数。
    
      - #### 相关知识
    
        - ##### 对象作为参数
    
          复杂的函数通常多达十几个参数，尽管`JavaScript`不做参数个数和类型的检查，但是调用时实参的顺序不能乱。开发人员需要检查每一个实参和形参的对应关系，这样效率很低。一种很好的解决方案是使用对象作为参数，函数会根据对象的**属性名**操作参数。
    
          ```js
          function myFunction(obj) {
              console.log(obj.name);
              obj.number++;
              return obj.number;
          }
          myObj = {name:"myObj",number:34};
          myFunction(myObj);//输出myObj
          console.log(myObj.number);//输出35
          ```
    
          这种情况下开发人员不需要记住或查阅形式参数的顺序。
    
      - #### 编程要求
    
        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：
    
        - 函数`objcetFunction()`的参数是一个对象，该函数的功能是返回`属性名1+':'+属性值1+','+属性名2+':'+属性值2+','+......+属性值n+','`；
        - 测试的时候我们会往`mainJs()`传入参数`1`或`2`或`3`，分别表示调用函数`objectFunction()`并传入对象参数`park`、`computer`或者`city`；
        - 比如对于第一个对象`park`，该函数需要返回`name:Leaf Prak,location:Fifth Avenue,todayTourists:4000,`；
        - 具体请参见后续测试样例。
    
      - #### 测试说明
    
        测试过程：
    
        - 平台将读取用户补全后的`ObjectArguments.js`；
        - 调用其中的`mainJs()`方法，并输入若干组测试数据；
        - 接着根据测试结果判断程序是否正确。
    
        以下是测试样例：
    
        - 测试输入：`1`
        - 预期输出： `name:Leaf Prak,location:Fifth Avenue,todayTourists:4000,`
    
    - ### 第7关：函数对象
    
      - #### 任务描述
    
        本关任务：求数组中奇数或者偶数元素的个数。
    
      - #### 相关知识
    
        `JavaScript`中一切都是对象，这句话同样适用于函数。函数对象可以作为函数的参数。
    
        - ##### 函数对象作为另一个函数的参数
    
          一个函数（为方便行文，称为`a`函数）可以作为另外一个函数（称为`b`函数）的**参数**，`b`函数最终可以返回一个具体的值。
    
          从原理上来说，`b`函数在自己的函数体内调用了`a`函数，所以需要把`a`函数的名字作为实际参数传递给`b`函数。如下：
    
          ```js
          //求最大值
          function getMax(a,b) {
              return a>b?a:b;
          }
          //求最小值
          function getMin(a,b) {
              return a<b?a:b;
          }
          //下面这个函数以函数作为参数，并最终返回一个值
          function getM(func,num1,num2) {
              return func(num1,num2);
          }
          getM(getMax,1,2);//返回2
          getM(getMin,1,2);//返回1
          ```
    
          我们把`a`函数的名字（`getMax`或者`getMin`）传给`b`函数（`getM（）`），然后在`b`函数内部调用闯入的`a`函数，得到相关的结果。
    
      - #### 编程要求
    
        本关的编程任务是补全右侧代码片段中`Begin`至`End`中间的代码，具体要求如下：
    
        - 已知`getOddNumber(a)`求数组`a`中奇元素的个数，`getEvenNumber(a)`求数组`a`中偶元素的个数；
        - 完成函数`getNumber(func,a)`，实现：根据函数参数`func`的不同，求数组`a`中奇元素的个数或者偶元素的个数；
        - 上一条提到的`func`的值只有可能是`getOddNumber`或者`getEvenNumber`。
    
      - #### 测试说明
    
        测试过程：
    
        - 平台将读取用户补全后的`FunctionObject.js`；
        - 调用其中的`mainJs()`方法，并输入若干组测试数据；
        - 接着根据测试结果判断程序是否正确。
    
        以下是测试样例：
    
        解释一下，分号前面是参数`func`的值，后面是参数`a`的值，受限于测试程序的编程语言，我们只能用这种方式呈现。
    
        - 测试输入：`getOddNumber;1,2,3`
    
          预期输出：`2`
    
  - ## 实验7-4：JS对象

    - ### 第1关：对象的创建

      - #### 任务描述

        本关任务：创建你的第一个`JavaScript`对象。

      - #### 相关知识

        `JavaScript`是一种基于对象`（Object-based）`的语言，在`JavaScript`中，对象的创建和`Java`不同，既有`Java`使用的构造函数方式，也有其他方法。

        - ##### 对象的定义

          `JavaScript`中的一切都是对象，这是该语言的一个很大的特点。像字符串、数组等已经定义的对象叫做内置对象。用户自己也可以定义对象，叫做自定义对象。本实训讲的对象特指自定义对象，自定义对象指数据和函数（又叫方法）的集合。数据指变量名和变量的值构成的组合。如下图所示：

          ![image-20230512141228539](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/4d016a90d52dfb80bbdebe56a0dba747.png)

          下面介绍五种创建对象的方法，其中通过对象字面量和使用构造函数创建对象最常用。

        - ##### 对象字面量

          这是最常用的创建对象的方法，通过新建一个键值对的集合（对象字面量）创建对象，如下：

          ```js
          var song = {
              name:"Liekkas",
                time:180,
                "song language":English,
                singer: {
                  singerName:"Sofia Jannok",
                      singerAge:30
              }
          };
          ```

          键值对中的键指的是属性的名字，若其中含有空格，名字需要用双引号包含在内。值指的是属性的值，可以是基本类型：如字符串，数字，布尔型，也可以是一个对象。键值对之间用逗号隔开，最后一个键值对后面没有逗号，所有的键值对在一个大括号中。

        - ##### 通过关键字new创建对象

          通过`new`关键字创建对象也是一个常用的方法。如下：

          ```js
          var Store = new Object();//创建对象的一个实例
          Store.name = "lofo Market";
          Store.location = "NO.13 Five Avenue";
          Store.salesVolume = 1000000;
          ```

          通过上面的代码，我们就能创建一个名为`Store`的对象。

        - ##### 通过工厂方法创建对象

          工厂方法就是通过函数创建对象，函数封装了创建对象的过程。

          这是一种通过函数创建对象的方法，函数封装了对象的创建过程，创建新对象时只需要调用该函数即可。这种方法适合于一次创建多个对象。

          ```js
          //对象的创建函数
          function createStoreObject(name,location,salesVolume) {
              var store = new Object();
              store.name = name;
              store.locaion = location;
              store.salesVolume = salesVolume;
              store.display = function() {
                    console.log(this.name);
              };
              return store;
          }
          //利用该函数创建一个对象
          var store1 = createStoreObject("panda express","No.1,People Street",200000);
          ```

          这样就创建了一个名为`store1`的对象，注意这个对象除了属性之外还有一个方法`display`。要创建更多的类似`store1`的对象，直接调用该函数即可。

        - ##### 使用构造函数创建对象

          上面虽然也是通过函数创建对象，但不是构造函数，只是普通函数。构造函数名必须以大写字母开头，函数体内没有返回语句。

          ```js
          //构造函数
          function Store(name,location,salesVolume) {
              this.name = name;
              this.locaion = location;
              this.salesVolume = salesVolume;
          }
          //创建对象的实例
          var myStore = new Store("KeyExp","No.1,L.Street",540000);
          ```

          上面的代码首先是`Store`对象的构造函数，然后用该构造函数创建了`Store`对象的一个实例`myStore`。

        - ##### 使用原型(prototype)创建对象

          当我们创建一个函数时，函数就会自动拥有一个`prototype`属性，这个属性的值是一个对象，这个对象被称为该函数的原型对象。也可以叫做原型。

          当用`new`关键字加函数的模式创建一个对象时，这个对象就会有一个默认的不可见的属性`[[Prototype]]`，该属性的值就是上面提到的原型对象。如下图所示：

          ![image-20230512141356520](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/96f1013ba8c8d7c9c9f3ca966387d88b.png)

          `JavaScript`中每个对象都有一个属性`[[Prototype]]`，指向它的原型对象，该原型对象又具有一个自己的`[[Prototype]]`，层层向上直到一个对象的原型为`null`。根据定义，`null` 没有原型，并作为这个原型链中的最后一个环节。如下图所示：

          ![image-20230512141407659](https://cdn-static.xxcheng.cn/static/blog/images/2023/05/12/be2eaf83b562f06652f8fc3b5c7bb2a1.png)

          这种方法是对使用构造函数创建对象的改进，使用构造函数创建一个对象时，会把构造函数中的方法（上面的构造函数只有属性的键值对，没有方法）都创建一遍，浪费内存，使用原型不存在这个问题。

          ```js
          function Store() {};
          Store.prototype.name = "SF Express";
          Store.prototype.locaion = "Hong Kong";
          Store.prototype.salesVolume = 1200000000;
          //创建对象
          var myStore = new Store();
          //创建一个新的对象
          var hisStore = new Store();
          hisStore.name = "STO Express";//覆盖了原来的name属性
          ```

          这种方法的好处是，创建一个新的对象时，可以更改部分属性的值。

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`begin`至`end`中间的代码，具体要求如下：

        - 使用对象字面量方法创建名为`student`的对象，有两个属性`name`和`gender`，他们的值分别是`mainJs()`函数的参数`a`和参数`b`；
        - 使用已给的构造函数`Car(plate,owner)`创建一个对象`myCar`，它的两个属性的值分别是参数`c`和参数`d`；
        - 使用原型创建一个对象`myJob`，它的构造函数是`Job(company,salary)`，它的两个属性的值已经被设置，你需要用参数`e`覆盖属性`company`的值；
        - 具体请参见后续测试样例。

        本关涉及的代码文件`CreateObject.js`的代码框架如下：

        ```js
        function Car(plate,owner) {
            this.plate = plate;
            this.owner = owner;
        }
        function Job() {};
        Job.prototype.company = "myCompany";
        Job.prototype.salary = 12000;
        function mainJs(a,b,c,d,e) {
            //请在此处编写代码
            /********** Begin **********/
            
            
            /********** End **********/
            return student.name+student.gender+myCar.plate+myCar.owner+myJob.company;
        }
        ```

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`CreateObject.js`；
        - 调用其中的`mainJs()`方法，并输入若干组测试数据；
        - 接着根据测试的输出判断程序是否正确。

        以下是测试样例：

        - 测试输入： `Alice,girl,JA12345,Zhang,ST Technology`
        - 预期输出： `AlicegirlJA12345ZhangST Technology`

    - ### 第2关：属性的增删改查

      - #### 任务描述

        `Luma Restaurant`以前的财务人员在统计销售额的时候不小心把数据弄错了，现在的财务人员想通过一个`JavaScript`函数方便的修改数据，并署上自己的名字，请你帮助她完成这个任务吧！

        本关任务：根据本小结内容，完成`JavaScript`对象属性值的获取和修改。

      - #### 相关知识

        在`Java`中，当实体类建立以后，类的属性只能获取与修改，不能增加与删除。但是因为`JavaScript`是动态类型的语言，`JavaScript`中对象的属性具有增删改查所有的操作。

        - ##### 属性的获取

          - **方式一**

            属性的获取有两种方式，一种是使用`.`符号，符号左侧是对象的名字，符号右侧是属性的名字，如下：

            ```js
            var student = {name:"Alice",gender:"girl"};
            console.log(student.name);//输出Alice
            ```

            这种情况下属性名必须是静态的字符串，即不能是通过计算或者字符串的拼接形成的字符串。

          - **方式二**

            另外一种是使用`[""]`符号，符号的左边是对象的名字，双引号中间是属性的名字，这种情况下属性名可以是一个表达式，只要表达式的值是一个字符串即可。如下：

            ```js
            var student = {name:"Alice",gender:"girl"};
            console.log(student["name"]);//输出Alice
            ```

            有两种情况必须使用第二种方式：

            - 属性名含有空格字符，如`student["first name"]`，这时不能用`student.first name`代替，编译器无法解释后者；

            - 属性名动态生成，比如用`for`循环获取前端连续`id`的值，这种`id`名之间一般有特定关系。如下面的例子:

              ```js
              for(int i = 0;i < 5;i ++) {
                  console.log(student["id"+i]);
              }
              ```

        - ##### 属性的修改与新增

          属性的修改指修改已有属性的值，这个直接用赋值符号即可。

          属性的新增与修改在形式上完全相同，区别仅在于编译器会根据属性的名字判断是否有该属性，有则修改，没有则新增。

          ```js
          var student = {
              name:"Kim",
              age:21
          };
          student.age = 20;//修改属性，覆盖了原来的值21
          student.gender = "female";//新增属性gender
          ```

        - ##### 删除属性

          `JavaScript`中的属性还可以删除，这在其他的面向对象语言如`Java`或者`C++`中是无法想象的，删除通过`delete`运算符实现。删除成功返回布尔型`true`，删除失败也是返回`true`，所以在删除之前需要判断一个属性是否存在，这个内容将在下一关讲解。

          需要注意的是，对象只能删除自己特有的属性，而不能删除继承自原型对象的属性。同时，对象在删除属性时，要防止删除被其他对象继承的属性，因为这样会导致程序出错。

          ```js
          var Store = new Object();
          Store.name = "lofo Market";
          Store.location = "NO.13 Five Avenue";
          console.log(delete Store.name);//删除成功，输出true
          console.log(Store.name);//已删除，返回undefined
          delete Store.prototype;//删除失败，非自有属性
          ```

      - #### 编程要求

        请补全右侧`begin`和`end`之间的代码片段，使函数`reviseAttribute(reviser,date,attvalue) `具备根据传入参数修改`store`对象的属性的功能，具体要求如下：

        - 如果调用函数`reviseAttribute（reviser,date,attvalue）`并传入值 `Alice,1,1000`那么对应`store`的`day1`属性的值就修改为`1000`,`accountant`属性的值修改为`Alice`；
        - 具体请参见后续测试样例。

        本关涉及的代码文件`operateAttribute.js`的代码框架如下：

        ```js
        var store = {
            name:"Luma Restaurant",
            location:"No 22,Cot Road",
            accountant:"Vivian Xie",
            day1:3200,
            day2:3200,
            day3:3200,
            day4:3200,
            day5:3200,
            day6:3200,
            day7:3200,
            day8:3200,
            day9:3200,
            day10:3200
        }
        function reviseAttribute(date,attValue) {
              //Convert string to integer
              attValue = parseInt(attValue);
            //请在此处编写代码
            /********** Begin **********/
            
            
            /********** End **********/
              var totalSales =  store["day1"]+store["day2"]+store["day3"]+store["day4"]+store["day5"]+store["day6"]+store["day7"]+store["day8"]+store["day9"]+store["day10"];
            return toalSales+store.accountant;
        }
        ```

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`operateAttribute.js`；
        - 调用其中的`mainJs()`方法，并输入若干组测试数据；
        - 接着根据测试的输出判断程序是否正确。

        以下是测试样例：

        - 测试输入： `Alice,2,1`

          预期输出： `28801Alice`

        - 测试输入： `Kim,10,0`

          预期输出： `28800Kim`

    - ### 第3关：属性的检测和枚举

      - #### 任务描述

        本关任务：给定一个属性的名字，请先判断它属于哪一个对象，然后返回该对象的所有自有属性名连接成的字符串。

        如：`school`对象有三个自有属性`name,location,studentNum`，如果给定`name`，你需要返回字符串`namelocationstudentNum`。

      - #### 相关知识

        在`JavaScript`编程实践中，如果我们调用别人的接口，常常需要了解实体是否具有某个属性。

        - ##### 属性的检测

          属性的检测指检查对象是否有某个属性或者方法，需要使用运算符`in`，`in`的左侧是属性或者方法名，右侧是检查对象，对象有该属性或者方法则返回`true`，否则返回`false`，如下：

          ```js
          var school = {
              name:"SJTU",
              location:"ShangHai",
              studentNum:40000,
              display:function() {
                    console.log(this.name);
              }
          };
          //检测属性
          console.log("name" in school);//输出true
          console.log("sales" in school);//输出false
          //检测方法
          console.log("display" in school);//输出true
          console.log("print" in school);//输出false
          ```

          这里的属性名是字符串，必须用双引号包含在内。

          还可以用`hasOwnProperty()`检测对象是否具有某个自有属性或方法。括号内的参数是属性或者方法的名字。

          所谓自有属性或者方法，是指对象自己定义的属性或者方法，而不是从原型链上继承来的。关于原型链，请参考本实训第一关。

          ```js
          var school = {
              name:"SJTU",
              location:"ShangHai",
              studentNum:40000,
              display:function() {
                    console.log(this.name);
              }
          };
          console.log(school.hasOwnProperty("studentNum"));//true
          console.log(school.hasOwnProperty("hasOwnProperty"));//false
          ```

          因为`hasOwnProperty`方法继承自`object`对象，不是自有方法，所以返回`false`。

        - ##### 属性的枚举

          定义：属性的枚举指按顺序逐个的列出属性的名字。如下面的例子：

          ```js
          var person = {
              name:"Ye",
              gender:"Gril",
              age:23,
              salary:23000,
              height:1.78
          }
          ```

          根据前面的知识，我们知道对象`person`有五个属性，所谓枚举，就是依次列出这五个属性的名字，即：`name、gender、age、salary、height`，至于它们排列的顺序，在不同的浏览器中的结果不同，这里不讨论。

          在继续下面的知识点之前，首先要知道一个概念：可枚举性（`enumerable`），这是对象的属性的一个性质，用户自己定义的属性默认为可枚举，系统内置的对象的属性默认为不可枚举。

          枚举属性有三种方法：

          - `for...in...`循环

            可以枚举所有可枚举的属性，包括继承的属性。如下：

            ```js
            //首先定义一个school对象，它从原型链上继承的属性都是不可枚举的，而下面自定义的四个属性或者方法都是可枚举的
            var school = {
              name:"SJTU",
              location:"ShangHai",
              studentNum:40000,
              display:function() {
                    console.log(this.name);
              }
            };
            //枚举school的属性
            //下面的圆括号中的att表示对象的属性，school表示对象
            for(var att in school) {
              //依次输出name,location,studentNum,display
              console.log(att);
            }
            ```

            圆括号里面的表达式中，`att`表示对象的属性，`school`表示该对象，这个循环将依次遍历对象的所有可枚举属性，每次输出一个属性的值。

          - `Object.getOwnPropertyNames()`

            括号中有一个参数，是要枚举的对象。该表达式将返回对象的所有自有属性的名字，不区分是否可枚举，结果以字符串数组的形式呈现，如下：

            ```js
            //定义一个school对象
            var school = {
              name:"SJTU",
              location:"ShangHai",
              studentNum:40000,
              display:function() {
                    console.log(this.name);
              }
            };
            //为school对象增加一个不可枚举的属性range
            Object.defineProperty(school, "range", {
              value: 4,//设置range属性的值
              enumerable: false//设置range属性为不可枚举
            });
            //输出["name","location","studentNum","display","range"]
            console.log(Object.getOwnPropertyNames(school));
            ```

            如果用上面的`for...in...`循环，`range`属性是不能够枚举到的。

          - `Object.keys()`；

            括号中有一个参数，是要枚举的对象。该表达式返回可枚举的自有属性，以字符串数组的形式。所以这里对属性的要求更加严格，既要求是自有属性，又要求可枚举。

            ```js
            var school = {
              name:"SJTU",
              location:"ShangHai",
              studentNum:40000,
              display:function() {
                    console.log(this.name);
              }
            };
            //为school对象增加一个不可枚举的属性range
            Object.defineProperty(school, "range", {
              value: 4,//设置range属性的值
              enumerable: false//设置range属性为不可枚举
            });
            //输出["name","location","studentNum","display"]
            console.log(Object.keys(school));
            ```

          总结一下上面三个方法对属性是否自有，是否可枚举的要求：

          |            方法名            | 是否要求可枚举 | 是否要求自有 |
          | :--------------------------: | :------------: | :----------: |
          |         for...in...          |       是       |      否      |
          | Object.getOwnPropertyNames() |       否       |      是      |
          |        Object.keys()         |       是       |      是      |

      - #### 编程要求

        本关的编程任务是补全右侧代码片段中`begin`至`end`中间的代码，具体要求如下：

        - 有两个可选的对象`orange`和`car`，判断给定的属性名`a`属于哪一个对象；
        - 返回该对象的所有自有属性名组成的字符串，例如：如果判断为`car`，则返回`brandpricemodel`；
        - 给定的两个对象的自有属性都是可枚举的；
        - 具体请参见后续测试样例。

        本关涉及的代码文件`AttributeDetect.js`的代码框架如下：

        ```js
        var orange = {
            weight:"200g",
            color:"orange",
            taste:"sour"
        };
        var car = {
            brand:"Jaguar",
            price:"$80000",
            model:"XFL"
        }
        function mainJs(a) {
            //请在此处编写代码
            /********** Begin **********/
            
            /********** End **********/
        }
        ```

      - #### 测试说明

        测试过程：

        - 平台将读取用户补全后的`AttributeDetect.js`；
        - 调用其中的`mainJs()`方法，并输入若干组测试数据；
        - 接着根据测试的输出判断程序是否正确。

        以下是测试样例：

        - 测试输入： `price`

          预期输出： `brandpricemodel`

- # <a id="pass-code-title" style="color:unset;border-bottom: unset;">测评代码</a>

  - ## 实验7-1：JS条件语句

    - ### 第1关：if-else类型

      ```js
      function mainJs(a) {
          a = parseInt(a);
      	//请在此处编写代码
      	/********** Begin **********/
          return a < 60 ? 'unpass' : 'pass';
      	/********** End **********/
      }
      ```

    - ### 第2关：switch类型

      ```js
      function mainJs(a) {
          a = parseInt(a);
      	//请在此处编写代码
      	/********** Begin **********/
          switch (a) {
              case 82414:
                  return 'Superior';
              case 59600:
                  return 'Huron';
              case 58016:
                  return 'Michigan';
              case 25744:
                  return 'Erie';
              case 19554:
                  return 'Ontario';
              default:
                  return 'error';
          }
      	/********** End **********/
      }
      ```

    - ### 第3关：综合练习

      ```js
      //判断一个年份是否为闰年
      function judgeLeapYear(year) {
      	//请在此处编写代码
      	/********** Begin **********/
          return year % 100 === 0 ?
              year % 400 === 0 ?
                  year + '年是闰年' :
                  year + '年不是闰年' :
              year % 4 === 0 ?
                  year + '年是闰年' :
                  year + '年不是闰年';
      	/********** End **********/
      }
      
      //对输入进行规范化处理
      function normalizeInput(input) {
      	/********** Begin **********/
          switch (input) {
              case '中共党员':
              case '党员':
              case '共产党员':
                  return '中共党员';
              case '中共预备党员':
              case '预备党员':
                  return '中共预备党员';
              case '团员':
              case '共青团员':
                  return '共青团员';
              case '大众':
              case '群众':
              case '市民':
              case '人民':
                  return '群众';
              default:
                  return '错误数据';
          }
      	/********** End **********/
      }
      
      //判断苹果是否为优质品
      function evaluateApple(weight,water) {
      	/********** Begin **********/
          return weight >= 200 || water >= .7 ? '是优质品' : '不是优质品';
      	/********** End **********/
      }
      ```

  - ## 实验7-2：JS循环语句

    - ### 第1关：while类型

      ```js
      function mainJs(a) {
          a = parseInt(a);
      	//请在此处编写代码
          /********** Begin **********/
          if (a === 1) return 0;
          var total = 0;
          var i = 2;
          while (i <= a) {
              if (isZhi(i)) {
                  total += i;
              }
              i++;
          }
          return total;
      	/********** End **********/
      }
      
      function isZhi(n) {
          var i = 2;
          while (i <= Math.sqrt(n)) {
              if (n % i === 0) {
                  return false;
              }
              i++;
          }
          return true;
      }
      ```

    - ### 第2关：do while类型

      ```js
      function mainJs(a,b) {
          a = parseInt(a);
          b = parseInt(b);
      	//请在此处编写代码
      	/********** Begin **********/
          var total = 0;
          var max = a > b ? a : b;
          var index = (a > b ? b : a)+1;
          while (index < max) {
              total += index++;
          }
          return total;
      	/********** End **********/
      }
      ```

    - ### 第3关：for类型

      ```js
      function mainJs(a){
          a = parseInt(a);
      	//请在此处编写代码
      	/********** Begin **********/
          var b = ''+a%10;
          while (a = parseInt(a / 10)) {
              b += a % 10;
          }
          return parseInt(b);
      	/********** End **********/
      }
      ```

    - ### 第4关：for in类型

      ```js
      var apple = {
          weight:"200克",
          level:"特级",
          locationProvince:"陕西省",
          locationCity:"榆林市"
      }
      function mainJs(a,b){
          apple[a]= b;
      	//请在此处编写代码
          /********** Begin **********/
          var str = '';
          for (var name in apple) {
              if (name.indexOf('location') === 0) {
                  str += apple[name];
              }
          }
          return str;
      	/********** End **********/
      }
      ```

    - ### 第5关：break和continue的区别——break

      ```js
      function mainJs(a) {
          var arr = a.split(",");
          for(var k = 0,length = arr.length;k < length;k++) {
              arr[k] = parseInt(arr[k]);
          }
      	//请在此处编写代码
      	/********** Begin **********/
          for (var v of arr) {
              if (isZhi(v)) return v;
          }
      	/********** End **********/
      }
      
      function isZhi(n) {
          if (n <= 1) return false;
          var i = 2;
          while (i <= Math.sqrt(n)) {
              if (n % i === 0) {
                  return false;
              }
              i++;
          }
          return true;
      }
      ```

    - ### 第6关：break和continue的区别——continue

      ```js
      function mainJs(a,b) {
          a = a.split(",");
          for(var i = 0,length = a.length;i < length;i++) {
              a[i] = parseInt(a[i]);
          }
          var sum = 0;
          for(var i = 0,length = a.length;i < length;i++) {
      	//请在此处编写代码
      	/********** Begin **********/
              if ((b > 0 && a[i] < 0) || ( b<0 && a[i]>0 ) ) {
                  continue;
              }
      	/********** End **********/
              sum += a[i];
          }
          return sum;
      }
      ```

  - ## 实验7-3：JS函数

    - ### 第1关：用函数语句定义函数

      ```js
      //请在此处编写代码
      /********** Begin **********/
      function mainJs(a,b) {
          return a + b;
      }
      /********** End **********/
      ```

    - ### 第2关：用表达式定义函数

      ```js
      function mainJs(a) {
          a = parseInt(a);
      	//请在此处编写代码
      	/********** Begin **********/
          function myFunc(a) {
              var x = parseInt(a / 100);
              var y = parseInt(a / 10 %10);
              var z = parseInt(a % 10);
              return x + y + z;
          }
      	/********** End **********/
          return myFunc(a);
      }
      ```

    - ### 第3关：函数的调用

      ```js
      //求最大值的函数
      function getMax(b,c) {
          return b>c?b:c;
      }
      
      //求最小值的函数
      var getMin = function(b,c) {
          return b>c?c:b;
      }
      
      //对象中的求和函数
      var myObject = {
          id:1,
          name:"function",
          myFunc:function(b,c) {
              return b+c;
          }
      }
      
      function mainJs(a,b,c) {
          a = parseInt(a);
          b = parseInt(b);
          c = parseInt(c);
      	//请在此处编写代码
      	/********** Begin **********/
          return a === 1 ? getMax(b, c) :
              a === 2 ? getMin(b, c) :
                  myObject.myFunc(b, c);
      	/********** End **********/
      }
      ```

    - ### 第4关：未定义的实参

      ```js
      function mainJs(a,b,c,d) {
      	//请在此处编写代码
      	/********** Begin **********/
          a = a || 'green';
          b = b || 'green';
          c = c || 'red';
          d = d || 'yellow';
          return a + '-' + b + '-' + c + '-' + d;
      	/********** End **********/
      }
      ```

    - ### 第5关：实参对象

      ```js
//请在此处编写代码
      /********** Begin **********/
      function getMax() {
          var max = arguments[0]||0;
          for (let v of arguments) {
              if (v > max) {
                  max = v;
              } 
          }
          return max;
      }
      /********** End **********/
      
      function mainJs(a) {
          a = parseInt(a);
          switch(a) {
              case 1:return getMax(23,21,56,34,89,34,32,11,66,3,9,55,123);
              case 2:return getMax(23,21,56,34,89,34,32);
              case 3:return getMax(23,21,56,34);
              case 4:return getMax(23,21,56,34,89,34,32,11,66,3,9,55,123,8888);
              case 5:return getMax();
              default:break;
          }
      }
      ```
      
    - ### 第6关：对象作为参数
    
      ```js
      var park = {
          name:"Leaf Prak",
          location:"Fifth Avenue",
          todayTourists:4000
      };
      
      var computer = {
          name:"Levenon",
          price:"$800",
          memory:"8G"
      };
      
      var city = {
          name:"HangZhou",
          country:"Chine",
          population:9400000
      }
      
      function objectFunction(object) {
      //请在此处编写代码
      /********** Begin **********/
          var str = '';
          for (let key in object) {
              str += key + ':' + object[key] + ',';
          }
          return str;
      /********** End **********/
      }
      
      function mainJs(a) {
          a = parseInt(a);
          switch(a) {
              case 1:return objectFunction(park);
              case 2:return objectFunction(computer);
              case 3:return objectFunction(city);
              default:break;
          }
      }
      ```
    
    - ### 第7关：函数对象
    
      ```js
      //求数组中奇数元素的个数
      function getOddNumber(a) {
          var result = 0;
          for(var i = 0;i < a.length;i++) {
              if(a[i]%2 != 0)
                  result++;
          }
          return result;
      }
      
      //求数组中偶数元素的个数
      function getEvenNumber(a) {
          var result = 0;
          for(var i = 0;i < a.length;i++) {
              if(a[i]%2 == 0)
                  result++;
          }
          return result;
      }
      
      function getNumber(func,a) {
      	//请在此处编写代码
      	/********** Begin **********/
          return func(a);
      	/********** End **********/
      }
      
      //测试接口
      function mainJs(b,a) {
          a = a.split(",");
          var aLength = a.length;
          for(var i = 0;i < aLength;i++) {
              a[i] = parseInt(a[i]);
          }
          if(b == "getEvenNumber") {
              return getNumber(getEvenNumber,a);
          } else {
              return getNumber(getOddNumber,a);
          }
      }
      ```
    
  - ## 实验7-4：JS对象
  
    - ### 第1关：对象的创建
  
      ```js
      function Car(plate,owner) {
          this.plate = plate;
          this.owner = owner;
      }
      
      function Job() {};
      Job.prototype.company = "myCompany";
      Job.prototype.salary = 12000;
      
      function mainJs(a,b,c,d,e) {
      	//请在此处编写代码
          /*********bigin*********/
          var student = {
              name: a,
              gender: b
          };
          var myCar=new Car(c, d);
          var myJob = new Job();
          myJob.company = e;
          /*********end*********/
          return student.name+student.gender+myCar.plate+myCar.owner+myJob.company;
      }
      ```
  
    - ### 第2关：属性的增删改查
  
      ```js
      var store = {
      	name:"Luma Restaurant",
      	location:"No 22,Cot Road",
      	accountant:"Vivian Xie",
      	day1:3200,
      	day2:3200,
      	day3:3200,
      	day4:3200,
      	day5:3200,
      	day6:3200,
      	day7:3200,
      	day8:3200,
      	day9:3200,
      	day10:3200
      }
      function reviseAttribute(reviser,date,attValue) {
          //Convert string to integer
          attValue = parseInt(attValue);
          //请在此处编写代码
          /*********begin*********/
          store.accountant = reviser;
          store['day' + date] = attValue;
      	/*********end*********/
          var totalSales =  store["day1"]+store["day2"]+store["day3"]+store["day4"]+store["day5"]+store["day6"]+store["day7"]+store["day8"]+store["day9"]+store["day10"];
          return totalSales+store.accountant;
      }
      ```
  
    - ### 第3关：属性的检测和枚举
  
      ```js
      var orange = {
          weight:"200g",
          color:"orange",
          taste:"sour"
      };
      var car = {
          brand:"Jaguar",
          price:"$80000",
          model:"XFL"
      }
      function mainJs(a){
      	//请在此处编写代码
          /*********begin*********/
          let str=''
          if (orange[a]) {
              for (var key in orange) {
                  str += key;
              }
              return str;
          }
          if (car[a]) {
              for (var key in car) {
                  str += key;
              }
              return str;
          }
          /*********end*********/
      }
      ```