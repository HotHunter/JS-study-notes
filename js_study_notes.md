#JS高程学习笔记

---

###第一章 javascript简介     P1
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

### 第二章 在HTML中使用JAvaScript      P10
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

### 第三章 基本概念        P19

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

undefined派生自null

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

    var lang = "Java";
    lang = lang + "Script";

    var age = 11;
    var ageAsString = age.toString();       //"11"
    var found = true;
    var foundAsString = found.toString();   //"true"

null和undefined 无toString()方法。toString()有一个基数参：

    var num = 10;
    alert(num.toString());      //"10"
    alert(num.toString(2));     //"1010"
    alert(num.toString(8));     //"12"
    alert(num.toString(10));    //"10"
    alert(num.toString(16));    //"a"

String():将任何类型的值转换为字符串：

    var value1 = 10;
    var value2 = true;
    var value3 = null;
    var value4;
    alert(String(value1));      //"10"
    alert(String(value2));      //"true"
    alert(String(value3));      //"null"
    alert(String(value4));      //"undefined"

可以使用“+”，将某个值与一个""字符串相加从而转化为字符串。

##### Object

一组数据和功能的集合。对象可以通过执行new后跟要创建的对象类型的名称创建。创建Object类型的实例并为其添加添加属性/方法以创建自定义对象。

    var obj = new Object();

Object类型是所有它的实例的基础，Object所具有的任何属性和方法同样存在于更具体的对象中：
* constructor:构造函数，保存用于创建当前对象的函数。上面的Object()
* hasOwnProperty(propertyName):检查给定的propertyName在当前对象实例中是否存在。
* isPrototypeOf(object):检查传入的对象是否是当前对象的原型。
* propertyIsEnumerable(propertyName):检查给定的属性是否能够使用for-in语句来枚举。
* toLocaleString():返回对象的字符串表示，该字符串与执行环境的地区对应。【这个啥意思没看懂】
* toString():返回字符串
* vlaueOf():返回字符串/数值/布尔值。通常与toString()返回值相同。

##### 操作符

++/-- ：
    
    var age = 29;
    ++age;      //age = age + 1;

在执行递增和递减时，变量的值都是在语句被求值以前改变的

    var age = 29;
    var anotherAge = --age + 2;
    alert(age);         //28
    alert(anotherAge);  //30

由于前置递增和递减操作与执行语句的优先级相等，因此整个语句会从左至右被求值。
    
    var num1 = 2;
    var num2 = 20;
    var num3 = --num1 + num2;       //21
    var num4 = num1 + num2;         //21

！：后置递增递减实在包含它们的语句被求值之后才执行。

    var num1 = 2;
    var num2 = 20;
    var num3 = num1-- + num2;       //22
    var num4 = num1 + num2          //21

操作符转换规则："2" -> 2 / "ff" -> NaN    /   false -> 0  /   true -> 1   /Object() -> valueOf()

    var s1 = "2";
    var s2 = "a";
    var b = false;
    var f = 1.1;
    var obj = {valueOf: function(){return -1}};
    //
    s1++;           //3
    s2++;           //NaN
    b++;            //1
    f--;            //0.10000000000000009(浮点舍入错误导致)
    obj--;          //-2   

位操作符，& | ~
以0填充移动后的空位
左移  <<    

    var oldValue = 2;                   //二进制的10
    var newValue = oldValue << 5;       //等于二进制的1000000，十进制的64

有符号的右移 >>

    var oldValue = 64;              //二进制的1000000
    var newValue = oldVlaue >> 5;   //二进制的10，2

无符号的右移  >>>     将负数的二进制码转换成正数的二进制码；负数无符号右移后结果非常大。

    var oldValue = -64;             //等于二进制的 11111111111111111111111111000000
    var newValue = oldValue >>> 5   //等于十进制的134217726 

布尔操作符：与或非

逻辑非（!）：将被操作数转换成布尔值之后求反
    
    alert(!false);      // true
    alert(!"blue");     // false
    alert(!0);          // true
    alert(!NaN);        // true
    alert(!"");         // true
    alert(!1234);       // false

逻辑与（&&）：可以用于任何类型的操作数

    var found = true;
    var result = (found && someUndefinedVariable);      //someUndefinedVariable未被定义，这里会出错

逻辑或（||）

    var myObject = preferredObject || backupObject;     //赋值方式

乘性操作符

    var result = 34 * 56;
    var result = 66 / 11;
    var result = 26 % 5;

加性操作符

    //加法
    var result1 = 5 + 5;
    alert(result1);         //10
    var result2 = 5 + "5";
    alert(result2);         //"55"

    var num1 = 5;
    var num2 = 10;
    var message = "The sum of 5 and 10 is " + (num1 + num2);
    alert(message);                 //"The sume of 5and 10 is 15"

    //减法
    var result1 = 5 - true;     //4  true -> 1
    var result2 = NaN - 1;      //NaN
    var result3 = 5 - 3;        //2
    var result4 = 5 - "";       //5  "" -> 0
    var result5 = 5 - "2";      //3
    var result6 = 5 - null      //5

注意以上加性乘性操作符对于+—0以及+—Infinity等特殊情况的规则。

关系操作符：

    var result1 = 5 > 3;     //true
    var result2 = 5 < 3;     //false

在比较字符串时，实际比较的是两个字符串中对应位置的每个字符的字符编码值。比较后再返回一个布尔值。
大写字母字符编码全部小于小写字母字符编码。
    
    var result = "Brick" < "alphabet";      //true
    var result = "23" < "3";                //true "2"的字符编码50，"3"51
    var result = "23" < 3;                  //false     
    var result = "a"  < 3;                  //false a->NaN
    var result1 = NaN < 3;                  //false
    var result2 = NaN >= 3;                 //false

相等操作符

相等和不相等（== / !=）：强转型后比较相等性
全等和不全等（=== / !===）：不转型直接比较

    var result1 = ("55" == 55);         //true
    var result2 = ("55" === 55);        //false
    var result3 = ("55" != 55);         //false
    var result4 = ("55" !== 55);        //true
    null == undefined                   //true
    null === undefined                  //false

条件操作符

    variable = boolean_expression ? true_value : false_value;
    var max = (num1 > num2) ? num1 : num2;

赋值操作符

    var num = 10;
    num = num + 10;
    num += 10;

* *=
* /=
* %=    模赋值
* +=
* -=
* <<=   左移赋值
* >>=   右移赋值

逗号操作符
逗号用于赋值时，总会返回表达式中最后一项

    var num1 = 1,num2 = 2, num3 = 3;
    var num = (5, 1, 4 ,8 ,0);  //num的值为0


##### 语句

###### if

    if(i > 25){
        alert("Greater then 25.");
    }else if(i < 0){
        alert("Less than 0.");
    }else {
        alert("Between 0 and 25, inclusive.")
    }

###### do-while

    do {
        statement
    } while (expression);
    //
    var i = 0;
    do {
        i += 2;
    } while (i < 10);
    alert(i);

###### while

    var i = 0;
    while (i < 10){
        i += 2;
    }

###### for

前测试循环语句，执行循环之前初始化变量和定义循环后要执行的代码的能力。
    
    var count = 10;
    //var i;
    for(i = 0;i < count;i++){
        alert(i);
    }

    for(;;){        //无限循环
        doSomething();
    }

###### for-in
更精准的迭代语句，可以用来枚举对象的属性。循环输出的顺序不可预测

    for(var propName in window){
        document.write(propName);
    }

###### label
添加标签以便将来使用。

    start: for(var i = 0;i < count; i++){
        alert(i);
    }

###### break / continue
break终止循环体，退出循环，强制继续执行循环后面的代码。
continue停止本次循环，但不会跳出循环体。

    var num = 0;
    for (var i = 1; i < 10; i++){
        if (i % 5 == 0){
            break;
        }
        num++;
    }
    alert(num);             //4

    var num = 0;
    for (var i = 1; i < 10; i++){
        if (i % 5 == 0){
            continue;
        }
        num++;
    }
    alert(num);             //8

break/continue 配合label 跳出到指定位置。

    var num = 0;
    outermost:
    for(var i = 0; i < 10; i++){
        for(var j=0; j < 10; j++){
            if ( i == 5 && j == 5){
                break outermost;
            }
        }
    }
    alert(num);                             //55

    var num = 0;
    outermost:
    for(var i = 0; i < 10; i++){
        for(var j=0; j < 10; j++){
            if ( i == 5 && j == 5){
                continue outermost;
            }
        }
    }
    alert(num);                             //95

###### with
将代码的作用域设置到一个特定的对象中
    
    var qs = location.search.substring(1);
    var hostName = location.hostname;
    var url = location.href;

可以使用with语句，改写成

    var qs = search.substring(1);
    var hostName = hostname;
    var url = href;

改写后意味着在with语句的代码块中，每个变量首先被认为是一个局部变量，而如果在局部环境中找不到该变量的定义，就会查询location兑现各种是否有同名的属性，如果发现了同名，则以location对象属性的值作为变量的值。
严格模式不允许使用with。

###### switch
    switch (i){
        case 15:        //合并两种情况
        case 25:
            alert("25&15");
            break;
        case 35:
            alert("35");
            break;
        case 45:
            alert("45");
            break;
        default:
            alert("Other");
    }

除此以外的case条件还可以有：

    case "Hello" + " world":
    case "goodbye":
    case num < 0:
    case num > 0 && num <=10:

##### 函数
    function functionName(arg0,arg1,...,argN) {
        statements
    }
    function sum(num1, num2){
        return num1 + num2;
        alert("这句永远不会被执行");
    }

不在意传入参数类型以及个数，参数存储在arguments对象中（类似数组）
参数数量为arguments.length，参数i为arguments[i]
    
    function howMaryArgs(){
        alert(arguments.length);
    }
    howMaryArgs("String", 45);      //2
    howMaryArgs();                  //0
    howMaryArgs(23);                //1

    function doAdd(num1, num2){
        if(arguments.length == 1){
            alert(num1 + 10);
        }else if(argument.length == 2){
            alert(arguments[0] + num2);
        }
    }

arguments的值永远与对应命名参数的值保持同步。

    function doAdd(num1, num2){
        arguments[1] = 10;
        alert(arguments[0] + num2);
    }

没有传递值的参数自动被赋予undefined。
ECMAScript的幻术不能像传统意义上实现重载。当定义两个名字相同的函数，改名字只属于后定义的函数
    
    function addSomeNumber(num){
        return num + 100;
    }
    function addSomeNumber(num){
        return num + 200;
    }
    var result addSomeNumber(100);              //300

---

### 第四章 变量、作用域和内存问题     P68

由于不存在定义某个变量必须要保存何种数据类型值的规则，变量的值以及其数据类型可以在脚本的生命周期内改变。

#### 基本类型和引用类型的值

undefined/null/boolean/number/string按值访问，属于基本类型。
引用类型的值保存在内存中，javascript无法直接操作内存，引用类型的值是按引用访问的。
当复制保存对象的某个变量时，操作的是对象的额引用。但在为对象添加属性时，操作的是实际的对象。

如果对象不被销毁或者这个属性不被删除，这个属性将一直存在。
但是不能给基本类型的值添加属性（尽管这样做不会导致任何错误），只能给引用类型动态添加属性。
    
    var person = new Object();
    person.name = "hothunter";
    alert(person.name);         //"guohaote"

    var name = "hothunter";
    name.age = 23;
    alert(name.age);            //undefined

复制：
基本类型值复制时，两个变量互相独立，，可以参与任何操作不会互相影响。
引用类型值复制时，实际产生一个指针，而这个指针只想存储在堆中的同一个对象，一年次改变其中一个变量，会影响另一个变量。
    
    var num1 = 5;
    var num2 = num1;
    //虽然两个变量值相同，但并不是同一个值 :)

    var obj1 = new Object();
    var obj2 = obj1;
    obj1.name = "Hothunter";
    alert(obj2.name);       //"Hothunter"

传参：
基本类型值传参时，被传递的值会复制给一个局部变量（arguments[i]）。
引用类型值，会把这个值在内存中的地址复制给一个局部变量，因此这个局部变量的变化会反映在函数外部。

    function addTen(num){
        num += 10;
        return num;
    }
    var count = 20;
    var result = addTen(count);
    alert(count);               //20
    alert(result);              //30

    function setName(obj){
        obj.name = "Hothunter";
    }
    var person = new Object();
    setName(person);
    alert(person.name);         //"Hothunter"

!:即使在函数内部修改了参数的值，但原始的引用仍然保持不变。实际上，挡在函数内部重写obj时，这个变量引用的就是一个局部对象了。而这个局部对象会在函数执行完毕后立即被销毁。

    function setName(obj){      //函数的参数即为局部变量
        obj.name = "Hothunter";
        obj = new Object();
        obj.name = "Greg";
    }
    var person = new Object();
    setName(person);
    alert(person.name);         //"Hothunter"

检测类型：

    var s = "hothunter";
    var b = "true";
    var i = 22;
    var u;
    var n = null;
    var o = new Object();
    alert(typeof s);        //string
    alert(typeof b)         //boolean
    alert(typeof i)         //number
    alert(typeof u)         //undefined
    alert(typeof n)         //object
    alert(typeof o)         //object

instance ：如果变量是给定引用类型的实例，instance操作符就会返回true。检测基本类型时返回false。
    
    alert(person instanceof Object);    //person是Object吗
    alert(colors instanceof Array);     //colors是Array吗
    alert(pattern instanceof RegExp);   //pattern是RegExp吗

####  执行环境及作用域：
定义了变量或者函数有权访问的其他数据，决定各自的行为。每个执行韩静都有一个与之关联的变量对象（variable object），环境中定义的所有变量和函数都保存在这个对象中。虽然编写的代码无法访问，但解析器在处理数据是在后台访问。
全局环境被认为window对象。
代码在一个环境中执行时，会常见变量对象的一个作用域链（scope chain）。保证对执行环境有权访问的所有变量和函数的有序访问。
表示服解析颜值作用域链意义搜索标识符。

    var color = "blue";
    function changeColor(){
        if (color === "blue"){
            color = "red";
        }else {
            color = "blue";
        }
    }
    changeColor();
    alert("color is now: " + color);

局部作用域中定义的变量可以再局部环境中与全局变量互换使用

    var color = "blue";
    function changeColor(){
        var anotherColor = "red";
        function swapColors(){
            var tempColor = anotherColor;
            anotherColor = color;
            color = tempColor;
            //这里可以访问color/anotherColor和tempColor
        }
        //这里可以访问color和anotherColor，但不能访问tempColor
        swapColors();
    }
    //这里只能访问color
    changeColor();

内部环境可以通过作用域链访问外部环境，但外部环境不能访问内部环境中的任何变量和函数。

延长作用域链：
try-catch/with语句，都会在作用域链的前端添加一个变量对象。
with：会将制定的对象添加到作用域链中。
catch：创建一个新的变量对象，包含的是被抛出的错误对象的声明。

    function buildUrl(){
        var qs = "?debug=true";
        with(location){
            var url = href + qs;
        }
        return url;
    }

没有块级元素：
不用于其他类C语言，javaScript无块级元素。
    
    if (true) {
        var color = "blue";
    }
    alert (color);      // "blue"

    for(var i = 0; i<10 ; i++){
        doSomeThing();
    }
    alert(i);           //10

使用var声明的变量会自动被添加到最近的环境中。
在函数内部，最近的环境就是函数的局部环境；
上例中的for/if则被添加到执行环境即全局环境。

    function add(num1, num2){
        var sum = num1 + num2;
        return sum;
    }
    var result = add(10 , 20);      //30
    alert(sum);                     //由于sum不是有效的变量，因此会导致错误。

    function add(num1, num2){
        sum = num1 + num2;
        return sum;
    }
    var result = add(10 , 20);      //30
    alert(sum);                     //30

在函数内部变量没有石油var关键字声明，则自动变成全局变量。

查询标识符的过程：

    var color = "blue";
    function getColor(){
        return color;
    }
    alert(getColor());      // "blue"

如果存在一个局部的变量的定义，搜索会自动停止，不再进入另一个变量对象，如果局部环境中存在同名标识符，就不会适应位于父环境中的标识符。

    var color = "blue";
    vunction getColor(){
        var color = "red";
        return color;
    }
    alert(getColor());      // "red"

#### 垃圾收集
执行环境会负责管理代码执行过程中使用的内存。
两个实现方法：

标记清除：
垃圾收集器会在运行的时候归给存储在内存中的所有变量都加上标记。然后会去掉环境中的变量以及被环境中的额变量引用的额变量的标记。而在之后再被加上标记的变量将被视为准备删除的变量，原因是环境中的变量已经无法访问到这些变凉了。最后完成内存清除工作，销毁那些带标记的值并收回他们占用的内存。

引用计数：
因为循环的问题，这个方法已被废弃，太长不看了。

性能问题：
因需要周期运行，如果变量分配的内存数量可观，导致回收工作量变大。
//TODO:感觉这里涉及到了页面性能优化。
    
    window.CollectGarbage();        //IE中立即调用垃圾回收
    window.opera.collect();         //Opert7以上版本立即调用垃圾回收

管理内存：
存在问题，因为分配给浏览器的可用内存数量小于分配给桌面应用的（出于安全考虑，防止javascript网页耗尽全部内存导致系统崩溃）。内存闲置问题不仅会影响给变量分配的内存，同事还会影响调用栈以及在一个贤臣各种能够同时执行的语句数量。
所以：一旦数据不再有用，最好通过将其值设置为null来释放其引用--解除引用(dereferencing)。适用于大多数全局变量和全局对象的属性。

    function createPerson(name){
        var localPerson = new Object();
        localPerson.name = name;
        return localPerson;
    }
    var globalPerson = createPerson("hothunter");
    //手工解除globalPerson的引用
    globalPerson = null;

localPerson在函数执行环境结束时自动被回收，但globalPerson则需要在不是用他的时候手工解除引用。
珍重目的不意味着自动回收该所占用的内存。真正作用是让值脱离执行环境，以便垃圾收集器下次运行将其回收。

---

### 引用类型

引用类型的值（对象）是引用类型的一个实例。在EMCAScript中，引用类型是一种数据结构，将数据和功能组织在一起。也被成为<b>类</b>、<b>对象定义</b>。
对象是某个特定引用类型的实例。

    var person = new Object();

↑ 创建了Object引用类型的一个新实例，然后把该实例保存在了变量person中。使用的构造函数是Object，它只为新兑现该定义了默认的属性和方法。

#### Object 类型

创建Object

    var person = new Object();
    person.name = "Hothunter";
    person.age = 23;

    var person = {
        "name" : "HotHunter",
        "age" : 23
    };

//TODO:上下文
表达式上下文（expression context）：
语句上下文（statement context）：

    var person = {
        "name"  :   "HotHunter",
        "age"   :   23,
        5       :   true
    };
   
    var person = {};        ==>     var person = new Object();

    function displayInfo(args){
        var output = "";
        if(typeof args.name = "string"){
            output += "Name:" + args.name + "\n";
        }
        if(typeof args.age == "namber"){
            output += "Age:" +　args.age + "\n";
        }
        alert(output);
    }
    displayInfo({
        name :　"Hothunter",
        age : 23
        });
    displayInfo({
        name : "gray";
        });

对那些必须治使用命名参数，而是用对象字面量来封装过多个可选参数。

    alert(person["name"]);      //"Hothunter"
    alert(person.name);         //"Hothunter"

从功能上看，这两种访问属性的方法没有任何区别。但方括号语法的主要优点是可以通过变量来访问属性。

    var propertyName = "name";
    alert(person[propertyName]);        //"Hothunter"
    person["first name"] = "Hothunter"      //first name中间有一个空格，只能用[]访问到。

通常，除非必须使用变量来访问属性，负责我们建议使用点表示法。


#### Array 类型
数组：数据的有序列表。
ECMAScript数组的每一项可以保存任何类型的数据。数组的大小是可以动态调整的，随着数组的添加自动增长一容纳新数据。

创建数组：

    var colors = new Array();
    var colors = new Array(20);
    var colors = new Array("red", "blue", "green");
    var colors = Array(3);
    //
    var name = [];          //一个空数组
    var values = [1,2,];    //不要这样！创建一个包含2或3项的数组
    var options = [,,,,,];  //不要这样！创建一个包含5或6项的数组

    var colors = ["red", "blue", "green"];
    alert(colors[0]);           //显示第一项
    colors[2] = "black";        //修改第三项
    colors[3] = "brown";        //新增第四项

    var colors = ["red", "blue", "green"];
    colors.length = 2;
    alert(colors[2]);           //undefined     .length缩小了数组长度

    var colors = ["red", "blue", "green"];
    colors.length = 4;
    alert(colors[3]);           //undefined     .length增加了长度

