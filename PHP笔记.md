# PHP学习笔记
<h2 id=menu>目录</h2>  

* # 基础
    1. ### [PHP代码标记](#easy1)
    2. ### [PHP注释](#easy2)
    3. ### [php语句分割符](#easy3)
    4. ### [变量](#easy4)
        * 变量的定义
        * 变量的类型
        * 预定义变量
        * 可变变量 & 可变函数
        * 变量传值
        * 操作变量的函数
        * 全局变量与局部变量
    5. ### [语言构成](#easy5)
    6. ### [常量](#easy6)
        * 常量的定义方式
        * 系统常量
        * 魔术常量
    7. ### [运算符](#easy7)
        * 算数运算符
        * 比较运算符
        * 逻辑运算符
        * 字符运算符
    8. ### [PHP基本控制结构](#easy8)
        * 线性结构
        * 选择结构
        * 循环结构
        * break和continue
    9. ### [字符串](#easy9)
        * 定义方法
        * 定义字符编码
        * 字符函数
    10. ### [数组](#easy10)
        * 数组类型
        * 定义方法
        * 增删改查
        * 常用函数
    11. ### [函数](#easy11)
        * 定义
        * 有条件的函数
        * 可变函数
        * 匿名函数
    12. ### [类和对象](#easy12)
* # 高级阶段
    1. ### [会话技术](#high1)
        * cookie
        * session
    2. ### [文件管理](#high2)
        * 文件包含
        * 文件上传
    3. ### [MYSQL](#high3)
        * PHP操作MYSQL的办法
        * 
***

# 基础阶段
> 1. <h2 id="easy1"> PHP代码标记 </h2>
```php
    #PHP有多种标记风格
    //1.短标记，需要在php.ini中进行配置，short_open_tag=On,才会被启动，默认是禁用的
    <? PHP代码写这里 ?>
    //2.标准标记
    <?php PHP代码写这里 ?>

    /*
        本来还有asp风格标记，还有<script language="php"></script>，这两种标记风格，不过我试了都没用，可能是被淘汰了吧
    */

```

> 2. <h2 id="easy2"> PHP注释 </h2>
```php
    PHP有三种注释方法
    1.#单行注释
    2.//单行注释
    3./*
        多行注释
    */
```

> 3. <h2 id="easy3"> PHP语句分割符 </h2>
```php
    php的每一条语句都是以;结尾。
    但是PHP语句的最后一个语句可以不写;号，因为?>就表示结束了，不写也行，不过还是建议写上，只是了解
    如果只写<?php,不写?>,是没什么问题的，这就表示从<?php开始到这个文件的末尾，都是php代码，都需要被解析。
```

> 4. <h2 id="easy4"> 变量 </h2>
* ### 变量的定义(在PHP中变量的定义只需要 $变量名 就能完成定义)
```php
    /*
        变量的命名规则：
            1.变量名一定是“$”开头
            2.变量名只能由 字母 数字 下划线构成，且不能以数字开头
            3.变量名可以是汉字，但是不建议使用
    */
    $val;
    $value = 10;
```

* ### 变量的类型
    `PHP只是在定义时不需要确定数据类型，但是实际上数据是有类型的`
    * 基础类型
        * int(4)
        * float/double(8)  
        * 注：如果数值的容量超过了int就会变为浮点型
        * string(大小不限)
        * bool
    * 复合类型
        * array
        * object
    * 特殊类型
        * resource(资源类型)
        * null

* ### 预定义变量(在PHP中已经提前定义好了的变量，注意：这些变量都是数组)
```php
    $_GET //获取HTTP请求包当中的GET参数
    $_POST //获取HTTP请求包当中的POST参数
    $GLOBALS //包含的定义的全部变量在里面，索引方法就是变量名，例如$GLOBALS["val"]
    $_FILES //获取上传文件的信息,这是一个二维数组，第一个数组名是form标签的name值
        /*
            $_FILES[""]["name"] //文件上传时的名称
            $_FILES[""]["type"] //文件的MIME类型，由浏览器提供
            $_FILES[""]["tmp_name"] //文件上传后作为临时文件时的名称
            $_FILES[""]["size"] //文件大小
            $_FILES[""]["error"] //文件上传过程中的错误
                UPLOAD_ERR_OK 
                值：0; 没有错误发生，文件上传成功。 
                UPLOAD_ERR_INI_SIZE 
                值：1; 上传的文件超过了 php.ini 中 upload_max_filesize 选项限制的值。 
                UPLOAD_ERR_FORM_SIZE 
                值：2; 上传文件的大小超过了 HTML 表单中 MAX_FILE_SIZE 选项指定的值。 
                UPLOAD_ERR_PARTIAL 
                值：3; 文件只有部分被上传。 
                UPLOAD_ERR_NO_FILE 
                值：4; 没有文件被上传。 
                值：5; 上传文件大小为0. 
        */
    $_SERVER //存储一些服务器的所获取的信息，并不是所有信息都会被获取，例如$_SERVER["SERVER_ADDR"]表示当前运行脚本所在的服务器的IP地址
    $_COOKIE //存储COOKIE值
    $_SESSION //存储SESSION值
    $_REQUEST //可以获取到GET和POST参数
    $_ENV //
```

* ### 可变变量 & 可变函数
```php
    #PHP支持可变变量和可变函数这两个概念
    /*
        什么是可变变量呢？
        $变量1 = "变量2的名字"
        $变量2 = "value"
        $$变量1
    */
    $a = "bb";
    $bb = "可变变量";
    echo $$a;
    #可以看看会输出什么

    /*
        什么是可变函数？
        PHP 支持可变函数的概念。这意味着如果一个变量名后有圆括号，PHP 将寻找与变量的值同名的函数，并且尝试执行它。可变函数可以用来实现包括回调函数，函数表在内的一些用途。
        但是可变函数不能用于语言构造器，例如
        echo，print，unset()，isset()，empty()，include，require 以及类似的语言结构。需要使用自己的包装函数来将这些结构用作可变函数。
    */

```

