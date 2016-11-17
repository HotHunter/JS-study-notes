#JS高程学习笔记

---

###第一章 javascript简介
一个完整的javascript实现由三部分组成：
* 核心（ECMAscript）
* 文档对象模型（DOM）		
* 浏览器对象模型（BOM）

#### ECMAscript
由ECMA-262定义的ECMAscript与web浏览器没有依赖关系。
ECMA-262规定了：语法/类型/语句/关键字/保留字/操作符/对象
ECMAScript就是对实现该标准的各个方面内容的语言的描述。

！：ECMAScript的版本/ECMAScript的兼容问题/web浏览器对ECMAScript的支持

#### DOM-文档对象模型
针对XML但经过扩展用于HTML的应用程序编程接口。将整个页面映射为一个多层次节点结构
DOM1级：
* DOM-core：规定如何映射基于XML的文档结构，便于简化对文档中任意部分的访问和操作。
* DOM HTML：在DOM核心模块上加以扩展，添加针对HTML的对象和方法。
DOM2级：
引入新接口：DOM视图。DOM事件。DOM样式。DOM遍历和范围
DOM3级：
进一步扩展：DOM加载和保存。DOM验证

#### BOM-浏览器对象模型
浏览器窗口对象模型，用于控制页面以外的部分。在hTML5中加入正式规范，习惯吧所有针对浏览器的javascript扩展算作BOM的一部分。

---

### 第二章 在HTML中使用JAvaScript
#### HTML中插入js
属性：
* async：立即加载
* charset：通过src制定的代码的字符集（被忽略）
* defer：延迟加载
* language：生命编写的脚本语言（已废弃）
* src：包含要执行代码的外部文件
* type：language的替代属性，表示编写代码使用的脚本语言内容类型
   
    <script type="text/javascript">
        function sayScript(){
            alert("<\/script>");
            //alert("</script>");
        }
    </script>

    <script type="text/javascript" src="javascript.js"></script>

!：带有src属性的script标签中不应该有额外的js代码，会被忽略

src可以加载外部域的js文件（同img）

无defer和async属性时按顺序加载js文件

按照传统，所有<script></script>应该放在<head></head>中，但为了页面相应速度，均将与页面样式无关的js放在<body></body>后加载。

!：异步脚本一定会在页面的load时间前执行，但可能会在DOMContentLoaded事件触发之前或之后执行。

引用外部js文件好处：可维护性，克缓存，适应未来。

文档模式：混杂模式/标准模式

### 第三章 基本概念

ECMAscript中的变量/函数名/操作符都区分大小写。

标识符的命名规则：第一个字符为字母/_/$   驼峰/下划线

“严格模式”：头部添加编译指示'use strict'

ECMAScript定义变量为松散型，变量作为占位符用于存储任何值。省略var操作符，变量为全局变量。

#### 数据类型
##### undefined

    var mes;
    alert(mes == undefined); //true

    var mes = undefined;
    alert(mes == undefined); //true

    var mes;
    //var age;
    alert(mes); //"undefined"
    alert(age); //错误
    alert(typeof mes);  //"undefined"
    alert(typeof age);  //"undefined"

##### Null
Null类型只有一个值，为null。null值表示一个空对象指针
    
    nar car = null;
    alert(typeof car);  //"object"

    //可作为判断一个初始化的对象是否产生值的变化
    if(car != null){
        //do something
    }

    alert(null == undefined); //true

#####  bolean
    var mes = "hello world";
    if(mes){
        alert("true");
    }

##### Number
同时表示整数和浮点数
    
    var intNum = 5; //整数
    var ocalNum1 = 070; //八进制的56
    var ocalNum2 = 079; //无效的八进制数值-解析为79
    var ocalNum3 = 08;  //无效的八进制数值-解析为8
    var hexNum1 = 0xA;  //十六进制的10
    var hexNum2 = 0x1f; //十六进制的31
在计算时，所有八进制和十六进制表的的数值都将转换成十进制数。

    var floatNum1 = 1.1;
    var floatNum2 = 0.1;
    var floatNum3 = .1;         //有效，但不推荐
    var floatNum4 = 1.;         //无效，解析为1
    var floatNum5 = 10.0;       //证书，解析为10
    var floatNum6 = 3.125e7     //对于极大或极小的数值，用e表示

！：0.1+0.2 != 0.3  实际为0.3000000000000000004。这是使用基于IEEE数值的浮点计算的通病。
    
    if (a + b == 0.3){      //不要做这样的测试
        alert("you get 0.3")
    }

NaN：非数值（Not a Number）审核书除以非数值返回NaN。任何设计NaN的操作（NaN/10）都会返回NaN。NaN与任何值都不相等，包括NaN本身

    alert(NaN == NaN);      //false
    alert(isNaN(NaN));      //true
    alert(isNaN(10));       //false(10为整数)
    alert(isNaN("10"));     //false(可以被转换为10)
    alert(isNaN("blue"));   //true(不能被转换为数值)
    alert(isNaN(true));     //false(可被转换为1)

Nubmer的转换规则：
* boolean，true/false分别转换为1/0
* 数字值，只是简单的传入和返回
* null，返回0
* undefined，返回NaN
* 字符串
 * 只包含数字，转换为十进制
 * 包含浮点数，转会为对应的浮点数
 * 包含有效十六进制，转为对应大小十进制
 * 为空，0
 * 包含除以上格式外的字符，NaN
* 对象，则调用valueOf()，在依照上面的规则转换

    var num1 = Number("hello");     //NaN
    var num2 = Number("");          //0
    var num3 = Number("000001");    //1
    var num4 = Number(true);        //1

parseInt():拥有第二参数：进制数(建议使用)

    var num = parseInt("1234blue");     //1234
    var num = parseInt("");             //NaN
    var num = parseInt("0xA");          //10
    var num = parseInt(22.5);           //22
    var num = parseInt("070");          //56
    var num = parseInt("70");           //70
    var num = parseInt("0xf");          //15
    var num = parseInt("0xAF" , 16);    //175
    var num = parseInt("AF" , 16);      //175
    var num = parseInt("AF");           //NaN


parseFloat()：只有第一个小数点有效；忽略前导零，十六进制始终转换成0。只解析十进制数

##### String

    var firstName = "hothunter";
    var lastName = 'hothunter';

转义序列：
* \n        换行
* \t        指标
* \b        退格
* \r        回车
* \f        进纸
* \\\       斜杠
* \'        单引号
* \"        双引号
* \xnn      以十六进制nn表示一个字符  \x41表示"A"
* \unnnn    以十六进制nnnn表示一个Unicode字符（n为0~F）\u03a3表示（求和符号sigma，我不会打。。）

例子：

    var text = "This is the letter sigma: \u03a3.";
    alert(text.length);     //28










