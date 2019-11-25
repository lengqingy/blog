# 命名

### 驼峰式命名法介绍

- Pascal Case 大驼峰式命名法：首字母大写。eg：StudentInfo、UserInfo、ProductInfo
- Camel Case 小驼峰式命名法：首字母小写。eg：studentInfo、userInfo、productInfo

### 文件资源命名
- 文件名不得含有空格
- 文件名建议只使用小写字母，不使用大写字母。( 为了醒目，某些说明文件的文件名，可以使用大写字母，比如README、LICENSE。 )
- 文件名包含多个单词时，单词之间建议使用半角的连词线 ( - ) 分隔。
- 引入资源使用相对路径，不要指定资源所带的具体协议 ( http:,https: ) ，除非这两者协议都不可用。

不推荐：

    <script src="http://cdn.com/foundation.min.js"></script>

推荐：

    <script src="//cdn.com/foundation.min.js"></script>
    
### 变量命名

命名方式 : 小驼峰式命名方法

命名规范 : 类型+对象描述的方式，如果没有明确的类型，就可以使前缀为名词

类型 | 小写字母
--| --|
array | arr|
boolean | bool|
function | fn |
int | int|
object | obj |
regular | reg |
string | str |


### 函数

命名方式 : 小驼峰方式 ( 构造函数使用大驼峰命名法 )

命名规则 : 前缀为动词

动词 | 含义 | 返回值
--| --| -- |
can | 判断是否可执行某个动作 ( 权限 ) | 函数返回一个布尔值。true：可执行；false：不可执行
has | 判断是否含有某个值| 函数返回一个布尔值。true：含有此值；false：不含有此值
is | 判断是否为某个值 | 函数返回一个布尔值。true：为某个值；false：不为某个值
get | 获取某个值| 函数返回一个非布尔值
set | 设置某个值 | 无返回值、返回是否设置成功或者返回链式对象

推荐：

    //是否可阅读
    function canRead(){
        return true;
    }
    //获取姓名
    function getName{
        return this.name
    }
    
### 常量
命名方法 : 全部大写

命名规范 : 使用大写字母和下划线来组合命名，下划线用以分割单词。

推荐：

     var MAX_COUNT = 10;
     var URL = 'http://www.baidu.com';
     
类的成员

- 公共属性和方法 : 同变量命名方式
- 私有属性和方法 : 前缀为下划线(_)后面跟公共属性和方法一样的命名方式


    function Student(name) {
        var _name = name; // 私有成员
        
        // 公共方法
        this.getName = function () {
            return _name;
        }
        
        // 公共方式
        this.setName = function (value) {
            _name = value;
        }
    }
    var st = new Student('tom');
    st.setName('jerry');
    console.log(st.getName()); // => jerry：输出_name私有变量的值
    

### 注释规范