由于数组最后一线搞得key始终是length-1，因此下一个新项的位置就是length

    var colors = ["red", "blue", "green"];
    colors[colors.length] = "black";
    colors[colors.length] = "brown";

    var colors = ["red", "blue", "green"];
    colors[99] = "black";
    alert(colors.length);       //100

检测数组：
    
    if (value instanceof Array){
        doSomeThing();
    }

    if (Array.isArray(value)){
        doSomeThing();
    }

转换方法：

    var colors = ["red", "blue", "green"];
    alert(colors.toString());           //red,blue,green
    alert(colors.valueOf());            //red,blue,green
    alert(colors);                      //red,blue,green

toString将每个值得字符串表示拼接成字符串，用逗号分隔。

    var person1 = {
        toLocaleString : function (){
            return "HotHunter";
        },
        toSring : function(){
            return "Hot";
        }
    };
    var person2 = {
        toLocaleString : function (){
            return "meishaonv";
        },
        toString : function (){
            return "msn";
        }
    };
    //
    var people [person1, person2];
    alert(people);                      //Hot,msn
    alert(people.toString());           //Hot,msn
    alert(people.toLocaleString());     //HotHunter,meishaonv

！：console.log和alert：
console.log可以打印任何类型数据。而alert()只能打印String，如果alert输出的是对象，自动调用toString()。
    
    console.log(people);                    //[Object, Object];
    console.log(people.toString());         //Hot,msn
    console.log(people.toLocaleString());   //HotHunter.meishaonv

数组集成的toLocaleString()/toString()/valueOf()在默认情况下都会以逗号分隔的字符串形式返回数组项。
使用join()方法可以使用不同的分隔符构建这个字符串

    var colors = ["red" ,"blue", "green"];
    alert(colors.join(","));        //red,blue,green
    alert(colors.join("||"));       //red||blue||green


栈方法：
栈数据结构的访问规则是LIFO（后进先出）。
数组可以表现栈，可以限制插入和删除项的数据结构。最新添加的项最早被移除。栈中的推入和弹出只发生在栈的顶部。
push()/pop()--推入、弹出。
push()接收任意数量的参数，诸葛添加到数组末尾，返回修改后数组的长度。
pop()从数组末尾一处最后一项，减少数组的length，返回溢出的项。

    var colors = new Array();
    var count = colors.posh("red", "green");        //推入两项
    alert(count);       //2
    //
    count = colors.push("black");           //推入一项
    alert(count);       //3
    //
    var item = colors.pop();
    alert(item);            //"black"
    alert(colors.length);       //2

可以将栈方法和数组其他方法一同使用：

    var colors = ["red", "blue"];
    colors.push("brown");
    colors[3] = "black";
    alert(colors.length);       //4
    //
    var item = color.pop();
    alert(item);        //"black"

队列方法：

栈数据结构的访问规则是LIFO（后进先出）。
队列数据结构的访问规则是FIFO（先进先出）。队列在列表的末端加项，从列表的前端移除项。
shift()，一处数组中第一个项并返回该项，结合push()实现队列一样使用数组

    var colors = new Array();
    var count = colors.push("red", "green");
    alert(count);   //2
    //
    count = colors.push("black");
    alert(count);   //3
    //
    var item = colors.shift();
    alert(item);    //"red"
    alert(colors.length);       //2

unshift()：在数组前端添加任意个项并返回新数组的长度。

    var colors = new Array();
    var count = colors.unshift("red", "green");
    alert(count);   //2
    //
    count = colors.unshift("black");
    alert(count);   //3
    //
    var item = colors.pop();
    alert(item);        //"green"
    alert(colors.length);       //2

重排序方法：
reverse():反转数组项的顺序。
sort():默认情况按升序排列数组项（上大后小）。调用每个数组项的toString()，然后比较得到字符串，确定排序。即使数组每一项都是数值，sort()比较的也是字符串

    var values = [1, 2, 3, 4, 5];
    values.reverse();
    alert(values);      //5, 4, 3, 2, 1

    var values = [0, 1, 5, 10, 15];
    values.sort();
    alert(values);      //0,1,10,15,5

sort()接受一个比较函数作为参数，以便制定哪个值位于前面。
比较函数接收两个参数，如果第一个参数应该位于第二个之前则返回负数，如果两个参数相等返回0，第一个参数位于第二个之后返回正数。

    function compare(value1, value2) {
        if (value1 < value2) {
            return -1;
        } else if (value1 > value2) {
            return 1;
        } else {
            return 0;
        }
    }
        //
    var values = [0, 1, 5, 10, 15];
    values.sort(compare);
    alert(values);          //0, 1, 5, 10, 15

reverse()和sort()方法的返回值是经过排序之后的数组。

更简单的比较函数：

    function compare(value1, value2){
        return value2 - value1;
    }
    
    var values = [2,5,6,44,8];
    alert(values.sort(compare));        //44, 8, 6, 5, 2


操作方法:

concat():给予当前数组中的所有项创建一个新数组。
当传递给concat()的是一个或多个数组，方法会将浙西额数组中的每一项都添加到结果数组中。如果传递的值不是数组，就会被简单地添加到结果数组的末尾。

    var colors = ["red", "green", "blue"];
    var colors2 = colors.concat("yellow", ["black", "brown"]);
         
    alert(colors);          //red,green,blue
    alert(colors2);         //red,green,blue,yellow,black,brown


slice():基于当前数组中的一或多个项创建一个新数组。
只有一个参数：slice()返回从该参数制定位置开始到当前数组末尾的所有项。
两个参数：返回其实和结束为止之间的项，但不包括结束为止的项。不影响原始数组。

    var colors = ["red", "green", "blue", "yellow", "purple"];
    var colors2 = colors.slice(1);
    var colors3 = colors.slice(1,4);
            
    alert(colors2);         //green,blue,yellow,pruple
    alert(colors3);         //green,blue,yellow

如果参数中有一个负数，则用数组长度加上该数来确定相应的位置。
在一个包含5项的数组，slice(-2,-1) == slice(3,4)。如果结束位置小于起始位置，返回空数组。


splice():主要用途项数组插入项。始终返回一个数组，返回被删除的项，若没删除任何项，返回空。
* 删除：删除任意数量项。splice(起始位置，删除的项数)。splice(0,2)删除数组的前两项。
* 插入：项指定位置插入任意数量的项。splice(起始位置,0(删除的项数),要插入的项); splice(2,0,"red","green")。
* 替换：向指定位置插入任意数量的项，同事删除任意数量的项。splice(起始位置,删除的项数，要插入的项);     splice(2,1,"green","red")。

    var colors = ["red", "green", "blue"];
    var removed = colors.splice(0,1);
    alert(colors);      //green,blue
    alert(removed);     //red
        
    removed = colors.splice(1, 0, "yellow", "orange");
    alert(colors);      //green,yellow,orange,blue
    alert(removed);     //空数组
        
    removed = colors.splice(1, 1, "red", "purple");
    alert(colors);      //green,red,purple,orange,blue
    alert(removed);     //yellow


位置方法：
indexOf()/lastIndexOf()：
两个参数：要查找的项和（可选）表示查找起点位置的索引。indexOf()从数组开始查找，lastIndexOf()从数组末尾开始查找。
都返回要查找的项在数组中的外置(下标)，没找到返回-1。要求查找的项必须全等(===)

    var numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
     
    alert(numbers.indexOf(4));          //3
    alert(numbers.lastIndexOf(4));      //5
      
    alert(numbers.indexOf(4, 4));       //5
    alert(numbers.lastIndexOf(4, 4));   //3
        
    var person = {name: "Hothunter"};
    var people = [{name: "Hothunter"}];
    
    var morePeople = [person];
    
    alert(people.indexOf(person));      //-1
    alert(morePeople.indexOf(person));  //0



迭代方法：
每个方法接收两个参数：
要在每一项上运行的函数和该函数的作用域对象（可选）--影响this的值。
传入这些方法中的函数会接收三个参数，数组项的值、该项在数组中的位置和数组对象本身。
根据使用方法不同，返回值不能确定会不会被该函数影响。
* every()：对数组的每一项运行函数。均true，则true。
* filter()：对数组的每一项运行函数。返回该函数会返回ture的项组成的数组。
* forEach()：对数组的每一项运行函数。无返回值。
* map()：对数组的每一项运行函数。返回每次函数调用的结果组成的数组。
* some()：对数组的每一项运行函数。任一项返回true，返回true。
以上方法均不修改原数组值。

    var numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
      
    var everyResult = numbers.every(function(item, index, array){
        return (item > 2);
        });
    alert(everyResult);     //false
    
    var someResult = numbers.some(function(item, index, array){
        return (item > 2);
        });
    alert(someResult);      //true
    
    var filterResult = numbers.filter(function(item, index, array){
        return (item > 2);
        });
    alert(filterResult);    //[3,4,5,4,3]
    
    var mapResult = numbers.map(function(item, index, array){
        return (item * 2);
        });
    alert(mapResult);       //[2,4,6,8,10,8,6,4,2]
    
    numbers.forEach(function(item, index, array){
        doSomeThing();
        });

最后一个forEach()，只是对数组中的每一项运行传入的函数。没有返回值，本质上与使用for循环迭代数组一样。


归并方法：

reduce()和reduceRight()：
迭代数组的所有项，构建一个最终返回的值。
reduce()方法从数组的第一项开始，遍历到最后。
reduceRight()方法从数组最后一项开始，向前遍历到第一项。
都接收两个参数：一个在每一项调用的函数和作为归并基础的初始值（可选）。
传给reduce()和reduceRight()的函数接收4个参数：前一个值、当前值、项的索引和数组对象。这个函数返回的任何值都会作为第一个参数自动传给下一项。第一次迭代发生在数组第二项上，因此第一个参数是数组的第一项，第二个参数就是数组第二项。

    var values = [1, 2, 3, 4, 5];
    var sum = values.reduce(function(prev, cur, index, array){
        return prev + cur;
        });
    alert(sum);         //15
    
    var sum2 = values.reduceRight(function(prev, cur, index,array){
        return prev + cur;
        });
    alert(sum2);        //15


#### Date类型

    var now = new Date();

Date.parse()/Date.UTC()

    var someDate = new Date(Date.parse("May 25, 2004"));
    var someDate = new Date("May 25, 2004");        // === ↑
    
    var y2k = new Date(Date.UTC(2000, 0));      //GMT时间2000/01/01午夜零时
    var allFives = new Date(Date.UTC(2005,4,5,17,55,55));   //GMT时间2005/05/05 5:55:55

Date.now()，返回表示调用这个方法时的日期和时间的毫秒数。

    var start = Date.now();
    doSomething();
    var stop = Date.new();
    var useTime = stop - start;

    var start = +new Date();
    doSomething();
    var stop = +new Date();
    var useTime = stop -start;          // ← === ↑


继承的方法：

同样重写了toString()/toLoaleString()/valueOf()方法。前两个不同浏览器返回格式也不同，同时也只在调试时有用。
valueOf()不反悔字符串，而是返回日期的毫秒。

    var date1 = new Date(2007, 0, 1);       //"January 1, 2007"
    var date2 = new Date(2007, 1, 1);       //"February 1, 2007"
    
    alert(date1 < date2);       //true
    alert(date1 > date2);       //false


日期格式化方法：
将日期格式化为字符串
toDateString()/toTimeSting()/toLocaleDateStrig()/toLocaleTimeString()/
toUTCString()
以上字符串格式方法的输出也是因浏览器而异，所以没有哪一个方法用来在用户界面中使用。

    
日期、时间组件方法：
表太长了我不写了。P102，用到了自己查吧。


#### RegExp类型

ECMAScript通过RegExp类型支持正则表达式。
    
    var expression = / pattern / flags ;

模式（pattern）只带正则表达式，可以包含字符类，限定符，分组，向前查找以及反响引用。每个正则表达式都可以带有一个或多个标志（flags），用以标明正则表达式的行为。
* g：表示全局（global）模式，应用于所有字符串，而非在发现第一个匹配项时立即停止。
* i:表示不区分大小写（case-insensitive）模式，即在确定匹配项时忽略模式与字符串的大小写；
* m：表示多行（multiline）模式，在到达一行文本末尾时还会继续查找下一行中是否存在于模式匹配的项。

    //匹配字符串中所有的“at”
    var pattern1 = /at/g;
    
    //匹配第一个“bat”或“cat”，不区分大小写
    var pattern2 = /[bc]at/i;
    
    //匹配所有已“at”结尾的3个字符的组合，不区分大小写
    var pattern3 = /.at/gi;

模式中使用的 元字符 都必须转移，包括：
(  [  {  \  ^  |  )  ?  *  +  .  ]  }

    //匹配第一个“bat”或“cat”，不区分大小写
    var pattern1 = /[bc]at/i;
    
    //匹配第一个" [bc]at"  ，不区分大小写
    var pattern2 = /\[bc\]at/i;
    
    //匹配所有以“at”结尾的3个字母的组合，不区分大小写
    var pattern3 = /.at/gi;
    
    //匹配所有".at" ， 不区分大写小写
    var pattern4 = /\.at/gi;

使用RegExp构造函数，两个参数：要匹配的字符串模式，可选的标志字符串。

    //匹配第一个“bat”或“cat”，不区分大小写
    var pattern1 = /[bc]at/i;
    
    //与pattern1相同，使用构造函数：
    var pattern2 = new RegExp("[bc]at", "i");

    var re = null,
        i;
    for (i = 0; i < 10; i++){
        re = /cat/g;
        re.test("catastrophe");
    }
    for (i = 0; i < 10; i++){
        re = new RegExp("cat", "g");
        re.test("catastrophe");
    }

以上两种方法，在ECMAScript5之后没有区别。


RegExp实例属性：
* global：布尔值，表示是否设置了g。
* ignoreCase：布尔值，表示是否设置了i。
* lastIndex：正数，表示开始搜索下一个匹配项的字符位置，从0算起。
* multiline：布尔值，表示是否设置了m。
* source：正则表达式的字符串表示，按照字面量形式而非传入构造函数中的字符串模式返回。

虽然方法多，不过都没什么用，因为构造的时候都写清楚了。

    var pattern1 = /\[bc]at/i;
    
    alert(pattern1.global);         //false
    alert(pattern1.ignoreCase);     //true
    alert(pattern1.lastIndex);      //0
    alert(pattern1.multline);       //false
    alert(pattern1.source);         //"\[bc\]at"
    
    var pattern2 = new RegExp("\\[bc\\]at", "i");
    
    alert(pattern1.global);         //false
    alert(pattern1.ignoreCase);     //true
    alert(pattern1.lastIndex);      //0
    alert(pattern1.multline);       //false
    alert(pattern1.source);         //"\[bc\]at"


RegExp实力方法：
exec()：一个参数：要应用模式的字符串，返回包含第一个匹配项信息的数组；没有匹配项的情况返回null。同时包含两个额外的属性：index和input。index表示匹配项在字符串的位置，input表示应用正则的字符串。

    var text = "mom and dad and baby";
    var pattern = /mom( and dad( and baby)?)?/gi;
    
    var matches = pattern.exec(text);
    alert(matches.index);       //0
    alert(matches.input);       //"mom and dad and baby"
    alert(matches[0]);          //"mom and dad and baby"
    alert(matches[1]);          //" and dad and baby"
    alert(matches[2]);          //" and baby"

    var text = "cat, bat, sat, fat";
    var pattern1 = /.at/;
    
    var matches = pattern1.exec(text);
    alert(mathces.index);           //0
    alert(mathces.[0]);             //cat
    alert(pattern1.lastIndex);      //0
    
    matches = pattern1.exec(text);
    alert(mathces.index);           //0
    alert(mathces.[0]);             //cat
    alert(pattern1.lastIndex);      //0
    
    var pattern2 = /.at/g;
    
    var matches = pattern2.exec(text);
    alert(matches.index);           //0
    alert(matches[0]);              //cat
    alert(pattern2.lastIndex);      //3
    
    matches = pattern2.exec(text);
    alert(matches.index);           //5
    alert(matches[0]);              //bat
    alert(matches.lastIndex);       //8

pattern2是全局模式，因此每次调用exec()都会返回字符串中的下一个匹配项，直至搜索到字符串末尾为止。
同时在全局模式下，lastIndex的值在每次调用exec()后都会增加，而在非全局模式下保持不变。

test()：接受一个字符串参数。匹配是返回true，否则false。

    var text = "000-00-0000";
    var pattern = /\d{3}-\d{2}-\d{4}/;
    
    if(pattern.test(text)){
        alert("the pattern was matched.");
    }

RegExp竭诚toLacaleString()和toString()都会返回正则表达式的字面量，与创建方式无关。

    var pattern = new RegExp("\\[bc\\]at", "gi");
    alert(pattern.toString());          //  /\[bc\]at/gi
    alert(pattern.toLacaleString());    //  /\[bc\]at/gi


RegExp构造函数属性
这些属性适用于作用域中所有正则表达式，并且给予所执行的最近一次正则表达式操作而变化。  
* input  $_  最近一次要匹配的字符串
* lastMatch   $&   最近一次的匹配项
* lastParen   $+   最近一次匹配的捕获组
* leftContext   $`   input字符串中lastMatch之前的文本
* multiline   $*   布尔值，表示是否所有表达式都使用多行模式
* regitContext   $'   Input字符串中LastMatch之后的文本

    var text = "this has been a short summer";
    var pattern = /(.)hort/g;
    
    //注意：Opera不支持input、lastMatch、lastParen和multilne属性
    //IE不支持multiline属性
    if (pattern.test(text)){
        alert(RegExp.input);            //this has been a short summer
        alert(RegExp.leftContext);      //this has been a
        alert(RegExp.rightContext);     //summer
        alert(RegExp.lastMatch);        //short
        alert(RegExp.lastParen);        //s
        alert(RegExp.multiline);        //false
    }

    var text  "this has been a short summer";
    var pattern = /(.)hort/g;
    
    if(pattern.test(text)){
        alert(RegExp.$_);               //this has been a short summer
        alert(RegExp["$`"]);            //this has been a
        alert(RegExp["$'"]);            //summer
        alert(RegExp["$&"]);            //short
        alert(RegExp["$+"]);            //s
        alert(RegExp["$*"]);            /false
    }

除此以外还有九个用于村粗捕获组的够赞函数属性。分别用于存储第一到第九个匹配的捕获组，在调用exec()/test()是，这些属性会被自动填充。
    
    var text = "this has been a short summer";
    var pattern  = /(..)or(.)g;
    
    if(pattern.test(text)){
        alert(RegExp.$1);           //sh
        alert(RegExp.$2);           //t
    }


模式的局限性：
ECMAScript值不支持一些高级正则表达式的特性。
* 匹配字符串开始和结尾的\A和\Z锚
* 向后查找（lookbehind）
* 并集和交集类
* 原子组（atomic grouping)
* Unicode支持（单个字符除外，如\uFFFF）
* 命名的捕获组
* s（single,单行）和x（free-spacing,无间隔）匹配模式
* 条件匹配
* 正则表达式注释


#### Function 类型

函数即为对象，每个函数都是Function类型的实例，都与其他引用类型一样具有属性和方法。函数名实际上也即是一个纸箱函数对象的指针，不会与某个函数绑定。

    function sum(num1, num2){
        return num1 + num2;
    }
    ===
    var sum = function(num1, num2){
        return num1 + num2;
    };

    var sum = new Function("num1", "num2", "return num1+num2"); //不推荐


函数名仅仅是纸箱函数体的指针，一个函数可能有多个名字

    function sum(num1 , num2){
        return num1 + num2;
    }
    alert(sum(10, 10));         //20
    
    var anotherSum = sum;
    alert(anotherSum(10, 10));  //20
    
    sum = null;
    alert(anotherSum(10, 10));  //20


没有重载：
ECMAScript中没有重载的概念
    
    var addSomeNumber = function (num){
        return num + 100;
    };
    
    addSomeNumber = function(num){
        return num + 200;
    };
    
    var result = addSomeNumber(100);        //300

后创建的函数会覆盖先创建的函数。


函数声明与函数表达式：

解析器在想执行环境中加载数据时，对函数声明和函数表达式处理不同。解析器会率先读取函数声明，并使其在执行任何代码之前可用；至于函数表达式，则必须等到解析器执行到它所在的代码行，才会真正被解释执行。
    
    alert(sum(10, 10));
    function sum(num1, num2){
        return num1 + num2;
    }

可以正常运行，代码在开始执行之前，通过一个函数声明提升（function declaration hoisting）的过程，读取并将函数声明添加到执行环境中。

    alert(sum(10, 10));
    var sum = function(num1 , num2){
        return num1 + num2;
    };