* ### 变量传值
    现在有 val1 和 val2，两个变量
    1. 值传递  
        * 将val1的变量传递给val2,这两者之间除了值是一样的以外，没有其他关系。
    2. 引用传递
        * 相当于给val1所对应的内存地址取了一个val2的别名，在这种情况下这两个变量实际上是一样的，如果改变val1的值，那么val2的值也会被改变。

    #### 内存的五大分区（运行中的程序在内存中存放的）
    ```
        1.栈区(statck) 
            由编译器自动分配和释放，这个区域存放了程序运行代码，例如用于存放局部变量、函数参数、函数返回值。特点：效率高，但空间大小有限。

        2.堆区(heap)
            由程序员手动申请和手动释放的，如果没有释放，在程序运行结束时可能由OS回收。特点：使用灵活，空间较大，但容易出错，效率低。

        3.数据区(data)
            由两个部分组成:数据区 & BBS
            数据区--存储初始化后的全局变量和静态变量还有常量放在该区。
            BBS--没有进行未初始化操作的全局变量和静态变量放在该区，会被自动初始化为0。
        
        4.文字常量区
            存放常量字符串，该区一般用于存储只读数据的

        5.代码区
            存放函数体的二进制代码，不会执行，只是存放代码。
    ```

    #### 变量赋值的背后过程
    ```php
        #例举下面这段代码的执行过程
        $a = "value";
        $b = $a;

        /*
            1.PHP编译器从PHP脚本文件中将代码读取出来，进行编译，将编译后好后的二进制数据放到代码段（字节码）
            2.系统从代码段中一行一行的执行代码
                a.当运行到$a=1这行时，系统会在栈区开辟一块内存空间给$a,在数据区开辟一块内存空间存储"value",这时$a这块内存中存储的就是"value"这个数据的内存地址
                b.当运行到$b=$a这行时，系统会在栈区再开辟一块内存空间给$b,然后发现进行的是赋值运算，就会取出$a的值，在数据区开辟一块内存储进去，然后$b的这块内存空间保存新开辟的这块内存的内存地址
        */
    ```
* ### 操作变量的函数
    * `unset($var)`删除变量
    * `isset(mixed $var[,mixed $var,...]):bool` 检测$var是否被定义，定义返回TRUE
    * `gettype($var)`返回变量的类型
    * `var_dump($var)`详细输出变量的类型和内容
    * `empty($var)`检测变量是否为空，不为空返回false,为空返回true
* ### 全局变量 与 局部变量
`在php中，没有被任何东西包裹，写在文件中的就是全局变量。而写在函数中的就是局部变量，还是那个道理，如果在函数中去直接改变全局变量是不可以的。`

#### 想要改变，有三种方法
1. 使用global关键字，在函数中用global关键字声明变量就可改变
```php
$a = 10;
function fun(){
    global $a;
    $a = a+10;
}
fun();
echo $a;
```
2. 使用$GLOBALS这个预定义变量
```php
$a = 10;
function fun(){
    $GLOBALS['a'] = $GLOBALS['a'] + 10;
}
fun();
echo $a;
```
3. 传参时，用的是引用传递
```php
$a = 10;
function fun(&$val){
    $val = $val + 10;
}
fun($a);
echo $a;
```
> 5. <h2 id="easy5">语言构成</h2>
```php
    /*在这里记语言结构这个概念，主要是因为之前有提到可变函数*/
    语言构成关键词
        echo() exit()print() die() isset() unset() include()
        require() array() list() empty()
        注意，include_once() require_once()是函数

    语言构成所使用到的单词其实是关键词，虽然它们用起来像函数，但是实际上更类似于if,while这种控制语句，而不是一个函数。
    
    例如说 echo "Hello"这个表达式，PHP并不会把他转换成函数调用，而是直接映射到一系列预先定义好的操作。

    语言构成和函数的区别：
        1.语言构成可以加括号，也可以不加括号，但是函数必须要有括号
        2.语言构成不支持可变名
        3.语言构成执行速度比函数要快
            函数要先被PHP解析器，分解成语言构成，中间比语言构成多了一个解析的操作，所以速度就慢了。
    
    如何判断是否是语言构造，使用function_exists(),函数
    function check_language($name){
        if( function_exists($name) ){
            echo "函数";
        }else{
            echo "语言构成";
        }
    }

```

> 6. <h2 id="easy6">常量</h2>
<strong>常量就是在程序运行中，不可被改变的量</strong>  

* ### 常量的定义方式
    <em>有两种定义方式（切记，常量在定义时一定要赋值）</em>  
    ```php
        #第一种，使用define("常量名",常量值,常量名大小写是否敏感[FALSE敏感/TRUE不敏感]，默认FALSE)
        define("MY_BIRTHDAY","10月10日");
        echo MY_BIRTUDAY;
        //使用constant函数输出常量
        constant(MY_BIRTHDAY);
        #第二种，使用const关键字定义
        const MY_
    ```
* ### 系统常量
```php
PHP_VERSION 当前PHP的版本号
PHP_OS 服务器的操作系统

```
* ### 魔术常量
```php
#魔术常量的格式是 __常量名__,叫魔术常量的原因是因为这些值都不是固定的，它是会改变的。
__LINE__ 这个变量所在文件中的行号
__FILE__ 文件的完整路径和文件名
__DIR__ 文件所在的目录，和__FILE__相比就是少了一个文件名
__FUNCTION__ 返回所在函数名
__CLASS__ 返回所在类名
__NAMESPACE__ 返回所在命名空间名
```

> 7. <h2 id="easy7">运算符</h2>

* ### 算数运算符
```php
    \+(加) \-(减) \*(乘) /(除) %(取模) 
    ++ --  
    /*  
        分前后置
        前置++的话，就是先运算在赋值  
        后置++的话，就是先赋值在运算  
        如果当使用前置后置都是一样的结果的时候，应该优先用前置  
    */
```
* ### 比较运算符
```php
    \>(大于) >=（大于等于） <（小于） <=（小于等于） !=(不等于) ==(等于) ===(恒等于)  !==(不恒等于)
    //比较运算符号都是返回bool类型，关于恒等，就是不仅要数据相同，而且类型也要相同
    三元运算符
    表达式？为真返回：为假返回
```
* ### 逻辑运算符
```php
&& || !
```
* ### 字符运算符
```php
.(点)，字符运算符会把字符拼接在一起，这里一定要切记，字符运算符就是 .,没有+
```

