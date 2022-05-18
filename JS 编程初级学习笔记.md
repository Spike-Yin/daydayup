# JS 编程初级学习笔记

## JS概念

* JS是一个网络脚本语言, 主要用途是来实现网页各种各样的功能
* JS有三种使用方式       内部使用(在body标签里写script)    外部调用(script标签里写src=...)   
* 还有一种, 很常见很重要的   **元素事件JS **   就是类似于那种dom啊,  document之类的

**元素属性的JS**

* 指的就是元素的"事件属性"直接编写JS或调用函数

```javascript
<div style="color:green;" onclick="alert(输出)">
    
    
    
 onclick 就是JS的一个单机监听事件    鼠标点击这个div里的字, 就会alert窗口,  单机的含义不用多说了吧
```





## JS的输出方式

* documen.write("在网页中输出一些文字")        ---就是那种在div或在p里写的那种, 用js也能写
* console.log("在控制台输出文字")           --这个就更不用说了, 在控制台, 懂得都懂
* alert("提示文字信息, 用弹窗的信息")                --懂得都懂
* confirm("弹窗提示信息, 会有确定取消两个按钮,  确定返回true, 取消返回false")   
* prompt("弹窗,  但是可以让用户输入一些信息")



##  JS的变量和常量

### Js的组成部分

* DOM是JS 的文档对象模型    DOM是html和xml的应用程序接口(API)
* 通过DOM提供的接口可以对页面上的各种元素进行操作(大小,位置,颜色等)
* css不能操作的, 那肯定是用DOM来操作, 比如页面滚动播放

-----

* BOM 是浏览器对象模型,   是用来对浏览器窗口进行访问和操作的,  比如常见的那个弹窗? alert\
* BOM是独立于页面内容的, 直接与浏览器窗口进行互动的对象结构, 通过BOM可以操作浏览器窗口
* 好吧, BOM  可以用来做**弹出框, 控制浏览器跳转, 获取分辨率**

------

* 浏览器实质上分成了两个部分, 渲染引擎和JS引擎
* 渲染引擎: **解析html和css   就是内核**
* JS引擎: **JS解释器, 用来读取网页中JS代码**

------

###  JS 关键字和保留字

* 命名跳过得了, 又不是不知道
* 什么break, case swich, while  ifelse var  void do instanceof  typeof return



### JS 的变量

* 变量就是一个记录数据地址的标签
* 哎对, 一个容器

```
声明变量
var  a ;
赋值
a=12;

一步到位,   又叫做变量的初始化
var a=12;
```

* **关键字的作用就是像计算机申请内存进行储存, 哎对, 就是这个意思, 打个招呼, 申请书, 专用格式**
* 变量命名, 略
* 可以同时声明多个变量,  变量的使用就不说了
* 还有**JS有个著名bug, 就是不用特定格式, 就是var啊, let啊, const啊,  直接写, 也会当变量, 不过这个变量会是全局变量,  会很麻烦,  所以变量定义一定要声明**

---

### JS的常量

* 常量是声明后再也不能改变的变量,  恒定的, 不变的,   所以叫常量
* 一般用const来声明



## JS的数据类型

* 数据类型吗, 为了方便计算机访问和查询, 所以分一堆数据类型, 这个类型存在这个内存, 那个类型存在那个内存
* JS总共只有两大种数据类型, 一种**基本数据类型**, 一种**引用数据类型**
* 两者的区别**基本数据类型只能存一个值, 引用数据类型能存多个值**

-----

* 基本数据类型5种: **数字number, 字符串string, 布尔值boolean, 未定义值undefined, 空值null**
* 引用数据类型:==**数组, 对象**==

---

* number直接保存int和float  不区分
* 常见进制,   前面有0是八进制,   有0x是十六进制
* Infinity  代表无穷大,    -Infinity  代表无穷小,     NaN   not a number 代表与一个非数值

---

* isNaN()     用来判断一个变量是否为**非数字**的类型,  会返回true和false

```
alert(isNaN(0%0));
```

----

* 字符串 略,  什么转义符,  什么单引号嵌套双引号,什么拼接字符串  啥比都会
* 获取字符串长度

```
var string=1232112321;
alert(string.length);
```