以上代码在运行期间产生错误，因为函数位于一个初始化语句中，而不是一个函数中。
除了什么时候可以通过变脸高方文函数这一点区别之外，函数声明与函数表达式的语法其实是等价的。


作为值得函数：
因为函数本身就是变量，所以函数可以作为值来使用。也就是说，不仅可以像传递参数一样把一个函数传递给另一个函数，而且可以将一个函数作为另一个函数的结果返回。

    function callSomeFunction(someFunction, someArgument){
        return someFunction(someArgument);
    }

    function add10(num){
        return num + 10;
    }
    
    var result1 = callSomeFunction(add10, 10);
    alert(result1);         //10
    
    function getFreeting(name){
        return "Hello, " + name;
    }
    
    var result2 = callSomeFunction(getFreeting, "HotHunter");
    alert(result2);         //  "Hello, HotHunter"

也可以从一个函数返回另一个函数。

    function createComparisonFunction(propertyName){
        return function(object1, object2){
            var value1 = object1(propertyName);
            var value2 = object2(propertyNmae);
            if (value1 < value2){
                return -1;
            }
            else if(value1 > value2){
                return 1;
            }
            else {
                return 0;
            }
        };
    }

    var data = [{name: "zachary", age: 28}, {name: "Nicholas", age: 29}];
    
    data.sort(createComparisonFunction("name"));
    alert(data[0].name);        //Nicholas
    
    data.sort(createComparisonFunction("age"));
    alert(date[0].name);        //Zachary


函数内部属性：
arguments / this
arguments:类数组对象，包含传入函数的所有参数。包含一个callee的属性，指向拥有这个arguments对象的额函数。
    
    function factorial(num){
        if (num <= 1){
            return 1;
        }else {
            return num * factorial(num - 1);
        }
    }

定义阶乘函数一般都要用到递归算法，上面函数有名字，并且之后名字不会改变的情况下，这样定义么有问题。但是这个函数执行与函数名factoial仅仅耦合。为了消除这种紧密耦合的现象，就可以使用arguments.callee.

    function factorial(num){
        if (num <= 1){
            return 1;
        } else {
            return num * arguments.callee(num - 1);
        }
    }

此时当函数名重写时，都可以保证正常完成递归。

    var truefactorial = factorial;
    factorial = function(){
        return 0;
    }
    alert(trueFactorial(5));
    alert(factorial(5));

this：引用的是函数执行的环境对象，当在网页的去阿奴作用域中调用函数时，this对象阴阳的就是window。

    window.color = "red";
    var o = {color : "blue"};
    
    function sayColor(){
        alert(this.color);
    }
    
    sayColor();     //"red"
    
    o.sayColor = sayColor;
    o.sayColor();       //"blue"

caller：保存着调用当前函数的函数的引用，在全局作用域中调用当前函数，值为null。

    function outer(){
        inner();
    }
    function inner(){
        alert(inner.caller);
    }
    outer();

以上代码将返回outer的函数源代码，为了防止耦合：

    function outer(){
        inner();
    }
    function inner(){
        alert(arguments.callee.caller);
    }
    outer();


函数的属性和方法：
每个函数都包含两个属性：length和prototype
length返回函数参数个数。

    fucntion sayName(name){
        alert(name);
    }
    function sum(num1, num2){
        return num1 + num2;
    }
    function sayHi(){
        alert("hi");
    }
    
    alert(sayName.length);          //1
    alert(sum.length);              //2
    alert(sayHi.length);            //0

prototype：保存它们所有实例方法的真正所在。例如toString()/valueOf()都保存在prototype名下，通过各自对象的实例访问。
每个函数都包含两个非继承的方法。apply()/call()。两个方法的用于都是在特定的作用与众调用函数，实际上等于设置函数体内的this的值。

apply()：接收两个参数：一个是在其中运行函数的作用域，另一个是参数数组，可以是Array实例对象，也可以是arguments对象。

    function sum(num1, num2){
        return num1 + num2;
    }
    function callSum1(num1, num2){
        return sum.apply(this, arguments);
    }
    function callSum2(num1, num2){
        return sum.apply(this, [num1, num2]);
    }
    
    alert(callSum1(10, 10));            //20
    alert(callSum2(10, 10));            //20

call()：与apply的区别在于接收参数不同，第一个参数this没有变化，剩下的参数都直接传递给函数。

    function sum(num1, num2){
        return num1 + num2;
    }
    function callSum(num1, num2){
        return sum.call(this, num1, num2);
    }
    alert(callSum(10, 10));             //20

apply()、call()真正作用在于扩充函数赖以运行的作用域

    window.color = "red";
    var o = {color : "blue"};
    
    function sayColor(){
        alert(this.color);
    }
    
    sayColor();         //red
    
    sayColor.call(this);            //red
    sayColor.call(window);          //red
    sayColor.call(o);               //blue

好处就是，对象不需要与方法有任何耦合关系

bind()：创建一个函数实例，其this值会被绑定到传给bind()函数的值。

    window.color = "red";
    var o = {color : "blue"};
    
    function sayColor(){
        alert(this.color);
    }
    
    var objectSayColor = sayColor.bind(o);
    objectSayColor();           //blue

每个函数继承的toLocaleString()/toString()/valueOf()返回函数代码


#### 基本包装类型

3个特殊的Boolean/Number/String。
与其他引用类型类似，同事也具有哦与各自的基本类型相应的特殊行为。当读取一个基本类型值的时候，后台会创建一个对应的基本包装类型的对象。

    var s1 = "some text";
    var s2 = s1.substring(2);       //me text

上述实际操作：
创建String类型的一个实例；
在实例上调用指定的方法；
销毁这个实例。

    var s1 = new String("some text");
    var s2 = s1.substring(2);
    s1 = null;

引用类型与基本包装类型的主要区别就是对象的生存期。使用new操作符创建的引用类型的实例，在执行流离开当前作用域之前都一直保存在内存中。而自动创建的基本包装类型的对象，则只存在于一行代码的执行瞬间，然后立刻被销毁。意味着不能再运行时为基本类型值添加属性和方法。

    var s1 = "some text";
    s1.color = "red";
    alert(s1.color);        //underfined

在绝对必要的情况下这样做，因为很容易让人分不清自己是在处理基本类型还是引用类型的值。
注意使用new调用基本包装类型的够赞函数，与直接调用同名的转型函数时不一样的。

    var value = "25";
    var number = Number(value);     //转型函数
    alert(typeof number);           //"number"
    
    var obj = new Number(value);
    alert(typeof obj);              //"object"


Boolean类型：

    var booleanObject = new Boolean(true);

    var falseObject =  new Bollean(false);
    var result = falseObject && true;
    alert(result);          //true
    
    var falseValue = false;
    result = falseValue && true;
    alert(result);          //false

基本类型和引用类型的boolean的区别：typeof操作符对基本类型返回"boolean"，对引用类型返回"object"。其次，由于Boolean对象是Boolean类型的实例，所以使用instanceof操作符测试Boolean对象会返回true，而测试基本类型的布尔值则返回false。

    alert(typeof falseObject);          //object
    alert(tyeof falseValue);            //boolean
    alert(falseObject instanceof Boolean);          //true
    alert(falseVlue instanceof Boolean);            //false

建议永远不要使用Boolean对象。


Number类型：

    var numberObject = new Number(10);

    var num = 10;
    alert(num.toString());          //"10"
    alert(num.toString(2));         //"1010"
    alert(num.toString(8));         //"12"
    alert(num.toString(10));        //"10"
    alert(num.toString(16));        //"a"

toFixed()按照指定的小数位返回数值的字符串表示：

    var num = 10;
    alert(num.toFixed(2));          //"10.00"

    var num = 10.005;
    alert(num.toFixed(2));          //"10.01"

toExponential()：返回以指数表示的数值的字符串。

    var num = 10;
    alert(num.toExponential(1));            //"1.0e+1"

toPrecision()：可能返回fixed格式，也可能返回exponential格式。会根据要处理的数值决定是调用toFixed()还是toExponential()。

    var num = 99;
    alert(num.toPrecision(1));          //"1e+2"
    alert(num.toPrecision(2));          //"99"
    alert(num.toPrecision(3));          //"99.00"

与Boolean类似，Number也可以实例化。但不推荐。

    var numberObject = new Number(10);
    var numberValue = 10;
    
    alert(typeof numberObject);         //"object"
    alert(typeof numberValue);          //"number"
    alert(numberObject instanceof Number);      //true
    alert(numberValue instanceof Number);       //false



String类型：

    var stringObject = new String("some text");

用构造函数创建的String类型，继承的valueOf()/toString()/toLocaleString()都返回对象表示的基本字符串值。
String类型的每个实例都有一个length属性，返回字符串中字符数量。

    var stringValue = "hello world";
    alert(stringValue.length);          //"11"

字符方法：

charAt()：接收一个参数，基于0的字符位置，以单字符字符串的形式返回给定位置的那个字符。

    var stringValue = "hello world";
    alert(stringValue.charAt(1));       //"e"

charCodeAt()：接收一个参数，基于0的字符位置，返回字符编码

    var stringValue = "hello world";
    alert(stringValue.charCodeAt(1));       //"101"

另一个访问单个字符的方法：

    var stringValue = "hello world";
    alert(stringValue[1]);          //"e"


字符串方法：

concat()：将一个或多个字符串拼接，返回拼接得到的新字符串。而且同时可以接收任意多个字符串作为参数进行拼接。

    var stringValue = "hello ";
    var result = stringValue.concat("world");
    alert(result);      //"hello world"
    alert(stringValue); //"hello "

    var stringValue = "hello ";
    var result = stringValue.concat("world", "!", " and", " fuck me", "!")
    alert(result);      //"hello world! and fuck me!"
    alert(stringValue); //"hello "

但是在实践中更多使用（+）操作符。简便易行。

slice()/substr()/substring()：截取字符串，不会修改字符串本身

    var str = "hello world";
    alert(str.slice(3));        //"lo world"
    alert(str.substring(3));    //"lo world"
    alert(str.substr(3));       //"lo world"
    alert(str.slice(3, 7));     //"lo w"
    alert(str.subtring(3, 7));  //"lo w"
    alert(str.substr(3, 7));    //"lo worl"

参数存在负数值：

    var str = "hello world";
    alert(str.slice(-3));               //"rld"
    alert(str.substring(-3));           //'hello world'
    alert(str.substr(-3));              //"rld"
    alert(str.slice(3, -4));            //"lo w"
    alert(str.substring(3, -4));        //"hel"
    alert(str.substr(3, -4));           //""



字符串位置方法：

indexOf()/lastIndexOf()：从一个字符串中搜索给定的字符串，返回字符串的位置（没有找到返回-1）。
indexOf()从开头向后搜索，lastIndexOf()从末尾向前搜索。

    var stringValue = "hello world";
    alert(stringValue.indexOf("o"));            //4
    alert(stringValue.lastIndexOf("o"));        //7

两个方法可接收第二个参数：表示从字符串中的哪个位置开始搜索。

    var string = "hello world";
    alert(string.indexOf("0", 6));          //7
    alert(string.lastIndexOf("o", 6));      //4

在使用第二个参数情况下，通过循环调用这两个方法来找到所有匹配的子字符串

    var string = "Lorem ipsum dolor sit amet, comsectetur adipisicing elit";
    var positions = new Array();
    var pos = string.indexOf("e");
    
    while (pos > -1){
        positions.push(pos);
        pos = string.indexOf("e", pos + 1);
    }
    
    alert(positions);       "3,24,32,35,52"


trim()：
创建一个字符串副本，删除前置及后缀的所有空格，然后返回结构。
由于返回字符串副本，所以原始字符串保持不变。
同时还有非标准的:trimLeft()/trimRight()

    var string = "     hello world       ";
    var trimmedString = string.trim();
    alert(string);          //"     hello world       "
    alert(trimmedString);       //"hello world"


字符串大小写转换方法：
toLowerCase()/toLocaleLowerCase()/toUpperCase()/toLocaleUpperCase()

    var string = "hello world";
    alert(string.toLocaleUpperCase());          //"HELLO WORLD"
    alert(string.toUpperCase());                //"HELLO WORLD"
    alert(string.toLocaleLowerCase());          //"hello world"
    alert(string.toLowerCase());                //"hello world"


字符串的模式匹配方法：
match()：在字符串上调用这个方法，本质上与调用RegExp的exec()方法相同。
只接受一个参数，一个正则表达式或者一个RegExp对象

    var text = "cat, bat, sat, fat";
    var pattern = /.at/;
    
    //与pattern.exec(text)相同
    var matches = text.match(pattern);
    alert(matches.index);           //0
    alert(matches[0]);              //"cat"
    alert(pattern.lastIndex);       //0

search()：参数与mathch()的相同，由字符串或RegExp对象指定的一个正则表达式，返回字符串中第一个匹配项的索引；没有找到匹配项，返回-1。

    var text = "cat, bat, sat, fat";
    var pos = text.search(/at/);
    alert(pos);         //1

replace()：替换字符串，两个参数：第一个可以是一个字符串或者一个RegExp对象（这个字符串不会被转换成正则表达式），第二个参数可以是一个字符串或者一个函数。

    var text = "cat, bat, sat, fat";
    var result = text.replace("at", "ond");
    alert(result);          //"cond, bat, sat, fat"
    
    result = text.replace("/at/g", "ond");
    alert(result);          //"cond, bond, sond, fond"

如果第二个参数是字符串，那么可以使用一些特殊的字符序列，将正则表达式操作得到的值插入到结果字符串中。

