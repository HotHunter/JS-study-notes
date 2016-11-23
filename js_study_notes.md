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
sort():默认情况按牲畜排列数组项（上大后小）。调用每个数组项的toString()，然后比较得到字符串，确定排序。即使数组每一项都是数值，sort()比较的也是字符串

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