单行注释 ( // )

- 单独一行：//(双斜线)与注释文字之间保留一个空格。
- 在代码后面添加注释：//(双斜线)与代码之间保留一个空格，并且//(双斜线)与注释文字之间保留一个空格。
- 注释代码：//(双斜线)与代码之间保留一个空格


    // 调用了一个函数；1)单独在一行
    setTitle();
    
    var maxCount = 10; // 设置最大量；2)在代码后面注释
    // setName(); // 3)注释代码

### 函数 ( 方法 ) 注释
函数(方法)注释也是多行注释的一种，但是包含了特殊的注释要求，参照 javadoc(百度百科)
语法

    /** 
    * 函数说明 
    * @关键字 
    */
    
常用注释关键字

注释名 | 语法 | 含义 | 示例 |
--| --| --| --|
@param | @param 参数名 {参数类型} 描述信息 | 描述参数的信息 | @param name {String} 传入名称 |
@return	| @return {返回类型} 描述信息| 描述返回值的信息 | @return | {Boolean} true:可执行;false:不可执行 |
@author | @author 作者信息 [附属信息：如邮箱、日期] | 描述此函数作者的信息 | @author 张三 2015/07/21 |
@version | @version XX.XX.XX| 描述此函数的版本号 | @version 1.0.3 |
@example | @example 示例代码 | @example setTitle('测试') | 如下 |

    /**
     - 合并Grid的行
     - @param grid {Ext.Grid.Panel} 需要合并的Grid
     - @param cols {Array} 需要合并列的Index(序号)数组；从0开始计数，序号也包含。
     - @param isAllSome {Boolean} ：是否2个tr的cols必须完成一样才能进行合并。true：完成一样；false(默认)：不完全一样
     - @return void
     - @author polk6 2015/07/21 
     - @example
     - _________________                             _________________
     - |  年龄 |  姓名 |                             |  年龄 |  姓名 |
     - -----------------      mergeCells(grid,[0])   -----------------
     - |  18   |  张三 |              =>             |       |  张三 |
     - -----------------                             -  18   ---------
     - |  18   |  王五 |                             |       |  王五 |
     - -----------------                             -----------------
    */
    function mergeCells(grid, cols, isAllSome) {
        // Do Something
    }


# HTML规范

### 文档规范
用 HTML5 的文档声明类型 : `<!DOCTYPE html>`

- DOCTYPE标签是一种标准通用标记语言的文档类型声明，它的目的是要告诉标准通用标记语言解析器，它应该使用什么样的文档类型定义（DTD）来解析文档。
- 使用文档声明类型的作用是为了防止开启浏览器的怪异模式。
- 没有DOCTYPE文档类型声明会开启浏览器的怪异模式，浏览器会按照自己的解析方式渲染页面，在不同的浏览器下面会有不同的样式。
- 如果你的页面添加了<!DOCTYP>那么，那么就等同于开启了标准模式。浏览器会按照W3C标准解析渲染页面。


### 脚本加载

说到js和css的位置，大家应该都知道js放在下面，css放在上面。
但是，如果你的项目只需要兼容ie10+或者只是在移动端访问，那么可以使用HTML5的新属性`async`，将脚本文件放在`<head>`内

**兼容老旧浏览器(IE9-)时：**

脚本引用写在 body 结束标签之前，并带上 async 属性。这虽然在老旧浏览器中不会异步加载脚本，但它只阻塞了 body 结束标签之前的 DOM 解析，这就大大降低了其阻塞影响。

**在现代浏览器中：**

本将在 DOM 解析器发现 body 尾部的 script 标签才进行加载，此时加载属于异步加载，不会阻塞 CSSOM（但其执行仍发生在 CSSOM 之后）。

综上所述，
所有浏览器中推荐:

    <html>
      <head>
        <link rel="stylesheet" href="main.css">
      </head>
      <body>
        <!-- body goes here -->
    
        <script src="main.js" async></script>
      </body>
    </html>
    
只兼容现代浏览器推荐:

    <html>
      <head>
        <link rel="stylesheet" href="main.css">
        <script src="main.js" async></script>
      </head>
      <body>
        <!-- body goes here -->
      </body>
    </html>
    
### 语义化

我们一直都在说语义化编程，语义化编程，但是在代码中很少有人完全使用正确的元素。使用语义化标签也是有理由SEO的。

    语义化是指：根据元素其被创造出来时的初始意义来使用它。
    意思就是用正确的标签干正确的事，而不是只有div和span。
    
### alt标签不为空

`<img>`标签的 alt 属性指定了替代文本，用于在图像无法显示或者用户禁用图像显示时，代替图像显示在浏览器中的内容。

假设由于下列原因用户无法查看图像，alt 属性可以为图像提供替代的信息：

- 网速太慢
- src 属性中的错误
- 浏览器禁用图像
- 用户使用的是屏幕阅读器


从SEO角度考虑，浏览器的爬虫爬不到图片的内容，所以我们要有文字告诉爬虫图片的内容

### 结构、表现、行为三者分离

尽量在文档和模板中只包含结构性的 HTML；而将所有表现代码，移入样式表中；将所有动作行为，移入脚本之中。

在此之外，为使得它们之间的联系尽可能的小，在文档和模板中也尽量少地引入样式和脚本文件。
建议：

- 不使用超过一到两张样式表
- 不使用超过一到两个脚本（学会用合并脚本）
- 不使用行内样式（`<style>.no-good {}</style>`）
- 不在元素上使用 style 属性（`<hr style="border-top: 5px solid black">`）
- 不使用行内脚本（`<script>alert('no good')</script>`）
- 不使用表象元素（`i.e. <b>, <u>, <center>, <font>, <b>`）
- 不使用表象 class 名（`i.e. red, left, center`）


### HTML只关注内容

- HTML只显示展示内容信息
- 不要引入一些特定的 HTML 结构来解决一些视觉设计问题
- 不要将`img`元素当做专门用来做视觉设计的元素
- 样式上的问题应该使用css解决


# js规范

### 变量声明

##### let
let声明变量，用法类似var。 
但是所有声明的变量只在let命令所在的代码块内有效

##### const
const用来声明常量，一旦声明，敞亮的值就不能改变。

### 使用严格等

总是使用 `===` 精确的比较操作符，避免在判断的过程中，由 JavaScript 的强制类型转换所造成的困扰。例如

    (function(log){
      'use strict';
    
      log('0' == 0); // true
      log('' == false); // true
      log('1' == true); // true
      log(null == undefined); // true
    
      var x = {
        valueOf: function() {
          return 'X';
        }
      };
    
      log(x == 'X');
    
    }(window.console.log));
    

**等同== 和严格等===的区别**

- ==， 两边值类型不同的时候，要先进行类型转换，再比较。
- ===，不做类型转换，类型不同的一定不等。

==等同操作符

- 如果两个值具有相同类型，会进行`===`比较，返回`===`的比较值
- 如果两个值不具有相同类型，也有可能返回true
- 如果一个值是null另一个值是undefined，返回true
- 如果一个值是string另个是number，会把string转换成number再进行比较
- 如果一个值是true，会把它转成1再比较，false会转成0


    console.log( false == null )      // false
    console.log( false == undefined ) // false
    console.log( false == 0 )         // true
    console.log( false == '' )        // true
    console.log( false == NaN )       // false
    
    console.log( null == undefined ) // true
    console.log( null == 0 )         // false
    console.log( null == '' )        // false
    console.log( null == NaN )       // false
    
    console.log( undefined == 0)   // false
    console.log( undefined == '')  // false
    console.log( undefined == NaN) // false
    
    console.log( 0 == '' )  // true
    console.log( 0 == NaN ) // false

总结一下==

- false 除了和自身比较为 true 外，和 0，"" 比较也为 true
- null 只和 undefined 比较时为 true， 反过来 undefined 也仅和 null 比较为 true，没有第二个
- 0 除了和 false 比较为 true，还有空字符串 ''" 和空数组 []
- 空字符串 '' 除了和 false 比较为 true，还有一个数字 0


===操作符：
- 要是两个值类型不同，返回false
- 要是两个值都是number类型，并且数值相同，返回true
- 要是两个值都是stirng，并且两个值的String内容相同，返回true
- 要是两个值都是true或者都是false，返回true
- 要是两个值都是指向相同的Object，Arraya或者function，返回true
- 要是两个值都是null或者都是undefined，返回true


### 真假判断
js中以下内容为假：
- false
- null
- undefined
- 0
- '' (空字符串)
- NaN


### 不使用eval()函数

就如eval的字面意思来说，恶魔，使用eval()函数会带来安全隐患。

eval()函数的作用是返回任意字符串，当作js代码来处理。

### this关键字
只在对象构造器、方法和在设定的闭包中使用 this 关键字。this 的语义在此有些误导。它时而指向全局对象（大多数时），时而指向调用者的定义域（在 eval 中），时而指向 DOM 树中的某一节点（当用事件处理绑定到 HTML 属性上时），时而指向一个新创建的对象（在构造器中），还时而指向其它的一些对象（如果函数被 call() 和 apply() 执行和调用时）。

正因为它是如此容易地被搞错，请限制它的使用场景：
- 在构造函数中
- 在对象的方法中（包括由此创建出的闭包内）

### 三元条件判断（if 的快捷方法）

用三元操作符分配或返回语句。在比较简单的情况下使用，避免在复杂的情况下使用。没人愿意用 10 行三元操作符把自己的脑子绕晕。

不推荐：

    if(x === 10) {
      return 'valid';
    } else {
      return 'invalid';
    }

推荐：

    return x === 10 ? 'valid' : 'invalid'
    

# css规范

ID和class的名称总是使用可以反应元素目的和用途的名称，或其他通用的名称，代替表象和晦涩难懂的名称

不推荐：

    .fw-800 {
      font-weight: 800;
    }
    
    .red {
      color: red;
    }
    
推荐：

    .heavy {
      font-weight: 800;
    }
    
    .important {
      color: red;
    }
    

### 合理的使用ID

一般情况下ID不应该被用于样式，并且ID的权重很高，所以不使用ID解决样式的问题，而是使用class

不推荐：

    #content .title {
      font-size: 2em;
    }