> <h2 id="easy8">PHP三大控制结构</h2>

* ### 线性（顺序）结构
    这个结构没什么好讲的，代码依次从上往下执行，没什么特别
* ### 选择结构
#### if语句
```php
    if(表达式){  
        满足执行  
    }else if(表达式){  
        满足执行  
    }else{  
        不满足执行  
    }  
    //注意：只要有一个满足，就会满足条件执行的语句，然后判断，不会接着执行下面的判断。
```
    
#### switch语句
```php
    switch (值){
        case val1:
            对上号就执行
            break;
        case val2:
            对上号就执行
            break;
        default:
            都没对上就执行
    }
    //关于switch语句，他的括号里面可以是表达式，但是一定要有结果
    //如果case后面没有break的话，那么它就会从匹配到的值那里一路执行下去，不会执行default
    //如果没有一个对的上的就会执行default
```
* ### 循环条件
```php
    while (表达式){
        只要满足表达式就一直执行
    }

    for(初始条件;表达式;增量){
        满足表达式一直执行
    }

    do{
        满足条件执行
    }while(表达式)

    //while和 do...while的区别就在于，do...while最少执行一次
```
* ### break语句和continue
```php
    break 语句可以用于跳出循环，执行到break直接退出循环
    continue 语句可以用于跳过这次循环，执行到continue就会直接进行下一次循环
```

> <h2 id="easy9">字符串</h2>
#### 定义方式
```php
//php有两种定义方式单引号和双引号
/*
    这里就说一下他们两个个区别
    单引号：
        不会解析变量，转义字符无效
    双引号：
        会解析变量，转义字符有效
*/
```

#### 定义字符编码
在文件中添加这行，最好写在最上面   
`header("Content-type:text/html;charset=utf-8")`  
它的主要作用是告诉浏览器，使用什么编码格式去解码文档，防止页面乱码

#### 字符函数
 * strlen(string $str) 返回字符串长度,注意一个汉字占三个字节
 * mb_strlen(str\[,encoding\]) 返回字符串长度,它可以指定解码方式，如果指定utf-8的话,那么一个汉字就会算一个字符
 * strpos(string $haystack,mixed $needle\[,int $offset=0\]) 匹配子串，返回第一个匹配到字串的下标位置
    ```php
        /*
            返回needle在haystack中第一次出现的次数，offset如果被设置的话，那么会从字符串该字符数的起始位置开始统计，如果是负数的话，会从字符串尾开始
            这个函数区分大小写，不然要不区分的话使用stripos()
        */
        $str = "I am Somali";
        echo strpos($str,"Somali");
    ```
 * strrpos(string $haystack,$string $needle\[,int $offseet=0\]) 匹配子串，返回最后一个匹配到字串的下标位置
    ```php
        /*
            这个函数区分大小写，如果要不区分的话，请使用strripos()
        */
        $str = "I am Somali Somali";
        echo strrpos($str,"Somali");
    ```
 * str_replace(string $search,string $replace,string $str\[,int $count\]) 在str中匹配saerch，然后替换成replace,search可以是一个数组，count是一个变量，它是用来统计发生了几次替换的
 * strtr(string $str,string $from,string $to) 指定字符，替换字符串,注意这是strtr不是strstr别混掉了
    ```php
        $str 要操作的字符串
        $from 要替换的字符串
        $to 替换字符
        $str = "I am Somali";
        echo strtr($str,"Soma","Gama");
    ```
 * substr(string $str,int $start\[,int $length\]) 指定位置,截取字符串
    ```php
        返回$str由$start和$length参数指定的字符串
        $str 要截取的字符串
        $start 开始的位置,下标从0开始
        $length 截取的个数，如果这个参数不写的话，默认到字符结尾

    ```
 * substr_replace(mixed $string,mixed $replacement,mixed $start\[,mixed $length\]) 替换指定位置的字符
    ```php
        $str = "I am Somali";
        echo substr_replace($str,"Goma",5,4); //输出的就是 I am Gomali
    ```
 * trim(string $str[,string $character_mask="\t\n\r\0\x0B"]) 去除首尾空格
    ```php
        返回去除首尾空白字符的结果,默认是去除
        " " (ASCII 32 (0x20))，普通空格符。
        "\t" (ASCII 9 (0x09))，制表符。
        "\n" (ASCII 10 (0x0A))，换行符。
        "\r" (ASCII 13 (0x0D))，回车符。
        "\0" (ASCII 0 (0x00))，空字节符。
        "\x0B" (ASCII 11 (0x0B))，垂直制表符。
        也可以指定去除字符，注意：设置了指定字符的话，前面的默认字符就会失效   
    ``` 
* strtoupper(string $str) 将字符串转为全大写
* strtolower(string $str) 将字符串转为全小写
* strtok(string $str,string $token) 分次切割字符
    ```php
        将字符串str分割成若干字串，每个字串以token中的字符分割
        strtok在第一次使用时，需要设置$str，后面每次调用只需要更换token就好了，因为它会记住它在$str中的位置,除非你要分割一个新的字符，不然是不需要设置$str的。

        每次一调用都会切割一次,$tok被去掉,$tok前的字符返回,$tok后的字符保留
        $str = "I am Somali";
        echo strtok($str,"");//这时候I被切下来了，还有am Somali;
        echo strtok("a");//这时候m Som被切下来了,li被保留了，第一个a由于前面没东西，所有没用，但是也会被去掉
        echo strtok("");//什么都不切，输出li
    ```
* strstr($string $haystack,mixed $needle\[,bool $before_needle=FALSE\]) 查找字符串首次出现的位置,区分大小写,stristr()不区分
    ```php
        返回haystack字符串从neeld第一次出现的位置到haystack结尾的字符串
        $str = "I am Somali";
        echo strstr($str,"S");
    ```
