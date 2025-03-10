# JS

详细参考详细参考[1]<font color='blue'> 菜鸟教程</font>

# 展示思路

## 基本

# 补充知识点

- [x] 深挖document.write( )

  - 误区：认为只是插入网页书写文档的一个作用

  - 深究

    ```js
    //不会打开新文档的情况：相当于当前页面已经执行了document.open()，这样调用write之后会再自动调用close()告诉浏览器完成页面加载
    //直接书写在<script>标签的第一层，不被函数包围的时候
    
    //会出现情况页面并且被代替的情况：相当于是再当前页面中重现调用了一个document.open()打开了一个新页面
    //被包围在函数内
    ```

    详细参考[2]<font color='blue'>Document：write() 方法</font>

- [x] 关于for in和for of

  ```js
  var obj = {a:1, b:2, c:3}; 
  for (key in obj) {
    document.write(key);
  }//遍历对象属性
  ```

  ```js
  var array1 = ['a', 'b', 'c'];
  for (key of array1) {
    document.write(key);
  }//遍历数组数值
  ```

- [x] 更多正则表达式可参考[3] <font color='blue'>正则表达式</font>

- [x] 探究JS表单的时候出现点击按钮刷新页面的bug

  - 在W3C浏览器中Button的type默认是submit提交表单类型，点击之后就会刷新当前页面
  - 只要使用submit属性，提交表单的时候就会刷新页面，所以我们这里把type设置成button这种方法虽然不会刷新页面了，但是无法实现提交表单的作用
  - 想要又能提交表单又不会刷新页面就可以学习后面内容——接口请求(Ajax)
  
- [x] JSON

  ```
  使用范围
  - 用于前后端之间的数据传输/存储和交换数据
  - 获取来自服务器的数据
  - 被使用在网页上面——实现动态交互和更新页面内容
  ```

  ```
  优势：
  简洁易读:JSON 的语法比较简洁，易于阅读和编写
  解析速度快:JSON 的解析速度通常比其他数据格式更快
  兼容性好:JSON和JavaScript的兼容性非常好，因为它们共享着相同的语法和数据结构
  ```

  - parse和eval的区别

    ```
    eval兼容性更强：可以解析格式不那么规范的JSON
    parse和eval都可以使用格式'{}'
    但是eval还可以格外使用格式"{}"
    ```

    ```
    eval安全性不够强
    比如出现第三方链接的时候会跳转网页
    ```

    详细参考[4] <font color='blue'>parse和eval</font>
  
- [x] 键值对：计算机系统和应用程序中的一种数据表示形式，格式为<属性名，值>

- [x] HTTP Cookie：服务器发送到用户浏览器并保存在本地的一小块数据

  简单例子：登陆教务系统的时候会出现自动保存密码

  ```
  会话状态管理：用户登录状态/购物车/游戏分数或者其他需要记录的信息
  个性化设置：用户自定义设置/主题和其他设置
  浏览器行为跟踪：如跟踪分析用户行为等
  ```

  ```
  onload/onunload:会在用户刷新页面或者卸载页面的时候出现，但是浏览器为了避免不必要的干扰，通常不允许unload事件处理程序中会阻塞页面卸载的操作，就比如说弹窗这种就可能无法显示，会干扰用体验，所以会使用beforeunload事件弹出提示，因为它可以允许一个返回值
  ```

- [x] 外部Js代码和内部js代码以及HTML解析等的执行顺序

  - 不使用额外属性的时候
    - 浏览器开始解析HTML文件，一次构建DOM树
    - 解析到<body>里面的HTML内容，创建对应的元素并且添加到DOM树中
    - 遇到外部js代码引入的代码，暂停HTML的解析，发起对外部js文件的请求
    - 等待外部js代码下载完毕之后分别对应执行
    - 若HTML还有后续内容就继续解析剩余的HTML代码
  - 使用async属性
    - 浏览器开始解析HTML文件，构建DOM树
    - 解析到<body>里面的HTML内容持续创建相应的元素并且添加到DOM树中
    - 遇到带有async属性的外部js文件的时候，继续解析HTML代码并且同时在后台下载外部js文件
    - 若js文件下载完成立刻执行该脚本，不管HTML解析进程如何，两个是独立的任务线
  - 使用defer属性
    - 浏览器开始解析HTML文件构建DOM树
    - 解析<body>里卖弄的HTML内容并且持续创建相应的元素并且添加到DOM树中
    - 遇到带有defer属性的外部Js文件，继续解析HTML代码并且后台开始下载外部js文件
    - 完成HTML解析构建完整的DOM树
    - 执行外部js代码

  ==查看==：![image-20250309143339592](image-20250309143339592.png)

