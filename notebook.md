<!-- TOC -->

- [第二周 Spring入门和模板语法](#第二周-spring入门和模板语法)
    - [2.1 SpringBoot工程 :https://start.spring.io/](#21-springboot工程-httpsstartspringio)
        - [2.1.1 工程的导入及导入后的目录构造](#211-工程的导入及导入后的目录构造)
    - [2.2 最简单的服务器：包含Contoller、service、DAO、database](#22-最简单的服务器包含contollerservicedaodatabase)
        - [1) controller：把URL做解析等](#1-controller把url做解析等)
        - [2）Service：用于业务，对数据库做一些综合的操作等](#2service用于业务对数据库做一些综合的操作等)
        - [3）DAO：跟数据库连接](#3dao跟数据库连接)
        - [4）Database：数据库存放所有的数据](#4database数据库存放所有的数据)
        - [2.3 代码演示: Controller、参数解析、 Velocity](#23-代码演示-controller参数解析-velocity)
        - [2.4 代码演示：Request/Response/Session](#24-代码演示requestresponsesession)
        - [2.5 重定向](#25-重定向)
    - [2.6 Error页面](#26-error页面)
    - [2.7 IOC：控制反转/依赖注入](#27-ioc控制反转依赖注入)
    - [2.8 AOP/Aspect：面向切面编程：面向切面，所有业务都要处理的任务。比如日志，是大家都要做的](#28-aopaspect面向切面编程面向切面所有业务都要处理的任务比如日志是大家都要做的)
- [第三周 数据库交互mybatis集成](#第三周-数据库交互mybatis集成)
    - [总结构：controller网页入口、DAO数据库层、service、model模型【和数据库里面都是一一匹配的】：controller调用service【你想读取还是做什么操作都在这里】，service去调用 DAO【定义了对数据库的操作，比如增删改查等】](#总结构controller网页入口dao数据库层servicemodel模型和数据库里面都是一一匹配的controller调用service你想读取还是做什么操作都在这里service去调用-dao定义了对数据库的操作比如增删改查等)
    - [3.1 数据库的设计](#31-数据库的设计)
    - [3.2 数据库的创建](#32-数据库的创建)
        - [3.2.1 数据库常用数据类型：int、varchar(n)【可变字符串】、datetime【一般都有个字段叫create_time】、float(m,d)【前面有几位，小数点后面几位】、text,65535【长的文本】](#321-数据库常用数据类型intvarcharn可变字符串datetime一般都有个字段叫create_timefloatmd前面有几位小数点后面几位text65535长的文本)
    - [3.3 CRUD操作](#33-crud操作)
        - [1) INSERT INTO table_name (列1, 列2,...) VALUES (值1, 值2,....),(值1, 值2,....)](#1-insert-into-table_name-列1-列2-values-值1-值2值1-值2)
        - [2) SELECT */列名称FROM 表名称](#2-select-列名称from-表名称)
        - [3) UPDATE 表名称 SET 列名称=新值 WHERE 列名称=某值](#3-update-表名称-set-列名称新值-where-列名称某值)
        - [4) DELETE FROM 表名称WHERE 列名称= 值](#4-delete-from-表名称where-列名称-值)
    - [3.4 MyBatis集成](#34-mybatis集成)
    - [3.4.2 为什么要用框架，因为正常情况下JDBC数据库操作读取数据这些是非常麻烦的：](#342-为什么要用框架因为正常情况下jdbc数据库操作读取数据这些是非常麻烦的)
    - [3.5 ViewObject和DateTool【自带的时间的工具】](#35-viewobject和datetool自带的时间的工具)
        - [3.5.1  ViewObject:方便传递任何数据到Velocity；实质就是一个map,把字段和对象对应起来](#351--viewobject方便传递任何数据到velocity实质就是一个map把字段和对象对应起来)
    - [3.6 首页开发](#36-首页开发)
        - [3.6.1 windows下关闭某个端口所处的进程](#361-windows下关闭某个端口所处的进程)
            - [1. netstat -ano |findstr  端口号    得到进程号   (findstr 很像linux下的grep命令)](#1-netstat--ano-findstr--端口号----得到进程号---findstr-很像linux下的grep命令)
            - [2.  taskkill /pid  进程号  /f](#2--taskkill-pid--进程号--f)
            - [3.  netstat -ano |findstr  端口号 可以再验证下该端口还开着没](#3--netstat--ano-findstr--端口号-可以再验证下该端口还开着没)
    - [3.7 基础整体流程：两大块 【这里是把所有数据全部都展现在一起，我们要做的是把它们按时间分开，以及实现用户的链接】](#37-基础整体流程两大块-这里是把所有数据全部都展现在一起我们要做的是把它们按时间分开以及实现用户的链接)
        - [1） 在Controller里通过ViewObject的方式把数据都放入 vo 里面【vo包含两个字段，则在home.html里可以直接通过这两个字段来访问数据】；将vo做成一个链表vos，则在home.html里就可以通过循环来遍历这些数据](#1-在controller里通过viewobject的方式把数据都放入-vo-里面vo包含两个字段则在homehtml里可以直接通过这两个字段来访问数据将vo做成一个链表vos则在homehtml里就可以通过循环来遍历这些数据)
        - [2） 在home.html文件中找到post，这就是一条一条的资讯，我们对其做一个循环就好](#2-在homehtml文件中找到post这就是一条一条的资讯我们对其做一个循环就好)
    - [3.8 进阶：把资讯按时间分开，以及实现用户的链接](#38-进阶把资讯按时间分开以及实现用户的链接)
        - [1） DateTool: velocity自带工具类导入 ,直接把toolbox.xml文件导入resource，再在application.properties里面配好将这个文件引用进来：那么就可以在velocity（home.html）页面上用这些类了](#1-datetool-velocity自带工具类导入-直接把toolboxxml文件导入resource再在applicationproperties里面配好将这个文件引用进来那么就可以在velocityhomehtml页面上用这些类了)
        - [2） 实现资讯按时间分开，在home.html文件](#2-实现资讯按时间分开在homehtml文件)
        - [3） 实现用户的链接](#3-实现用户的链接)
- [第四周 用户注册登录管理](#第四周-用户注册登录管理)
    - [4.1 注册](#41-注册)
    - [4.2 登陆](#42-登陆)
    - [4.3 浏览](#43-浏览)
    - [4.4 Interceptor](#44-interceptor)
    - [4.5 未登录跳转](#45-未登录跳转)
    - [4.6 数据安全性](#46-数据安全性)
    - [4.7 AJAX](#47-ajax)
    - [4.8 DEV-TOOL](#48-dev-tool)
    - [问题：](#问题)

<!-- /TOC -->
# 第二周 Spring入门和模板语法
## 2.1 SpringBoot工程 :https://start.spring.io/

* 可以自动帮你生成一个工程
    * 选择Web
    * velocity:因为网页页面的显示用的velocity模板
    * AOP：面向切面编程
### 2.1.1 工程的导入及导入后的目录构造
* intellij用import点击pom.xml导入工程
    * maven是一种软件项目管理工具，提供了一个项目对象模型（POM）文件的概念来管理项目的创建，相关性和文档。最强大的功能就是能自动下载项目依赖库
    * pom.xml：maven的配置文件，能配置项目的依赖包和工程的说明。【利用pom.xml能指定你依赖于哪些包，直接在pom.xml文件关联即可，它会自动帮你加载】
* 工程导入后的目录结构：
    * main:
        * java :包含了工程核心的Java代码，你写的代码就在这里：controller、service等
        * resourse：
            * static：网站相关的动态，比如css,图片，JavaScript等文件
            * templates：模板，你页面想显示什么，就放在这里
            * application.properties：配置文件，Spring容器？
    test：
## 2.2 最简单的服务器：包含Contoller、service、DAO、database
### 1) controller：把URL做解析等
### 2）Service：用于业务，对数据库做一些综合的操作等
### 3）DAO：跟数据库连接
### 4）Database：数据库存放所有的数据

### 2.3 代码演示: Controller、参数解析、 Velocity
* 从controller根据地址进来，把变量传给模板，模板决定要显示什么

* 所有的入口都是controller，在spring里，都是通过注解来指定你这个类是用来做什么的
    *  URL：https://www.nowcoder.com/discuss/12560?type=0&order=0&pos=1&page=4
        * www.nowcoder.com：IP，相当于127.0.0.1：8080
        * discuss：属于pathVariable，比如{usesrId}、{GroupId}等
        * 12560?type=0&order=0&pos=1&page=4：属于请求参数即RequestParam
    * 你可以简单返回参数，但正常情况下应该返回一个模板，模板可以做任何事
    * 牛客网头部和尾部长得一样，可以把网页的公共部分提取出来，一个模板又包含另一个模板，比如news.vm包含header.vm
```Java

//com/nowcode/toutiao/controller/IndexController.java

@Controller
public class IndexController {

    @RequestMapping(path = {"/","/index"})  //地址的处理入口，指定接收的URL，一旦访问的是这个地址，我就用这个函数来处理
    @ResponseBody          //因为有返回，即有body
    public String index(){
        return "hello nowcode";
    }


    //value和path互为别名，等价
    @RequestMapping(value = {"/profile/{groupId}/{userId}"}) //整体叫path，{groupId}叫path里的变量
    @ResponseBody
    public String profile(@PathVariable("groupId") String groupId, //我们把path变量值groupId定义为int类型的变量，叫groupId
                          @PathVariable("userId") int userId, //"userId"双引号里面的值代表的是传进来的实际值
                          @RequestParam(value = "type",defaultValue = "1") int type,
                          @RequestParam(value = "key",defaultValue = "nowcoder") String key){
        return String.format("{%s},{%d},{%d},{%s}",groupId,userId,type,key);
    }

    @RequestMapping(value = {"/vm"})
    public String news(Model model){//可以通过Model传值给模板
        model.addAttribute("value1","vv1"); //加了一个value1 变量，值为vv1,然后就可以在模板里显示
        List<String> list = Arrays.asList(new String[]{
                "RED","BLUE","BLACK","YELLOW"
        });
        Map<String ,String> map =new HashMap<>();
        for (int i=0; i<4; i++){
            map.put(String.valueOf(i),String.valueOf(i*i));
        }
        model.addAttribute("colors",list);
        model.addAttribute("map",map);

        //向模板传递数据，除了基础数据结构，也可以传递自定义的类，比如model的user类
        model.addAttribute("user",new User("Jim"));
        return "news"; //返回一个模板，对应的是templates里的模板news.vm,自己就会对应起来
    }
    
}

```

```
##templates/news.vm

<html> //返回的是一个HTML页面
<body>
<pre>
    ##注释，velocity模板语言
        hello vm!!

    ## 变量，有叹号表示如果不存在这个变量就不显示,{}可以有也可以不要
        $value2
        $!value1

        #foreach($color in $colors)  ##velocity语法，遍历
            Color : $!{foreach.index}/$!{foreach.count} : $!{color}
        #end

    ## 方法类似Java，比如map
        #foreach($key in $map.keySet())
            num: ${key} :$map.get($key)
        #end

    ##使用自定义的类
        $user.name;
        $user.getName();

    ##给另一个模板header.vm的变量赋值，以及把另一个模板包含进来,包含用#include和#parse都可以
    ##但include只是纯粹地把模板包含进来，而parse会把模板编译转换，即用你定义的变量值去替换变量
        #set($title = "nowcode") ## title 是header.vm里面定义的变量，设置变量的值
        #include("header.vm")<br>
        #parse("header.vm")

    ## 定义一个函数，然后就可以调用函数
        #macro(render_color ,$color,$index) ##定义函数render_color，color和index是它传入的参数
            Color By Macro $index,$color  ##这个函数要显示的内容
        #end

        #foreach($color in $colors)
            #render_color($color,$foreach.index) ##调用函数
        #end

    ## 定义变量
    #set($hello = "hello")
    #set($hworld1 = "$!{hello} world") ##双引号才会对其进行解析，用值来替换变量
    #set($hworld2 = '$!{hello} world') ## 单引号不会进行解析，原样输出

    $hworld1 ##显示变量
    $hworld2


</pre>
</body>
</html>
```

### 2.4 代码演示：Request/Response/Session
* 浏览器：右键->检查，即可打开开发者环境，里面的network,按F12可显示name
* request：参数解析、cookie读取、http请求字段、文件上传
    * HttpServletResponse
        * response.addCookie(new Cookie(key, value));
        * response.addHeader(key, value);
* response：页面内容返回、cookie下发、http字段设置，headers
    * HttpServletRequest
        * request.getHeaderNames();
        * request.getMethod()
        * request.getPathInfo()
        * request.getQueryString
```Java
 //网页请求与回复
    @RequestMapping(value = {"/request"})
    @ResponseBody
    public String request(HttpServletRequest request, //这些都是javax里面的类：javax.servlet.http.HttpServletRequest;
                          HttpServletResponse response,
                          HttpSession session){ //这几个变量是所有http请求都会包含的，
        //会自动把http请求信息包装成request，返回信息包装成response这两个变量
        StringBuilder sb = new StringBuilder();
        Enumeration<String> headername = request.getHeaderNames();//把所有头名字打印出来
        while (headername.hasMoreElements()) {
            String name = headername.nextElement();  //得到头
            sb.append(name +":" + request.getHeader(name)+"<br>"); //根据头可以得到所有的头部信息,记得加换行
        }
        //增加打印cookie
        for(Cookie cookie:request.getCookies()){
            sb.append("Cookie");
            sb.append(cookie.getName());
            sb.append(":");
            sb.append(cookie.getValue());
            sb.append("<br>");
        }

        //还有很多其他信息，比如method等
        sb.append("getMethod: " + request.getMethod()+"<br>");
        sb.append("getPathInfo: " + request.getPathInfo()+"<br>");
        sb.append("getQueryString: " + request.getQueryString()+"<br>");
        sb.append("getRequestURI: " + request.getRequestURI()+"<br>");

        return sb.toString();
    }

    @RequestMapping(value = {"/response"})
    @ResponseBody
    public String response(@CookieValue(value = "nowcodeid",defaultValue = "a") String nowcoder,  //cookie值需要你来传，就是从URI传入，不然默认为“a"
                           @RequestParam(value = "key",defaultValue = "key") String key, //跟前面一样，这是请求带来的参数
                           @RequestParam(value = "value",defaultValue = "value") String value,
                           HttpServletResponse response){
        response.addCookie(new Cookie(key,value));  //在返回里把cookie加上，就是返回体里面会有这些
        response.addHeader(key,value);  //把header也加上
        return "Nowcoderid from cookie: "+nowcoder;
    }

```

### 2.5 重定向
* 重定向：当你打开一个页面，这个页面已经不存在了，那么需要重定向到另一个页面
    * 301：永久转移
    * 302：临时性转移
```Java
 @RequestMapping(value = {"/redirect/{code}"})
    public RedirectView redirectView(@PathVariable("code") int code){ //返回一个重定向的view
        RedirectView redirect = new RedirectView("/",true);//把页面跳到首页去
        //根据传入进来的参数判断，如果是301，则是强制性的跳转（永久性），否则是临时性跳转
        if (code == 301){
            redirect.setStatusCode(HttpStatus.MOVED_PERMANENTLY);//因为默认是302，所以我们要设置成301转移
        }
        return redirect;
        
        //或者直接可以只用一句话
        //return "redirect:/"  来直接跳转到首页，但只用于302
    }
```
* session：用户在一次打开浏览器访问我们网页，就是一个session，通过session来传递消息
```Java

    @RequestMapping(path = {"/","/index"})  //地址的处理入口，指定接收的URL，一旦访问的是这个地址，我就用这个函数来处理
    @ResponseBody          //因为有返回，即有body
    public String index(HttpSession session){
        return "hello nowcode，" + session.getAttribute("msg");//会话用于在一次通信中传递消息，不是重定向到达的首页，那么msg不会被赋值，为null
    }

     @RequestMapping(value = {"/redirect/{code}"})
    public String redirectView(@PathVariable("code") int code,
                               HttpSession session){ //用户在一次打开浏览器访问我们网页，就是一个session
        session.setAttribute("msg","jump from redirect"); //可以通过session来传递一些消息，在首页中加上一个session，并打印信息
    
        return "redirect:/";  //来直接跳转到首页，但只用于302
    }
```

## 2.6 Error页面
```Java
//用户传入密码，如果错误就抛出异常
    @RequestMapping(value = {"/admin"})
    @ResponseBody
    public String admin(@RequestParam(value = "key",required = false) String key){//允许入参为空
        if ("mima".equals(key)){
            return "hello admin"; //如果用户输入的密码是mima，那么这个用户合法
        }
        throw new IllegalArgumentException("key error"); //非法的用户我就抛出个异常
    }

    //自己来处理错误，不用spring默认的错误处理页面
    //Spring MVC外的Exception或Spring MVC没有处理的Exception,可以通过我们定义的函数把它捕获下来
    @ExceptionHandler()
    @ResponseBody
    public String error(Exception e){
        return "error: "+ e.getMessage();  //我的处理方式是返回错误信息error: key error
    }

```

## 2.7 IOC：控制反转/依赖注入
* 思想：把它的实现放在一个地方，用到它的时候用最简单的方式把它注入进来
* 业务在service实现，创建com.nowcode.toutiao.service.ToutiaoService，这是业务，之前的代码在controller包，现在的代码在service包
```Java
package com.nowcode.toutiao.service;

import org.springframework.stereotype.Service;

@Service  //有了这个注解，它就会自动创建一个对象
public class ToutiaoService { //会实现很多很多的功能，然后我们就可以在controller里直接调用对象
    public String say(){
        return "This is toutiaoService";
    }
}

```
```Java
@Controller
public class IndexController {
    @Autowired  //spring核心部分会把它创建好的对象自动赋给该变量[依赖注入]，不用你自己new
    private ToutiaoService toutiaoService;  //controller只是一个入口，这才是业务的实现

    @RequestMapping(path = {"/","/index"})  //地址的处理入口，指定接收的URL，一旦访问的是这个地址，我就用这个函数来处理
    @ResponseBody          //因为有返回，即有body
    public String index(HttpSession session){
        return "hello nowcode，" + session.getAttribute("msg")+ "<br>" + toutiaoService.say();//会话用于在一次通信中传递消息
    }
}
```

## 2.8 AOP/Aspect：面向切面编程：面向切面，所有业务都要处理的任务。比如日志，是大家都要做的
* spring的AOP底层用的Aspectj
* 你新建一个包叫aspect，然后里面再写这个类，你甚至都不需要显式地使用这个类的对象，这需要定义这个类，然后运行的时候它自己会调用这个方法

```Java

@Aspect   //这是面向切面
@Component
public class LogAspect {
    private static final org.slf4j.Logger logger = LoggerFactory.getLogger(LogAspect.class);

    //在执行这些函数之前都要执行beforeMethod
    @Before("execution(* com.nowcode.toutiao.controller.*Controller.*(..))")  //长这样的类的所有方法，正则表达式
    public void beforeMethod(JoinPoint joinPoint) {  //把交互的这个口做了一个包装，叫切点
        StringBuilder sb = new StringBuilder();
        for (Object arg : joinPoint.getArgs()) {
            sb.append("arg:" + arg.toString() + "|");
        }
        logger.info("before method: " + sb.toString());
    }

    //在执行这些函数之后要执行afterMethod
    @After("execution(* com.nowcode.toutiao.controller.*Controller.*(..))")
    public void afterMethod(JoinPoint joinPoint) {
        logger.info("after method: ");
    }
}

```

# 第三周 数据库交互mybatis集成
## 总结构：controller网页入口、DAO数据库层、service、model模型【和数据库里面都是一一匹配的】：controller调用service【你想读取还是做什么操作都在这里】，service去调用 DAO【定义了对数据库的操作，比如增删改查等】

* 利用mybatis这框架从数据库读取数据，并在页面上显示：把每天的资讯在页面上分成小块，数据的来源是数据库，用户也是从数据库中读取过来的，做一个最基本的数据库的读取和velocity前端的展示连起来
* * mybatis框架就是为了把数据库的表和你的model做一个映射，你有一张user表，一般就有一个user的model，方便你读取
## 3.1 数据库的设计
* 收到产品经理的原稿后怎么设计：数据库是一张张的表，有些表与表之间是关联的。
* 目标：做一个头条资讯的网站，每个人都可以发新闻，用户可以点赞，站内信
    * 1）分析产品，有哪些东西是看得见的，看得见的东西分实体(单元)
        * 有用户，用户本身登录等相关信息
        * 有站内信：人与人的交互
        * 有资讯：一条条的资讯
        * 有评论：每一条资讯下面有评论
    * 2）分完实体后，再分析每个实体内有哪些内容，代码里叫属性，数据库里叫列/字段，然后他们有一些关联，数据的同步和来源的问题，比如资讯里面的comment_count来源于comment表
        * user表
            * id              //作为主键方便查询
            * name            //名字
            * password        //密码
            * salt            //加强密码的一个额外的字段
            * head_url        //有一张头像
        * 站内信（Message）
            * id
            * fromid   //谁发的
            * toid      //发给谁
            * content   //内容
            * conversation_id
            * created_date  //日期
        * 资讯（News）
            * id
            * title   //标题
            * link     //它的链接是哪
            * image    //首图
            * like_count  //有多少赞
            * comment_count  //有多少评论，通过异步的方式来更新这个字段
            * user_id     //谁发的
            * created_date
        * 评论（Comment）
            * id
            * content
            * user_id  //谁发的
            * created_date   //日期
            * news_id   //属于哪一条资讯的
## 3.2 数据库的创建
* 设计好有哪些表及表的字段后，就可以创建数据库了：先用GUI工具 MySQL workbench 创建好脚本后，再去运行
### 3.2.1 数据库常用数据类型：int、varchar(n)【可变字符串】、datetime【一般都有个字段叫create_time】、float(m,d)【前面有几位，小数点后面几位】、text,65535【长的文本】

## 3.3 CRUD操作
### 1) INSERT INTO table_name (列1, 列2,...) VALUES (值1, 值2,....),(值1, 值2,....)
### 2) SELECT */列名称FROM 表名称
### 3) UPDATE 表名称 SET 列名称=新值 WHERE 列名称=某值
### 4) DELETE FROM 表名称WHERE 列名称= 值
## 3.4 MyBatis集成
* 1）首先application.properties：
    * 引入一个数据库，所以需要配置数据库，在application.properties增加spring配置数据库链接地址
```
#数据库地址
spring.datasource.url=jdbc:mysql://localhost:3306/toutiao?useUnicode=true&amp;characterEncoding=utf8&amp;useSSL=false
#用户名和密码
spring.datasource.username=root
spring.datasource.password=5340126

# 加配置？
mybatis.config-location=classpath:mybatis-config.xml
# maven构建项目时候resource目录就是默认的classpath
# classPath即为java文件编译之后的class文件的编译目录一般为web-inf/classes，src下的xml在编译时也会复制到classPath下

```
* 2）其次pom.xml：
    * 引用mybatis肯定需要在pom.xml文件中引进来：mybatis-spring-boot-starter【将mybatis和spring boot集成起来】<br>
    mybatis要用MySQL数据库的链接，所以要把相关jar包引进来：mysql-connector-java【因为用的是MySQL，所以要用MySQL的连接器】
```

		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>1.1.1</version>>
		</dependency>

```
* 3）创建一个sql脚本 init_schema.sql ，包含表的创建，放在test下建的一个resource包里

* 3) 接着考虑怎么读取，要建一个model包，里面包含user、News类【对应数据库的表】,表有哪些列，它们就需要有哪些字段，比如
```Java
public class News {

  private int id;

  private String title;

  private String link;

  private String image;

  private int likeCount;

  private int commentCount;

  private Date createdDate;

  private int userId;

  public int getId() {
    return id;
  }

  public void setId(int id) {
    this.id = id;
  }

  public String getTitle() {
    return title;
  }

  public void setTitle(String title) {
    this.title = title;
  }

  public String getLink() {
    return link;
  }

  public void setLink(String link) {
    this.link = link;
  }

  public String getImage() {
    return image;
  }

  public void setImage(String image) {
    this.image = image;
  }

  public int getLikeCount() {
    return likeCount;
  }

  public void setLikeCount(int likeCount) {
    this.likeCount = likeCount;
  }

  public int getCommentCount() {
    return commentCount;
  }

  public void setCommentCount(int commentCount) {
    this.commentCount = commentCount;
  }

  public Date getCreatedDate() {
    return createdDate;
  }

  public void setCreatedDate(Date createdDate) {
    this.createdDate = createdDate;
  }

  public int getUserId() {
    return userId;
  }

  public void setUserId(int userId) {
    this.userId = userId;
  }
}

```

* 4）创建dao包，有几个表创建几个接口，记住是Interface，不是class，比如UserDAO，NewsDAO，比如UserDAO这个接口是用来插入数据库的，里面定义了要对user表执行的操作
    * 1）用注解的方式
* 变量是#{变量}
```Java
@Mapper //表示我和数据库中的表是一一匹配的
public interface UserDAO {
//    @Insert({"insert into user(name,password) values()"})
//    int addUser(User user);    //最简单的插入一条数据，但一般我们不这样，而是下面一种写法


    //常用方式，这是为了方便更改，只需要改这里就可以全部都改了
    String TABLE_NAME="user";
    String INSERT_FIELDS="name,password,salt,head_url";
    String SELECT_FIELDS ="id,name,password,salt,head_url";
    @Insert({"insert into",TABLE_NAME,"(",INSERT_FIELDS,")","values(#{name},#{password},#{salt}," +
            "#{headUrl})"})
    int addUser(User user);  //这个方法的作用就是对数据库进行插入操作，它自己会在你给的user对象[你调用这个方法需要传入一个user对象]里找对应的属性

    @Select({"select" ,SELECT_FIELDS,"from ",TABLE_NAME, "where id=#{id}"})
    User selectById(int id); //因为只有一个id ，所以可以这么写，这个函数会在initDatabaseTests里要用到这个功能的时候调用

    @Update({"update",TABLE_NAME,"set password=#{password} where id=#{id}"})
    void updatePassword(User user);

    @Delete({"delete from",TABLE_NAME,"where id=#{id}"})
    void deleteById(int id);
}

```
* 2）用xml的方式：比注解的好处就是可以写一些复杂的操作；此处用xml写NewsDAO的selectByUserIdAndOffset方法
```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.nowcode.toutiao.dao.NewsDAO">
    <sql id="table">news</sql>
    <sql id="selectFields">id,title, link, image, like_count, comment_count,created_date,user_id</sql>
    <select id="selectByUserIdAndOffset" resultType="com.nowcode.toutiao.model.News">
        SELECT
        <include refid= "selectFields"/>
        FROM
        <include refid="table"/>

        <if test="userId != 0">
            WHERE user_id = #{userId}
        </if>
        ORDER BY id DESC
        LIMIT #{offset},#{limit}
    </select>
</mapper>

```


* 5）可以写一个测试用例，可以直接复制test下面的ToutiaoApplicationTests，然后黏贴改名为InitDatabaseTests，这是初始化数据库的测试用例
```java
package com.nowcode.toutiao;

import com.nowcode.toutiao.dao.NewsDAO;
import com.nowcode.toutiao.dao.UserDAO;
import com.nowcode.toutiao.model.News;
import com.nowcode.toutiao.model.User;
import org.junit.Assert;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.SpringApplicationConfiguration;
import org.springframework.test.context.jdbc.Sql;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import org.springframework.test.context.web.WebAppConfiguration;

import java.util.Date;
import java.util.Random;

@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(classes = ToutiaoApplication.class)
@WebAppConfiguration
@Sql({"/init-schema.sql"}) //在执行用例前先执行这份文件，创建数据库
public class InitDatabaseTests {

	@Autowired
	UserDAO userDAO;  //

	@Autowired
	NewsDAO newsDAO;

	@Test
	public void contextLoads() {
		Random random=new Random();
		for (int i=0; i<10 ;i++){ //创建10个用户
			User user = new User();  //创建一了一个user对象，并且给它的属性赋值
			user.setHeadUrl(String.format("http://images.nowcoder.com/head/%dt.png",random.nextInt(1000)));
			user.setName(String.format("USER%d",i));
			user.setPassword("");
			user.setSalt("");
			//插入用户，在userDAO里面定义有addUser，它就是用来往数据库插入用户的
			userDAO.addUser(user);  //UserDAO这个接口是用来插入数据库的，里面定义了要执行的操作

			//测试newsDAO,
			News news = new News();
			news.setCommentCount(i);
			//为了方便我们后面对资讯进行分页，所以设置每条资讯时间相隔5个小时1000毫秒*
			Date date = new Date();
			date.setTime(date.getTime()+1000*3600*5);
			news.setCreatedDate(date);
			news.setImage(String.format("http://images.nowcoder.com/head/%dm.png",random.nextInt(1000)));
			news.setLikeCount(i+1);
			news.setUserId(i+1);
			news.setTitle(String.format("TITLE{%d}",i));
			news.setLink(String.format("http://www.nowcoder.com/%d.html",i));//超链接，这条资讯跑到哪去了
			newsDAO.addNews(news);



			//在数据库根据ID跟新密码，在userDAO定义有这个函数
			user.setPassword("newpassword");
			userDAO.updatePassword(user); //把user的属性值更新到数据库中去
		}
		//在数据库查询id=1的用户的密码,因为这是一个测试用例，所以可以用Assert来帮助
		Assert.assertEquals("newpassword", userDAO.selectById(1).getPassword());
		userDAO.deleteById(1);//在数据库删除id为1的用户
		Assert.assertNull("null",userDAO.selectById(1));
	}

}


```
## 3.4.2 为什么要用框架，因为正常情况下JDBC数据库操作读取数据这些是非常麻烦的：
* 1.加载驱动
* 2.创建数据库链接
* 3.创建Statement对象【select、update等都叫statement对象【语句的表达的对象】】
* 4.执行SQL获取数据（框架让我们只需要关注这里）
* 5.数据转化
* 6.资源释放

## 3.5 ViewObject和DateTool【自带的时间的工具】
### 3.5.1  ViewObject:方便传递任何数据到Velocity；实质就是一个map,把字段和对象对应起来
```Java
public class ViewObject {
    private Map<String, Object> objs = new HashMap<String, Object>();
    public void set(String key, Object value) {
        objs.put(key, value);
    }

    public Object get(String key) {
        return objs.get(key);
    }
}

```



## 3.6 首页开发
### 3.6.1 windows下关闭某个端口所处的进程
#### 1. netstat -ano |findstr  端口号    得到进程号   (findstr 很像linux下的grep命令)

#### 2.  taskkill /pid  进程号  /f 

#### 3.  netstat -ano |findstr  端口号 可以再验证下该端口还开着没



## 3.7 基础整体流程：两大块 【这里是把所有数据全部都展现在一起，我们要做的是把它们按时间分开，以及实现用户的链接】
### 1） 在Controller里通过ViewObject的方式把数据都放入 vo 里面【vo包含两个字段，则在home.html里可以直接通过这两个字段来访问数据】；将vo做成一个链表vos，则在home.html里就可以通过循环来遍历这些数据
```Java
@Controller
public class HomeController {
    @Autowired
    NewsService newsService;

    @Autowired
    UserService userService;

    @RequestMapping(path={"/","/index"},method = {RequestMethod.GET,RequestMethod.POST})
    public String index(Model model){
        //因为是首页，取前10条新闻数据
        List<News> newsList= newsService.getLatesNews(0,0,10);

        List<ViewObject> vos = new ArrayList<>();
        for (News news: newsList){
            ViewObject vo=new ViewObject();
            vo.set("news",news);//vo就是一个map,通过“news”字段就可以得到news
            vo.set("user",userService.getUser(news.getUserId())); //这条资讯对应的作者
            vos.add(vo);
        }

        //把Vos传到home.html
        model.addAttribute("vos",vos);// 这样，到了home.html就可以通过news字段访问news，通过user字段访问user
        return "home";
        //返回一个模板，前面注解不能加@ResponseBody，因为我们返回的是模板，把home.html放在templates下
        //注意这里我们的是home.html，而不是.vm，把相关的前端的东西放到resource下的static目录下
        //因为后缀名是html,所以还要在application.properties里配置一下
    }
}
```
### 2） 在home.html文件中找到post，这就是一条一条的资讯，我们对其做一个循环就好
```html
        <div class="container" id="daily">
            <div class="jscroll-inner">
                <div class="daily">   <!--每一天的资讯-->

                    <h3 class="date">  <!--这是它的标题-->
                        <i class="fa icon-calendar"></i>
                        <span>头条资讯</span>
                        <small>2016-05-24</small>
                    </h3>

                    <div class="posts">
                        #foreach($vo in $vos)  <!--一条一条的资讯，通过循环来显示-->
                        <div class="post">
                            <div class="votebar">
                                <button class="click-like up" aria-pressed="false" title="赞同"><i class="vote-arrow"></i><span class="count">$!{vo.news.likeCount}</span></button>
                                <button class="click-dislike down" aria-pressed="true" title="反对"><i class="vote-arrow"></i>
                                </button>
                            </div>
                            <div class="content" data-url="$!{vo.news.link}">  <!--资讯链接-->
                                <div class="content-img">
                                    <img src="$!{vo.news.image}" alt="">  <!--新闻图片-->
                                </div>
                                <div class="content-main">
                                    <h3 class="title">   <!--资讯的title，他要链接到link上去-->
                                        <a target="_blank" rel="external nofollow" href="$!{vo.news.link}">$!{vo.news.title}</a>
                                    </h3>
                                    <div class="meta">
                                        $!{vo.news.link}  <!--显示的？-->
                                        <span>
                                            <i class="fa icon-comment"></i> $!{vo.news.commentCount} <!--评论数-->
                                        </span>
                                    </div>
                                </div>
                            </div>
                            <div class="user-info">  <!--用户-->
                                <div class="user-avatar">  <!--user的超链接、用户的图片-->
                                    <a href="/user/$!{vo.user.id}"><img width="32" class="img-circle" src="$!{vo.user.headUrl}"></a>
                                </div>

                                <!--
                                <div class="info">
                                    <h5>分享者</h5>

                                    <a href="http://nowcoder.com/u/251205"><img width="48" class="img-circle" src="http://images.nowcoder.com/images/20141231/622873_1420036789276_622873_1420036771761_%E8%98%91%E8%8F%87.jpg@0e_200w_200h_0c_1i_1o_90Q_1x" alt="Thumb"></a>

                                    <h4 class="m-b-xs">冰燕</h4>
                                    <a class="btn btn-default btn-xs" href="http://nowcoder.com/signin"><i class="fa icon-eye"></i> 关注TA</a>
                                </div>
                                -->
                            </div>
                            <!--来自哪个用户-->
                            <div class="subject-name">来自 <a href="/user/$!{vo.user.id}">$!{vo.user.id}</a></div>
                        </div>
                        #end
                        <!--
                        <div class="alert alert-warning subscribe-banner" role="alert">
                          《头条八卦》，每日 Top 3 通过邮件发送给你。      <a class="btn btn-info btn-sm pull-right" href="http://nowcoder.com/account/settings">立即订阅</a>
                        </div>
                        -->
                    </div>
                </div>
            </div>
        </div>
```

## 3.8 进阶：把资讯按时间分开，以及实现用户的链接
### 1） DateTool: velocity自带工具类导入 ,直接把toolbox.xml文件导入resource，再在application.properties里面配好将这个文件引用进来：那么就可以在velocity（home.html）页面上用这些类了
```
#数据库地址
spring.datasource.url=jdbc:mysql://localhost:3306/toutiao?useUnicode=true&amp;characterEncoding=utf8&amp;useSSL=false
#用户名和密码
spring.datasource.username=root
spring.datasource.password=5340126

# 加配置？
mybatis.config-location=classpath:mybatis-config.xml

#velocity默认的后缀名是.vm,为什么要用html呢，因为这样我们就可以在网页里面直接编辑，vm很弱智
#logging.level.root=DEBUG
spring.velocity.suffix=.html
spring.velocity.cache=false

#引用进toolbox.xml
spring.velocity.toolbox-config-location=toolbox.xml
```

### 2） 实现资讯按时间分开，在home.html文件
```HTML
       <div class="container" id="daily">
            <div class="jscroll-inner">
                <div class="daily">

                    #set($cur_date = '')
                    #foreach($vo in $vos)
                    #if ($cur_date != $date.format('yyyy-MM-dd', $vo.news.createdDate))
                    #if ($foreach.index > 0)
                </div> ## 上一个要收尾
                #end
                #set($cur_date = $date.format('yyyy-MM-dd', $vo.news.createdDate))
                <h3 class="date">
                    <i class="fa icon-calendar"></i>
                    <span>头条资讯 &nbsp; $date.format('yyyy-MM-dd', $vo.news.createdDate)</span>
                </h3>

                <div class="posts">
                    #end
                    <div class="post">
                        <div class="votebar">
                            <button class="click-like up" aria-pressed="false" title="赞同"><i class="vote-arrow"></i><span class="count">$!{vo.news.likeCount}</span></button>
                            <button class="click-dislike down" aria-pressed="true" title="反对"><i class="vote-arrow"></i>
                            </button>
                        </div>
                        <div class="content" data-url="http://nowcoder.com/posts/5l3hjr">
                            <div >
                                <img class="content-img" src="$!{vo.news.image}" alt="">
                            </div>
                            <div class="content-main">
                                <h3 class="title">
                                    <a target="_blank" rel="external nofollow" href="$!{vo.news.link}">$!{vo.news.title}</a>
                                </h3>
                                <div class="meta">
                                    $!{vo.news.link}
                                    <span>
                                            <i class="fa icon-comment"></i> $!{vo.news.commentCount}
                                        </span>
                                </div>
                            </div>
                        </div>
                        <div class="user-info">
                            <div class="user-avatar">
                                <a href="/user/$!{vo.user.id}/"><img width="32" class="img-circle" src="$!{vo.user.headUrl}"></a>
                            </div>

                            <!--
                            <div class="info">
                                <h5>分享者</h5>

                                <a href="http://nowcoder.com/u/251205"><img width="48" class="img-circle" src="http://images.nowcoder.com/images/20141231/622873_1420036789276_622873_1420036771761_%E8%98%91%E8%8F%87.jpg@0e_200w_200h_0c_1i_1o_90Q_1x" alt="Thumb"></a>

                                <h4 class="m-b-xs">冰燕</h4>
                                <a class="btn btn-default btn-xs" href="http://nowcoder.com/signin"><i class="fa icon-eye"></i> 关注TA</a>
                            </div>
                            -->
                        </div>

                        <div class="subject-name">来自 <a href="/user/$!{vo.user.id}/">$!{vo.user.name}</a></div>
                    </div>

                    <!--
                    <div class="alert alert-warning subscribe-banner" role="alert">
                      《头条八卦》，每日 Top 3 通过邮件发送给你。      <a class="btn btn-info btn-sm pull-right" href="http://nowcoder.com/account/settings">立即订阅</a>
                    </div>
                    -->
                    #if ($foreach.count == $vos.size()) ##最后有个元素要收尾
                </div>
                #end

                #end


            </div>
        </div>
```
### 3） 实现用户的链接

```Java
@Controller
public class HomeController {
    @Autowired
    NewsService newsService;

    @Autowired
    UserService userService;

    private List<ViewObject> getNews(int userId, int offset, int limmit){
        //取前10条新闻数据
        List<News> newsList= newsService.getLatesNews(userId,offset,limmit);

        List<ViewObject> vos = new ArrayList<>();
        for (News news: newsList){
            ViewObject vo=new ViewObject();
            vo.set("news",news);//vo就是一个map,通过“news”字段就可以得到news
            vo.set("user",userService.getUser(news.getUserId())); //这条资讯对应的作者
            vos.add(vo);
        }
        return vos;
    }
    //用于获得资讯那一页的模板
    @RequestMapping(path={"/","/index"},method = {RequestMethod.GET,RequestMethod.POST})
    public String index(Model model){
        //把Vos传到home.html
        model.addAttribute("vos",getNews(0,0,10));// 这样，到了home.html就可以通过news字段访问news，通过user字段访问user
        return "home";
    }


    //用于获得用户那一页的模板，这个用户的链接我们在home.html里配了的
    @RequestMapping(path={"/user/{userId}"},method = {RequestMethod.GET,RequestMethod.POST})
    public String userIndex(Model model, @PathVariable("userId" ) int userId){
        //把Vos传到home.html
        model.addAttribute("vos",getNews(userId,0,10));// 这样，到了home.html就可以通过news字段访问news，通过user字段访问user
        return "home";

    }
}

```



# 第四周 用户注册登录管理
## 4.1 注册
## 4.2 登陆
## 4.3 浏览
## 4.4 Interceptor
## 4.5 未登录跳转
## 4.6 数据安全性
## 4.7 AJAX
## 4.8 DEV-TOOL









## 问题：
* 1、这里面的@CookieValue是什么意思，为什么
    * 输入是http://127.0.0.1:8080/response?key=nowcodeid&value=54时才会返回Nowcoderid from cookie: 54
    

```Java
@RequestMapping(value = {"/response"})
    @ResponseBody
    public String response(@CookieValue(value = "nowcodeid",defaultValue = "a") String nowcoder,  //cookie值需要你来传，就是从URI传入，不然默认为“a"
                           @RequestParam(value = "key",defaultValue = "key") String key, //跟前面一样，这是请求带来的参数
                           @RequestParam(value = "value",defaultValue = "value") String value,
                           HttpServletResponse response){
        response.addCookie(new Cookie(key,value));  //在返回里把cookie加上，就是返回体里面会有这些
        response.addHeader(key,value);  //把header也加上
        return "Nowcoderid from cookie: "+nowcoder;
    }
```