* str_repeat(string $input, int $multiplier) 重复字符串
* ord(string $str) 转换字串第一个字节为0-255之间的值
* addcslashes(string $str, string $charlist) 以 C 语言风格使用反斜线转义字符串中的字符
    ```php
        $str = "hello\n";
        echo addcslashes($str,"hello\n"); //写在charlist里面的字符不是被当成整体去匹配的，而是一个一个字符匹配
    ```
* addslashes(string $str) 使用反斜线引用字符串
    ```php
        /*
            自动给一些特殊字符加上转义
            这些字符是:
                单引号（'）
                双引号（"）
                反斜线（\）
                NUL（NULL 字符）。 
        */
    ```
* strrchr(string $haystack,string $needle) 截取字符串，从needle出现的最后位置开始截取，一直到末尾，注意needle可以写一个字符串，但是它只会取第一个字符
* expload(string $delimiter,string $str)  将字符串转为数组，用delimiter去切割str,切割出来的字串，当成数组返回

> <h2 id="easy10">数组</h2>
    PHP的数组，可以存储多种类型
### 数组的类型
#### 索引数组
#### 关联数组
<br />  

### 定义方法
定义方法有多种
#### 索引数组  
* 第一种： $arr = array(元素1，元素2，元素3)
* 第二种:  $arr = \[元素1，元素2，元素3\]
#### 关联数组
* 第一种： $arr = array("key1"=>val1,"key2"=>val2)
* 第二种:  $arr = \["key1"=>1,"key2"=>2\]
* 注意：不管哪种方法，key一定是字符串
<br />

### 增删改查
#### 增
$arr[] = "end"; //这样会在数组末尾添加数据
#### 删
unset($arr[0]); //删除$arr[0]这个元素，不过注意，数组内元素不会往前移动
#### 改
arr[0] = "hello"; //直接通过索引去改
#### 查
直接通过索引去查
<br />

### 常用函数
* count(array $arr) 返回$arr的长度
* join(string $glue,array $pieces) 将一个一维数组的值转化为字符串，它是implode的别名,用glue拼接数组元素
    ```php
        $arrays = ['www','baidu','com'];
        echo join(".",$arrays);
    ```
* array_key_exists(string $key,array $arr) //判断$arr这个关联数组是否存在$key这个键值
* in_array(mixed $needle,array $haystack) //判断$var这个值是否在$arr里
* array_keys(array $arr) //把索引数组，变成关联数组返回。

*数组排序函数，会改变数组内的元素排序,成功返回TRUE，失败返回FALSE*  
* sort(array $arr) //对数组内的元素进行升序排序
* rsort(array $arr)  //对数组内的元素进行降序排序

*关联数组，最好用这个排序，这会保存它原本的关联属性，不会的话会变成索引数组*
* asort(array $arr)
* arsort(array $arr)

* array_rand(array $array[,int $num = 1]) //从数组中随机取出一个或多个元素
* array_pop(array $arr) //弹出$arr最后一个单元，然后数组array的长度减一
* array_push(array $arr,val1,va2) //往数组末尾插入元素，可以插入多个


### 遍历方法
除了使用普通的循环语句以外，这里说一种专门遍历数组的方法  
foreach(array $arr as \[key\]=>变量名) 注意这里的key是可选项，如果不是关联数组的话，可以不要
```php
    $index_arr = [1,2,3,4,5];
    $gl_arr = ["a"=>1,"b"=>2,"c"=>3];
    //下面这两种姿势，不管什么类型的数组都能用
    foreach($index_arr as $each){ 
        echo $each;
    }

    foreach($gl_arr as $key => $each){
        echo $key.$each;
    }  
```

> <h2 id="easy11">函数</h2>
*函数的作用：进行代码模块化，功能化，方便使用，减少代码重复量，以及耦合度*
### 定义
```php
//无参
function funame(){
    //代码功能模块
}
//有参
function funame($val){

}
```
### 有条件的函数
*在php中，函数的调用可以在定义之前，那是没有问题的*
```php
<?php
    bar();
    function bar(){
        echo "hello";
    }
?>
```
*所谓有条件的函数，就是只有条件满足时，函数才会被定义*
```php
<?php
    $makebar = 1;
    if($makebar){
        function bar(){
            echo "hello";
        }
    }
    bar();
?>
```
*在函数中被定义的函数，只有当父函数被调用时，它才会存在，且这个函数也可以被外部调用*
```php
<?php
    function bar1(){
        function bar2(){
            echo "hello";
        }
    }
    bar1();
    bar2();
?>
```
### 可变函数
*所谓可变函数，只不过是函数名存储在变量中，这样的话改变变量名即可改变调用的函数*  
    什么是可变函数？
    PHP 支持可变函数的概念。这意味着如果一个变量名后有圆括号，PHP 将寻找与变量的值同名的函数，并且尝试执行它。可变函数可以用来实现包括回调函数，函数表在内的一些用途。
    但是可变函数不能用于语言构造器，例如
    echo，print，unset()，isset()，empty()，include，require 以及类似的语言结构。需要使用自己的包装函数来将这些结构用作可变函数。
```php
    <?php
        $funName = "hello";
        function hello(){
            echo "你好";
        }
        $funName();
    ?>
```
### 匿名函数
*php支持使用匿名函数*
```php
<?php
    //这里就写几种方式，第一种直接定义函数在需要用的地方  
    echo preg_replace_callback('~-([a-z])~', function ($match) {
        return strtoupper($match[1]);
    }, 'hello-world');
    // 输出 helloWorld

    //第二种，赋值给变量名
    $fun = function ($say){
        echo $say;
    };
    $fun("hello");
?>
```

> <h2 id="easy12">类和对象</h2>
### 定义
#### 定义格式
    每个类的定义都要以class开头，后面跟着类名，类名后面跟着一对花括号，里面包含有类的属性与方法的定义。
    类名的命名规则和变量一样，不过通常类名的第一个字母都是大写
