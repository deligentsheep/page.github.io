# HTML+CSS混合

详细参考[1]<font color='blue'>HTML教程</font>

详细参考[2]<font color='blue'>CSS教程</font>

# 展示思路

## HTML

按照网页顺序讲

## CSS

- [x] 添加样式的方式
  - [x] 外部样式表
  - [x] 内部样式表
  - [x] 内联样式
  - [x] 多重样式看谁详细
  - [x] 多重样式优先级：内联>内部>外部>浏览器默认
- [x] 以内部样式表为例按顺序展示功能
  - [x] 按照顺序以及各个小网页功能展示来讲

# 查漏补缺问题

## html部分

- [x] 跳转新网页时候会出现的安全漏洞

  - 跳转网页涉及理论：

    场景：B由A跳转打开，无干扰情况下A网站的所有window变量会跟着迁移到B网站的window.opener变量中，相当于只通过查看B就可以访问A网站的window变量，坏人就可以从B钻空子，比如就可以把本应该打开的网站A更换成一个钓鱼网站。

  - 两种漏洞情况以及解决方式

    ```html
    <!--通过html超链接方式打开新网站时候-->
    <a href="http://127.0.0.1:5500/show.html" target="_blank" rel="noopener noreferrer">点击打开新网站</a>
    <!--加上rel后面的noopener属性就可以解决这个漏洞，noreferrer是浏览器兼容问题-->
    ```

    ```js
    //通过js函数打开新网站时候
    function openinnewtap(){
        var newTab = window.open();
        newTab.opener = null;//加上这句使传到B的opener值为null
        newTab.location = "show.html";
    }
    ```

    了解window.opener参考[3]<font color='blue'>opener</font>

    了解此方面安全漏洞参考[4]<font color="blue">安全漏洞</font>

- [x] 各个列表的区别

  - 无序列表：适用于并列关系的项目，比如菜单选项、特点列举等
    - ==无需列表的缩进为什么一定要加上ul，不能直接使用li==
  - 有序列表：适用于强调顺序的项目，比如步骤说明、排名列表等
  - 自定义列表：与另外两个相比存在缩进，适用于包含或者描述关系，比如词汇表、术语解释等。

- [x] 块级和行级元素的==嵌套==:可以通过浏览器进行代码检查

  - 特殊的块级元素p/h1~h6/dt:只能嵌套行内元素，不可以嵌套块级元素，就算块级元素被display设置成inline也是不行的；
  - 其余块级元素：可以嵌套任何元素;
  - 行级元素的嵌套：按规则看行内元素只能包含数据和其他行内元素，但是经过实践发现行内元素也可以嵌套块级元素。

- [x] id、class和name的区别

  - id

    ```
    作用：需要精确标识并且定位单个特定元素
    js操作：document.getElementById()或document.querySelector('#id')
    ```

  - class

    ```
    适用于：需要对元素进行样式化和归类
    js操作：document.getElementsByClassName()
    ```

    class的优先级比id低，所以一般是先对外框设置class属性,之后再对特定的元素进行id属性进渲染。

  - name

    ```
    适用于：表单提交
    作用：name属性指定的字段名称将与该字段的值一起传到服务器端，服务器端就可以通过这个名称来获取相应字段的值以方便处理用户提交的数据
    js操作：通过document.getElementsByName()方法获取具有特定name属性的所有元素
    ```
    
    DOM树：浏览器渲染基本原理——可以学习到如何通过console获取name具体属性

- [x] lable标签中的for属性和输入框的绑定关系：常用于表单

  - 误区：认为这种绑定关系是位置相邻的捆绑
  - 事实：for标签通过关联id属性来进行触发绑定，此时点击lable标签就可以直接跳转到输入框中。

- [x] value属性

  - 对于不同的输入类型，value的使用方法和含义也不同

    ```
    button/submit:按钮显示文字
    text/password:输入字段的默认值
    checkbox/radio:与输入相关联的值(提交发送的值)
    ```

  - 特殊

    ```html
    <select>
        <option value="cn">CN</option>
    </select>
    <!--value里面是传给后台表单的值，而option中间的内容则是网页显示的值
    	好处：这样更改网页显示的值的时候就不用更改后台程序啦-->
    ```
    