* $$      ＄
* $&      匹配整个模式的子字符串。=== RegExp.lastMatch
* $'      匹配的子字符串之前的字符串。=== RegExp.leftContext
* $`      匹配的子字符串之后的子字符串。 === RegExp.rightContext
* $n      匹配第n个捕获组的子字符串
* $nn     匹配第nn个捕获组的子字符串。

end
    
    var text = "cat, bat, sat, fat";
    result = text.replace(/(.at)/g, "word ($1)");
    alert(result);      //word (cat), word (bat), word (sat), word(fat)

如果第二个参数是函数，只有一个匹配项的情况下，回向这个函数传递3个参数：模式的匹配项、模式匹配项在字符串中的位置和原始字符串。这个函数返回一个字符串，表示应该被替换的匹配项。

    function htmlEsape(text){
        return text.replace(/[<>"&]/g, function(match, pos, originalText){
            switch(match){
                case "<":
                    return "&lt;";
                case ">":
                    return "&gt;";
                case "&":
                    return "&amp;";
                case "\":
                    return "&quot;";
            }
        });
    }
    alert(htmlEscape("<p class=\"greeting\">Hello world!</p>"));
        //&lt;p class=&quot;greeting&quot;&gt;Hello world!&lt;/p&gt;

以上代码实现：使用正则表达式查找字符，然后定义一个能够针对每个匹配的字符餐返回特定HTML实体的函数。

split()：基于制定的分隔符将一个字符串分割成多个字符串，放在一个数组中。分隔符可以是字符串，也可以是一个RegExp对象（但不会讲字符串看成正则）。可以接收第二个参数：用于制定数组的大小，防止返回的数组超过既定大小。

    var colorText = "red,blue,green,yellow";
    var colors1 = colorText.split(",");    //["red", "blue", "green", "yellow"]
    var colors2 = colorText.split(",", 2);  //["red", "blue"]
    var colors3 = colorText.split(/[^\,]+/);    //["", ",", ",", ",", ""]


localeCompare()：比较两个字符串，返回：
如果字符串在字母表中排在字符串参数之前，返回一个负数。
如果字符串等于字符串参数，返回0；
如果字符串在字母表中应该排在字符串参数之后，返回一个正数

    var str = "yellow";
    alert(str.localeCompare("brick"));          //1
    alert(str.localeCompare("yellow"));         //0
    alert(str.localeCompare("zoo"));            //-1

localeCompare()返回的数值取决于实现

    fucntion determineOrder(value){
        var result = str.localeCompare(value);
        if (result < 0){
            alert("the string 'yellow' comes before the string '" +　value + "' .");
        } else if (result > 0){
            alert("the string 'yellow' comes after the string '" +　value + "' .");
        } else {
            alert("the string 'yellow' is equal to the string '" +　value + "' .");
        }
    }
    determineOrder("brick");
    determineOrder("yellow");
    determineOrder("zoo");


fromCharCode()：接收一个或多个字符编码，转换成一个字符串。从本质上看，这个方法与实例方法charCodeAt()执行的是相反的操作。

    alert(string.fromCharCode(104, 101, 108, 111));     //"hello"


HTML方法：
不怎么用，P130自己翻书看吧。


#### 单体内置对象

已经被实例化，不需要显示地实例化。除了Object、Array、String
还有Global和Math

Gloal:
不属于任何其他对象的属性和方法，最终都是它的属性和方法。所有在全局作用域中定义的属性和函数，都是Global的属性。例如isNaN()/isFinite()/parseInt()/parseFloat()

URI编码方法：
encodeURI()和encodeURIComponent()可以对URI编码，不能包含空格
encodeURI()主要用于整个URI，不对本身属于URI的特殊字符编码，冒号，正斜杠，问号和#
encodeURIComponent()用于对URI中的某一段进行编码。对它发现的任何非标准字符进行编码

    var uri = "http://www.wrox.com/illegal value.htm#start";
    
    //"http://www.wrox.com/illegal%20value.htm#start"
    alert(encodeURI(uri));
    
    //"http%3A%2F%2Fwww.wrox.com%2Fillegal%20value.htm%23start"
    alert(encodeURIComponent(uri));

与上面两个方法对应的，分别为：
decodeURI()/decodeURIComponent()

    var uri = "http%3A%2F%2Fwww.wrox.com%2Fillegal%20value.htm%23start";
    
    //http%3A%2F%2Fwww.wrox.com%2Fillegal value.htm%23start
    alert(decodeURI(uri));
    
    //http://www.wrox.com/illegal value.htm#start
    alert(decodeURIComponent(uri));


eval()：
类似一个完整的ECMAScript解析器，只接受一个参数。要执行的javascript字符串

    var ("alert('hi')");
    ==
    alert("hi");

    var msg = "hello world";
    eval("alert(msg)");         //"hello world"

    eval("function sayHi(){alert('hi');}");
    sayHi();

    eval("var msg = 'hello world'");
    alert(msg);

在eval()创建的任何变量或者函数都不会被提升，因为在解析代码的时候，被好汉在一个字符串中；只在eval()执行时被创建。


global对象的属性：
undefined / NaN / Infinity / Object / Array / Function / Boolean / String / Number / Date / RegExp / Error / EvalError / RangeError / ReferenceError / SyntaxError / TypeError / URIError
明确禁止将undefined/NaN/Infinity赋值


window对象：
虽然无法直接访问Global对象，但Web浏览器都将这个全局对象作为window兑现给定而一部分加以实现。
    
    var color = "red";
    function sayColor(){
        alert(window.color);
    }
    window.sayColor();          //"red"

另一种取得Global对象的方法：

    var global = function(){
        return this;
    }();


Math:
对象属性：
* Math.E            //自然对数的底数
* Math.LN10         //10的自然对数
* Math.LN2          //2的自然对数
* Math.LOG2E        //以2为底e的对数
* Math.LOG10E       //以10为底e的对数
* Math.PI           //π的值
* Math.SORT1_2      //1/2的平方根（2的平方根的倒数）
* Math.SQRT2        //2的平方根

<b>虽然讨论这些值的含义和用途超出了本书的范围，但你确实可以随时使用它们</b>

min()和max():
确定最大值和最小值。可以接收任意多个参数。

    var max = Math.max(3, 54, 32, 16);
    alert(max);         //54
    
    var min = Math.min(3, 54, 32, 16);
    alert(min);         //3

    var values = [1, 2, 3, 4, 5, 6, 7, 8];
    var max = Math.max.apply(Math, values);

把Math对象作为apply()的第一个参数，从而正确设置this。便可以将任何数组作为第二个参数。

舍入方法：
Math.ceil():向上舍入，总是将数值向上舍入为最接近的整数
Math.floor()：向下舍入，总是将数值向下舍入为最接近的整数
Math.round()：四舍五入为最接近的整数。

    alert(Math.ceil(25.9));         //26
    alert(Math.ceil(25.6));         //26
    alert(Math.ceil(25.1));         //26
    
    alert(Math.floor(25.9));        //25
    alert(Math.floor(25.6));        //25
    alert(Math.floor(25.1));        //25
    
    alert(Math.round(25.9));        //26
    alert(Math.round(25.6));        //26
    alert(Math.round(25.1));        //25


random()：

Math.random()返回大于等于0 小于1的一个随机数。有公式：

    值 = Math.floor(Math.random() * 可能值的总数 + 第一个可能值);

    var num = Math.floor(Math.random() * 10 + 1);    

    function selectFrom(lowerValue, upperValue){
        var choices = upperValue - lowerValue + 1;
        return Math.floor(Math.random() * choices + lowerValue);
    }
    
    var num = selectFrom(2, 10);
    alert(num);         //介于2和10之间（包括2和10）的一个随机数

    var colors = ["red", "green", "blue", "yellow", "blace", "purple", "brown"];
    var color = colors[selectFrom(0, colors.length-1)];
    alert(color);           //可能是数组中包含的任何一个字符串

其他方法：
P136，一些高大上的Math方法

---


### 面向对象的程序设计

面向对象类语言的共同特点：就是都有类的概念。基于类以创建任意多个具有相同属性和方法的对象。
对象：无需属性的集合，起属性可以包含基本值、对象或者函数。
每个对象都基于宇哥因引用类型创建，可以是原生类型，也可是自定义类型

#### 理解对象

名为person的对象，拥有三个属性（name/age/job），一个方法(sayName):

    var person = new Object();
    person.name = "hothunter";
    person.age = 29;
    person.job = "software engineer";       //niubi
    
    person.sayName = function(){
        alert(this.name);
    }

    var person = {
        name : "hothunter",
        age : 29,
        job : "software engineer",
        sayName : function(){
            alert(this.name);
        }
    };


属性类型：
包含一个数据值的位置。可以读取和写入。
* [[Configurable]]:能否通过delete删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性。
* [[Enumerable]]:能否通过for-in循环返回属性。
* [[Writable]]:能否修改属性的值。
* [[Value]]:包含这个属性的数据值

修改属性默认特性：Object.defineProperty()。三个参数：属性所在对象、属性名、描述符对象（configurable/enumerable/writable/value）。

    var person = {};
    Object.defineProperty(person, "name", {
        writable: false,            //该name属性在此处被设置为只读
        value: "hothunter"
        });
    
    alert(person.name);         //"hothunter"
    person.name = "lianhui";
    alert(person.name);         //"hothunter"

    var person = {};
    Object.defineProperty(person, "name", {
        configurable: false,            //在此处被设置为不可删除
        value: "hothunter"
        });
    
    alert(person.name);         //"hothunter"
    delete person.name;         
    alert(person.name);         //"hothunter"


访问器属性：
getter/setter函数。读取和写入访问器属性时，分别调用这两个函数。
访问器属性特性：
* [[Configurable]]:表示能否通过delete删除属性从而重新定义属性。
* [[Enumerable]]:表示能否通过for-in 循环返回属性。
* [[Get]]:在读取属性时调用的函数。默认undefined
* [[Set]]:在写入属性时调用的函数。默认undefined

仍然需要使用Object.defineProperty()定义
    
    var book = {
        _year = 2015,
        edition: 1
    };
    Object.defineProperty(book, "year", {
        get: function(){
            return this._year;
        },
        set: function(newValue){
            if(newValue){
                this._year = newValue;
                this.edition += newValue - 2015;
            }
        }
        });
    
    book.year = 2016;
    alert(book.edition);            //2

设置一个属性的值会导致其他属性发生变化。

定义多个属性：
Object.defineProperties():通过描述符一次定义定义多个属性。两个参数：要添加和修改其属性的对象；第二个对象的属性与第一个对象中要添加或修改的属性一一对应。

    var book = {};
    Object.defineProperties(book, {
        _year : {
            writable : true;
            value : 2004
        },
        edition : {
            writable : true,
            value : 1
        }，
        year : function(){
            get: function(){
                return this._year;
            },
            set : function(newValue){
            if(newValue > 2004) {
                this._year = newValue;
                this.edition += newValue - 2004;
            }
        }
    }  
    });


读取属性的特性：

Object.getOwnPropertyDescriptor()：取得给定属性的描述符。两个参数：所在的对象和要读取其描述符的属性名称。返回一个对象。

    var book = {};
    Object.defineProperties(book , {
        _year : {
            value : 2004
        },
        edition : {
            value : 1
        },
        year : {
            get : function(){
                return this._year;
            },
            set : function(newValue){
                if (newValue > 2004) {
                    this._year = newValue;
                    this.edition += newValue - 2004;
                }
            }
        }
        });
    
    var descrioptor = Object.getOwnPropertyDescriptor(book, "_year");
    alert(descriptor.value);                //2004
    alert(descriptor.configurable);         //false
    alert(typeof descriptor.get);           //"undefined"
    
    var descriptor = Object.getOwnPropertyDescriptor(book, "year");
    alert(descriptor.value);                //undefined
    alert(descriptor.configurable);         //false
    alert(typeof descriptor.get);           //"function"


#### 创建对象

之前创建对象的缺点：使用同一个接口常见很多对系那个，会产生大量的重复代码。

工厂模式：

抽象创建具体对象的过程。

    function createPerson(name, age, job){
        var o = new Object();
        o.name = name;
        o.age = age;
        o.job = job;
        o.sayName = function(){
        alert(this.name);
    };
    return o;
    }
    
    var person1 = createPerson("hothunter", 23, "software enginner");
    var person2 = createPerson("lianhui", 9, "doubi");

缺点：无法解决对象识别的问题（即怎样知道一个对象的类型）

构造函数模式：

以下代码经历四个步骤：
1. 创建一个新对象；
2. 将构造函数中的作用域给新对象（因此this指向新对象）；
3. 执行构造函数中的代码（为新对象添加属性）；
4. 返回新对象。

    function Person(name, age, job){
        this.name = name;
        this.age = age;
        this.job = job;
        this.sayName = function(){
            alert(this.name);
        };
    }
    
    var person1 = new Person("hothunter", 23, "noob");
    var person2 = new Person("lianhui", 9, "motherfucker");

与之前createPerson()相比，没有显示创建对象；直接将属相和方法赋给了this；没有return。
够赞函数始终都应该以一个大写字母开头，非构造函数应该以一个小写字母开头。

    alert(person1 instanceof Object);               //true
    alert(person1 instanceof Person);               //true
    alert(person2 instanceof Object);               //true
    alert(person2 instanceof Object);               //true

将构造函数当做函数：任何函数，只要通过new操作符来调用，那它就可以作为构造函数；如果不能通过new操作符调用，那和普通函数也没什么区别。

    //当做构造函数
    var person = new Person("hothunter", 23, "software engineer");
    person.sayName();               //"hothutner"
    
    //作为普通函数
    Person("hothunter", 23, "fucker");          //添加到window
    window.sayName();           //"hothunter"
    
    //在另一个对象的作用域中调用
    var o = new Object();
    Person.call(o, "Hothunter", 23, "fucker");
    o.sayName();            //"hothunter"

构造函数的问题:
每个方法都要在每个实例上重新创建一遍。
函数时对象因此每定义一个函数，也就是实例化了一个对象。

    function Person(name, age, job){
        this.name = name;
        this.age = age;
        this.job = job;
        this.sayName = new Function("alert(this.name)");    
        //与声明函数在逻辑上等价
    }
    
    alert(person1.sayName == person2.sayName);          //false
    //以此方式创建函数，会导致不同的作用域链和标识符解析，但创建Function新实例的机制任然相同。因此不同实例上的听命函数时不相等的。

    function Person(name, age, job){
        this.name = name;
        this.age = age;
        this.job = job;
        this.sayName = sayName;
    }
    function sayName(){
        alert(this.name);
    }
    
    var person1 = new Person("hothunter", 23, "fucker");
    var person2 = new Person("lianhui", 9, "doubi");

以上方法，由于sayName包含的是一个指向函数的指针，因此person1和person2共享了在全局作用域中定义的同一个sayName()函数。
但问题是如果需要方法很多，就需要在全局环境创建多个全局函数，繁琐的同时会失去函数的封装性。

原型模式(prototype)：

prototype这个属性是一个指针，指向一个对象，而这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法。prototype通过调用构造函数而创建的那个对象实例的原型对象。使用原型对象的好处是可以让所有对象实例共享它所包含的属性和方法。

    function Person(){
    }
    
    Person.prototype.name = "hothunter";
    Person.prototype.age = 23;
    Person.prototype.job = "fucker";
    Person.prototype.sayName = function(){
        alert(this.name);
    };
    
    var person1 = new Person();
    person1.sayName();          //"hothunter"
    
    var person2 = new Person();
    person2.sayName();          //"hothunter"
    
    alert(person1.sayName == person2.sayName);          //ture

理解原型对象：

无论什么时候，只要创建了一个新函数，就会根据一组特定的规则为该函数创建一个prototype属相，这个属性指向函数的原型对象。当调用构造函数创建一个新实例后，该实例的内部将包含一个指针，指向构造函数的原型对象。即[[prototype]]。虽然无法直接访问，但各个浏览器都支持一个属性：__proto__。
这个连接存在于实例与构造函数的原型对象之间，而不是存在于实例与构造函数之间。

虽然所有实现中都无法访问[[prototype]]，但可以通过isPrototypeOf()方法确定对象之间是存在这种关系。

    alert(Person.prototype.isPrototypeOf(person1));     //true
    alert(Person.prototype.isPrototypeOf(person2));         //true

Object.getPrototypeOf():返回[[prototype]]的值

    alert(Object.getPrototypeOf(person1) == Person.prototype);      //true
    alert(Object.getPrototypeOf(person2).name);     "hothunter"


如果在实例中添加了一个属性，而该属性与实例原型中的一个属性同名，那就在实例中创建该属性，该属性将会屏蔽原型中的那个属性。

    function Person(){}
    
    Person.prototype.name = "hothunter";
    Person.prototype.age = 23;
    Person.prototype.job = "asdf";
    Person.prototype.sayName = function(){
        alert(this.name);
    };
    
    var person1 = new Person();
    var person2 = new Person();
    
    person1.name = "lianhui";
    alert(person1.name);        //"lianhui"
    alert(person2.name);        //"hothunter"

使用delete可以完全删除实例属性，从而让我们能够重新访问原型中的属性。

    function Person(){}
    
    Person.prototype.name = "hothunter";
    Person.prototype.age = 23;
    Person.prototype.job = "asdf";
    Person.prototype.sayName = function(){
        alert(this.name);
    };
    
    var person1 = new Person();
    var person2 = new Person();
    
    person1.name = "lianhui";
    alert(person1.name);        //"lianhui"     --  来自实例
    alert(person2.name);        //"hothunter"       --  来自原型
    
    delete person1.name;
    alert(person1.name);        //"hothunter"       --  来自原型

hasOwnProperty():检测一个属性存在于实例中，还是原型中。只在给定属性存在于对象实例中时，才返回true

    function Person(){}
    
    Person.prototype.name = "hothunter";
    Person.prototype.age = 23;
    Person.prototype.job = "asdf";
    Person.prototype.sayName = function(){
        alert(this.name);
    };
    
    var person1 = new Person();
    var person2 = new Person();
    
    alert(person1.hasOwnProperty("name"));      //false
    
    person1.name = "lianhui";           
    alert(person1.name);        //"lianhui" --来自实例
    alert(person1.hasOwnProperty("name"));      //true
    
    alert(person2.name);        //"hothunter"   --来自原型
    alert(person1.hasOwnProperty("name"));      //false
    
    delete person1.name;
    alert(person1.name);        //"hothunter"   --来自原型
    alert(person1.hasOwnProperty("name"));      //false


原型与in操作符

两种使用in：单独使用、在for-i循环中使用。

    function Person(){}
    
    Person.prototype.name = "hothunter";
    Person.prototype.age = 23；
    Person.prototype.job = "jsfourt";
    Person.sayHello = function(){
        alert(this.name);
    };
    
    var person1 = new Person();
    var person2 = new Person();
    
    alert(person1.hasOwnProperty("name"));          //false
    alert("name" in person1);            //true
    
    person1.name = "lianhui";
    alert(person1.name);            //"lianhui"--来自实例
    alert(perosn1.hasOwnProperty("name"));          //true
    alert("name" in person1);            //true
    
    alert(person2.name);            //"hothunter"--来自原型
    alert(person2.hasOwnProperty("name"));          //false
    alert("name" in person2);           //true
    
    delete person1.name;
    alert(person1.name);            //"hothunter"--来自原型
    alert(person1.hasOwnProperty("name"));          //false
    alert("name" in person1);           //true

无论属性存在于实例中还是存在于原型中，调用"name" in person都返回true。
同时使用hasOwnProperty()就能确定属性存在于实例中还是原型中。

    function hasPrototypeProperty(object, name){
        return !object.hasOwnProperty(name) && (name in object);
    }

    function Person(){}
    
    Person.prototype.name = "hothunter";
    Person.prototype.age = 23；
    Person.prototype.job = "jsfourt";
    Person.sayHello = function(){
        alert(this.name);
    };
    
    var person = new Person();
    alert(hasPrototypeProperty(person, "name"))         //true
    
    person.name = "doubi";
    alert(hasPrototypeProperty(person, "name"));        //false

使用for-in循环时，返回的是所有嫩巩固通过对象访问、可枚举的（enumerated）属性，包括存在于实例、原型中的属性。屏蔽了原型中不可枚举的属性（[[Enumerable]]被标记为false）的实例也会被循坏。

    var o ={
        toString: function(){
            return "My object";
        }
    };
    for (var prop in o){
        if (prop == "toString"){
            alert("Found toString");
        }
    }

要取得对象说有可枚举的实例属性，可以使用Object.keys()方法。接受一个对象作为参数，返回一个包含所有可枚举属性的字符串数组。

    function Person(){}
    
    Person.prototype.name = "hothunter";
    Person.prototype.age = 23；
    Person.prototype.job = "jsfourt";
    Person.sayHello = function(){
        alert(this.name);
    };
    
    var keys = Object.keys(Person.prototype);
    alert(keys);                //"name,age,job,sayName"
    
    var p1 = new Person();
    p1.name = "Rob";
    p1.age = 31;
    var p1keys = Object.keys(p1);
    alert(p1keys);              //"name,age"


如果要得到所有实例属性，无论是否可枚举，可以使用Object.getOwnPropertyNames()

    var keys = Object.getOwnPropertyNames(Person.prototype);
    alert(keys);            //"constructor,name,age,job,sayName"


为了减少不必要的输入，也为了从视觉上更好的封装原型功能，更常见的做法是用一个包含所有属性和方法的对象字字面量来重写整个原型对象。

    function Person(){}
    
    Person.prototype = {
        name : "hothunter",
        age : 23,
        job : "student"
        sayName : function(){
            alert(this.name);
        }
    };

但此时使用constructor已经无法确定对象的类型。本质上重写了默认的prototype对象，因此constructor属性也就变成了新对象的constructor(指向Object构造函数)

    var friend = new Person();
    
    alert(friend instanceof Object);            //true
    alert(friend instanceof Person);            //true
    alert(friend.constructor == Person);        //false
    alert(friend.constructor == Object);        //true

如果constructor的值真的很重要，则可以：

    function Person(){}
    
    Person.prototype = {
        constructor : Person,
        name : "hothunter",
        age : 23,
        job : "student"
        sayName : function(){
            alert(this.name);
        }
    };

默认情况下constructor属性不可被枚举，可以使用Object.defineProperty();

    function Person(){}
    
    Person.prototype = {
        name : "hothunter",
        age : 23,
        job : "student"
        sayName : function(){
            alert(this.name);
        }
    };
    //重设构造函数，只适用于ECMAScript5兼容的浏览器
    Object.defineProperty(Person.prototype, "constructor", {
        enumerable : false,
        value : Person
        });

原型的动态性：由于在原型中查找值得过程是一次搜索，因此对原型对象所做的任何修改都能够立即从实例反应（即使是先创建了实例后修改原型也如此）

    var friend = new Person();
    
    Person.prototype.sayHi = function(){
        alert("Hi");
    };
    friend.sayHi();         //"Hi"

可以随时为原型添加属性和方法，并且修改能够立即在所有对象实例中反映出来，但如果是重写整个原型对象，情况就会不同。
实例中的指针仅指向原型，而不指向构造函数

    function Person(){}
    
    var friend = new Person();
    
    Person.prototype = {
        constructor: Person,
        name : "hothunter",
        age : 23,
        job : "study",
        sayName : function(){
            alert(this.name);
        }
    };
    
    friend.sayName();           //error

上面例子首先创建Person实例，然后又重写了原型。在调用friend.sayName()发生错误，因为friend指向的二元性中不包含以改名字命名的属性


所有原生引用类型（Object/Array/String等）都在其构造函数的原型上定义了方法。

    alert(typeof Array.prototype.sort);             //"function"
    alert(typeof String.prototype.substring);       //"function"

通过原生对象的原型，可以取得所有默认方法的引用，而且也可以自定义新方法。可以像修改自定义对象的原型一样修改原对象的原型 ，因此可以随时添加方法。

    String.prototype.startsWith = function (text) {
        return this.indexOf(text) == 0;
    };
    
    var msg = "Hello world";
    alert(msg.startsWith("Hello"));     //true

当前环境中所有的字符串都可以调用startWith()方法。

原型对象的问题：
首先，省略了为构造函数传递初始化参数这一环节，结果所有实例会默认情况下都将取得相同的属性值。虽然会在某种程度上带来一些不方便，但原型模式的最大问题是由其共享的本性所导致的。
原型中所有属性被很多实例共享，这种共享对于函数非常合适。通过在实例上添加一个同名属性，可以隐藏原型中对应属性。然而对于包含引用类型值来说问题就突出了

    function Person(){}
    
    Person.prototype = {
        constructor : Person,
        name : "hothunter",
        age : 23
        friends : ["lianhui", "doubi"],
        sayName : function(){
            alert(this.name);
        }
    };
    
    var person1 = new Person();
    var person2 = new Person();
    
    person1.friends.push("Van");
    
    alert(person1.friends);         //"lianhui,doubi,Van"
    alert(person2.friends);         //"lianhui,doubi,Van"
    alert(person1.friends === person2.friends);     //true

实例一般都要有属于自己的全部属性。这个问题正是很少看到有人单独使用原型模式的原因。


组合使用构造函数模式和原型模式：

构造函数模式用于定义实例属性，而原型模式用于定义方法和共享的属性。每个实例都会有自己的一份实例属性的副本，同时又共享对方法的引用。同时还支持向构造函数产地参数。

    function Person(name, age, job){
        this.name = name;
        this.age = age;
        this.job = job;
        thi.friends = ["fri1", fri2];
    }
    
    Person.prototype = {
        constructor : Person,
        sayName : function(){
            alert(this.name);
        }
    }
    
    var person1 = new Person("hothunter", 23, "student");
    var person2 = new Person("lianhui", 9, "doubi");
    
    person1.friends.push("fri3");
    alert(person1.friends);         //"fri1,fri2,fri3"
    alert(person2,firends);         //"fri1,fri2"
    alert(person1,firends === person2.friends);     //false
    alert(person1.sayName === person2.sayName);     //true

这种够赞函数与原型混成的模式，是目前默认的定义引用类型的模式。


动态原型模式：

把所有信息都封装在构造函数中，而通过在构造函数中初始化原型，又保持了同事使用构造函数和原型的有点。
通过检查某个应该存在的方法是否有效，来决定是否需要初始化原型。

    function Person(name, age, job){
        //属性
        this.name = name;
        this.age = age;
        this.job = job;
        //方法
        if (typeof this.sayName != "function"){
            Person.prototype.sayName = function(){
                alert(this.name);
            };
        }
    }
    
    var friend = new Person("hothunter", 23, "study");
    friend.sayName();

这段代码只会在初次调用构造函数式才会执行。伺候原型已经完成初始化，不需要再做什么修改。但这里对原型所做的修改，能够立即在所有实例中得到反映。if语句煎炒的可以是初始化之后应该存在的任何属性或方法--不必用一大堆if语句检查每个属性和每个方法；只需要一个即可。对于采用这种模式创建的对象，还可以使用instanceof判断。


计生构造函数模式：
寄生（parasitic）构造模式的基本思想是创建一个函数，作用仅仅是封装创建对象的代码，然后再返回新创建的对象。

    function Person(){
        var o = new Object();
        o.name = name;
        o.age = age;
        o.job = job;
        o.sayName = function(){
            alert(this.name);
        };
        return o;
    }
    
    var friend = new Person("hothunter", 23, "study");
    friend.sayName();       //"hothunter"

这个模式可以在特殊的额情况下用来为对象创建构造函数。假设常见一个具有额外方法的特殊数组。由于不能直接修改Array，因此：

    function SpecialArray(){
        //创建数组
        var values = new Array();
        //添加值
        value.push.apply(values, arguments);
        //添加方法
        value.toPipedString = function(){
            return this.join("|");
        };
        //返回数组
        return values;
    }
    
    var colors = new SpecialArray("red", "blue", "green");
    alert(colors.toPipedString());      //"red|blue|green"

需要说明：首先，返回的对象与构造函数或者与构造函数的原型属性之间没有关系；也就是说构造函数返回的对象与在构造函数外部创建的对象没有不同。不能依赖instanceof操作符来确定对象类型。


稳妥构造函数模式：

稳妥对象（durable objects）：指没有公共属性，而且其方法也不引用this对象。同时也不使用new

    function Person(name, age, job){
        //创建要返回的对象
        var o = new Object();
        //可以再这里定义私有变量和函数
        //添加方法
        o.sayName = function(){
            alert(name);
        };
        //返回对象
        return o;
    }
    
    var friend = new Person("hothunter", 23, "study");
    friend.sayName();               //"hothunter"

除了调用sayName方法，么有别的方式可以访问数据成员。即使有其他代码会给这个而对象添加方法或者数据成员，蛋液不可能有别的而方法啊访问传入到构造函数中的原始数据


#### 继承

ECMAScript只支持实现继承，实现继承主要是依靠原型链实现。

原型链：

利用原型让一个引用类型继承另一个引用类型的属性和方法。
每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针。使原型对象等于另一个类型的实例，显然此时的原型对象见更包含一个纸箱另一个原型的指针，相应的，另一个原型中也包含着一个指向另一个构造函数的指针。假如另一个原型又是另一个类型的实例，上述关系依然成立，构成了实例与原型的链条。。

    function SuperType(){
        this.property = true;
    }
    
    SuperType.prototype.getSuperValue = function(){
        return this.property;
    }
    
    function SubType(){
        this.subproperty = false;
    }
    
    //继承了SuperType
    SubType.prototype = new SuperType();
    
    SubType.prototype.getSubValue = function(){
        return this.subproperty;
    };
    
    var instance = new SubType();
    alert(instance.getSuperValue());            //true

原来存在于SuperType是所有属性和方法，现在也存在于SubType.prototype中。

所有函数的默认原型都是Object的实例，因此默认原型都会包含一个内部指针，指向Object.prototype。也整死所有自定义类型都会继承toString()、vlaueOf()等默认方法的根本原因。

一句话，SubType继承了SuperType，而SuperType继承了Object。当调用instance.toString()时，实际上调用的是保存在Object.prototype中的那个方法。

确定原型和实例的关系：
两种方式确实能够原型和实例之间的关系。
第一种使用instanceof，只要用这个操作符来测试实例与原型链中出现过的构造函数，结果就会返回true

    alert(instance instanceof Object);          //true
    alert(instance instanceof SuperType);       //true
    alert(instance instanceof SubType);         //true

第二种使用isPrototypeOf()方法，与上面原理相同

    alert(Object.prototype.isPrototypeOf(instance));            //true
    alert(SuperType.prototype.isPrototypeOf(instance));         //true
    alert(SubType.prototype.isPrototypeOf(instance));           //true


谨慎定义方法：
子类型有时候需要覆盖超类型中的某个方法，或者需要添加超类型中不存在的某个方法。但不管怎样，给原型添加方法的代码一定要放在替换原型的语句之后。
重写后的方法会屏蔽原来的方法。

    function SuperType(){
        this.property = true;
    }
    
    SuperType.prototype.getSuperValue = function(){
        return this.property;
    }
    
    function SubType(){
        this.subproperty = false;
    }
    
    //继承了SuperType
    SubType.prototype = new SuperType();
    
    //添加了新方法
    SubType.prototype.getSubValue = function(){
        return this.subproperty;
    };
    
    //重写超类型中的方法
    SubType.prototype.getSuperValue = function(){
        return false;
    };
    
    var instance = new SubType();
    alert(instance.getSuperValue());            //false

通过原型链实现继承时，不能使用对象字面量创建原型方法。这样做就会重写原型链。

    function SuperType(){
        this.property = true;
    }
    
    SuperType.prototype.getSuperValue = function(){
        return this.property;
    }
    
    function SubType(){
        this.subproperty = false;
    }
    
    //继承了SuperType
    SubType.prototype = new SuperType();
    
    //使用字面量添加新方法，会导致上一行代码无效
    SubType.prototype = {
        getSubValue : function (){
            return this.subproperty;
        },
        someOtherMethod : function(){
            return false;
        }
    };
    
    var instance = new SubType();
    alert(instance.getSuperValue());            //error!

原型链的问题：
在通过原型来实现继承时，原型实际上会变成另一个类型的实例。。原先的实例属性也就变成了现在的原型属性了。

    function SuperType(){
        this.colors = ["red", "blue", "green"];
    }
    
    function SubType(){
    }
    
    //继承了SuperType
    SubType.prototype = new SuperType();
    
    var instance1 = new SubType();
    instance1.colors.push("black");
    alert(instance1.colors);            //"red,blue,green,black"
    
    var instance2 = new SubType();
    alert(instance2.colors);            //"red,blue,green,black"

第二个问题：在创建子类型的时候，不能先沟槽类型的构造函数中床底参数。实际上应该是没有办法在不影响所有对象实例的情况下，给超类型的构造函数传递参数。

实践中很少会使用原型链。


借用构造函数：
也叫做伪造对象或经典继承。在子类型构造函数的内部调用超类型构造函数。因为函数时在特定环境中执行代码的对象，因此通过apply()/call()可以在新的创建的对象上执行构造函数。

    function SuperType(){
        this.colors = ["red", "blue", "green"];
    }
    
    function SubType(){
        //继承了SuperType
        SuperType.call(this);
    }
    
    var instance1 = new SubType();
    instance1.colors.push("black");
    alert(instance1.colors);            //"red,blue,green,black"
    
    var instance1 = new SubType();
    alert(instance2.colors);            //"red,blue,green"


相对于原型链，急用构造函数有一个喊打的有事，可以再子类型构造函数中项超类型构造函数传递参数。

    function SuperTYpe(name){
        this.name = name;
    }
    
    function SubType(){
        //继承了SuperType，同时传递参数
        SuperType.call(this,"hothunter");
    
        //实例属性
        this.age = 29;
    }
    
    var instance = new SubType();
    alert(instance.name);           //"hothunter"
    alert(instance.age);            //29

但是仅仅使用构造函数模式，函数将无法服用，而且在超类型的原型中定义的方法，对子类型而言也是不可见的，结果是所有的类型都只能使用构造函数模式


组合继承：
将原型链和借用构造函数的技术组合，使用原型链实现对原型属性和方法的竭诚，通过急用构造函数来实现对实例属性的继承。

    function SuperType(name){
        this.name = name;
        this.colors = ["red", "blue", "green"];
    }
    
    SuperType.prototype.sayName = function() {
        alert(this.name);
    }
    
    function SubType(name, age){
        //继承属性
        SupertType.call(this, name);                       ///此处调用
        this.age = age;
    }
    
    //继承方法
    SubType.prototype = new SuperType();                   ///此处调用
    SubType.prototype.constructor = SubType;
    SubType.prototype.sayAge = function(){
        alert(this.age);
    };
    
    var instance1 = new SubType("hothunter", 23);
    instance1.colors.push("balck");
    alert(instance1.colors);            //"red,blue,green,black"
    instance1.sayName();                //"hothunter"
    instance1.sayAge();                 //23
    
    var instance2 = new SubType("doubi", 9);
    alert(instance2.colors);            //"red,blue,green,black"
    instance2.sayName();                //"doubi"
    instance2.sayAge();                 //9

组合式继承最大不足是无论什么情况下，都会调用两次超类型构造函数：一次是在创建子类型原型的时候，另一次是在子类型构造函数内部。


原型式继承：

    function object(o){
        function F(){}
        F.prototype = o;
        return new F();
    }

从本质讲，object()对传入的对象执行了一次浅复制。

    var person = {
        name : "hothunter",
        friends : ["fri1", "fri2", "fri3"]
    };
    
    var anotherPerson = object(person);
    anotherPerson.name = "doubi";
    anotherPerson.friends.push("fri4");
    
    var yetAnotherPerson = object(person);
    yetAnotherPerson.name = "naocan";
    yetAnotherPerson.friends.push("fri5");
    
    alert(person.friends);          //"fri1,fri2,fri3,fri4,fri5"

ECMAScript5新增Object.create()规范化原型式继承。接收两个参数：一个用作新对象原型的对象和（可选的）一个味新兑现规定以额外属性的对象，在传入一个参数的情况下，Object.create()与Object()相同。

    var person = {
        name : "hothunter",
        friends : ["fri1", "fri2", "fri3"]
    };
    
    var anotherPerson = object.create(person);
    anotherPerson.name = "doubi";
    anotherPerson.friends.push("fri4");
    
    var yetAnotherPerson = object.create(person);
    yetAnotherPerson.name = "naocan";
    yetAnotherPerson.friends.push("fri5");
    
    alert(person.friends);          //"fri1,fri2,fri3,fri4,fri5"

    var person = {
        name : "hothunter",
        friends : ["fri1", "fri2", "fri3"]
    };
    
    var anotherPerson = Object.create(person, {
        name : {
            value : "doubi"
        }
        });
    
    alert(anotherPerson.name);          //"doubi"

只想让一个对象与另一个对象保持类似的情况下，原型式继承是完全可以胜任的。但包含应用类型值的属性始终都会共享相应的值，就像使用原型模式一样。


寄生式继承：

创建一个仅用于封装继承过程的函数，该函数在内部已某种方式来增强对象，最后再返回对象。

    function createAnother(original){
        var clone = Object(original);           //通过调用函数创建一个新对象
        clone.sayHi = function(){               //以某种方式增强这个对象
            alert("hi");
        };
        return clone;                           //返回这个对象
    }

可以这样使用：

    var person = {
        name : "hothunter",
        friends : ["fri1", "fri2", "fri3"]
    };
    
    var anotherPerson = createAnother(person);
    anotherPerson.sayHi();          //"hi"

在主要考虑对象而不是自定义类型和构造函数的任何情况下，寄生式继承也可以，object()函数不是必须的，任何能够返回新对象的函数都适用于此模式。


寄生组合式继承：

通过借用构造函数来继承属性，通过原型链的混成形式来继承方法。可以不必为了制定子类型的原型而调用超类型的构造函数。本质上，就是一哟啊那个寄生式继承来继承超类型的原型，然后再将结果指定给子类型的原型：

    function inheritPrototype(subType, superType){
        var prototype = Object(superType, subType);     //创建对象
        prototype.constructor = subType;                //增强对象
        subType.prototype = prototype;                  //指定对象
    }

第一步创建超类型原型的一个副本。第二部为创建的对象复制给子类型的原型。

    function SuperType(name){
        this.name = name;
        this.colors = ["red", "blue", "green"];
    }
    
    SuperType.prototype.sayName = function(){
        alert(this.name);
    }
    
    function SubType(name, age){
        SuperType.call(this, name);
        this.age = age;
    }
    
    inheritPrototype(SubType, SuperType);
    
    SubType.prototype.sayAge = function{
        alert(this.age);
    }

该例子高效率体现在，只调用一次SuperType构造函数，并且因此比秒了在SubType.prototype上面创建不必要的、多余的属性。与此同时，原型链还能保持不变；因此，还能够正常使用instanceof和isPrototypeOf()方法。普遍认为继承组合继承时应用类型最理想的继承方式。


---


### 函数表达式

定义函数的两种方式：函数声明，函数表达式。
函数声明：

    function functionName(arg0, arg1, arg2){
        //函数体
    }

关于函数的一个重要特征：函数声明提升：执行代码前会先读取函数声明。

    sayHi();
    function sayHi(){
        alert("hi");
    }


函数表达式：

    var functionName = function(arg0, arg1, arg2){
        //函数体
    };

看起来像常规的变量赋值语句，即常见一个函数并将它复制给变量functionName。这种情况下创建的函数叫匿名函数（达姆拉函数），function关键字后面没有标识符。
在使用前必须先赋值。一下代码是错误的：

    sayHi();
    var sayHi = function(){
        alert("hi");
    }

另一个区别：

    //这么做是错误的：
    if(condition==1){
        function sayHi(){
            alert("hi");
        }
    }else{
        function sayHi(){
            alert("Yo!");
        }
    }

    //可以这么做：
    var sayHi;
    
    if(condition){
        sayHi = function(){
            alert("hi");
        }
    }else{
        sayHi = function(){
            alert("yo");
        }
    }

能够创建函数再赋值给变量，也就能够把函数作为其他函数的值返回

    function createComparisonFunction(propertyName) {
        return function(object1, object2){
            var value1 = object1[propertyName];
            var value2 = object2[propertyName];
            if (value1 < value2){
                return -1;
            } else if (value1 > value2){
                return 1;
            } else {
                return 0;
            }
        };
    }


#### 递归

递归函数是在一个函数通过名字调用自身的情况下构成的

    function factorial(num){
        if(num <= 1){
            return 1;
        } else {
            return num * factorial(num - 1);
        }
    }

注意函数名的耦合问题：

    var anotherFactorial = factorial;
    factorial = null;
    alert(anotherFactorial(4));         //出错

使用arguments.callee指向正在执行的函数的指针，实现递归：

    function factorial(num){
        if (num <= 1){
            return 1;
        }else {
            return num * arguments.callee(num - 1);
        }
    }

严格模式下无法访问arguments.callee，达到相同目的需要使用命名函数表达式：

    var factorial = (function F(num){
        if (num <= 1) {
            return 1;
        }else {
            return num * F(num - 1);
        }
    });


#### 闭包

闭包是指有权访问另一个函数作用域中的变量的函数。创建闭包的常见方式，就是在一个函数内部创建另一个函数。

    function createComparisonFunction(propertyName) {
        return function(object1, object2){
            var value1 = object1[propertyName];
            var value2 = object2[propertyName];
            if (value1 < value2){
                return -1;
            } else if (value1 > value2){
                return 1;
            } else {
                return 0;
            }
        };
    }

当某个函数被调用时，会创建一个执行环境（execution context）及相应的作用域链。然后使用argumengts和其他命名参数的值来初始化函数的活动对象（activation object）。但在作用域链中，外部函数的而活动对象始终处在第二位，外部函数的外部函数的活动对象出于第三位，···直至作为作用域链重点的全局执行环境
在函数执行过程中，为读取和写入变量的值，就需要在作用域链中查找变量。

    function compare(value1, value2){
        if (value1 < value2){
            return -1;
        }
        else if (value1 > value2){
            return 1;
        }
        else {
            return 0;
        }
    }
    
    var result = compare(5, 10);

后台的每个执行环境都有一个表示变量的对象--变量对象。全局环境的变量对象始终存在，而像compare()函数这样的局部环境的变量对象，则值在函数执行的过程中存在。创建compare()函数时，会创建一个预先包含全局变量对象的作用域链，这个作用域链被保存在内部的[[Scope]]属性中。当调用compare()时，会为函数常见一个执行环境，然后通过赋值函数的[[Scope]]属性中的对象构建起执行环境的作用域链。此后，又有一个活动对象（再次作为变量对象使用）被创建并被推入执行环境作用域链的前端。
作用域链本质上是一个纸箱变量对象的指针列表，它只引用但不实际包含变量对象。

无论什么时候在函数中访问一个变量时，就会从作用域链中搜索具有相应名字的变量。一般来说，当函数执行完毕后，局部活动对象就会被销毁。内存中仅保存全局作用域（全局执行环境的变量对象）。


闭包与变量：

作用域链的配置机制的副作用：闭包只能取得包含半数中任何变量的最后一个值。

    funciton createFunctions(){
        var result = new Array();
        for (var i=0; i < 10; i++){
            result[i]  = function(){
                return i;
            };
        }
        return result
    }

当createFunctions()函数返回后，变量i的值是10，此时每个函数都引用保存变量i的同一个变量对象，所以在每个函数内部i的值都是10。
可以通过创建两一个匿名函数强制让闭包的行为符合预期：

    function createFunctions(){
        var result = new Array();
        for (var i=0; i < 10; i++){
            result[i] = function(num){
                return function(){
                    return num;
                };
            }(i);
        }
        return result;
    }


关于this：
匿名函数的执行环境具有全局性，因此其this对象通常指向window。

    var name = "the window";
    
    var object = {
        name : "My Object",
        getNameFunc : function(){
            return function(){
                return this.name;
            };
        }
    };
    alert(object.getNameFunc()());          //"the window"  ---     非严格模式

由于getNameFunc()返回一个函数，因此调用getNameFunc()()就会立即调用它返回的函数。
内部函数在搜索这两个变量时，只会搜索到其活动对象为止，因此永远不可能直接访问外部函数中的变量。
不过可以这样：

    var name = "the window";
    
    var object = {
        name : "My object",
        getNameFunc : function(){
            var that = this;
            return function(){
                return that.name;
            };
        }
    };
    alert(object.getNameFunc()());          //"My Object"

this的值意外改变的情况：

    var name = "the window";
    var object = {
        name : "my object",
        getName : function(){
            return this.name;
        }
    };
    
    object.getName();           //"my object"
    (object.getName)();         //"my object"
    (object.getName = object.getName)();            //"the window"

    
内存泄漏：

如果闭包的作用域链中保存着一个HTML元素，那么就意味着该元素将无法被销毁。

    function assignHandler(){
        var element = document.geElementById("someElement");
        element.onclick = function(){
            alert(element.id);
        };
    }

有匿名函数保存了一个对assignHandler()的活动对象的引用，因此就会导致无法减少element的引用数。只要牛冥函数存在，element的引用数就至少也是1，因此它所占用的内存就永远不会被回收。
可以这样：

    function assignHandler(){
        var element = document.getElementById("someElement");
        var id = element.id;
        element.onclick = function(){
            alert(id);
        };
        element = null;
    }


#### 模仿块级作用域

js从不提示是否多次声明了同一个变量。是会对后续的声明视而不见（不过会执行变量初始化）。匿名函数可以同来模仿块级作用域并避免这个问题。

    (function(){
        //这里是块级作用域
    })();

以上代码定义并立即调用了一个匿名函数。将函数声明包含在一对圆括号中，表示它实际山格式一个函数表达式。紧随其后的一对圆括号会立即调用这个函数。
函数声明后不可加()，但是函数表达式可以。将函数声明转化函数表达式，再加上圆括号即可。

    function outputNumbers(count){
        (function(){
            for (var i=0; i < count; i++){
                alert(i);
            }
        })();
        alert(i);
    }

在匿名函数中定义的任何变量，都会在执行结束时被销毁

    function outputNumbers(count){
        (function () {
            for (var i=0; i < count; i++){
                alert(i);
            }
        })();
        alert(i);           //此处出错
    }

这种技术经常在全局作用域中被用在函数外部，从而限制向全局作用域中添加过多的变量核函数。一般来说，都应该尽量少向全局作用域中添加变量和函数。
在一个由多人开发的大型项目中，过多的全局变量和函数很容易导致命名冲突。通过创建私有作用域，每个开发既可以使用自己的变量，又不必担心担心搞乱全局作用域。

    (function(){
        var now = new Date();
        if(now.getMonth() == 0 && now.getDate() == 1){
            alert("Happy new year!");
        }
    })();


#### 私有变量

严格来讲，js中没有私有成员的概念，所有的对象属性都是公有的。
但在任何函数中定义的变量，都可以被认为是私有变量，不能再函数的外部访问这些变量。私有变量包括函数的参数、局部变量和在函数内部定义的其他函数。

    function add(num1, num2){
        var sum = num1 + num2;
        return sum;
    }

在此函数内部创建一个闭包，闭包通过自己的作用域链也可以访问这些变量。就可以创建用于访问私有变量的公有方法：
吧有权访问私有变量和私有函数的公有方法称为特权方法（privileged method）。
两种方式，第一张在构造函数中定义特权方法：

    function Myobject(){
        //私有变量和私有函数
        var privateVariable = 10;
        function privateFunction(){
            return false;
        }
    
        //特权方法
        this.publicMethod = function (){
            privateVariable++;
            return privateFunction();
        };
    }

创建MyObject的实例后，吃了使用publicMethod()这个途径外，没有任何办法可以直接访问privateVariable和privateFunction()

利用私有和特权成员，可以隐藏不应被直接修改的数据。

    function Person(name){
        this.getName = function(){
            return name;
        };
        this.setName = function(value){
            name = value;
        };
    }
    
    var person = new Person("hothunter");
    alert(person.getName());            //"hothunter"
    person.setName("doubi");
    alert(person.getName());            //"doubi"


静态私有变量：

通过在私有作用域中定义私有变量或函数，同样也可以创建特权方法：

    (function(){
        //私有变量和私有函数
        var privateVariable = 10;
        function privateFunction(){
            return false;
        }
        //构造函数
        MyObject = function(){
        };
        //公有/特权方法
        MyObject.prototype.publicMethod = function(){
            privateVariable++;
            return privateFunction();
        };
    })();

初始化未经声明的变量，总是会创建一个全局变量。因此MyObject就成了全局变量，能够在私有作用域中被访问到。
这个模式与在构造函数中定义特权方法的主要区别，就在于私有变量和函数时由实例共享的。有特权方法是在原型上定义的，因此所有实力都是用同一个函数。而这个特权方法作为一个闭包，总是保存着对包含作用域的引用。

    (function(){
        var name = "";
        Person = function(value){
            name = value;
        };
        Person.prototype.getName = function(){
            return name;
        };
        Person.prototype.setName = function(value){
            name = value;
        };
    })();
    
    var person1 = new Person("hothunter");
    alert(person1.getName());           //"hothunter"
    person1.setName("doubi");
    alert(person1.getName());           //"doubi"
    
    var person2 = new Person("lianhui");
    alert(person1.getName());           //"lianhui"
    alert(person2.getName());           //"lianhui"

这种方式创建静态私有变量会因为使用原型而增加代码复用，但每个实例都没有自己的私有变量。

模块模式（module pattern）：
为单例创建私有变量和特权方法。单例（singleton）指的就是只有一个实例的对象。

    var singleton = {
        name : value,
        method : function(){
            //这里是方法的代码
        }
    };

模块模式通过为单例添加私有变量和特权方法能够使其得到增强：

    var singleton = function(){
        //私有变量和私有函数
        var privateVariable = 10;
        function privateFunction(){
            return false;
        }
        //特权、公有方法和属性
        return {
            publicProperty : true,
            publicMethod : function(){
                privateVariable++;
                return privateFunction();
            }
        };
    }();

返回的对象有钱访问私有变量和函数。这个对象字面量定义的是单例的公共接口。这种模式在需要对单例进行某些初始化，同时又需要维护其私有变量时有用。

    var application = function(){
        //私有变量和函数
        var components = new Array();
        //初始化
        components.push(new BaseComponent());
        //公有
        return {
            getComponentCount : function(){
                return components.length;
            }
            registerComponent : function(component){
                if (typeof component == "object"){
                    components.push(component);
                }
            }
        };
    }();

如果必须创建一个对象并以某些数据对其进行初始化，同事还要公开一些能够访问这些私有数据的方法，那么就可以使用模块模式。这种模式创建的每个单例都是Object的实例，因为最终要通过一个对象字面量来表达。

增强的模块模式：

进一步增强：在返回对象之前加入对其增强的代码。适用于那些单例必须是某种类型的实例，同事还必须添加某些属性和方法对其加以增强的情况。

    var singleton = function(){
        //私有变量和私有函数
        var privateVariable = 10;
        function privateFunction(){
            return false;
        }
        //创建对象
        var object = new CustomType();
        //添加特权、公有属性和方法
        object.publicProperty = true;
        object.publicMethod = function(){
            privateVariable++;
            return privateFunction();
        };
        //返回这个对象
        return object;
    }();

对于上面application的例子，如果对象必须是BaseComponent：

    var application = function(){
        //私有变量和函数
        var components = new Array();
        //初始化
        components.push(new BaseComponent());
        //创建application的一个局部副本
        var app = new BaseComponent();
        //公共接口
        app.getComponentCoune = function(){
            return conponents.length;
        }
        app.registerComponent = function(){
            if(typeof component == "object"){
                components.push(component);
            }
        };
        //返回这个副本
        return app;
    }();


----

### BOM

BOM(浏览器对象模型)是Web中使用JavaScript的核心。

#### window对象

BOM的核心对象是window，表示浏览器的一个实例。既是通过JavaScript访问浏览器窗口的一个接口，又是ECMAScript规定的Global对象。在网页中定义的任何一个对象、变量和函数，都以window作为其golbal对象，因此有权访问parseInt()等方法。

全局作用域：
在全局作用域中声明的变量、函数都会贬称gwindow对象的属性和方法

    var age = 23;
    function sayAge(){
        alert(this.age);
    }
    alert(window.age);          //23
    sayAge();                   //23
    window.sayAge();            //23

定义全局变量与在window对象上直接定义属性有差别：全局变量不嫩沟通过delete操作符删除，而直接在window对象上定义的属性可以。

    var age = 23;
    window.color = "red";
    
    //在IE<9出错，其他所有浏览器中返回false
    delete window.age;
    
    //在IE<9出错，其他所有连蓝旗返回true
    delete window.color;     //returns true
    
    alert(window.age);          //29
    alert(window.color);        //undefined

使用var添加的window对象的[[Configurable]]属性会被设置为false，这样定义的属性不可通过delete操作符删除。

尝试访问未声明的变量会抛出错误，但是通过查询window对象，可以知道某个可能为声明的变量是否存在。

    //这里抛出错误，因为oldValue未定义
    var newValue = oldValue;
    
    //这里不会，这是一次属性查询
    //newValue的值是undefined
    var newValue = window.oldValue;


窗口关系及框架：

如果页面中包含框架，则每个框架都拥有自己的window对象，保存在frames集合中。在frames集合中，可以通过数值索引（从0开始，从左至右，从上到下）或者框架名称来访问相应的window对象，每个window对象都有一个name属性，包含框架的名称

    <html>
        <head>
            <title>Frameset Example</title>
        </head>
        <frameset rows="160,*">
            <frame src="frame.htm" name="legrFram">
            <frameset cols="50%,50%">
                <frame sre="anotherframe.htm" name="leftFrame">
                <frame sre="yetanotherfame.htm" name="rightFrame">
            </frameset>
        </frameset>
    </html>

可以通过window.frames[0]或者window.frames["topFrame"]来引用上方的框架。
不过最好使用top.frames[0]。
top对象始终指向最高（最外）曾框架。也就是浏览器窗口。可以确保在一个框架中正确的访问另一个框架。对于在一个框架中编写的额任何代码来说，其中的window对象指向的都是那个框架的特定实例，而非最高层的框架。

与top相对的另一个window对象是parent（父）。始终指向当前框架的直接尚岑该框架。在某写情况parent等于top。没有框架情况下，parent一定等于top（同时都等于window）。

注意，除非最高层窗口是通过window.open()打开的，否则其window对象的name属性不会包含任何值。

所有这些对象都是window的属性，可以通过window.parent/window.top等形式来访问，同时也意味着可以将不同层次的window对象连缀起来，例如
    
    window.parent.parent.farmes[0];


窗口位置

用来确定和修改window对象位置的属性和方法有很多。screenLeft/screenTop，分别用于表示窗口相对于屏幕左边和上边的位置：

    var leftPos = (typeof window.screenLeft == "number") ?
                    window.screenLeft : window.screenX;
    var TopPos = (typeof window.screenLeft == "number") ?
                    window.screenTop : window.screenY;

moveTo()接收新位置的x和y的坐标值，
moveBy()接收的是在水平和垂直方向上移动的像素数。
通常这两个方法都是被浏览器禁用的。
    
    //将窗口移动到屏幕左上角
    window.moveTo(0, 0);
    
    //将窗口向下移动100像素
    window.moveBy(0, 100);
    
    //将窗口移动到(20, 300);
    window.moveTo(20, 300);
    
    //将窗口向左移动50像素；
    window.moveBy(-50, 0);


窗口大小：

innerWidth、innerHeight、outerWidth、outerHieght。
四个方法在不同浏览器行为不同。很难确定浏览器窗口位置。

    var pageWidth = window.innerWidth,
        pageHeight = window.innerHeight;
    
    if (typeof pageWidth != "number"){
        if (document.compatMode == "CSS1Compat"){
            pageWidth = document.documentElement.clientWidth;
            pageHeight = document.documentElement.clientHeight;
        } else {
            pageWidth = document.body.clientWidth;
            pageHieght = document.body.clientHeight;
        }
    }

对于移动设备，widow.innerWidth和window.innerHeight保存着可见视口，也就是屏幕上可见页面区域大小。
在其他移动浏览器中，document.documentElement度量的是布局视口，即渲染后页面实际大小（与可见视口不同，可见视口只是整个页面中的一小部分）。
对于此功能，必须要先确定浏览器种类，同时检测一下是否在使用移动设备。

调整浏览器窗口大小：
resizeTo()：接收浏览器窗口新宽度和新高度。
resizeBy()：接收新窗口与原窗口的宽度和高度之差。
这两个方法也可能被浏览器禁用。

    //调整到100 * 100
    window.resizeTo(100, 100);
    
    //调整到200 * 150
    window.resizeBy(100, 50);
    
    //调整到300 * 300
    window.resizeTo(300, 300);


导航和打开窗口

window.open()可以导航到一个特定的URL，也可以打开一个新的浏览器窗口。
接收四个参数：要加载的URL，窗口目标，一个特性字符串以及一个表示新页面是否取代浏览器历史记录中当前加载页面的布尔值。通常只传递一个参数，最后一个参数只在不打开新窗口的情况下使用。
如果window.open()传了第二个参数，而且该参数是已有窗口或框架的名称，那么就会在具有改名陈歌德窗口或框架中加载第一个参数制定的URL。
此外，第二个参数也可以是下列任何一个特殊的窗口名称：_self / _parent / _top / _blank

    //等同于<a href="http://www.hothunter.cc" target="topFrame"></a>
    window.open("http://www.hothunter.cc", "topFrame");

弹出窗口
如果给window.open()传递第二个参数并不是一个已经存在的窗口或框架，那么就会根据第三个参数位置上传如的字符串创建一个新窗口或者标签页。如果没有传入第三个参数，就会打开一个带有去全部默认设置的新浏览器窗口。在不打开新窗口情况下，会忽略第三个参数。
第三个参数的表格：  P200

    window.open("http://www.hothunter.cc", "wroxWindow", "height=400,width=400,top=10,left=10,resizable=yes");

window.open()方法会返回一个指向新窗口的引用。引用的对象与其他window对象大致相似，但可以对其进行更多控制，例如有些浏览器的默认情况下可能不允许针对主浏览器窗口调整大小或者移动位置，但允许针对通过window.open()创建的窗口调整大小或者移动位置。

    var wroxWin = window.open("http://www.hothunter.cc", "wroxWindow",  "height=400,width=400,top=10,left=10,resizable=yes");
    //调整大小
    wroxWin.resizeTo(500, 500);
    
    //移动位置
    wroxWin.moveTo(100, 100);

调用close()方法可以关闭新打开的窗口
    
    wroxWin.close();

仅适用于通过window.open()打开的弹出窗口。队友浏览器的主窗口，如果没有得到用户的允许是不能关闭的。。不过，弹出窗口可以调用top.close()在不经用户允许的情况下关闭自己。

新创建的window对象有一个opener属性，保存着打开它的原始窗口对象。只在弹出窗口中的最外层window对象(top)中有定义，而且指向调用window.open()窗口或框架

    var wroxWin = window.open("http://www.hothunter.cc", "wroxWindow",  "height=400,width=400,top=10,left=10,resizable=yes");
    alert(wroxWin.opener == window);            //true

原始窗口没有这样指针指向弹出窗口。窗口不跟踪记录他们打开的弹出窗口，因此只能在必要的时候自己手动实现跟踪。
将opener属性设置为null就是告诉浏览器新创建的标签页不需要与打开它的标签页通信，因此可以再独立的进程中运行。
标签之间的联系一旦切断，将没有办法恢复。


安全限制：P201
大概就是出于安全的考虑对弹出窗口的各种限制。。


弹出窗口的屏蔽程序：
用户可以将绝大多数不想看到弹出窗口屏蔽掉，在屏蔽时，如果是浏览器内置的额屏蔽程序组织的弹出窗口，那么window.open()很可能就会返回null。此时只要检测这个返回值就可以确定个弹出窗口是否被屏蔽了。

    var wroxWin = window.open("http://www.hothunter.cc", "_blank");
    if (wroxWin == null) {
        alert("The popup was blocked!");
    }

如果是浏览器扩展或者其他程序组织的弹出窗口，那么window.open()通常会抛出一个错误。因此想要准确检测弹出窗口是否被屏蔽，必须在检测返回值的同时，将对window.open()的调用封装在一个try-catch中：

    var blocked = false;    
    
    try {
        var wroxWin = window.open("http://www.hothunter.cc", "_blank");
        if (wroxWin == null){
            blocked = true;
        }
    } catch (ex) {
        blocked = true;
    }
    
    if (blocked) {
        alert("The popup was blocked!");
    }

在任何情况下，以上代码都可以检测出调用window.open()打开的弹出窗口是不是被屏蔽了。但要注意的是，检测弹出窗口是否被屏蔽只是一方面，并不会组织浏览器显示与被屏蔽的弹出窗口有关的消息。


间歇调用和超时调用：

javascript是单线程语言，但允许通过设置超时值和间歇时间值来调度代码在特定的时刻执行。前者是在指定的时间过后执行代码，而后者则是每个指定的时间就执行一次代码。

超时调用使用window对象的setTimeout()方法，两个参数：要执行的代码和以毫秒表示的时间（在执行代码前要等待的毫秒）。第一个参数可以是包含JavaScript代码的字符串（类似eval()），也可以是一个函数

    //不建议传递字符串！可能导致性能损失
    setTimeout("alert('Hello world')", 1000);
    
    //推荐的调用方式：
    setTimeout(function(){
        alert("hello world");
    }, 1000);

第二个参数是一个表示等待多长时间的毫秒，但经过该事件后指定的代码不一定会执行。jaascript是一个单线程的解释器，因此一定时间内只能执行一段代码。为了控制要执行的代码，就有一个favascript任务队列。任务会按照将他们添加到队列的顺序执行。第二个参数告诉再过多长时间把当前任务添加到队列中。如果队列空，那么添加的代码立即执行。如果队列不为空，就等前面的代码执行完了再执行。

setTimeout()调用后会返回一表示超时调用的数值ID，是计划执行的唯一标识符
clearTimeout()取消尚未执行的调用计划。

    //设置超时时间
    var timeoutId = setTimeout(funtion(){
        alert("hello world");
        }, 1000);
    
    //把它取消：
    clearTimeout(timeoutId);

间歇调用：按照指定的时间间隔重复执行代码，直至间歇调用被取消挥着页面被卸载。
setInterval()，接受的参数与setTimeout()相同。

    //不建议传递字符串！
    setInterval("alert("hello world")" , 10000);
    
    //推荐的调用方式
    setInterval(function(){
        alert("hello world");
        }, 10000);

调用setInterval()也会返回一个ID，用于在将来某个时刻取消间歇调用。

    var num = 0;
    var max = 10;
    var intervalId = null;
    function incrementNumber(){
        num++;
        //如果执行次数达到了max设定的值，则取消后续尚未执行的调用
        if (num == max) {
            clearInterval(intervalId);
            alert("Done");
        }
    }
    inTervalId = setInterval(incrementNumber, 500);

这个例子也可以使用超时调用实现

    var num = 0;
    var max = 10;
    function incrementNumber(){
        num ++
        //如果执行次数未达到max设定的值，则设置另一次超时调用
        if (num < max){
            setTimeout(incrementNumber, 500);
        } else {
            alert("Done");
        }
    }
    setTimeout(incrementNumber, 500);

在开发中很少使用真正的间歇调用，盐饮食后一个间歇调用可能会在前一个间歇调用结束之前启动。像上面使用超时调用模拟间歇完全可以避免这一点。最好不要使用间歇调用。


系统对话框：

alert()/confirm()/prompt()：可以调用系统对话框。
显示这些对话框时候代码会停止运行，关掉之后会恢复。

对于confirm()，可以确定用户点击了ok还是cancel。

    if (confirm("Are you sure?")) {
        alert("i am so glad you're sure!");
    } else {
        alert("I am sorry to hear you're not sure..");
    }

prompt()：提示框，两个参数：要显示给用户的文本提示和文本输入域的默认值（可以是一个空字符串）。

    var result = prompt("What is your name?", "");
    if (result !== null) {
        alert("Welcome, " + result);
    }

除此以外还有
window.find()/window.print();   分别调用浏览器的查找和打印对话框


#### location对象

location对象体用了与当前窗口中加载的文档又换的信息，还提供了一些导航功能。
它既是window对下那个的属性，也是document对象的属性。同时location对象还将URL解析为独立的片段。

* bash      "#contents"     返回URL中的hash(#号后跟零或多个字符串)，如果URL不存在，返回空。
* host      "www.wrox.com:80"   返回服务器名称和端口号（如果有）
* hostname      "www.wrox.com"      返回不带端口号的服务器名称
* href      "http://www.wrox.com"       返回当前加载页面的完整URL，同时location的toString()方法也返回这个。
* pathname      "/WileyCDA"     返回URL中的目录和（或）文件夹。
* port      "8080"      返回URL中制定的端口号。如果URL中不包含端口号，则这个属性返回空字符串。
* protocol      "http:"     返回页面使用的协议，通常是http:/https:
* search        "?q=javascript"  返回URL的查询字符串。?开头

查询字符串参数：

location.search返回从问号到URL末尾的所有内容，但却没有办法逐个访问其中的每个查询字符串参数。可以：

    function getQueryStringArgs(){
        //取得查询字符串并去掉开头的问号
        var qs = (location.search.length > 0 ? location.search.substring(1) : ""),
        //保存数据对象
        var args = {},
        //取得每一项
        items = qs.length ? qs.split("&") : [],
        items = null,
            name = null,
            value = null,
            //在for循环中使用
            i = 0,
            len = items.length;
        //逐个将每一项添加到args对象中
        for (i=0; i < len; i++){
            item = items[i].split("=");
            name = decodeURIComponent(item[0]);
            value = decodeURIComponent(item[1]);
            if (name.length) {
                args[name] = value;
            }
        }
        return args;
    }

    //假设查询字符串是?q=javascript&num=10
    
    var args = getQueryStringArgs();
    
    alert(args["q"]);           //"javascript"
    alert(args["num"]);         //"10"


位置操作：

使用assign()方法为其传递一个URL，可以立即打开新URL并在浏览器的历史记录中生成一条记录：

    location.assign("https://www.hothunter.cc");

如果是将location.href/window.location 设置一个URL值，也会以该值调用assign()

    window.location = "http://www.hothunter.cc";
    location.href = "http://www.hothunter.cc";

另外修改location对象的其他属性也可以改变当前加载的页面：

    //假设初始的URL为 http://www.hothunter.cc/index
    
    //将URL修改为 "http://www.hothunter.cc/index/#section1"
    location.hash = "#section1";
    
    //将URL修改为"http://www.hothunter.cc/index/?q=javascript"
    location.search = "?q=javascript";
    
    //将URL修改为"http://www.google.com/index"
    location.hostname = "www.google.com";
    
    //将URL修改为"http://www.google.com/hothunter"
    location.pathname = "hothunter";
    
    //将URL修改为"http://www.google.com:8080/index"
    location.port = 8080;

每次修改location的属性（hash除外），页面都会以新的URL重新加载

当浏览器位置改变时，要禁用浏览器的“后退”功能可以使用replace()，调用之后，用户不能回到前一个页面。

    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="UTF-8">
        <title>You won't be able to get back here</title>
    </head>
    <body>
        <p>Enjoy this page for a second, because you won't be coming back here.</p>
        <script type="text/javascript">
            setTimeout(function(){
                location.replace("http://www.hothunter.cc")
                }, 1000);
        </script>
    </body>
    </html>

reload()：重新加载当前显示地页面。如果调用而不传递任何参数，页面就会以组有效的方式重新加载。如果页面自上次请求以来并没有改变过，页面就会从浏览器缓存中重新加载。如果要强制从了服务器重新加载，需要：

    location.reload();          //重新加载（有可能从缓存加载）
    location.reload(true);      //重新加载（从服务器重新加载）

位于reload()之后的代码可能会不被执行，取决于网络延迟或者系统资源等因素。


#### navigator 对象

用于识别客户端浏览器类型。见P210表格。


检测插件：

对于非IE浏览器，可以使用plugins数组，包涵下列属性：

* name：插件的名字
* description：插件的文件名
* filename：插件的文件名
* length：插件所处理的MIME类型数量。

name属性会包含检测插件必须的所有信息，但有时候也不完全如此，检测插件时，需要循环迭代每个插件并将插件的name与给定的名字进行比较：

    function hasPlugin(name){
        name = name.toLowerCase();
        for (var i=0; i< navigator.plugins.length; i++){
            if (navigator.plugins[i].name.toLowerCase().indexOf(name) > -1){
                return true;
            }
        }
        return false;
    }
    
    //检测Flash
    alert(hasPlugin("Flash"));
    
    //检测QuickTime
    alert(hasPlugin("QuickTime"));

对于IE，检测插件的唯一方式就是使用专有的ActiveXObject类型，并尝试创建一个特定插件的实例。IE是以COM对象的方式实现插件的，而COM对象使用唯一标识符来标识。因此想检查特定的插件，就必须知道其COM标识符。例如，FLash的标识符是ShockwaveFlash.ShockwaveFlash。

    //检测IE中的插件
    function hasIEPlugin(name){
        try{
            new ActiveXObject(name);
            return true;
        } catch (ex) {
            return false;
        }
    }
    
    //检测Flash
    alert(hasIEPlugin("ShockwaveFlash.ShockwaveFlash"));
    
    //检测QuickTime
    alert(hasIEPlugin("QuickTime.QuickTime"));

典型的做法是针对每个插件分别创建检测函数，而不是使用前面两种：

    //检测所有浏览器中的Flash
    function hasFlash(){
        var result = hasPlugin("Flash");
        if(!result){
            result = hasIEPlugin("ShockwaveFlash.ShockwaveFlash");
        }
        return result;
    }
    
    //检测所有浏览器中的QuickTime
    function hasQuickTime(){
        var result = hasPlugin("QuickTime");
        if(!result){
            result = hasIEPlugin("QuickTime.QuickTime");
        }
        return result;
    }
    
    //检测Flash
    alert(hasFlash());
    
    //检测QuickTime
    alert(hasQuickTime());


注册处理程序：

registerContentHandler()/registerProtocolHandler()。可以让一个站点指明它可以处理特定类型信息。随着RSS阅读器和在线电子邮件兴起，这玩意就有用了。
registerContentHandler()接收三个参数：要处理MIME类型、可以处理该MIME类型的页面URL以及应用程序的名称。
要将一个站点注册为处理RSS源的处理程序，可以：

    navigator.reguisterContentHandler("application/rss+xml", "http://www.somereader.com?fead=%s", "Some Reader");

registerProtocolHandler()方法，接收三个参数：要处理的协议（例如mailto或者ftp）、处理该协议的页面的URL和应用程序的名称。

    navigator.registerProtocolHandler("mailto", "http://www.somemailclient.com?cmd=%s", "Some Mail Client");


#### screen 对象

JavaScript有几个对象在编程中用处不大，这个screen就是其张志义。只用来表明客户端的能力。包括浏览器窗口外部的显示器的信息，像素宽度和高度等。
P214。表格
主要出现在测定客户端能力的站点跟踪工具中，通常不用于影响功能。


#### history 对象

history对象个保存着用户上网的历史记录，从窗口被打开的那一刻算起。history是window对象的属性，因此每个浏览器窗口。每个标签页乃至每个框架，都有自己的history对象与特定的window对象关联。出于安全考虑，开发人员无法得知用户浏览过的URL。不过借由用户访问过的页面列表，同样可以在不知道实际URL的情况下实现后退和前进。

使用go()方法可以再用户的历史记录中任意跳转，可以向后也可以向前。接收一个参数，表示向后或想起跳转的页面数的饿一个整数值。

    //后退一页
    history.go(-1);
    
    //前进一页
    history.go(1);
    
    //前进两页
    history.go(2);

也可以传递字符串，浏览器会跳转到历史记录中包含该字符串的第一个位置，可能后退也可能前进，具体看哪个位置最近。如果历史不存在包含该字符串，那就什么都不做。

    //跳转到最近的hothunter.cc
    history.go("hothunter.cc");

同时还有back()/forward()来代替go()。

    //后退一页
    history.back();
    
    //前进一页
    history.forward();

history还有一个length属性，保存着历史记录的数量。包括所有历史记录。


---


### 第九章 客户端检测

//TODO:这个先放一放


---

### 第十章 DOM

DOM（文档对象模型）是针对HTML和XML文档的一个API。描绘了一个层次化的节点树，允许开发人员添加、移除和修改页面的某一部分。

#### 节点层次

    <html>
        <head>
            <title>Sample Page</title>
        </head>
        <body>
            <p>Hello World!</p>
        </body>
    </html>

文档节点是每个文档的根节点。这个例子中，文档节点只有一个子节点即<html>元素，称之为文档元素。文档元素是文档的最外层元素，文档中的其他所有元素都包含在文档元素中。每个文档只能有一个文档元素。在HTML页面中，文档元素始终都是<html>元素。在XML中，没有预定义的元素，因此任何元素都可能成为文档元素。
每一段标记口可以通过树中的一个节点来表示：HTML元素通过元素节点表示，特性（attribute）通过特性节点表示，文档类型通过文档类型节点表示，而注释通过注释节点表示，公有12种节点类型，都继承自一个基类型。


Node类型：
除了IE之外，在其他所有浏览器中都可以访问这个类型。JavaScript中的所有节点类型都继承自Node类型。每个节点都有一个nodeType属性，用于表明节点的类型。：

* Node.ELEMENT_NODE(1);
* Node.ATTRIBUTI_NODE(2);
* Node.TEXT_NODE(3);
* Node.CDATA_SECTION_NODE(4);
* Node.ENTITY_REFERENCE_NODE(5);
* Node.ENTITY_NODE(6);
* Node.PROCESSING_INSTRUCTION_NODE(7);
* Node.COMMENT_NODE(8);
* Node.DOCUMENT_NODE(9);
* Node.DOCUMENT_TYPE_NODE(10);
* Node.DOCUMENT_FRAGMENT_NODE(11);
* Node.NOTATION_NODE(12);

通过上面这些常量，就可以容易确定根节点的类型

    if (someNode.nodeType == Node.ELEMENT_NODE){
        alert("Node is an element");
    }    

对于任意浏览器：

    if (someNode.nodeType == 1){
        alert("node is an element");
    }

nodeName和nodeVlaue:
两个属性的值完全取决于节点的类型。在使用这两个值以前，最好先检查：

    if(someNode.nodeType == 1){
        value = someNode.nodeName;          //nodeName  的值是元素的标签名
    }


节点关系：
节点间的各种关系可以用传统的家族关系来描述，相当于把文档树比喻成家谱。在HTML中，body为html的子元素，html为body的父元素，body和head为同胞元素。
每个节点都有一个childNodes属性，保存着一个NodeList对象。是一种类数组对象，用于保存一组有序节点，通过位置来访问这些节点。注意NodeList并不是Array的实例。它基于DOM结构动态之星查询的结果，因此DOM结构的变化能够自动反应在NodeList对象中。是有生命、有呼吸的对象。

    var firstChild = someNode.childNodes[0];
    var secondChild = someNode.childNodes.item(1);
    var count = someNode.childNodes.length;

可以类似对arguments对象使用Array.prototype.slice()方法将其转换为数组，对NodeList对象也可以采用同样的方法转换为数组:

    //在<=IE8中无效
    var arrayOfNodes = Array.prototype.slice.call(someNode.childNodes,0);

对于IE：
    
    function converToArray(nodes){
        var array = null;
        try{
            array = Array.prototype.slic.call(nodes, 0);  //针对非IE
        } catch (ex) {
            array = new Array();
            for (var i=0, len=nodes.length; i < len; i++){
                array.push(nodes[i]);
            }
        }
        return array;
    }

每个节点都有一个parentNode属性，该属性指向文档树中的父节点。
使用列表中每个节点的previousSibling和newxSibling属性，可以访问同一列表中的其他节点。列表中第一个节点的previousSibling属性值为null，列表中最后一个节点的nextSibling属性的值同样是null。

    if (someNode.nextSibling === null) {
        alert("last node in the parent's childNodes list.");
    } else if {
        alert("first node in the parent's childNodes list.");
    }

父节点与其第一个和最后一个节点之间的关系。父节点的firstChild和lastChild属性分别指向其childNodes列表中的第一个和最后一个节点。其中，someNode.firstChild === someNode.ChildNOdes[0]，而someNode.lastChild === someNode.childNodes[someNode.childNodes.length-1]。
虽有节点都有的最后一个属性时ownerDocument，该属性指向表示整个文档的文档节点。表示的是任何节点都属于它所在的文档，任何节点都不能同时存在于两个或更多个文档中。通过这个属性，可以不必再节点层次中通过层层回溯到达顶端，而是可以直接访问文档节点。

操作节点：

关系指针都是只读的。
操作节点的方法：最常用的，appendChild()，用于向childNodes列表的末尾添加一个节点。添加后，childNodes的新增节点、父节点以及以前的最后一个子节点的关系指针会相应的更新。更新后，appendChild()返回新增的节点。

    var returnedNode = someNode.appendChild(newNode);
    alert(returnedNode == newNode);             //true
    alert(someNode.lastChild == newNode);       //true

如果传入到appendChild()中的节点已经是文档的一部分，结果就是将该节点从跟原来的位置转移到新位置。即使可以将DOM树看成是由一系列指针连接起来的，但任何DOM节点也不能同时出现在文档中的多个位置上。
如果在调用appendChild()时传入了父节点的第一个子节点，那么该子节点就会成为父几点的最后一个子节点。

    //someNode有多个子节点
    var returnedNode = someNode.appendChild(someNode.firstChild);
    alert(returnedNode == someNOde.firstChild);     //false
    alert(returnedNode == someNode.lastChild);      //true

insertBefore()：吧节点放在childNodes列表中某个特定位置。两个参数：要插入的节点和作为参照的节点。

    //插入后成为最后一个子节点
    returnedNode = someNode.insertBefore(newNode, null);
    alert(newNode == someNode.lastChild);   //ture
    
    //插入后成为第一个子节点
    var returnedNode = someNode.insertBefor(newNode, someNode.firstChild);
    alert(returnedNOde == newNode);     //true
    alert(newNode == someNOde.firstChild);      //ture
    
    //插入到最后一个子节点前面
    returnedNOde = someNode.insertBefore(newNode, someNOde.lastChild);
    alert(newNode==someNode.childNodes[someNode.childNodes.length-2]);//true   

replaceChild()：替换节点，两个参数：要插入的节点和要替换的节点。要替换的节点将由这个方法韩慧并从文档树中北一处，同时由要插入的节点占据其位置。
在使用replaceChild()插入一个节点时，该节点的所有关系指针都会从被它替换的节点赋值过来。从技术讲，被替换的节点仍然还在文档中，但它在文档中已经没有了自己的位置。

    //替换第一个子节点
    var returnedNode = someNode.replaceChild(newNode,someNode.firstChild);
    
    //替换最后一个子节点
    returnedNode = someNode.replaceChild(newNode, someNOde.lastChild);

removeChild()：移除节点，一个参数：要被移除的节点。被移除的节点将成为方法的返回值。

    //移除第一个子节点
    var formerFirstChild = someNode.removeCHild(someNOde.firstChild);
    
    //移除最后一个子节点
    var formerLastChild = someNode.removeChild(someNode.lastChild);


其他方法：

cloneNode()：用于创建调用这个方法的节点的一个完全相同的副本。接收一个布尔值参数，表示是否执行深度赋值。在参数为true时，深复制，也就是复制节点及其整个子节点树；false时，浅复制，值赋值节点本身。复制后返回的节点副本属于文档所有，但并没有未它指定父节点。

    <ul>
        <li>item 1</li>
        <li>item 2</li>
        <li>item 3</li>
    </ul>

    var deepList = myList.cloneNode(ture);
    alert(deepList.childNodes.length);          //3(IE<9)或7（其他浏览器）
    
    var shallowList = myList.cloneNode(false);
    alert(shallowList.chidNodes.length);        //0

normalize()：处理文档树中的文本节点。听说后面还有讲。


Document 类型

JavaScript通过Document类型表示文档。在浏览器中,document对象是HTMLDocument（继承自Document类型）的一个实例，表示整个HTML页面。document对象是window对象的一个属性，因此可以将其最为全局对象访问。

* nodeType的值为9
* nodeName的值为"#document"
* nodeValue的值为null
* parentNode的值为null
* ownerDocument的值为null
* 其子节点可能是一个DocumentType（最多一个）、Element（最多一个）、ProcessingInstruction或Comment


文档和子节点：

快速便捷访问子节点的方式：
documentElement：该属性始终指向HTML页面中的<html>元素
childNodes：使用该列表访问文档元素

    <html>
        <body>
            
        </body>
    </html>

    var html = document.documentElement;        //取得对<html>的引用
    alert(html === document.childNodes[0]);     //true
    alert(html === document.firstChild);            //true

document.body：直接指向<body>元素：

    var body = document.body;           //取得对<body>的引用

DocumentType：通常和<!DOCTYPE>标签看成一个与文档其他部分不同的实体，通过doctype属性来访问

    var doctype = document.doctype;

但是不同浏览器对document.doctype的支持都不一致，因此这个属性的用处有限。

注释：出现在<html>元素外部的注释应该算是文档的子节点。然而不同的浏览器在是否解析这些注释以及能否争取处理他们等方面，存在差异。P255

文档信息：

document.title：

    //取得文档标题
    var originalTitle = document.title;
    
    //修改文档标题
    document.title = "welcome to HotHunter.cc";

URL属性包含页面中完整的URL、
domain属性中只包含页面的域名
referrer属性中保存着链接到当前页面的那个页面的URL。在没有来源的情况下，referrer属性中可能会包含空字符串。
所有这些信息都存与请求的HTTP头部，只不过通过这些属性能够在JavaScript中访问

    //取得完整的URL
    var url = document.URL;
    
    //取得域名
    var domain = document.domain;
    
    //取得链接来的URL
    var referrer = document.referrer;

domain可以被修改，但是有限制：

    //假设页面来自p2p.hothunter.cc域
    
    document.domain = "hothunter.cc";       //成功
    
    document.domain = "baidu.com";          //失败

另一个domain的限制：

    //假设页面来自p2p.hothunter.cc域
    
    document.domain = "hothunter.cc";       //松散的-成功
    
    document.domain = "resume.hothunter.cc";        //紧绷的-失败


查找元素：

getElementById()：一个参数，要取得的元素的ID。这里的ID必须与页面中元素id特性（attribute）严格匹配，包括大小写
    
    <div id="myDiv">Some text</div>

    var div = document.getElementById("myDiv");

页面中多个元素的ID值相同，getElementById()值返回文档中第一次出现的元素。

getElementsBuTagName()：接收一个参数：要取得严肃的标签名，返回的额是包含零或多个元素的NodeList。在HTML文档中，这个方法返回一个HTMLCollection对象，作为一个“动态”集合，该对象与NodeList类似。

    var images = document.getElementsByTagName("img");

    alert(images.length);       //输出图像的数量
    alert(images[0].src);       //输出第一个图像元素的src特性
    alert(images.item(0).src);  //输出第一个图像元素的src特性

HTMLCollection还有一个方法：namedItem()，可以通过元素的name特性取得集合中的项。

    <img src="myimage.gif" name="myImage">

    var myImage = images.namedItem("myImage");

还可以：

    var myImage = images["myImage"];

想要取得文档中的所有元素，可以向getElementsByTagName()中传入"*"。在JavaScript及CSS中，（\*）通常表示"全部"。

    var allElements = document.getELementsByTagName("*");

这一行代码返回HTMLCollection中，包含了整个页面中的所有元素，按照出现先后顺序，同时包含所有注释节点。

getElementsByName()：返回带有给定name特性的所有元素。最常用的是返回单选按钮。

    <fieldest>
        <legend></legend>
        <ul>
            <li>
                <input type="redio" value="red" name="color" id="colorRed">
                <lable for="colorRed">Red</lable>
            </li>
            <li>
                <input type="redio" value="blue" name="color" id="colorBlue">
                <lable for="colorBlue">Blue</lable>
            </li>
            <li>
                <input type="redio" value="green" name="color" id="colorGreen">
                <lable for="colorGreen">Green</lable>
            </li>
        </ul>
    </fieldest>

    var radios = document.getElementsByName("color");


特殊集合：

除了属性和方法，document对象还有一些特殊结合，都是HTMLCollection对象：

* document.anchors:包含文档中所有带name特性的 <a>元素；
* document.applets：包含文档中所有<applet>元素，因为不再推荐使用该元素，因此这个集合已经不被建议使用。
* document.forms：包含文档中所有<form>元素，与document.getElementsByTagNmae("form")得到的结果相同。
* document.images：包含文档中所有<img>元素，与document.getElementsByTagName("img")得到的结果相同
* document.links：包含文档中所有带href特性的<a>元素。


DOM一致性检测：

document.implementation：检测浏览器实现了DOM的哪些部分。
DOM1只提供一个方法：hasFeature()，接收连个参数：要检测的DOM功能歌德名称以及版本号。
    
    var hasXmlDom = document.implementation.hasFeature("XML", "1.0");

P259。可以检测的不同的值的列表


文档写入：

将输入流写入到网页中的能力。
write()/writeln()/open()/close()。
write()和writeln()方法都接受一个字符串阐述，即呀写入输入流中的文本。write()会原样写入，writeln()会在字符串的末尾添加一个换行（\n）。

    <html>
    <head>
        <title>document.write() Example</title>
    </head>
    <body>
        <p>The current date and time is:
            <script type="text/javascript">
                document.write("<strong>"+(new Date()).toString() + "</strong>");
            </script>
        </p>
    </body>
    </html>



Element 类型：

Element类型用于表现XML或HTML元素，提供了元素标签名、子节点及特性的访问。

* nodeType的值为1
* nodeName的值为元素的标签名；
* nodeValue的值为null
* parentNode肯能是Document或者Element；
* 其子节点可能是Element/Text/Comment/ProcessingInstruction/CDATASection/EntityReference

要访问元素的标签名使用nodeName属性，也可以使用tagName属性。返回相同的值（使用后者主要是为了清晰）

    <div id="myDiv"></div>

    var div = document.getElementById("myDiv");
    alert(div.tagName);         //"DIV"
    alert(div.tagName == div.nodeName);         //true

在HTML中，标签名始终都以全部大写表示，在XML（有时包括XHTML）中标签名始终会与源码中保持一致。

HTML元素：

所有HTML元素都由HTMLElement类型表示，不是直接通过这个类型，也是通过它的子类型来表示。HTMLElement类型直接继承自Element并添加了一些属性。

* id，元素在文档中的唯一标识符。
* title，有关元素的附加书名信息，一般通过工具提示条显示。
* lang，元素内容的语言代码，很少使用
* dir，语言的方向，值为"ltr"(left-to-right,从左至右)或"rtl"，很少使用。
* className，与元素的class特性对应，即为元素制定的CSS类。没有将这个属性命名为class，是因为class是ECMAScript的保留字。

示例：

    <div id="myDiv" class="bd" title="BodyText" lang="en" dir="ltr"></div>

    var div = document.getElementById("myDiv");
    alert(div.id);          //"myDiv"
    alert(div.className);       //"bd"
    alert(div.title);       //"BodyText"
    alert(div.lang);        //"en"
    alret(div.dir);         //"ltr"

    div.id = "someOtherId";
    
    div.className = "fuckme";
    div.title = "haokunxiang huijia shujiaole ";
    div.lang = "zh";
    div.dir = "rtl";

所有HTML元素以及与之关联的类型：P263。


取得特性：

操作特性的DOM方法主要有三个：
getAttribute()/setAttribute()/removeAttribute()。可以针对任何特性使用，包括以HTMLElement类型属性的形式定义的特性。

    var div = document.getElementById("myDiv");
    alert(div.getAttribute("id"));          //"myDiv"
    alert(div.getAttribute("class"));       //"bd"
    alert(div.getAttribute("title"));       //"BodyText"
    alert(div.getAttribute("lang"));        //"en"
    alert(div.getAttribute("dir"));         //"ltr"

同时也可以取得自定义特性：

    <div id="myDiv" hothunter="hello"></div>

    var value = document.getElementById("myDiv").getAttribute("hothunter");


通过getAttribute()访问style时，返回的style特性值值包含的是CSS文本，而通过属性来访问它则会返回一个对象。由于style属性是用于以变成方式访问元素样式的，因此没有直接映射到style特性。
通过getAttribute()访问包含事件的元素时，会返回相应代码的字符串。如果访问事件的属性，则会返回一个JavaScript函数（如果未在元素中制定相应特性，返回null）。原因是事件处理程序属性本身就应该被赋予函数值。


设置特性：

setAttribute()：两个参数：要设置的特性名和值。如果特性存在，会以制定的值替换现有的，如果不存在，会创建该属性并赋值。

    div.setAttribute("id", "someOtherId");
    div.setAttribute("class", "fuckmeagain");
    div.setAttribute("title", "yuelaiyuekunle");
    div.setAttribute("lang", "zh");
    div.setAttribute("dir", "rtl");

和上面getAttribute()一样，setAttribute()同样也可以操作自定的特性。
通过这个方法设置的特性名会被统一被转换为小写形式。
因为所有特性都是属性，所以直接给属性赋值可以设置特性的值：

    div.id = "someOtherId";
    div.align = "center";

为DOM添加一个自定义的属性，该属性不会自动成为元素的特性。

    div.mycolor = "red";
    alert(div.getAttribute("mycolor"));         //null(IE除外)

在大多数浏览器中，上面这个属性都不会自动贬称该元素的特性，因此想通过getAttribute()取得同名特性的值，结果都会返回null。
可自定义属性在IE中会被当做元素的特性，反之亦然。

removeAttribute()，用于彻底删除元素的特性，调用这个方法不仅会清除特性的值，而且会从元素中完全删除特性。

    div.removeAttribute("class");

在序列化DOM元素时，可以通过它来确切指定要包含哪些特性。


attributes属性

Element类型是使用attributes实行的唯一一个DOM节点类型。attributes属性中包含一个NamedNodeMap，与NOdeList蕾西，也是一个“动态”集合。元素的每一个特性都有一个Attr节点表示，每个节点都保存在NamedNodeMap对象中。

* getNamedItem(name)：返回nodeName属性等于name的节点；
* removeNamedItem(name)：从列表中移除nodeName属性定于name的节点；
* setNamedItem(node)：向列表中添加节点，以几点的nodeName属性为索引；
* item(pos)：返回位于数字pos位置处的节点。

attributes属性中包含一系列节点，每个节点的nodeName就是特性的名称，而节点的nodeValue就是特性的值。

    var id = element.attributes.getNamedItem("id").nodeValue;
    //简写方式：
    var id = element.attributes["id"].nodeValue;

设置特性的值：

    element.attributes["id"].nodeValue = "someOtherId";

removeNamedItem()：返回表示被删除特性的Atrr节点：

    var oldAttr = element.attributes.removeNamedItem("id");

将DOM的狗序列化为XML或XHTML字符串，使用更便利元素属性。
迭代元素每一个特性，构造成name="value" name="value"这样的形式：

    function outputAttributes(element){
        var pairs = new Array(),
                    attrName,
                    attrValue,
                    i,
                    len;
        for(i=0; len=element.attributes.length; i<len; i++){
            attrName = element.attributes[i].nodeName;
            attrValue = element.attributes[i].nodeValue;
            apirs.push(attrName + "=\"" + attrValue + "\"");
        }
        return pairs.join(" ");
    }


创建元素：
使用document.createElement()方法可以创建新元素，接受一个参数：要创建元素的标签名。

    var div = document.createElement("div");

同时也设置了ownerDocument属性。可以同时操作元素的特性，添加子节点等：

    div.id = "myNewDiv";
    div.className = "box";

在新元素设置这些特性只是给它们赋予了相应的信息，由于新元素尚未被添加到文档树中，因策设置这些特性不会影响浏览器的显示。可以使用appendChild()/insertBefore()/replaceChild()方法将新元素添加到文档树。

    document.body.appendChild(div);

另一种使用方法：

    var div = document.createElement("<div id=\"myNewDiv\" class=\"box\"></div>");


元素的子节点：

如果需要childNodes属性遍历子节点，为避免浏览器之间差异，操作前通常都要先检查nodeType属性：

    far(var i=0, len=element.childNodes.length; i<len; i++){
        if (element.childNodes[i].nodeType == 1){
            doSomeThing();
        }
    }

获取某个特定的标签名的自己诶按点或后代节点：元素也支持getElementByTagName()方法：

    var ul = document.getElementById("myList");
    var items = ul.getElementByTagName("li");

需要注意，这里<ul>的后代中只包含直接子元素。不过如果包含更多层次的后代元素，那么各个层次中包含<li>元素也都会返回。


Text 类型

包含的是可以照字面解释的纯文本内容。可以包含转义后的HTML字符，但不嫩更包含HTML代码。

* nodeType的值为3
* nodeName的值为"#text"
* nodeValue的值为节点所包含的文本
* parentNode是一个Element
* 不支持（没有）子节点

可以通过nodeValue睡醒或data属性访问Text节点中包含的文本，这两个属性中包含的值相同，对nodeValue的修改也会通过data反映出来。

* appendDate(text)：将text添加到节点的末尾。
* deleteDate(offset, count)：从offset指定的位置开始删除count个字符。
* insertData(offset, text)：在offset指定的位置插入text。
* replaceDate(offset, count, text)：用text替换从offset开始到offset+count处的字符串。
* splitText(offset)：从offset指定的位置将当前文本节点分成两个节点。
* substringData(offset, count)：提取从offset指定的位置开始到offset+count为止处的字符串。
* 还有一个length属性，保存着节点中字符的数量。== nodeValue.length == data.length

默认每个可以包含内容的元素最多只能有一个文本节点，并且必须确实有内容存在。

    <!没有内容，也就没有文本节点-->
    <div></div>
    
    <!-->有空格，因而有一个文本节点</!-->
    <div> </div>
    
    <!-->有内容，有一个文本节点</!-->
    <div>Hello World</div>

访问文本子节点：

    var textNode = div.firstChild;      //或者div.childNodes[0];

修改文本节点：

    div.firstChild.nodeValue = "some other message";

在修改文本文档时，字符串会经过转义：

    //输出："Some &lt;strong;other&lt;/strong&gt; message"
    div.firstChild.nodeValue = "Some <strong>other</strong> message";


创建文本节点：

document.createTextNode()：接收一个参数：要插入节点中的文本
    
    var textNode = document.createTextNode("<strong>Hello</strong> world!");

创建一个div并向其添加一条信息：

    var element = document.creatElement("div");
    element.className = "massage";
    
    var textNode = document.createTextNode("Hello world");
    element.appendCHild(textNode);
    
    document.body.appendChild(element);

某些情况下一个元素肯恩更包含多个文本子节点

    var element = document.createElement("div");
    element.className = "message";
    
    var textNode = document.createTextNode("Hello");
    element.appendChild(textNode);
    
    var anotherTextNode = document.createTextNode("world");
    element.appendCHild(anotherTextNode);
    
    document.body.appendChild(element);


规范化文本节点：

normalize()：如果在一个包含两个或多个文本节点的父元素上调用normailze()方法，则会将所有文本节点合并成一个节点，结果节点的nodeValue等于将合并前每个文本节点的nodeValue值凭借其来的值：

    var element = document.createElement("div");
    element.className = "message";
    
    var textNode = document.creteTextNode("Hello world");
    element.appendChild(textNode);
    
    var anothertextNode = document.createTextNode("Yippee!");
    element.appendChild(anothertextNode);
    
    document.body.appendChild(element);
    
    alert(element.childNodes.length);       //2
    
    element.normailze();
    alert(element.childNodes.length);       //1
    alert(element.firstChid.nodeValue);     //"Hello world!Yippee!"


分割文本节点

splitText()：与normailze()相反。将一个文本节点分成两个文本节点，按照指定的位置分割nodeValue值。原来的文本节点包含从开始到指定位置之前的内容，新文本节点包含剩下的文本。返回一个新文本节点，该节点与原节点的parentNode相同。

    var element = document.createElement("div");
    element.className = "message";
    
    var textNode = document.createTextNode("Hello world");
    element.appendChild(textNode);
    
    document.body.appendChild(element);
    
    var newNode = element.firstChild.splitText(5);
    alert(element.firstChild.nodeValue);            //"Hello"
    alert(newNode.nodeValue);                       //"world"
    alert(element.childNodes.length);               //2


Comment 类型

注释在DOM通过Comment类型表示，特征：

* nodeType：8;
* nodeName: "#comment";
* nodeValue: 即注释的内容;
* prentNode: 可能是Document或者Element；
* 不支持（没有）子节点

Comment类型Text类型继承自相同的基类，因此又有除splitText()之外的所有字符串操作方法。与Text相似，也可以通过nodeValue或data属性来取得注释的内容。

    <div id="myDiv"><!-- A comment --></div>

    var div = document.getElementById("myDiv");
    var comment = div.firstChild;
    alert(comment.data);            //"A comment"

使用document.createComment()并为其传递注释文本也可以创建注释节点

    var comment = document.createComment("A comment");


CDATASection 类型

只针对基于XML的文档。不看了  P274


DocumentType 类型

也不常用。包含着与文档的doctype有关的所有信息。

    alert(document.doctype.name);


DocumentFragment 类型

在所有节点类型中，只有DocumentFragment在文档中没有对用标记。DOM规定文档片段（document fragment）是一种“轻量级”文档，可以包含和控制节点，但不会像完整的文档那样占用额外的资源。

* nodeType: 11;
* nodeName: "#document-fragment";
* nodeValue: null;
* parentNode: null;
* 子节点可以是Element/ProcessingInstruction/Comment/Text/CDATASection/EntityReference

虽然不能把文档片段直接添加到文档中，但可以将它作为一个“仓库”，保存将来可能会添加的文档中的节点：

    var fragment = document.createDocumentFragment();

如果将文档中的节点添加到文档片段中，就会从文档树中移除该节点，也不会从浏览器中再看到该节点。添加到文档片段中的新节点同样也不属于文档树。可以通过appendChild()或insertBefore()将文档片段中内容添加到。在将文档片段作为参数床底给这两个方法是，世界上只会将文档片段的所有子节点添加到相应位置上；文档片段永远不会成为文档树的一部分。

    <ul id="myList"></ul>

    var fragment = document.createDocumentFragment();
    var ul = document.getElementById("myList");
    var li = null;
    
    for(var i=0; i<3; i++){
        li = document.createElement("li");
        li.appendCHild(document.createTextNode("Item" + (i + 1)));
        fragment.appendChild(li);
    }
    
    ul.appendChild(fragment);


Attr 类型

元素的特性在DOM中以Attr类型表示。从技术角度讲，特性就是存在于元素的attributes属性中的节点

* nodeType: 2;
* nodeName: 特性的名称；
* nodeValue: 特性的值；
* parentNode: null;
* 在HTML中不支持（么有）子节点；
* 在XML中子节点可以是Text或EntityReference

特性不被认为是DOM文档树的一部分。
更多使用getAttrrbute()/setAttribute()/removeAttribute()方法，很少直接引用特性节点。

Attr 三个属性：name：特性名称（与nodeName的值相同）/ value：特性的值（与nodeValue的值相同）/ specified：布尔值，用来区别特性是在代码中指定的，还是默认。

为元素添加align：

    var attr = doc
    ument.createAttribute("align");
    attr.value = "left";
    element.setAttributeNode(attr);
    alert(element.attributes["align"].value);           //"left"
    alert(element.getAttributeNode("align").value);     //"left"
    alert(element.getAttribute("align"));               //"left"


#### DOM 操作技术

由于浏览器中 充斥着隐藏的陷阱和不兼容问题，用s代码处理DOM的某些部分有一些需要注意。

动态脚本：

这里指的是页面加载时不存在，但将来的某一时刻通过修改DOM动态添加的脚本。插入外部文件、直接插入js代码

    <script type="text/javascript" src="client.js"></script>

    var script = document.createElement("script");
    script.type = "text/javascript";
    script.src = "client.js";
    document.body.appendChild(script);

整个过程可以用函数封装：

    function loadScript(url){
        var script = document.createElement("script");
        script.type = "text/javascript";
        script.src = url;
        document.body.appendChild(script);
    }
    
    loadScript("client.js");

另外一种方式：

    <script type="text/javascript">
        function sayHi(){
            alert("hi");
        }
    </script>

IE将<script>视为一个特殊的元素，不允许DOM访问其子节点。不过可以使用</script>元素的text属性来指定js代码：

    var script = document.createElement("script");
    script.type = "text/javascript";
    script.text = "function sayHi(){alert("Hi")}";
    document.body.appendChild(script);

使用函数：

    function loadScriptString(code){
        var script = document.createElement("script");
        script.typt = "text/javascritp";
        try{
            script.appendChild(document.createTextNode(code));
        }catch (ex){
            script.text = code;
        }
        document.body.appendChild(script);
    }
    
    loacScritpString("function sayHi(){alert("Hi")}");


动态样式：

把CSS样式包含到HTML：<link rel="stylesheet" href="">/<style></style>

    <link rel="stylesheet" href="styles.css" type="text/css">

    var link = document.createElement("link");
    link.rel = "stylesheet";
    link.type = "text/css";
    link.href = "style.css";
    var head = document.getElementByTagName("head")[0];
    head.appendChild(link);

使用函数：

    function loadStyles(url){
        var link = document.createElement("link");
        link.rel = "stylesheet";
        link.type = "text/css";
        link.href = url;
        var head = document.getElementsByTagName("head")[0];
        head.appendChild(link);
    }
    
    loadStyles("styles.css");

使用style标签：

    <style type="text/css">
        body{
            background-color: red;
        }
    </style>

使用函数：

    function loadStyleString(css){
        var style = document.createElement("style");
        style.type = "text/css";
        try{
            style.appendChild(document.createTextNode(css));
        }catch (ex){
            style.styleSheet.cssText = css;
        }
        var head = document.getElementsByTagName("head")[0];
        head.appendChild(style);
    }
    
    loadStrleString("body{background-color:red;}");


操作表格：

<table></table>涉及的标签多，因而使用核心DOM方法创建和修改表格往往免不了要编写大量的代码。（老实说这里我不想写了，太多了。。）

    <table border="1" width="100%">
        <tbody>
            <tr>
                <td>cell 1, 1</td>
                <td>cell 2, 1</td>
            </tr>
            <tr>
                <td>cell 1, 2</td>
                <td>cell 2, 2</td>
            </tr>
        </tbody>
    </table>

要使用核心DOM创建这些元素，需要很多代码，这里不写了。P281

包括HTML DOM为<table></table>/<tbody></tbody>和<tr></tr>元素添加了一些属性和方法：  P282

使用这些属性和方法，可以极大的减少创建表格所欲的代码数量。

    //创建table
    var table = document.createElement("tabel");
    table.border = 1;
    table.width = 100%;
    
    //创建tbody
    var tbody = document.createElement("tbody");
    table.appendChild(tbody);
    
    //创建第一行
    tbody.insertRow(0);
    tbody.rows[0].insertCell(0);
    tbody.rows[0].cells[0].appendChild(document.createTextNode("cell 1, 1"));
    tbody.rows[0].insertCell(1);
    tbody.rows[0].cells[1].appendChild(document.createTextNode("cell 2, 1"));
    
    //创建第二行
    tbody.insertRow(1);
    tbody.rews[1].insertCell(0);
    tbody.rews[1].cells[0].appendChild(document.createTextNode("cell 1, 2"));
    tbody.rews[1].insertCell(1);
    tbody.rews[1].cells[1].appendChild(document.createTextNode("cell 2, 2"));
    
    //将表格添加到文档主体中
    document.body.appendChild(table);


使用 NodeList

NodeList/NamedNodeMap/HTMLCollection
这三个集合都是“动态的”，每当文档结构发生变化时，它们都会得到更新。从本质上说，所有NodeList兑现个都是在访问DOM文档时实时运行的查询。
下面代码会导致无限循环：

    var divs = document.getElementsByTagName("div"),
        i,
        div;
    
    for(i=0; i < divs.length; i++){
        div = document.createElement("div");
        document.body.appendChild(div);
    }

如果想要迭代一个NodeList，最好使用length属性初始化第二个变量，然后将迭代器与该变量比较。

    var divs = document.getElementsByTagName("div"),
        i,
        len,
        div;
    
    for (i=0, len=divs.length; i<len ;i++) {
        div = document.createElement("div");
        document.body.appendChild(div);
    }


---

### 第十一章  DOM扩展

对DOM的两个注意卓展是Selectors API（选择符API）和HTML5。都源自开发社区

#### 选择符API

根据CSS选择符现则与某个模式匹配的DOM元素。实际上jQuery的核心就是通过css选择符查询DOM文档取得元素的引用，从而抛开getElementById()和getElementsByTagName()。
两个核心方法：querySeletor()和querySelectorAll()。在兼容的浏览器中，可以通过Document和Element类型的实例调用他们。

querySelector()：接收一个css选择符，返回与该模式匹配的第一个元素，如果没有找到匹配的元素，返回null。

    //取得body元素
    var body = document.querySelector("body");
    
    //取得ID为'myDiv'的元素
    var myDiv = document.querySelector("#myDiv");
    
    //取得类为"selected"的第一个元素
    var selected = document.quertSelector(".selected");
    
    //取得类为"button"的第一个图像元素
    var img = document.body.querySelector("img.button");

通过Document类型调用querySelector()方法时，会在文档元素的范围内查找匹配的元素。而通过Element类型调用querySelector()方法时，之后再该元素后代元素的范围内查找匹配的元素。

querySelectorAll()：接收的参数与querySelector()相同，都是css选择符，但返回的是所有匹配的元素而不只是一个元素，返回的是一个NodeList实例。
具体来说，返回的值实际上是带有说有属性和方法的NodeList，而底层实现类似一组元素的快照，而非不断对文档进行搜索的带动态查询。、
能够调用querySelectorAll()的类型包括Document/Element和DocumentFragment。

    //取得某<div>中所有<em>元素（类似getElementsByTagName("em")）
    var ems = document.getElementById("myDiv").quertSelectorAll("em");
    
    //取得类为"selectored"的所有元素
    var selectoreds = document.quertSelectorAll(".selectored");
    
    //取得所有<p>元素中的所有<strong>元素
    var strongs = document.querySelectorAll("p storng");

要取得返回的NodeList中的每一个元素，可以使用item()方法，也可以使用方括号：

    var i, len, strong;
    for(i=0, len=strongs.length; i<len; i++) {
        strong = strongs[i];     //或者strongs.item[i]
        strong.className = "improtant";
    }


matchesSelector()  :  Selectors API Level2为Element新增的方法。接收一个css的选择符，如果调用元素与该选择符匹配，返回true。否则false。

    if(document.body.mathcesSelector("body.page1")){
        //true
    }

    在取得某个元素引用的情况下，使用这个而方法能够方便检测是否会被querySelector()或querySelectorAll()方法返回。



####  元素遍历

对于元素间的空格，除了IE9及之前版本，所有浏览器都会返回文本节点，就导致了使用childNodes和firstChild等属性时行为不一致。为了解决这个问题，新定义了一组属性规范。

* childElementCount: 返回子元素（不包括文本节点和注释）的个数
* firstElementChild: 指向第一个子元素； firstCHild的元素版
* lastElementChild: 指向最后一个子元素；lastChild的元素版
* previousElementSibling: 指向前一个同辈元素；previousSibling的元素版
* nextElementsibling: 指向后一个同辈元素；nextSibling的元素版

看一个要夸浏览器遍历某元素的而所有子元素的例子：

    var i, len, child = element.firstChild;
    whild(child != element.lastChild){
        if(child.nodeType == 1){        //检查是不是元素
            processChild(child);
        }
        child = child.nextSibling;
    }

若使用Element Traversal的新增元素：

    var i, len, child = element.firstElementChild;
    while(child != element.lastElementChild){
        processChild(child);        //已知其实元素
        child = child.nextElementSibling;
    }


#### HTML5

HTML5规则围绕如何使用新增标记定义了大量JS API。其中一些API与DOM重叠，定义了浏览器应该支持的DOM扩展。

class在html中被大量使用，为了简化css类的用法：

getElementByClassName(): 接收一个参数，一个包含一或多个类名的字符串，返回带有指定类的所有元素的NodeList。传入多个类名时，类名的先后顺序不重要。

    //取得所有类中包含"username"和"current"的元素，类名的先后顺序无所谓。
    var allCurrentUsernames = document.getElementsByClassName("username current");
    
    //取得ID为“myDiv”的元素中带有类名"selected"的所有元素
    var selected = document.getElementById("myDiv").getElementByClassName("selected");

使用这个方法可以更方便为带有某些类的元素添加时间处理程序，从而不必再局限于使用ID或标签名。不过因为返回的兑现格式NodeList，所以使用这个方法与使用getElementsByTagName()以及其他返回NodeList的DOM方法都具有同样的性能问题。

在操作类名时，通过修改className属性进行添加、删除和替换类名。因className是一个字符串，因此每次操作时需要重新设置整个字符串的值。

    <div class="bd user disabled">...</div>

    //删除"user"
    
    //首先取得类名字符串并拆分成数组
    var classNames = div.className.split(/\s+/);
    
    //找到要删除的类名
    var pos = -1, i, len;
    for (i=0, len=classNames.length; i<len; i++) {
        if(classNames[i] == "user") {
            pos = i;
            break
        }
    }
    
    //删除类名
    classNames.splice(i,1);
    
    //把剩下的类名拼成字符串
    div.classNames = classNames.join(" ");