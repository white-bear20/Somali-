<style>
    body{
        font-size:16px;
    }
    h2{
        background-color:#00F9A5;
        font-size:40px;
        width:400px;
        text-align:center;
    }
    h3{
        color:#FF3C3C;
    }
</style>

# JavasScript学习笔记

<h2 id="menu">目录</h2>

1. # 基础
    * ### [什么是JavaScript?](#1)
        * 特点
    * ### [JavaScript写在哪?](#2)
        * 三种引入JS代码的方式
        * 三种样式的优先级
    * ### [注释](#3)
    * ### [输入和输出](#4)
    * ### [变量](#5)
    * ### [数据类型](#6)
        * js数据类型的分类
        * 关于数据类型的函数
        * 强制转换 & 隐式转换
    * ### [运算符](#7)
        * 算数运算符
        * 逻辑运算符
        * 比较运算符
        * 三目运算符
        * 逗号运算符
    * ### [函数](#8)
        * 定义
        * 作用域
        * 什么时候被调用
    * ### [数组](#9)
        * 什么是数组
        * 数组的类型
        * 数组的定义方式
        * 数组方法
    * ### [对象](#10)
        * 对象的创建
        * this关键字
    * ### [流程控制](#11)
        * 编程语言的三大流程结构
        * 判断结构
        * 循环结构
    * ### [日期](#12)
        * 定义方法
        * 日期格式
        * 日期的获取方法
        * 日期的设置方法
    * ### [数学](#13)
        * 数学方法
    * ### [正则表达式](#14)
        * 一些正则使用的函数
    * ### [异常](#15)
    * ### [this](#16)
    * ### [let](#17)
2. # 进阶
    * ### [DOM](#18)
        * 什么是DOM
        * 对象的HTML DOM树
        * DOM的一些方法
        * 查找HTML元素
        * 改变HTML
    * ### [事件](#19)
        * 什么是事件？
        * 常见的事件有哪些
        * 如何触发事件
        * 事件监听器
        * 使用事件监听器的好处
        * 事件冒泡和事件捕获
    * ### [DOM节点](#20)
        * 插入节点
        * 删除节点
        * 替换节点
    * ### [BOM](#21)
        * window对象
        * window.location对象
        * 获取cookie
3. # 高级
    * ### [AJAX](#22)
    * ### [JSON](#23)