#### 例子
```php
    class Person{
        public $name; //定义一个公有属性
        private $age; //年龄是私有属性，不给别人看，这个东西要保密
        static public $city = '江西';
        const COUNTRY = '中国';
        public function show(){ //定义一个公有方法
            echo $this->name; //$this 是对当前对象的引用。 
            echo $this->age;
        }
    }
    $person = new Person(); //创建对象之前一定要用new
    $person->name = "张三";
    $person->show(); 
```
### 属性
#### 什么是属性
    在类中的变量成员，被成为属性，属性是由关键字声明的，有public(公有的),private(私有的),protected(保护的)，然后跟一个普通变量声明来组成，属性中的变量是可以初始化的，但是必须是常数，初始化之后，如果类被实例化，确没有给变量赋值，那么它就会是那个值。
#### 访问属性的方法
    访问类中的非静态成员变量用->,例如 $this->name,访问静态成员变量用::,例如 self::city;
#### 什么是 非静态 & 静态  
```
    非静态是指在类中的定义的属性，每次实例化新的对象时，这些属性都会有自己的值，简单来说，就是这个东西是实例化对象私有的，每个实例化对象都只能访问自己的非静态属性，而静态属性不同，静态属性是属于类的。
    而声明静态属性的方法就是static关键字，静态属性不用实例化对象，就可以直接通过类访问，而且实例化的对象反而访问不到静态属性，但是可以访问静态方法。

    在这里有两个关键字,self parent
    self:指向当前类
    parent:指向父类
```
#### 类常量
    定义类常量的方法是const关键字，定义后的类常量必须有初值，而且类常量可以直接使用类名方法，或者使用实例化对象访问，用::,例如 Person::country。
```PHP
<?php
   class Person{
        public $name; //定义一个公有属性
        private $age; //年龄是私有属性，不给别人看，这个东西要保密
        static public $city = '江西';
        const  COUNTRY = '中国'; //注意定义时不要加$,而且不用写访问关键词
    }
    $person = new Person();
    //这里注意看，不管是用类名还是实例化对象名都能直接访问到，这和static是不同的
    echo Person::COUNTRY;
    echo $person::COUNTRY;
?>
```

### 构造函数与析构函数
    function __construct(){} //构造函数,当类被实例化时自动调用,如果构造函数的设置了形参，那么你实例化对象的时候，就必须要去写参数，如果不写就会报错,除非形参设置了默认参数
    function __destruct() //析构函数,当实例化对象被释放时自动调用
```php
<?php
   class Person{
        public $name;
        public $age;
        static public $city = '江西';
        const  COUNTRY = '中国';

        //构造函数
        function __construct()
        {
            echo "构造函数被调用";
        }
        //析构函数
        function __destruct()
        {
            echo "析构函数被调用";
        }
    }
    $person = new Person();
?>
```
### 访问控制
    前面有说过三个关键字，public(公有的),private(私有的),protected(保护的)，这三个关键字就是用来控制属性和方法的访问权限的
        public 公有的，谁都可以访问
        private 私有的，只有类自己可以访问（通过自己定义的方法），不能直接通过对象访问属性的方式去访问（访问不到）
        protected 保护的，它其实和private差不多，它和private的区别就在于继承，private是不会被子类基础的,protected是会被子类继承
```php
<?php
   class Person{
        public $name;
        private $age;
        protected $sex;
        static public $city = '江西';
        const  COUNTRY = '中国';

        public function setAge($age){
            $this->age = $age;
        }
        public function getAge(){
            return $this->age;
        }
    }
    $person = new Person();
    //姓名是公有属性，可以直接设置值和访问。
    $person->name = "张三";
    echo $person->name = "张三";
    echo "<br/>";
    //名字是私有属性，只能通过方法去设置值和访问
    $person->setAge('18');
    echo $person->getAge();
    //性别是保护属性，不能直接设置,会报错
    // $person->sex;
?>
```
### 对象继承
    格式: class son extends father{}
    子类会继承父类的所有公有的，以及受保护的属性和方法，除非子类重写父类方法，不然的话父类的方法会保持原有功能。
### 抽象类
    一个类中如果有一个方法被声明为抽象的，那么这个类就是抽象类。被抽象的方法只是声明调用方法，不能定义其具体功能，抽象类是不能被实例化的，而且继承的子类，必须要实现被抽象的方法。

    //抽象类定义
    abstract class 类名{
        abstract public function getValue(); //声明调用方法，但是不用定义具体功能，这是留给子类去实现的
    }
### 对象接口
    使用接口可以指定某个类必须实现哪些方法，但不需要定义这些方法的具体内容，可能感觉和抽象类很像，但是是不一样的，首先它只定义方法，而且它的方法都不要具体内容

    注意：使用接口之后，接口中的方法必须被实现，如果不实现就会报错
    //定义对象接口
    interface 接口名{
        public function 方法名();
    }
    class 类名 implemeents 接口名{
        属性;
        方法;
    }
### 匿名类
    定义方法:
        new class{
            方法;
            属性;
        }
### 重载
    PHP中的重载的概念，与C+中重载的概念是不同的,PHP中的重载是指动态的创建类属性和方法，通过魔术方法来实现，当调用当前环境下未被顶或者不可见的类属性或方法时，重载方法就会被调用。

    在给不可访问属性赋值时，__set(string $name,mixed $value)会被调用
    读取不可以访问属性时,__get(string $name)会被调用
    当对不可访问属性调用时,例如isset()或empty(),__isset(string $name)就会被调用
    当对不可访问属性调用unset()时，__unset(string $name)会被调用

### 遍历对象
    使用foreach来遍历对象中所有的可见属性
    foreach(objectName as $key=>$value){
        //$key是属性名,$value是属性值
    }
    也可以在类中写一个这种方法，把objectName改成$this即可
### final
    被final声明的类，是不能被继承的，被final声明的方法是不能被覆盖的。
# 高级阶段
> 1. <h2 id="high1">会话技术</h2>
*不管是cookie还是session，这两种会话技术，都是用来弥补http协议的无状态这个问题*
### Cookie
#### 什么是Cookie
    Cookie实际上是一小段的文本信息。客户端请求服务器，如果服务器需要记录该用户状态，就使用response向客户端浏览器颁发一个Cookie。客户端浏览器会把Cookie保存起来。当浏览器再请求该网站时，浏览器把请求的网址连同该Cookie一同提交给服务器。服务器检查该Cookie，以此来辨认用户状态。服务器还可以根据需要修改Cookie的内容。
