### 实验4-1：HTML表格：日常消费账单表格展示网页

- #### 第1关：HTML表格：日常消费账单表格展示网页

  ```html
  <!DOCTYPE html>
  <html>
    <!--------- Begin-------->
  
  <head>
      <meta charset="utf-8">
      <title>HTML表格</title>
      <meta name="description" content="HTML表格知识讲解">
      <meta name="keywords" content="HTML,表格, Table">
      <style type="text/css">
      table {
          border-collapse: collapse;
         
      }
  
      caption {
          font-weight: bold;
          margin-bottom: .5em;
      }
  
      th,
      td {
          padding: .5em .75em;
          border: 1px solid #000;
      }
  
      tfoot {
          font-weight: bold;
      }
      </style>
  </head>
  
  <body>
      <table border="1" style="margin:auto" width="400">
          <caption>日常消费账单</caption>
          <thead>
              <!-- 表格头部 -->
              <tr>
                  <th align="left" scope="col">消费项目</th>
                  <th align="right" scope="col">一月</th>
                  <th align="right" scope="col">二月</th>
              </tr>
          </thead>
          <tbody>
              <!-- 表格主体 -->
              <tr>
                  <th align="left" scope="row">食品烟酒</th>
                  <td align="right">￥1241.00</td>
                  <td align="right">￥1250.00</td>
              </tr>
              <tr>
                  <th align="left" scope="row">衣物</th>
                  <td align="right">￥330.00</td>
                  <td align="right">￥594.00</td>
              </tr>
              <tr>
                  <th align="left" scope="row">居住</th>
                  <td align="right">￥2100</td>
                  <td align="right">￥2100</td>
              </tr>
              <tr>
                  <th align="left" scope="row">生活用品及服务</th>
                  <td align="right">￥700.00</td>
                  <td align="right">￥650.00</td>
              </tr>
              <tr>
                  <th align="left" scope="row">医疗保健</th>
                  <td align="right">￥150.00</td>
                  <td align="right">￥50.00</td>
              </tr>
              <tr>
                  <th align="left" scope="row">教育、文化和娱乐</th>
                  <td align="right">￥1030.00</td>
                  <td align="right">￥1250.00</td>
              </tr>
              <tr>
                  <th align="left" scope="row">交通和通信</th>
                  <td align="right">￥230.00</td>
                  <td align="right">￥650.00</td>
              </tr>
              <tr>
                  <th align="left" scope="row">其他用品和服务</th>
                  <td align="right">￥130.40</td>
                  <td align="right">￥150.00</td>
              </tr>
          </tbody>
          <tfoot>
              <!-- 表格尾部 -->
              <tr>
                  <th align="left" scope="row">总计</th>
                  <th align="right">￥5911</th>
                  <th align="right">￥6694</th>
              </tr>
          </tfoot>
      </table>
  </body>
    <!--------- End-------->
  
  </html>
  ```

### 实验4-2：HTML表格的综合案例

- #### 第1关：表格的综合案例

  ```html
  <!-- ********* Begin ********* -->
  <!DOCTYPE html>
  <html>
  <head>
      <meta charset="UTF-8">
  </head>
  <body>
      <style>
      body{
         margin:30px;
      }
      table{
          text-align:center;
      }
      </style>
      <table border="2px" width="100%" cellspacing="0" cellpadding="6">
          <caption>本周财政计划</caption>
          <tr>
              <td colspan="2" rowspan="2">项目</td>
              <td colspan="2">本周发生</td>
              <td rowspan="2">备注</td>
          </tr>
          <tr>
              <td>收入</td>
              <td>支出</td>
          </tr>
          <tr>
              <td rowspan="3">收入</td>
              <td>贷款收回</td>
              <td>8700</td>
              <td>0</td>
              <td></td>
          </tr>
          <tr>
              <td>内部转款</td>
              <td>6000</td>
              <td>0</td>
              <td></td>
          </tr>
          <tr>
              <td>收入合计</td>
              <td>14700</td>
              <td>0</td>
              <td></td>
          </tr>
          <tr>
              <td rowspan="3">支出</td>
              <td>采购支出</td>
              <td>0</td>
              <td>5000</td>
              <td></td>
          </tr>
          <tr>
              <td>工资支出</td>
              <td>0</td>
              <td>7000</td>
              <td></td>
          </tr>
          <tr>
              <td>支出合计</td>
              <td>0</td>
              <td>12000</td>
              <td></td>
          </tr>
      </table>
  </body>
  </html>
  <!-- ********* End ********* -->
  ```

### 实验4-3：HTML表单元素的综合案例

- #### 第1关：表单元素的综合案例

  ```html
  <!DOCTYPE html>
  <html>
  <head>
      <meta charset="UTF-8">
      <style>
  
      .container{
          width: 40%;
          margin: 20px auto;
      }
      .common{
          width:230px;
          height: 30px;
      }
      span{
          display:inline-block;
          width: 150px;
  
          text-align: right;
      }
      div{
          margin-bottom: 10px;
      }
      </style>
  </head>
  <body>
  
      <form class="container">
      <!-- ********* Begin ********* -->
  <div>
      <span>用户名：</span>
      <input type="text" class="common"/>
  </div>
  <div>
      <span>昵称：</span>
      <input type="text" class="common"/>
  </div>
  
  <div>
      <span>性别：</span>
      <input type="radio" id="male" name="sex">
      <label for="male">男</label>    
      <input type="radio" id="female" name="sex">
      <label for="female">女</label> 
      <input type="radio" id="other" name="sex" disabled="disabled">
      <label for="other">保密</label> 
  </div>
  
  <div>
      <span>教育程度：</span>
  <select class="common">
      <option >高中</option>
      <option>中专</option>
      <option>大专</option>
      <option selected="selected">本科</option>
      <option>硕士</option>
      <option>博士</option>
      <option>其他</option>
  </select>
  </div>
  
  <div>
      <span>婚姻状况：</span>
      <input type="radio" id="single" name="state" checked="checked">
      <label for="single">未婚</label>    
      <input type="radio" id="married" name="state">
      <label for="married">已婚</label> 
      <input type="radio" id="secret" name="state">
      <label for="secret">保密</label> 
  </div>
  
  <div>
      <span>兴趣爱好：</span>
      <input type="checkbox" id="football" name="hobby" />
      <label for="football">踢足球</label>  
      <input type="checkbox" id="basketball" name="hobby" />
      <label for="basketball">打篮球</label> 
      <input type="checkbox" id="film" name="hobby" checked="checked"/>
      <label for="film">看电影</label> 
  </div>
  
  <div>
      <span>描述自己的特点：</span>
      <textarea class="common"  maxlength="20"></textarea>
  </div>
  <div>
      <span></span>    
      <input type="submit" class="common" value="提交"/>
  </div>
  <!-- ********* End ********* -->
  </form>
  </body>
  </html>
  ```

#### 参考链接

- [CSDN-HTML表格：日常消费账单表格展示网页](https://blog.csdn.net/qq_45823118/article/details/114853141)
- [CSDN-HTML——表单元素的综合案例](https://blog.csdn.net/weixin_46946002/article/details/115209407)