推荐：

    .content .title {
      font-size: 2em;
    }

### css选择器中避免使用标签名

从结构、表现、行为分离的原则来看，应该尽量避免css中出现HTML标签，并且在css选择器中出现标签名会存在潜在的问题。

### 使用子选择器

很多前端开发人员写选择器链的时候不使用 直接子选择器（注：直接子选择器和后代选择器的区别）。

有时，这可能会导致疼痛的设计问题并且有时候可能会很耗性能。

然而，在任何情况下，这是一个非常不好的做法。

如果你不写很通用的，需要匹配到DOM末端的选择器， 你应该总是考虑直接子选择器。

不推荐:

    .content .title {
      font-size: 2rem;
    }

推荐：

    .content > .title {
      font-size: 2rem;
    }

### 尽量使用缩写属性

尽量使用缩写属性对于代码效率和可读性是很有用的，比如font属性。

不推荐：

    border-top-style: none;
    font-family: palatino, georgia, serif;
    font-size: 100%;
    line-height: 1.6;
    padding-bottom: 2em;
    padding-left: 1em;
    padding-right: 1em;
    padding-top: 0;
    
推荐：

    border-top: 0;
    font: 100%/1.6 palatino, georgia, serif;
    padding: 0 1em 2em;
    
### 0后面不带单位