#### Cookie的使用
1. 创建Cookie  
`setcookie(name,value,expire,path,domain,secure)`
```
    name //cookie的名称
    value //cookie值，相当于键值对
    expire //可选，cookie的有效期，如果不设置的话，那么默认的有效期是一个会话时间，如果设置的话，那就是到设置的时间
    path // 可选，规定cookie的服务器路径,这个参数是基于domain参数在做细分的，下面具体看例子
    domain // 可选，规定cookie域名，可以让cookie只在某个子域名下有效
    secure // 可选，规定是否通过安全的HTTPS连接来传输cookie,默认是0，如果设置为1，那么只有在HTTPS下才能使用
```
实例
```php
#创建cookie并发送，设置失效时间
function initcookie(){
    if(!isset($_COOKIE['flag'])){
        setcookie("flag","AD1SA5452Q412DG",time()+3600);
    }else{
        echo "cookie已被设置";
    }  
}

initcookie();
```
2. Cookie的path限制
```php
    #假设当前有test1和test2目录，用test1中的代码去创建cookie,然后利用test2中的代码去访问，看看是否成功
    #test1
    function initcookie(){
        if(!isset($_COOKIE['test1'])){
            setcookie("test1","AD1SA5452Q412DG",time()+3600,"/test1","localhost");
        }else{
            echo "cookie已被设置";
        }
    }
    initcookie();
    #test2
    if(!isset($_COOKIE['test1'])){
        echo "cookie没有被设置";
    }else{
        echo "cookie已被设置";
    }
```
3. Cookie的删除
```php
    #test1创建cookie
    function initcookie(){
        if(!isset($_COOKIE['test1'])){
            setcookie("test1","AD1SA5452Q412DG",time()+3600);
        }else{
            echo "cookie已被设置";
        }
    }
    initcookie();
    #test2删除cookie
    function checkcookie(){
        if(isset($_COOKIE['test1'])){
            setcookie("test1","AD1SA5452Q412DG",time()-1); //通过设置时间，让它过期
            echo "cookie已被删除";
        }else{
            echo "没有cookie";
        }
    }
```

### Session
#### 什么是Session
    Session是另一种记录客户状态的机制，不同的是Cookie保存在客户端浏览器中，而Session保存在服务器上。客户端浏览器访问服务器的时候，服务器把客户端信息以某种形式记录在服务器上。这就是Session。客户端浏览器再次访问时只需要从该Session中查找该客户的状态就可以了。
    如果说Cookie机制是通过检查客户身上的“通行证”来确定客户身份的话，那么Session机制就是通过检查服务器上的“客户明细表”来确认客户身份。Session相当于程序在服务器上建立的一份客户档案，客户来访的时候只需要查询客户档案表就可以了。
#### Session的使用
1. 启动Session`<?php session_start(); ?>`
    * 使用前必须要先启用，而且在html页面中，一定要在`<html>`标签之前,且`session_start()`被调用之前不能有任何输出。
    * 当`session_start()`函数被调用后Seesion_start()函数就会创建一个唯一的Session ID，并自动通过HTTP的响应头，将这个Session ID保存到客户端Cookie中。同时，也在服务器端创建一个以Session ID命名的文件，用于保存这个用户的会话信息,存储这个文件的位置可以在php.ini中的session.save_path查看。
2. 删除Session
```php
//第一步：开启Session并初始化
session_start();
//第二步：删除所有Session的变量，也可以用unset($_SESSION[XXX])逐个删除
$_SESSION = array();
//第三步：如果使用基于Cookie的session，使用setCookkie()删除包含Session ID的cookie(删除客户端的记录)
if(isset($_COOKIE[session_name()])) {
    setCookie(session_name(), "", time()-42000, "/");
}
//第四步：最后彻底销毁session，因为session_destory()不会销毁session中的变量以及客户端的ID所以才要用上面的方法
session_destroy();
```

> <h2 id="high2">文件管理</h2>
### 文件包含
#### 文件包含常用的四个函数
* require
* include
上面两个函数作用是一样的，区别就在于`对于错误的处理方式不同`，都是用来包含文件，但是include包含的文件如果出现错误，那么还是会执行下面的其他php代码的，但是require不会
* require_once
* include_once
加了_once的区别就在于，不会重复包含
### 文件上传
基本操作函数
* realpath(".")从执行文件开始计算目录,例如.是当前目录路径,..是上一级
* opendir(".") 打开当前目录
* readdir(".") 读取当前目录
* closedir() 关闭打开文件资源
```php
    #打印当前目录文件
    $filedir = opendir(".")
    while($row = readdir($filedir)){
        echo $row."<br />";
    }
    closedir($filedir)
```
* is_dir("dirname") 判断是否是目录
* file_exists("file") 判断文件是否存在
* unlink("filename") 删除文件
* file_get_content("文件名") 读取文件内容
* file_put_content("文件名") 写入文件内容 
```php
#文件上传必须用Post提交，且form的enctype要="multipart/form-data"才行
#然后php中通过$_FILES这个预定义数组去获取文件的信息,这是一个二维数组，第一个索引值是表单中文件上传的name属性值
/*
    $_FILES的变量以及作用
    $_FILES["filename"]["name"] 获取文件名
    $_FILES["filename"]["type"] 获取文件类型
    $_FILES["filename"]["tmp_name"] 文件的临时存储位置
    $_FILES["filename"]["error"] 文件上传的错误提示码
    $_FILES["filename"]["size"] 文件的大小
*/
#要实现文件上传，还需要一个函数move_uoloaded_file(文件位置（临时位置）,要存储到的位置),dirname(__DIR__)可以获取执行文件所在目录位置
#mkdir(目录名[,mode=权限[0777],是否递归创建[FALSE]] )创建目录
```