* [问题栏](#problem)
***
# 基础
> <h2 id="1">什么是JavaScript?</h2>
*JavaScript是一种在网络浏览器上运行的编程语言。*  
### 特点
```  
    1.脚本语言。 JavaScript是一种解释型脚本语言。和要先编译再执行的C，C ++和其他语言不同，JavaScript是在程序运行期间逐行解释运行的。  
    2.基于对象。 JavaScript是一种基于对象的脚本语言，不仅可以创建对象，还可以使用现有对象。  
    3.简单。 JavaScript语言使用弱类型的变量类型。它对使用的数据类型没有严格要求。它是一种基于Java基本语句和控件的脚本语言。它的设计既简单又紧凑。  
    4.动态。 JavaScript是一种事件驱动的脚本语言，无需通过Web服务器即可响应用户输入。访问网页时，鼠标可以在网页上单击鼠标，或上下移动并移动窗口。 JavaScript可以直接响应这些事件。  
    5.跨平台。 JavaScript脚本语言不依赖于操作系统，仅需要浏览器支持。因此，只要机器上的浏览器支持JavaScript脚本语言，那么编写后就可以在任何机器上使用JavaScript脚本。目前，大多数浏览器都支持JavaScript。  
```
> <h2 id="2">JavaScript写在哪?</h2>
    JavaScript代码都是写在:
        <script type=type = "text/javascript" src="#"></script>
    这个标签当中的,一个页面当中可以有多个JS代码，它们并不冲突，采用从上往下的顺序执行
    它可以出现在页面中的任何位置
    注意：JS代码都要以 分号(;)结尾

    关于script标签的两个属性:
        type:用于指定使用语言，text/javascript表示js，一般不用写，默认就是js
        src:外部js文件位置，用于引入外部js文件，不过切记，如果引入了js文件，那么这个script标签中就不能写代码
### 三种引入JS代码的方式
        1.内联样式，代码写在HTML文件的<script>中，
        2.外部样式，代码写在js文件中，通过script标签的src属性引入
        3.行内样式，代码写在标签中
### 三种样式的优先级
    行内样式的优先级最低，内联样式和外部样式谁在前面谁就先执行。
    <!-- ps:其实我觉得行内样式的优先级应该也不是最低的，还是应该是谁在前面谁先执行，只不过行内样式一般都是写一些事件，需要被触发，才能执行，所以才感觉要低一等 -->

> <h2 id="3">注释</h2>
        JavaScript的注释方法有两种
        1. 单行注释，//
        2. 多行注释, /**/

> <h2 id="4">输入和输出</h2>
        1. prompt(string placeholder) 弹出一个输入框，用户可以进行输入，placeholder是输入框的提示信息
        2. alert(string str) 弹出一个警告框，str为警告框中的内容
        3. console.log(string str) 在浏览器控制台中输出str

> <h2 id="5">变量</h2>
    JS是弱类型语言，也就是说，在定义变量时，不需要指定数据类型。
    使用 var 关键字定义变量,例如：
    var value = 10; //这样就定义了一个变量

    定义变量名时，需要遵从变量的命名规范：
        1、由字母、数字、下划线(_)、美元符($)组成，且不能是数字开头。
        2、变量名长度不能超过255个字符。
        3、变量名区分大小写。(javascript是区分大小写的语言)。
        4、变量名必须放在同一行中。
        5、不能使用脚本语言中保留的关键字、保留字、true、false 和 null 作为标识符。

>  <h2 id="6">数据类型</h2>
### js数据类型的分类
1. 简单数据类型
    * 数字型(Number)  
        如果要写十六进制的话，可以在数字前面加上0x，就表示十六进制,`var num = 0xaf`  
        Infinity(表示无穷大)  
        -Infinity(表示无穷小)
        Number.MAX_VALUE //定义了最大值
        Number.MIN_VALUE //定义了 最小值
        * 数字对象方法:  
            num.toString() //把数字以字符串返回  
            num.toFixed(x) //返回包含了x位的小数的数字，不够就0补充，多了就截断  
            num.toPrecision(x) //返回x长度的数字,长度不够就小数点后加0来补

    * 字符型(String)
        str.length //返回字符串长度
        + //拼接字符串
        * 字符对象方法:  
            str.indexOf(val) //返回str中第一次匹配到val的下标  
            str.lastIndexOf(val) //返回str中最后一次匹配到val的下标   
            str.substr(start,length) //从strart位置开始截取length个字符  
            str.clice(start,end) //从start开始截取字符串,end为截取的结束位置，不过不写end，就到字符串结尾  
            str.substring(start,end) //和clice一样，唯一的不同点就是substring不支持负数索引  
            str.replace(oldstr,newstr) //用newstr去替换str中的oldstr，它默认只替换首次匹配到的字符，而且它不会改变原有字符，而是返回一个新的字符，大小写敏感  
            str.toUpperCase() //转为全大写  
            str.toLowerCase() //转为全小写  
            str.concat(str2,str3,...) //连接字符串，作用和+一样  
            str.trim() //删除字符两边空格  
            str.charAt(index) //根据下标获取字符  
            str.charCodeAt(index) //根据下标获取字符，返回unicode编码  
            str.split(char) //根据char作为分隔符，切割元素组成数组，如果不写char，字符串就是数组第一个元素  
    * 布尔类型(Boolean)
    * 未定义类型(Undefined)
    * 空值(Null)
2. 复杂数据类型
    * function
    * object
    * array
### 关于数据类型的函数
    typeof var //返回变量的类型
    isNaN(var) //判断变量是否是一个非数字值，不是数字返回True,是数字返回False
### 强制转换 & 隐式转换
    隐式转换:
        数值类型 + 字符类型 = 字符类型
        布尔类型 + 字符类型 = 字符类型
        数值类型 + 布尔类型 = 数值类型
        数值 [-*/] 字符类型 = 数值类型
    强制转换:
        num.toString() //数字转为字符串
        String(var) //把var的值传入，然后返回字符对象
        parseInt(numString) //将字符串转为整型
        parseFloat(numString) //将字符串转为字符型        
        Number(var) //转为数字型,字符一定要是纯数字
        Boolean(var) //把var转为布尔类型

>  <h2 id="7">运算符</h2>
### 赋值运算符
`=(赋值运算符) += -= *= /= %= &= |= **= ^=`
### 算数运算符
`+(加) -(减/负) *(乘) /(除) %(取余) ++(递增) --(递减) **(幂运算)`            
### 逻辑运算符
`&&(与) ||(或) !(非) &(按位与) |(按位或) ^(异或)`
### 比较运算符
`>(大于) <(小于) >=(大于等于) <=(小于等于) ==(等于) !=(不等于) ===(恒等于) !==(恒不等)`
### 三目运算
表达式1?值1:值2
### 逗号运算符
表达式1,表达式2,表达式3
按从左到右的顺序执行表达式

>  <h2 id="8">函数</h2>
### 定义
    function 函数名(参数1，参数2，...){
        功能代码
        return vaule;
    }
    //参数可以有多个，也可以没有，传参的目的主要是获取函数外变量的值。
    //函数可以有返回值，也可以没有
    function test(num){
        alert(num);
    }
    var n = 10;
    test(n)
    //这就相当于 num = n,函数中num怎么变无所谓，n是不会改变的
### 作用域
    在函数中，有全局变量和局部变量。
    全局变量
        JS中，在函数体外声明的变量就是全局变量，在函数中可以直接调用
    局部变量
        函数中声明的变量为局部变量，只能在声明它的函数中使用。
    注：如果函数中的变量名有和全局变量名重叠，那么函数中会使用局部变量，而不是全局
### 什么时候被调用
    1. 当事件发生时
    2. 当JS代码调用时
    3. 自动调用
### 可变参数
 使用arguments可以获取到传递给函数的所有变量,它是一个伪数组，可以通过下标去索引数据，但是不能插入数据和修改数据
```javascript
    function num(){
        alert(arguments);
    }
    num(11,2,1,5,9,4,10);
```
> <h2 id="9">数组</h2>
### 什么是数组
    数组是一个可以用来存储多个数据的东西，而且JS中的数组不限制存储是元素类型。
### 数组的类型
    索引数组 
    关联数组
### 数组的定义方式
`数组的定义方式比较多样，但大抵都差不多`
1. 使用Array()对象
```javascript
    $arrays = new Array(); //声明一个空数组
    $arrays = new Array("1",2,true); //声明一个有三个元素的素组
    console.log($arrays[0]);//打印字符1
```
2. 使用[]定义数组
```javascript
    $arrays = [];
    $arrays = ["1",2,true];
    console.log($arrays[0]); //这种方式和上面的一样，只是写法不同
```
`定义关联数组的方法`  
1.先定义数组，然后在赋值
```javascript
    $person = Array();
    $person['name'] = '张三';
    console.log($array['name']);
```
2.使用定义对象的{}，去定义，然后用的时候像数组一样用，不建议这样做，容易混淆
```javascript
    $person = {
        'name':'张三',
        'age':18
    };
```
### 数组方法
array.toString() //把数组转为字符串,但是拼接后的字符串，会用,号间隔,不会改变原有数组
array.join('拼接符')//把数组转为字符串，每个元素之间会用拼接符间隔,不会改变原有数组
array.pop() //弹出数组最后一个元素(删除数组最后一个元素，并返回这个元素)  
array.push() //向数组的末尾添加一个新的元素  
array.shift() //弹出数组首个元素，并且所有元素往前移一位  
array.unshift(element) //向数组首位插入元素，返回当前数组长度  
array.length //返回当前数组长度  
delete array\[index\]  //删除元素,被这种方法删除的元素它的数组位置的值会变成undefined,数组的长度不会改变，其他元素位置不动  
array.splice(start,length,val1,val2,...)//修改数组，从start开始删除length个元素，然后把后面的元素插进去,这个方法返回的是删除的元素，它会直接改变数组的值    
array.concat(array2) //合并数组，返回合并后的素组，但是不会改变原有数组  
array.slice(start\[,end\]) //切片，如果只有start，那么从start开始返回后面的元素，如果指定了end的话，那么到了end的位置就会结束(不包括end),它不会改变原有数组，而是返回一个新数组
array.sort() //排序，从小到大排序，会改变数组，而且它是从第一个字符开始比较,所以在比较数字时，可能会出现一些问题  
```javascript
    //解决这个问题的方法就是在sort中加入自定义函数，例如我要升序排序
    array.sort(function(a,b){ return a - b});
    //降序排序
    array.sort(function(a,b){ return b - a});
```
array.reverse() //将数组内的元素逆序   
array.forEach(callback(currentValue, index, array)) //这个方法会为数组中每个元素调用一次函数(回调函数),这个方法没有返回值   
array.map() //这个方法和array.forEach有点类似，不过也有不同之处，例如它有返回值  
```javascript
    //currentValue 是调用函数的当前元素
    //index 是当前调用元素的下标
    //array是当前调用forEach的数组
    //如果想改变数组中每个元素的值的话可以这样写
    var $array = Array(1,2,3,4,5);
        $array.forEach(function($element,$index,$arr){
            $arr[$index] = $element * 2;
        });
        console.log($array);
```
array.filter(callback(currentValue, index, array)) //这个方法会创建一个包含通过测试的数组元素的新数组。
```javascript
    var $array = Array(1,2,3,4,5);
        var $arr = $array.filter(function($element){
            return $element > 1
        });
        console.log($arr); //打印2,3,4,5
```
array.some(callback(currentValue, index, array)) //判断数字元素
array.every(callback(currentValue, index, array)) //这两个判断函数是一样的使用方法，some是只要有一个为true，就返回true，every是每个又要为true才返回true,她们不会改变原有数组，而是返回布尔值  
```javascript
    var $array = Array(1,2,3,4,5);
        var $bool = $array.every(function($element){
            return $element >= 5;
        });
        console.log($bool);
```
array.indexOf(element) // 方法在数组中搜索元素值并返回其位置。  
array.lastIndexOf(element) //从数组结尾开始搜索。  
array.find(callback(currentValue, index, array)) //方法返回通过测试函数的第一个数组元素的值。
array.findIndex(callback(currentValue, index, array) //方法返回通过测试函数的第一个数组元素的索引。


>  <h2 id="10">对象</h2>
*JS中对象创建方法，我认为是和其他语言有挺大区别的，首先它不用class关键字，其次它的定义是在{}中完成的*
### 对象的创建
    //第一种方法,通过{},在里面使用变量名:值/方法的方式来定义属性或者方法
    var Person = {
        name:"张三",
        age:18,
        show:function(){
                console.log("姓名:" + this.name + "年龄" + this.age);
            }
    };
    console.log(Person.show());
    //第二种方法，通过new Object(),把要创建的属性和方法，通过定义变量的方式去创建
    var Person = new Object();
        Person.name = "张三";
        Person.age = 18;
        Person.show = function(){
            console.log("名字：" + this.name + "年龄：" + this.age);
        };
        Person.show();
    //访问对象的属性，除了用.还可以用[],例如Person['name'] == Person.name
### this关键字
    在对象当中，this关键字表示对象本身（实例化），我们可以通过this在当前对象的函数中去调用当前对象中的值。
### get和set(对象访问器)
    var Person = {
        fullName:'',
        get full(){
            return this.fullName;
        },
        set full(name){
            return this.fullName = name;
        }
    };
    //设置好get和set之后，我们就能直接通过对象名去更改对应的属性
    Person = '张三';
    console.log(typeof Person);
### 对象构造器
*其实刚看JS对象的时候，我是有疑问的，因为如果对象的属性值什么的都要在定义的时候给上的话，那么意义在哪里，如果我要创建多个不同属性的对象，是不是多次定义呢，看到构造器我的问题有了答案*
```javascript
    //完整的写一边定义一个对象的过程，包括实例化
    function Pereson(name,age,sex){
        this.name = name;
        this.age = age;
        this.sex = sex;
        this.showTime = function(){
            console.log('我叫:' + name + '年龄:' + age + '性别为:' + sex);
        }
    }
    var person = new Person("张三","18","男");
    person.showTime();
    
```
### 原型继承
使用 prototype 属性，可以像对象添加属性或者方法
格式：对象名.prototype.addr = "江西";
用上面的那个例子的话，`Person.prototype.addr= this.addr;`

> <h2 id="11">流程控制</h2>
### 编程语言的三大流程结构
    1. 顺序结构
        代码从上往下执行，按部就班
    2. 判断结构
        进行条件判断，满足就执行，不满足就不执行
    3. 循环结构
        根据条件，来不停的执行同一段代码。

### 判断结构
`判断结构有两种语法，if和switch`

    //if
    1.满足就执行
        if(表达式){
            //表达式为真执行的代码
        }
    2.不是这个就是那个
        if(表达式){
            //表达式为真执行的代码
        }else{
            //表达式为假执行的代码
        }
    3.是这个还是那个，还是都不是
        if(表达式1){
            //表达式1为真执行的语句
        }else if(表达式2){
            //当表达式1为假，表达式2为真的时候就执行的语句
        }else{
            //都不满足的时候执行的语句
        }
`注意：判断结构，不管有几个分支，它只会执行一个，只要有一个条件满足，那么就执行其对应的代码快块，然后其他的判断分支代码块就都不会被执行了`
```javascript
    //switch
    switch(值){
        case n: 
            代码块
            break;
        case n2:
            代码块
            break;
        case n3:
            代码块
            break;
        default:
            代码块
    }
    //注意：switch(n)中的n一定要是一个具体的值，因为它是根据值去匹配括号中的n的，如果有匹配到的n，那么它就会执行后面的代码块，如果没有一个匹配到的就会执行default中的代码块
    //可以发现每个代码块后面都有一个break，这个关键字的作用是用来结束的，如果不加这个关键字的话，它会从匹配到的位置开始依次执行下面的代码块，例如从n匹配到了,它会执行n,n2,n3中的代码块
    //只要有匹配到的代码块，那么default就不会被执行
```

### 循环结构
`循环结构有三种语句样式while,do...while,for`
```javascript
    //while
    while(表达式){
        //代码块，只要表达式为true，代码块就会被执行，一直为ture，就一直执行
    }

    //do...while
    do
    {
        //代码块，这个和while类似，它们的区别就在于，while会先进行判断，如果不满足，就执行，那么它是有可能一次都不执行的，但是do...while是先执行然后在判断，那么就是最少要进行一次判断。
    }while(表达式);

    //for
    for(变量名;表达式;改变值){
        //代码块
        //关于for循环，第一次执行时，它会按顺序执行第一个和第二个语句
        //下次开始就会执行第三个语句，然后在进行判断（第二个语句）
    }
    //打印1-9
    for(var $each=1; $each < 10; ++$each){
        console.log($each);
    }
```
#### break关键字和continue关键字
    break的作用在于跳出，跳出当前语句
    continue的作用在于跳过，跳过当次循环
> <h2 id="12">日期</h2>
`在JS中有日期对象(Date),我们可以通过它来操作时间`
### 定义方法
    1. new Date()
        var $date = new Date(); //获取当前日期时间
        $date //为 Fri Oct 30 2020 09:00:48 GMT+0800 (中国标准时间) 注意时间不是固定的
    2. new Date(年,月,日,小时,分钟,秒,毫秒)
        //定义一个指定日期的对象，注意它这里月份是从0开始记录的，也就是说11表示12月
    3. new Date(milliseconds)
        /*
            JavaScript 将日期存储为自 1970 年 1 月 1 日 00:00:00 UTC（协调世界时）以来的毫秒数。
            零时间是 1970 年 1 月 1 日 00:00:00 UTC。
            所以通过这个方法，可以用毫秒定义时间
        */
    4. new Date(date string)
        //通过日期字符创建一个新的对象
        var $date = new Date("October 13, 2014 11:13:00"); 
### 日期格式
    ISO 日期 	"2018-02-19" （国际标准）
        var date = new Date("2018-02-19T10:20:00")
        //日期和时间之间通过大写字母T来分隔
    短日期 	"02/19/2018" 或者 "2018/02/19"
        var date = new Date('2018/02/19');
    长日期 	"Feb 19 2018" 或者 "19 Feb 2019"
        var date = new Date("Feb 19 2018");
    完整日期 	"Monday February 25 2015"
        var date = new Date("Mon Feb 19 2018 06:55:23 GMT+0800 (中国标准时间)");
### 日期获取方法
    Date.getDate() //以数值返回天(1-31);
    Date.getDay() //以数值获取周名(0-6),0表示星期天;
    Date.getFullYear() //以获取四位的年(yyyy)
    Date.getHours() //获取小时
    Date.getMilliseconds() //获取毫秒
    Date.getMinutes() //获取分钟
    Date.getMonth() //获取月(0-11)
    Date.getSeconds() //获取秒(0-59)
    Date.getTime() //获取时间
### 日期设置方法
    Date.setDate() 	//以数值（1-31）设置日
    Date.setFullYear() 	//设置年（可选月和日）
    Date.setHours() 	//设置小时（0-23）
    Date.setMilliseconds() 	//设置毫秒（0-999）
    Date.setMinutes() 	//设置分（0-59）
    Date.setMonth() 	//设置月（0-11）
    Date.setSeconds() 	//设置秒（0-59）
    Date.setTime() 	//设置时间（从 1970 年 1 月 1 日至今的毫秒数）

> <h2 id="13">数学</h2>
`JS里面内置了一个关于数学运算的对象，它里面有一些数学运算的方法，以及一些常量，例如Math.PI,表示圆周率，返回3.141592653589793`
#### 数学方法
    Math.round(x) //返回一个整数，有小数的话就四舍五入
    Math.pow(x,y) //返回x的y次幂,JS种有 **来做幂运算
    Math.sqrt(x) //返回X的平方根
    Math.abs(x) //返回X的绝对值
    Math.ceil(x) //返回X上舍后的值
    Math.floor(x) //返回X下舍后的值
    Math.min(1,2,3,4,5) //返回数集中的最小值
    Math.max(1,2,3,4,5) //返回数集中的最大值
    Math.random() //返回一个0与1(不包括)之间的随机数
        使用Math.floor配合Math.random使用，指定随机范围，Math.floor(Math.random() * 10) + 1就是0~10之间

> <h2 id="14">正则表达式</h2>
`JS中的正则表达式写在//中就好,语法:/pattern/modifiers;`  
`pattern的语法就不说了,modifiers的选择有i(不区分大小写)，g(全局匹配，不会匹配到一个后就停止),m(多行匹配)`
### 一些使用正则的函数
    str.search(pattern) //返回匹配字符串的下标
    str.replace(pattern,new str)//返回被替换后的字符

    正则方法
    var $pattern = /\d/;
    $pattern.test('123456789'); //test方法，搜索字符串，匹配成功返回true,不成功返回false
    $pattern.exec('123456789'); //exec方法，返回匹配到的文本

> <h2 id="15">异常</h2>
### 语法
    try{
        测试的代码块
    }catch(err){
        处理错误的代码,//上面这个err是一个Error对象，它有name和message两个属性，name为出现异常的名称，message为异常的提示消息
    }
### throw关键字
    throw关键字是用于抛出异常的，它能抛出的错误可以是字符串，数字，布尔，或者对象
    例如下面这个案例
```javascript
    var $input = prompt('请输入0-5之间的数');
    try{
        if($input >= 0 && $input <= 5){
            throw true;
        }else{
            throw false;
        }
    }catch(err){
        console.log(err);
    }
```
### finally语句{
    //finally的代码块是在try和catch后必须执行的代码，无论结果如何
    try{

    }catch(err){

    }finally{
        
    }
}

> <h2 id="16">this</h2>
*JS中的this作用还是挺多的*
### 什么是this?
    JavaScript this 关键词指的是它所属的对象。
    它拥有不同的值，具体取决于它的使用位置：
    在方法中，this 指的是所有者对象。
    单独的情况下，this 指的是全局对象。
    在函数中，this 指的是全局对象。
    在函数中，严格模式下，this 是 undefined。
    在事件中，this 指的是接收事件的元素。

> <h2 id="17">let 和 const</h2>
### let
    let这个关键字也是用来声明变量的，它和var的区别就在于作用域，let声明的变量，如果在块中，那么它的作用域只作用于块
        {
            var a = 0;
            let b = 0; 
        }
        console.log(a); //能访问
        console.log(b); //不能访问
### const
    通过const声明的变量它的作用域是和let一样的，但是注意通过const定义的常量并不是不可更改的，它只是不能被重新赋值
    例如:
        const list = [1,2,3,4,5];
        list = null; //这是不行的
        list[0] = 10; //这是可以的

# 进阶
> <h2 id="18">DOM</h2>
### 什么是DOM
    DOM(Document Object Model)被称为文档对象模型，当页面被加载时，浏览器会创建页面的文档对象模型(DOM),通过这个模型我们可以对页面中的元素进行操作。

    w3school解释
        DOM 是一项 W3C (World Wide Web Consortium) 标准。
        DOM 定义了访问文档的标准：
        “W3C 文档对象模型（DOM）是中立于平台和语言的接口，它允许程序和脚本动态地访问、更新文档的内容、结构和样式。” 
        W3C DOM 标准被分为 3 个不同的部分：
            Core DOM - 所有文档类型的标准模型
            XML DOM - XML 文档的标准模型
            HTML DOM - HTML 文档的标准模型

### 对象的HTML DOM树
![alt DOM](./image/dom.gif)

### DOM的一些方法
`文档对象代表网页，如果我们要访问HTML中的任何元素，那么是要从document对象开始的`

    查找HTML元素
        document.getElementById(id) //通过元素id查找元素
        document.getElementByTagName(name) //通过标签名来查找元素
        document.getElementByClassName(name) //通过类名来查找元素
    改变HTML元素
        element.innerHTML = new html content 	//改变元素的inner HTML
        element.attribute = new value 	//改变 HTML 元素的属性值
        element.setAttribute(attribute, value) 	//改变 HTML 元素的属性值
        element.style.property = new style 	//改变 HTML 元素的样式
    添加和删除元素
        document.createElement(element) 	//创建 HTML 元素
        document.removeChild(element) 	//删除 HTML 元素
        document.appendChild(element) 	//添加 HTML 元素
        document.replaceChild(element) 	//替换 HTML 元素
        document.write(text) 	//写入 HTML 输出流
    查找HTMLL对象
        document.baseURI 	返回文档的绝对基准 URI
        document.cookie 	返回文档的 cookie
        document.documentURI 	返回文档的 URI
        document.documentMode 	返回浏览器使用的模式
        document.domain 	返回文档服务器的域名
### 查找HTML元素
#### 1.通过ID查找
```javascript
    //html
    <p id='demo'></p>
    //js
    var myElement = document.getElementById('demo');
```    
#### 2.通过标签寻找
```javascript
    //获取所有的P标签，它返回的是一个对象，我们可以通过数组索引的方式去访问里面所有的P标签
    var myElement = document.getElementByTagName('p');
```
#### 3.通过类名查找
```javascript
    //获取所有类名为demo的标签，它也是通过下标去对这个集合进行索引
    var myElement = document.getElementByClassName('demo');
```
#### 4.通过 CSS 选择器查找 HTML 元素
```javascript
    //这里获取的是所有类名为intro的p标签
    var x = document.querySelectorAll("p.intro");
```
### 改变HTML
#### 改变输出
    document.write('<h2>hello</h2>'); //直接在页面底部插入元素
#### 改变HTML内容
    doucment.getElementById('x').innerHTML = "modify"; //把指定对象的元素内容改为modify
#### 改变属性值
    document.getElementById('x').attribute = new value; //attribute为要改变的属性名，new value为新的属性值
#### 更改CSS样式
    document.getElementById(id).style.property = new style   //property为要改变的样式属性，new style 为新的属性值 

>  <h2 id="19">事件</h2>
### 什么是事件
    事件，是发生在HTML元素上的事情，当HTML页面中使用了JS时，我们就可以使用JS来设置事件的触发要求，以及触发后要做什么。
    可以把事件想成一系列事情，只不过这些事情需要某种触发条件，只要条件满足，那么就会执行设定好的一系列代码。
### 常见的事件有哪些？
    //下面关键词后面写的都是触发事件的要求
    onclick 点击
    onchange 元素改变
    onmouseover 在HTML元素上移动鼠标
    onmouseout 从HTML元素上移开
    onkeydown 用户按下键盘
    onload 页面加载完成
### 如何触发事件
    //两种调用方法
    //1.在标签内调用
    <a onclick="myclick();">
    //2.通过JS获取标签，然后调用onclick方法
    //这里用了window.onload主要是解决一个，JS代码先被加载执行，导致找不到元素的问题
    window.onload = function(){
            //获取标签id属性值为show的标签
            var oA = document.getElementById('show');
            //设置点击事件
            oA.onclick = function(){
                alert('1');
            }
    };
### 事件监听器
`如果正常给元素添加事件处理程序，那么添加重复的事件是会覆盖掉之前的事件的，而使用事件监听器可以解决这个问题`  
语法:`element.addEventListener(event, function, useCapture);`   
    第一个参数是事件的类型（比如 "click" 或 "mousedown"）。  
    第二个参数是当事件发生时我们需要调用的函数。  
    第三个参数是布尔值，指定使用事件冒泡还是事件捕获。此参数是可选的,默认是false冒泡，使用true就是捕获。  
注意:不要在事件前面加on
### 使用事件监听器的好处
    addEventListener() 方法为指定元素指定事件处理程序。
    addEventListener() 方法为元素附加事件处理程序而不会覆盖已有的事件处理程序。
    您能够向一个元素添加多个事件处理程序。
    您能够向一个元素添加多个相同类型的事件处理程序，例如两个 "click" 事件。
    您能够向任何 DOM 对象添加事件处理程序而非仅仅 HTML 元素，例如 window 对象。
    addEventListener() 方法使我们更容易控制事件如何对冒泡作出反应。
    当使用 addEventListener() 方法时，JavaScript 与 HTML 标记是分隔的，已达到更佳的可读性；即使在不控制 HTML 标记时也允许您添加事件监听器。
    您能够通过使用 removeEventListener() 方法轻松地删除事件监听器。


### 事件冒泡和事件捕获
    在 HTML DOM 中有两种事件传播的方法：冒泡和捕获。
    事件传播是一种定义当发生事件时元素次序的方法。假如 <div> 元素内有一个 <p>，然后用户点击了这个 <p> 元素，应该首先处理哪个元素“click”事件？
    在冒泡中，最内侧元素的事件会首先被处理，然后是更外侧的：首先处理 <p> 元素的点击事件，然后是 <div> 元素的点击事件。
    在捕获中，最外侧元素的事件会首先被处理，然后是更内侧的：首先处理 <div> 元素的点击事件，然后是 <p> 元素的点击事件。
    在 addEventListener() 方法中，可以通过使用“useCapture”参数来规定传播类型

> <h2 id='20'>DOM节点</h2>
`DOM中的节点就是HTML文档中的那些元素`
### 插入节点
#### 在节点末尾插入节点
```javascript
    //HTML
    <div id="div1">
        <p>我是一个标签</p>
    </div>
    
    //JS
    <script>
        var para = document.createElement('p') //这里创建了一个p的节点，也就是p标签
        var node = document.createTextNode('我是新来的标签'); //这里创建了一个文本节点
        para.appendChild(node); //把node节点加入到para节点中，相当于 <p>我是新来的标签</p>
        var fatherBox = document.getElementById('div1');//获取父容器节点
        fatherBox.appendChild(para); //将刚才创建好的节点插入到div1这个节点的末尾
    </script>
```
#### 指定位置插入
```javascript
    //HTML
    <div id="div1">
        <p id="p1">我是一个标签</p>
    </div>

    //JS
    <script>
        var para = document.createElement('p');
        var node = document.createTextNode('我是新来的标签');
        var fatherBox = document.getElementById('div1');
        var pElement = document.getElementById('p1');
        para.appendChild(node);
        fatherBox.insertBefore(para,pElement); //注意这个位置，使用的是insertBefore方法，第一个参数是要插入的节点，第二个参数是插入的位置，插入的节点会出现在插入的位置上面
    </script>
```
### 删除节点
`删除指定元素需要获取到父元素，下面有两种方法`
```javascript
    var fatherBox = document.getElementById('div1');
    var child = document.getElemenetById('p1');
    fatherBox.removeChild(child);//这样就删除了id=p1的子节点

    //还有一种方法
    var child = document.getElementById('p1');
    child.parentNode.removeChild(child); //这里是利用parentNode去获取child这个节点的父元素，然后对节点进行删除
```

### 替换节点
```javascript
    var para = document.createElement('p');
    var node = document.createTextNode('冒名顶替');
    var fatherBox = document.getElementById('div1');
    var pElement = document.getElementById('p1');
    para.appendChild(node);
    fatherBox.replaceChild(para,pElement);
```

> <h2 id="21">BOM</h2>
`BOM(Browser Object Model)浏览器对象模型，它可以让JS与浏览器进行对话，即与浏览器进行交互`
### window对象
    window表示浏览器的窗口，所有的全局Javascript对象，函数和变量自动变成window对象的属性，全局函数是window对象的方法，之前我们使用的document也是window对象的属性之一。
#### 获取窗口尺寸
    windows.innerHeight //浏览器窗口内的高度(像素)
    windows.innerWidth //浏览器窗口内的宽度(像素)
### window.location对象
`window.location对象可用于获取当前页面地址(URL)并把浏览器重定向到新页面，书写时可以不带window前缀`
    location.href //返回当前页面的url
    location.hostname //返回web主机的域名
    location.pathname //返回当前页面的路径或者文件名
    location.protocol //返回使用的WEB协议
    location.port //返回端口
### 获取cookie
    document.cookie
***

# 高级
> <h2 id='22'>AJAX</h2>
### 什么是AJAX
    AJAX = Asynchronous JavaScript And XML.
    AJAX 并非编程语言。
    AJAX 仅仅组合了：
        浏览器内建的 XMLHttpRequest 对象（从 web 服务器请求数据）
        JavaScript 和 HTML DOM（显示或使用数据）
    Ajax 是一个令人误导的名称。Ajax 应用程序可能使用 XML 来传输数据，但将数据作为纯文本或 JSON 文本传输也同样常见。
    Ajax 允许通过与场景后面的 Web 服务器交换数据来异步更新网页。这意味着可以更新网页的部分，而不需要重新加载整个页面。
### AJAX实现的功能
    1.不刷新页面更新网页
    2.在页面加载后从服务器请求数据
    3.在页面加载后从服务器接收数据
    4.在后台向服务器发送数据
### 使用AJAX
#### XMLHttpRequest对象
`作为AJAX的核心，所以我们有必要先从它开始，通过它我们可以与幕后服务器交换数据，而不用重新加载整个页面`  
```
    XMLHttpRequest对象方法
        abort() //取消请求
        getAllResponseHeaders() //返回头部信息
        getResponseHeader() //获取特定的头部信息
        open(method,url,async,user,psw) //规定请求
            method:请求类型GET/POST
            url:请求的文件位置
            async:是否异步，true异步，false同步
            user:用户名
            psw:密码
        send() //发送GET请求
        send(string) //发送POST请求，send("key1=val1&key2=val2")
        setRequestHeader(header,value) //像要发送的报头添加键值对,header是规定头部的名称，value是规定头部值
    
    XMLHttpRequest对象属性
        onreadystatechange 	//定义当 readyState 属性发生变化时被调用的函数
        readyState 	//保存 XMLHttpRequest 的状态。
                0：请求未初始化
                1：服务器连接已建立
                2：请求已收到
                3：正在处理请求
                4：请求已完成且响应已就绪
        responseText 	//以字符串返回响应数据
        responseXML 	//以 XML 数据返回响应数据
        status 	//返回请求的状态号
                200: "OK"
                403: "Forbidden"
                404: "Not Found"
        statusText 	返回状态文本（比如 "OK" 或 "Not Found"）
```
#### 第一次使用AJAX
`使用AJAX进行GET提交`  
PHP代码
```php
    <?php
        if(isset($_GET['note'])){
            echo $_GET['note'];
        }
    ?>
```
HTML代码
```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>ajax</title>
        <script>
            // AJAX函数
            function ajax(){
                var XMLHttp = new XMLHttpRequest();
                var str = document.getElementById('text').value;
                XMLHttp.onreadystatechange = function(){
                    if(this.status == 200 && this.readyState == 4){
                        document.getElementById('p1').innerHTML = XMLHttp.responseText;
                    }
                }
                XMLHttp.open('GET','ajax.php?note='+str,true);
                XMLHttp.send();
            }
        </script>
    </head>
    <body>
        <form>
            <input type="text" id="text">
            <input type="submit" value="提交" onclick="ajax();">
            <p id="p1"></p>
        </form>
    </body>
    </html>
```
#### AJAX实现POST请求
PHP代码
```php
    <?php
        if(isset($_POST['note'])){
            echo $_POST['note'];
        }
```
HTML代码
```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>AJAX的POST实现</title>
        <script>
            function ajax(){
                var XMLHttp = new XMLHttpRequest();
                var oInput = document.getElementById('input').value;
                XMLHttp.onreadystatechange = function(){
                    if(this.status == 200 && this.readyState == 4 ){
                        document.getElementById('p1').innerHTML = this.responseText;
                    }
                }
                XMLHttp.open('POST','ajax.php',true);
                XMLHttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");//这条属性要加，是用来做POST提交时要求像form标签那样对数据进行编码的
                XMLHttp.send('note='+oInput);
            }
        </script>
    </head>
    <body>
        <input type="text" name="note" id="input">
        <input type="button" value="提交" onclick="ajax();">
        <p id="p1"></p>
    </body>
    </html>
```
### 跨域问题
    跨域是指从一个域名的网页去请求另一个域名的资源。比如从www.baidu.com 页面去请求 www.google.com 的资源。但是一般情况下不能这么做，它是由浏览器的同源策略造成的，是浏览器对JavaScript施加的安全限制。跨域的严格一点的定义是：只要 协议，域名，端口有任何一个的不同，就被当作是跨域
    所谓同源是指，域名，协议，端口均相同。这里说的js跨域是指通过js在不同的域之间进行数据传输或通信，比如用ajax向一个不同的域请求数据，或者通过js获取页面中不同域的框架中(iframe)的数据。

> <h2 id="23">JSON</h2>
### 什么是JSON
JSON: JavaScript Object Notation（JavaScript 对象标记法,JSON 是一种存储和交换数据的语法,JSON 是通过 JavaScript 对象标记法书写的文本。  
一种用来做数据传输，非常轻便快捷的数据结构,本质上就是一个字符串
### 语法格式
JSON的数据格式为
{
    "键":"值",  
    "键":"值",  
    "键":"值",  
    "键":"值"  
}

JS定义JSON数据
```javascript
    var myOBJ = {
        name:"Bill",
        age:"18",
        city:"南昌",
        phone:[1,2,3]
    }
    var myJSON = JSON.stringify(myOBJ);
```

JS解析JSON数据
```javascript
    var myOBJ = JSON.parse(myJSON);
```
### 本地存储JSON
```javascript
//存储数据：
    myObj = { name:"Bill Gates",  age:62, city:"Seattle" };
    myJSON =  JSON.stringify(myObj);
    localStorage.setItem("testJSON", myJSON); //通过使用localStorage.setItem来进行本地数据存储，testJSON就是存储值的键
    //接收数据：
    text = localStorage.getItem("testJSON"); //通过键取出值
    obj =  JSON.parse(text);
    document.getElementById("demo").innerHTML = obj.name;
```
### JSON数据书写方式
    JSON 语法衍生于 JavaScript 对象标记法语法：
        数据在名称/值对中
        数据由逗号分隔
        花括号容纳对象
        方括号容纳数组
    JSON中的名称(键)必须是字符串，用双引号包围

    JSON中的值必须是以下几样
        字符串
        数字
        对象（JSON 对象）
        数组
        布尔
        null
        函数
        日期
        undefined
    
    JSON文件
        JSON 文件的文件类型是 ".json"
        JSON 文本的 MIME 类型是 "application/json"

***
> <h1 id="problem">问题栏</h1>
### 1. 如果在引入文件的script标签中书写js代码，会是什么结果
    只会执行JS文件中的代码。
### 2. 两个JS文件，在JS1中定义变量，JS2中输出，是否能够成功
    如果JS1的引入在JS2上面的话，那么是没问题的
### 3.如果只写函数名，不加(),它返回的是什么
    返回的是函数的定义(函数的具体代码)
### 4.感觉JS写起来太松散了，因为很多错误都会有人搽屁股，那么怎么让它严格一点呢
    在函数或者代码的开头加上`"use strict"`,JS就会以严格模式执行，它不允许很多事情，例如不写var去定义变量
[回到顶部](#menu)