- show.html:0.6ms+3ms=3.6ms![image-20250309143728308](C:/Users/张若晨/AppData/Roaming/Typora/typora-user-images/image-20250309143728308.png)

## 接口请求

- [ ] Ajax（异步JavaScript和XML）

  - 作用：允许浏览器与服务器之间进行==异步数据交互（等待服务器响应的同时继续与页面进行其他交互）==，可以在不重新加载整个页面的情况下更新页面的部分内容

  - 关键技术:XHR(浏览器内置对象，允许JS在后台异步向服务器发送请求并且结束服务器返回的数据)+JS(编写客户端脚本，用于调用XHR对象、处理服务器返回的数据以及更新DOM树中的部分内容)

  - 数据格式：XHR/JSON等；

  - 好处：实现动态加载数据、实时验证、提高用户体验

  - 两种请求

    ```
    GET请求：将序列化后的表单数据附加到URL后面,但是这样就会暴露数据，不安全
    
    POST请求：为请求头（比如Content-Type）赋值（比如：application/x-www-form-urlencoded）之后，将序列化之后的表单数据作为请求体发送
    Content-Type请求头：用于指定请求体中的数据格式，服务器会根据这个信息来解析请求体中的数据（也就是服务器在面对不同请求头的时候会有不同的解析方式）
    application/x-www-form-urlencoded
    用途：这是表单数据默认的编码方式，当使用POST方式提交数据时，浏览器会自动将数据编码为这种格式。
    格式：数据会被编码为键值对的形式，键和值之间用等号（=）连接，不同的键值对之间用和号（&）分隔，同时会对特殊字符进行URL编码。
    例如：username=john&password=123456。
    application/json
    用途：在前后端分离的开发中，常用于传递 JSON 格式的数据。JSON 是一种轻量级的数据交换格式，易于阅读和编写，也便于机器解析和生成。
    格式：请求体中的数据是一个合法的 JSON 对象或数组。例如：{"username": "john", "password": "123456"}。
    ```
    

  详细参考[5] <font color='blue'>html如何发送ajax请求</font>

  以及[6] <font color='blue'>ajax基础知识总结</font>

- [ ] Fetch：一种现代化网络请求方法，通过使用Promise处理异步操作

  详细参考[7]<font color='blue'>fetch使用方法</font>

- [ ] 区别

  ```
  Fetch简洁而直观，支持各种功能和API，如设置请求头、传递参数、处理流数据、上传下载文件等。Fetch的优势在于其基于Promise的设计，使得异步操作更加简洁易懂。
  Ajax可以实现页面的无刷新更新和动态交互。
  
  Ajax逐渐被Fetch和Axios等更现代的工具所替代。
  ```

  详细参考[8]<font color='blue'>从ajax到fetch的理解</font>

- [ ] 补充

  - XMLHttpRequest(XHR)对象：用于与服务器进行交互，通过XML就可以在不刷新的情况下请求特定URL获取数据，这就允许了网页在不影响用户操作的情况下更新页面的局部内容

  - HTTP状态码

    ```
    304:未修改，这时候客户端会使用本地缓存的资源，因此即使状态码为304仍然能够正常显示响应内容。
    200：请求成功，这时服务器成功处理了客户端的请求，并且可以正常返回客户端所请求的资源。
    ```

    

# 参考资料

1. [JS教程](https://www.runoob.com/js)——菜鸟教程

2. [Document：write() 方法](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/write)——MDN

3. [正则表达式](https://www.runoob.com/regexp/regexp-syntax.html)——菜鸟教程

4. [parse和eval](https://www.cnblogs.com/lovesong/p/6036650.html)

5. [html如何发送ajax接口请求](/www.cnblogs.com/yilangcode/p/18085967)——博客园

6. [ajax基础知识总结](https://juejin.cn/post/7243240589293617213?searchId=20250302150107FC85D05D9114803611C4)——稀土掘金

7. [fetch使用方法](https://juejin.cn/post/7125361809669750791)——稀土掘金

8. [从ajax到fetch的理解](https://juejin.cn/post/6844903762008473607?searchId=2025030215051979AE477372BA93351D0E)