## css部分

- [x] 区分文本样式和字体样式

  - ```html
    <!--文本（一组字或者字符）：描述对文本的处理方式-->
    text-indent:文本缩进
    text-decoration:文本装饰(划线等)
    
    text-align:文本对齐(left/right/center/justify：通常联合flex出现)
    
    text-transform:文本转换(默认none/文本单词全大写uppercase/文本英文单词全小写lowercase/文本英语单词首字母都大写capitalize)
    line-height:行高
    letter/word-spacing:字符/单词间距
    字符间距和单词间距的区别：字符间距是单个字符之间的距离(空格不算)，单词间距是每个字符串之间的距离(空格的长度)
    ```
    
    ==发现==：直接使用justify不生效——文本只有一行的时候，justify对最后一行文本不生效
    
    ==解决==：
    
    ```
    方法一
    直接在后面添加或者换成text-align-last:justify;
    
    方法二
    使用伪元素增加一行空内容，这样就不是最后一行了
    .test::after{
        display:inline-block;
        width:100%;
        content:"";
    } 
    ```
    
    ==思考==：有没有直接文本样式可以使得垂直对齐呢
    
    ==答==：最新改进，align-content:center已经可以适用于普通的块级元素了
    
    ​      详细参考[5]<font color='blue'>好消息，align-content垂直居中也适用普通元素啦</font>
    
  - ```html
    <!--字体：描述对一类字体的大小和外观-->
    font-family:字体族
    font-size/weight:字体大小/粗细
    font-style:字体样式(默认正常，有斜体等)
    简写按顺序(否则浏览器无法正常解释声明值):
    必须标注size/family
    必须按照顺序weight/style+size+family
    ```
  
  - 优先级：就近原则，行内样式大于内部和外部
  
- [x] CSS常用单位

  - 绝对长度单位：px，像素

  - 相对长度单位

  - ```
    em:相对于本元素所用的字体的大小
    
    rem：相对于根元素(浏览器默认字体)的字体大小
    比如：设置html根元素font-size为10px的时候1rem=10px
    
    vw/vh:相对于视图窗口的宽度和高度
    比如：当前视图窗口高度为850px，则对应的100vw就是850px
    ```

- [x] 伪类和伪元素

  - 伪类：单冒号开头，用于选择处于特定状态下的元素
  - 伪元素：双冒号开头，用于在文档中插入虚构元素
  
  详细参考[6] <font color='blue'>伪类和伪元素</font>
  
- [x] 三大布局使用习惯——float/flex/grid

  - grid一般用作整体页面的布局：二维
  - flex一般用于组件布局：一维
  - float一般用于补充说明：一维

- [x] 响应式布局

  - 百分比布局:可以设置宽度和高度百分百，这样会自适应不同浏览器(定义一个元素的宽度和高度必须按照设计稿换算成百分比单位)

  - 媒体查询布局

    ```css
    @media screen and (max-width: 1280px) {
       body{
    	background-color: aqua;
      }
    /* 用两种适配方案
    1.移动端到PC端：min-width，从小到大
    2.PC端到移动端：max-width，从大到小 */
    ```

  - rem布局：只要知道对应根元素字体大小，之后把所有的px改成rem单位就可

  - vw/vh布局：只要知道视图窗口的尺寸，把对应盒子px改成vw/vh即可

  - flex实现布局

# 参考资料

[1] [HTML教程](https://www.runoob.com/html/html-tutorial.html)——菜鸟教程

[2] [CSS教程](https://www.runoob.com/css/css-tutorial.html)——菜鸟教程

[3] [opener](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/opener)——MDN

[4] [安全漏洞](https://cloud.tencent.com/developer/article/2239052)——腾讯云

[5] [好消息，align-content垂直居中也适用普通元素啦](https://www.zhangxinxu.com/wordpress/2024/09/css-align-content/)——zhangxinxu的blog

[6] [伪类和伪元素](https://juejin.cn/post/7136087057542086693)——稀土掘金