---

* undefined   就是声明一个变量, 没赋值,    网页输出这个变量就是undefined
* undefined+1        就是加一个数字,   **输出NaN**      是不能相加的,  只能输出不是一个数
* null和defined     一样,  就加字符串,  默认输出**null字符串**      **defined字符串**

* null+数字       只输出数字
* null+true          输出1
* undefined+ture     输出NaN



##  JS typeof操作符

* typeof  只有两种操作形式,    一种加括号,  一种不加括号,    加括号是为了**确保优先级**

```
typeof operand
typeof (operand)

var number;
alert(typeof number);      //输出number

var number =new object;
alert(typeof number);             //输出object
```

* 识别数据类型,   这种功能不需要我多说了吧       返回的可以是基础数据类型,  也可以是引用类型

* 最重要的是函数类型和对象类型      function  object

---

* 处理undefined类型的值       typeof 只识别三个,     undefined    未声明的变量,    已声明未初始化的变量
* 处理number类型的值       **数字,   number类型的变量, Number.MAX_VALUE,  Math对象的静态变量值,  Math.PI,    NaN     ,        Infinty和-Infinty    ,    数值类型的Number()**{不推荐!!!}
* 字符串略
* 函数类型(function)      **函数定义function,       使用class关键字定义的类,     某些内置对象的特定函数如Math.sin**

------

* **处理Object类型的值,   这个很麻烦**
* 对字面量的形式 例如{name:"king"};
* 数组,      [1,2,3]      或 array[1,2,3]
* 所有构造函数通过new操作实例化得到的对象     如**new Date(),   new function()**

```
typeof {a:1}    //输出'Object'
typeof [1,2,3]   //'Object'
typeof new Date()    // 'Object
```

-----

* 处理Null类型的值,     会输出**Object**
* JS在设计之初,   每种数据类型会用3bit来表示 ,   表示'Object'  为000     而Null也是000    所以会输出object



## JS隐式转换

* JS变量可以转换为新变量和其他数据类型
* 通过JS内部程序的自动转换就是**隐式转换**
* 通过JS**内置函数**转换的数据类型就是强制转换

---

* ==**隐式转换的目的是学习这些数据类型被转换后是怎么显示,  是以一种什么形式显示**==

-----

* 其他类型转换为Boolean类型

```
Boolean                
String       非空字符串会变成true                 空字符串会变成flase
Number       任何非零数字(包括无穷大) True         0和NaN  变成false
Object       任何对象都会变成true                 只有null变成false
Function     任何function类型都会变成true 没有false
undefined             都转换成false
null                  都转换成false
```



------





* 其他类型转换成numbe类型      **统一转换成对应的十进制并返回**

```
Undefined                 NaN             // 就算输出undefined+数字,   输出还是NaN
null                      0
true                      1
false                     0
字符串                     数字或NaN
其他对象                   NaN
```



---





* 其他类型转换为字符串类型

```
undefined                undefined
null                     null
true                     true
false                    false
number                   NaN  0   或数值对应的字符串
其他对象                  转换为toString()    否则转换为undefined
```





##   Boolean函数

* 强制转换函数,    把其他数据类型强制转换boolean

```
var a ="abc";
console.log(typeof Boolean(a));
```





## Number函数

```
Number(10);         //10
Number(010);        //8      010  八进制
Numbe(0x10);        //16,    0x10 十六进制

Number(null);              //  0
Number(undefined);         //  NaN
```

* Number()  最需要注意的就是 八进制,  十六进制数转十进制



## ParseInt函数

* ParseInt函数   用于解析一个字符串,    并按指定的基数返回对应的**整数值**
* 这是一个取整函数,  浮点数都会直接取整
* ParseInt(String,10/8/16/2);            //十进制, 八进制, 十六进制, 二进制

```
ParseInt有两种表现形式

ParseInt('0x12',16);      //直接转十六进制    18
ParseInt(0x12,16);        // 非字符串自带有number强转,  所以是先转十进制再转十六进制      24
```

* ParseInt有一个机制, 那就是逐字解析
* 比如ParseInt(fg123);         // 只解析了f, 返回16,    其他的解析不了直接不解析



## ParseFloat函数