省略0后面的单位

不推荐：

    padding-bottom: 0px;
    margin: 0em;

推荐：

    padding-bottom: 0;
    margin: 0;
    
### 属性格式

- 为了保证一致性和可扩展性，每个声明应该用分号结束，每个声明换行。
- 属性名的冒号后使用一个空格。出于一致性的原因，
属性和值（但属性和冒号之间没有空格）的之间始终使用一个空格。
- 每个选择器和属性声明总是使用新的一行。
- 属性选择器或属性值用双引号（””），而不是单引号（”）括起来。
- URI值（url()）不要使用引号。


作为最佳实践，我们应该遵循以下顺序（应该按照下表的顺序）：

结构性属性：

1、display

2、position, left, top, right

3、overflow, float, clear

4、margin, padding

表现性属性：

1、background, border

2、font, text

不推荐：

     .box {
      font-family: 'Arial', sans-serif;
      border: 3px solid #ddd;
      left: 30%;
      position: absolute;
      text-transform: uppercase;
      background-color: #eee;
      right: 30%;
      isplay: block;
      font-size: 1.5rem;
      overflow: hidden;
      padding: 1em;
      margin: 1em;
    }


推荐：

    .box {
      display: block;
      position: absolute;
      left: 30%;
      right: 30%;
      overflow: hidden;
      margin: 1em;
      padding: 1em;
      background-color: #eee;
      border: 3px solid #ddd;
      font-family: 'Arial', sans-serif;
      font-size: 1.5rem;
      text-transform: uppercase;
    }