> <h2 id="high3">MYSQL</2>
### PHP操作MYSQL
    有两种模式：
        1.MySQLi extension(i 表示 improved)
        2.PDO(PHP Data Object)
    相同点:
        这两者都是面向对象，都支持预处理语句
    不同点:
        1.MySQLi只针对MySQL数据库,PDO应用在12种不同的数据库
        2.虽然两者都是面向对象，但是MySQLi还提供了API接口
    下面的学习中只有MySQLi(面向对象)和PDO，虽然MySQLi也有面向过程的方法，但是这里不用。
### 参数讲解
*MYSQLi*
```javascript
    mysqli($host = null, $username = null, $passwd = null, $dbname = null, $port = null, $socket = null)//mysqli类用于创建mysqli对象，主机 用户名 密码 数据库名 端口 socket,如果只是连接，参数可以就填host,username,password,dbname关于port默认是3306，除非你不是这个端口，不然不用改。
    //假设创建好了一个mysqli对象，名称为 $conn_mysqli
    $conn_mysqli->connect_error //如果有连接有错误就会返回描述错误的字符串，如果没问题就返回NULL
    $conn_mysqli->query() //对数据库进行一次查询,失败时返回FALSE，如果执行成功返回TRUE，但是如果进行的是SELECT的话，它会返回一个mysqli_result对象
    $conn_mysqli->multi_query() //可以执行多条语句，语句之间有;间隔,执行成功会返回TRUE,注意，如果第一条语句执行错误，那么后面的也不会被执行，但是如果后面的语句执行错误，它并不会报错,关于获取执行多条语句的结果，有下面几个方法
        $conn_mysqli->use_result() //返回mysqli_result对象，失败返回FALSE
        $conn_mysqli->more_result() //批量检测查询中是否还有查询结构，有就返回TRUE，没有就返回FALSE
    mysqli_result对象的方法
        mysqli_result->num_rows //结果集中的数据行数
        mysqli_result->fetch_assoc() //返回结果集中的一行数据,从上往下走，每调用一次就会往下移一次
    $conn_mysqli->prepare("SQL语句，参数的位置换成?") //预处理，SQL绑定,会返回一个预处理对象
```
*PDO*
```javascript
    PDO($dsn,$username = null,$passwd = null,$options = null) //pdo类，用于创建pdo对象,dsn选择数据库产品并指定host(例如:mysql:host=$serverName;dbname=dbname),username数据库账号 password数据库密码
    //假设创建好了一个PDO对象，名称为$conn_pdo
    $conn_pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION); //设置PDO的错误模式
            // PDO::ERRORMODE_SLIENT 静默模式，不会报错，只会设置PDO的errorCode
            // PDO::ERRORMODE_WARNING 会报warning错误
            // PDO::ERRORMODE_EXCEPTION 抛出异常
    $conn_pdo->exec() //在一个单独的函数调用中执行一条 SQL 语句，返回受此语句影响的行数，如果没有受此语句影响的行数，就返回0，但是这并不是错误，例如执行insert就是返回0
    $conn_pdo->query() //执行一个SQL语句
    //事务，用于一次处理多个语句
    $conn_pdo->beginTransaction() //开始事务
    $conn_pdo->commit() //结束事务 

``` 
   

### 实例代码
*下面我会用MySQLi和PDO分别写出操作数据库的完整代码,以供学习*
```php
    $servername = 'localhost';
    $username = 'root';
    $password = 'root';
    $dbname = 'test';
```
#### MySQLi
```php
    //连接数据库
    $connect = new mysqli($servername,$username,$password,$dbname);
    if ($connect->connect_error){
        die('连接失败:'.$connect->connect_error);
    }
    echo '连接成功';
    //创建数据库,由于数据库已经建好了，这里也就把语句写在注释中了
    /*
        $sql = "CREATE DATABASE test";
        if(connect->query($sql) == TRUE){
            echo "数据库创建成功";
        } else{
            echo "数据库创建失败,".$conn->error;
        }
    */
    //创建数据表
    $sql = "CREATE TABLE Customer(
            id INT AUTO_INCREMENT PRIMARY KEY,
            fullName VARCHAR(50) NOT NULL,
            phoneNumber VARCHAR(30) NOT NULL,
            email VARCHAR(30) NOT NULL
        )";
    if($connect->query($sql) == TRUE){
        echo " 数据表Customer 创建成功";
    }else{
        echo "数据表创建错误:".$connect->error;
    }
    //插入数据
    echo "<br/>";
    $sql = "INSERT INTO Customer(fullName,phoneNumber,email) VALUES('张三','13677892015','z13677892015@163.com')";
    if( $connect->query($sql)){
        echo "插入成功";
    }else{
        echo "插入失败:".$connect->error;
    }
   //插入多条数据,如果哪条插入语句出错了，那么后面的插入语句就不会执行了，但是前面的会执行
    $sql = "INSERT INTO Customer(fullName,phoneNumber,email) VALUES('张三','13677892015','z13677892015@163.com');"; //这里注意里面有分号，因为要插入多条语句
    $sql .= "INSERT INTO Customer(fullName,phoneNumber,email) VALUES('李四','13677892014','z13677892014@163.com');"; //注意从这里开始是 .= 因为是拼接
    $sql .= "INSERT INTO Customer(fullName,phoneNumber,email) VALUES('王五','13677892013','z13677892013@163.com')"; 
    if($connect->multi_query($sql) === TRUE){
        echo "新记录插入成功";
    }else{
        echo "插入错误:".$connect->error;
    }
    //获取数据
    $sql = "SELECT fullName,phoneNumber,email FROM Customer";
    $result = $connect->query($sql); //返回一个对象
    if($result->num_rows > 0){  //num_rows属性是数据的行数
        while($row = $result->fetch_assoc()){
            echo "$row[fullName],$row[phoneNumber],$row[email]";
        }
    }
    //关闭连接
    $connect->close();
```
#### PDO
```php
    //连接数据库,PDO在连接出现问题时会抛出异常
    try{
        $connect = new PDO("mysql:host=$servername;dbname=$dbname",$username,$password);
        echo "连接成功";
        //下面这个用于设置PDO错误的模式，用于抛出异常，PDO通过设置ATTR_ERROMDE属性来控制SQL执行出错时的表现行为，有三个值
        /* 
            PDO::ERRORMODE_SLIENT 静默模式，不会报错，只会设置PDO的errorCode
            PDO::ERRORMODE_WARNING 会报warning错误
            PDO::ERRORMODE_EXCEPTION 抛出异常
         */
        $connect->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
        //创建数据库,由于数据库已经建好了，这里也就把语句写在注释中了
        /*
            $sql = "CREATE DATABASE test";
            $connect->exec($sql); //exec方法，它一次只可以执行一条语句，而且它的返回值是受这条语句影响的行数，而这里只是创建了一个库，所以返回为0 
        */
        //创建数据表
        $sql = "CREATE TABLE Customer(
            id INT AUTO_INCREMENT PRIMARY KEY,
            fullName VARCHAR(50) NOT NULL,
            phoneNumber VARCHAR(30) NOT NULL,
            email VARCHAR(30) NOT NULL
        )";
        //插入数据
        /* echo "<br/>";
        $sql = "INSERT INTO Customer(fullName,phoneNumber,email) VALUES('张三','13677892015','z13677892015@163.com')"; */
        $connect->exec($sql);
        //插入多条数据
        //开始事务
       $connect->beginTransaction();
        //sql语句
       $connect->exec("INSERT INTO Customer(fullName,phoneNumber,email) VALUES('张三','13677892015','z13677892015@163.com')");
       $connect->exec("INSERT INTO Customer(fullName,phoneNumber,email) VALUES('李四','13677892014','z13677892014@163.com')");
       $connect->exec("INSERT INTO Customer(fullName,phoneNumber,email) VALUES('张三','13677892013','z13677892013@163.com')");
        //提交事务
       $connect->commit();
       //读取数据,使用预编译
      $stmt = $connect->prepare("SELECT fullName,phoneNumber,email FROM Customer");
      $stmt->execute();
      $result = $stmt->setFetchMode(PDO::FETCH_ASSOC);//设置结果集为关联数组
      foreach($stmt->fetchAll() as $each){
          echo "<br/>fullName:$each[fullName],phoneNumber:$each[phoneNumber],email:$each[email]";
      }
       echo "插入成功";
    }catch(PDOException $PDOError){
        echo $PDOError->getMessage();
    }
    //关闭连接
    $connect = null;
```
### 预处理语句
`本来预处理我应该也加到上面的代码中去写的，但是我感觉这样不方便学习，所以就单独写吧`
#### 什么是预处理语句？
    预处理语句是提前将要使用的SQL语句给写好，并进行参数绑定，这样做有很多好处
    1.  预处理语句减少了分析时间，多次执行，但是只算是一次查询
    2.  减少了服务器带宽，因为发送的只是查询的参数，而不是整个语句
    3.  针对SQL注入，非常有用，因为参数发送后使用的协议不同，保证了数据的合法性
#### 如何使用
##### 插入的方法
`PDO插入`
```php
//预处理
    //预处理SQL
$stmt = $connect->prepare("INSERT INTO Customer(fullName,phoneNumber,email) VALUES(:fullName,:phoneNumber,:email)");
    //绑定参数
$stmt->bindParam(':fullName',$fullName);
$stmt->bindParam(':phoneNumber',$phoneNumber);
$stmt->bindParam(':email',$email);
    //插入参数
$fullName = "张三";
$phoneNumber = "13766238894";
$email = "z13766238894@163.com";
    //执行
$stmt->execute();
```
`MYSQLi插入`
```php
//预处理
$stmt = $connect->prepare("INSERT INTO Customer(fullName,phoneNumber,email) VALUES(?,?,?)"); //三个?占位,表示三个参数
    //绑定参数
$stmt->bind_param("sss",$fullName,$phoneNumber,$email); //s是类型占位符,表示字符串,然后和三个变量进行绑定,i表示整形,d表示双精浮点型，s字符串,b二进制大对象
    //设置参数
$fullName = "王五";
$phoneNumber = "15366238879";
$email = "w15366238879@163.com";
    //执行
$stmt->execute();
```
##### 查询的方法
`MYSQLi查询`
```php
    //绑定查询语句
    $stmt = $conn->prepare("SELECT fullName,phoneNumber,email FROM Customer WHERE id=? LIMIT 0,1");
        //对查询的参数进行绑定
        $stmt->bind_param("i",$id);
        if(isset($_GET['id'])){
            $id = $_GET['id'];
            //对接受结果的变量进行绑定
            $stmt->bind_result($fullName,$phoneNumber,$email);
            //执行
            $stmt->execute();
            //$stmt->store_result() ,是否有结果，有返回1没有返回0
            if($stmt->store_result()>0){
                //$stmt->num_rows() //这是可以获取结果集的行数，如果只有一行的话那就不需要
                $stmt->fetch(); //从结果中取出一行
                //然后用之前绑定的接受结果的变量进行输出
                echo "姓名:$fullName<br/>电话:$phoneNumber<br/>邮箱:$email";
            }else{
                echo "获取失败";
            }
            
        }
    $stmt->close(); //注意，如果要进行多次预处理语句的使用，那么在使用之前上一个预处理语句一定要被关闭
    $conn->close(); //mysqli是一个持久连接，需要手动关闭
```
`PDO查询`
```php
    $conn = new PDO("mysql:host=localhost;dbname=test","root","root");
    //绑定语句
    $stmt = $conn->prepare("SELECT fullName,phoneNumber,email FROM Customer WHERE id=:id LIMIT 0,1");
    //绑定参数
    $stmt->bindParam(":id",$id);
    if(isset($_GET['id'])){
        $id = $_GET['id'];
        //执行语句
        $stmt->execute();
        //$stmt->rowCount()返回有多少行结果
        if($stmt->rowCount() > 0){
            //fetch获取一行数据，PDO::FETCH_ASSOC设置为关联数组
            $result = $stmt->fetch(PDO::FETCH_ASSOC);
            echo "姓名:$result[fullName]<br/>电话:$result[phoneNumber]<br/>邮箱:$result[email]";
        }else{
            echo "查询失败";
        }
    }
```
*** 
[回到顶部](#menu)