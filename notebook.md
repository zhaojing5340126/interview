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
        - [2.4 代码演示：Request/Response/Session【HttpServletReques、HttpServletResponse、HttpSession】](#24-代码演示requestresponsesessionhttpservletrequeshttpservletresponsehttpsession)
        - [2.5 重定向](#25-重定向)
    - [2.6 Error页面](#26-error页面)
    - [2.7 IOC：控制反转/依赖注入：思想：把它的实现放在一个地方，用到它的时候用最简单的方式把它注入进来](#27-ioc控制反转依赖注入思想把它的实现放在一个地方用到它的时候用最简单的方式把它注入进来)
    - [2.8 AOP/Aspect：面向切面编程：面向切面，所有业务都要处理的任务。比如日志，是大家都要做的](#28-aopaspect面向切面编程面向切面所有业务都要处理的任务比如日志是大家都要做的)
- [第三周 数据库交互mybatis集成](#第三周-数据库交互mybatis集成)
    - [总结构：controller网页入口、DAO数据库层、service、model模型【和数据库里面都是一一匹配的】：controller调用service【你想读取还是做什么操作都在这里】，service去调用 DAO【定义了对数据库的操作，比如增删改查等】](#总结构controller网页入口dao数据库层servicemodel模型和数据库里面都是一一匹配的controller调用service你想读取还是做什么操作都在这里service去调用-dao定义了对数据库的操作比如增删改查等)
    - [3.1 数据库的设计【四张表：user表、站内信（Message）、资讯（News）、评论（Comment）】](#31-数据库的设计四张表user表站内信message资讯news评论comment)
    - [3.2 数据库的创建](#32-数据库的创建)
        - [3.2.1 数据库常用数据类型：int、varchar(n)【可变字符串】、datetime【一般都有个字段叫create_time】、float(m,d)【前面有几位，小数点后面几位】、text,65535【长的文本】](#321-数据库常用数据类型intvarcharn可变字符串datetime一般都有个字段叫create_timefloatmd前面有几位小数点后面几位text65535长的文本)
    - [3.3 CRUD操作](#33-crud操作)
        - [1) INSERT INTO table_name (列1, 列2,...) VALUES (值1, 值2,....),(值1, 值2,....)](#1-insert-into-table_name-列1-列2-values-值1-值2值1-值2)
        - [2) SELECT */列名称FROM 表名称](#2-select-列名称from-表名称)
        - [3) UPDATE 表名称 SET 列名称=新值 WHERE 列名称=某值](#3-update-表名称-set-列名称新值-where-列名称某值)
        - [4) DELETE FROM 表名称WHERE 列名称= 值](#4-delete-from-表名称where-列名称-值)
    - [3.4 MyBatis集成](#34-mybatis集成)
    - [3.4.2 为什么要用框架，因为正常情况下JDBC数据库操作读取数据这些是非常麻烦的：](#342-为什么要用框架因为正常情况下jdbc数据库操作读取数据这些是非常麻烦的)
    - [3.5 ViewObject【视图对象】和DateTool【自带的时间的工具】](#35-viewobject视图对象和datetool自带的时间的工具)
        - [3.5.1  ViewObject:方便传递任何数据到Velocity，给模板提供数据；实质就是一个map,把字段和对象对应起来](#351--viewobject方便传递任何数据到velocity给模板提供数据实质就是一个map把字段和对象对应起来)
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
        - [4.1.1 注册注意事项：【用户名和密码】](#411-注册注意事项用户名和密码)
            - [1. 用户名合法性检测（长度（数据库字段长度是有限制的），敏感词（比如管理员等），重复，特殊字符（比如颜文字（对网站的展示有影响）、HTML代码等））](#1-用户名合法性检测长度数据库字段长度是有限制的敏感词比如管理员等重复特殊字符比如颜文字对网站的展示有影响html代码等)
            - [2. 密码长度要求（以及大小写等，后台会对强度进行判定）](#2-密码长度要求以及大小写等后台会对强度进行判定)
            - [3. 密码salt加密，密码强度检测（md5库）](#3-密码salt加密密码强度检测md5库)
            - [4. 用户邮件/短信激活【防止机器人注册很多账号】](#4-用户邮件短信激活防止机器人注册很多账号)
        - [4.1.2 代码 步骤：](#412-代码-步骤)
            - [1) 在开发过程中，我们会对不同的业务做不同的Controller，比如登陆注册，我们会再做一个LoginController](#1-在开发过程中我们会对不同的业务做不同的controller比如登陆注册我们会再做一个logincontroller)
            - [2）注册对数据库来说就是要往里面加一个人](#2注册对数据库来说就是要往里面加一个人)
    - [4.2 登陆/登出（浏览器演示）](#42-登陆登出浏览器演示)
        - [4.2.1 登陆：](#421-登陆)
            - [1.服务器密码校验/三方校验回调，token登记](#1服务器密码校验三方校验回调token登记)
            - [1.1 服务器验证密码正确后，会生成一个token关联userid,下发token给客户端](#11-服务器验证密码正确后会生成一个token关联userid下发token给客户端)
            - [1.2 客户端存储token(app存储本地，浏览器存储cookie),每次和服务器进行交互的时候就带上这个token，服务器就知道你是谁了](#12-客户端存储tokenapp存储本地浏览器存储cookie每次和服务器进行交互的时候就带上这个token服务器就知道你是谁了)
            - [2.服务端/客户端token有效期设置（记住登陆）](#2服务端客户端token有效期设置记住登陆)
            - [注:token可以是sessionid【那么你浏览器不会有变化，只要你没关，都是这个sessionid】，或者是cookie里的一个key](#注token可以是sessionid那么你浏览器不会有变化只要你没关都是这个sessionid或者是cookie里的一个key)
        - [4.2.2 代码步骤：](#422-代码步骤)
            - [1）登录就是把你的用户名，密码这些进行判断验证](#1登录就是把你的用户名密码这些进行判断验证)
            - [2）验证成功了，就要给用户下发一个ticket【token】](#2验证成功了就要给用户下发一个tickettoken)
                - [2.1）在这里要数据库新创建一个 login_ticket 表【我这里的建法是在通过InitDatabaseTests，在测试的时候建的，一般我们写一个DAO都会测试一下有没有问题】](#21在这里要数据库新创建一个-login_ticket-表我这里的建法是在通过initdatabasetests在测试的时候建的一般我们写一个dao都会测试一下有没有问题)
                - [2.2）同时对应一个 LoginTicket的 model，](#22同时对应一个-loginticket的-model)
                - [2.3）及LoginTicketDAO【插入ticket、查、更新状态】](#23及loginticketdao插入ticket查更新状态)
                - [2.4) LoginController里写登陆的函数 login](#24-logincontroller里写登陆的函数-login)
        - [4.2.2 登出：把ticket对应的status设置为1，就表示这个无效了](#422-登出把ticket对应的status设置为1就表示这个无效了)
            - [1） 首先LoginController里进入登出的函数](#1-首先logincontroller里进入登出的函数)
            - [2） 其次，userService里写一个logout函数](#2-其次userservice里写一个logout函数)
    - [4.3 浏览](#43-浏览)
        - [4.3.1 整体步骤：](#431-整体步骤)
            - [1.客户端：带token【也就是ticket】的HTTP请求](#1客户端带token也就是ticket的http请求)
            - [2.服务端：【用户是否登陆、用户有无权限】](#2服务端用户是否登陆用户有无权限)
                - [1).根据token获取用户id](#1根据token获取用户id)
                - [2).根据用户id获取用户的具体信息](#2根据用户id获取用户的具体信息)
                - [3).用户和页面访问权限处理](#3用户和页面访问权限处理)
                - [4).渲染页面/跳转页面【成功就渲染、不成功就跳转到非法页面】](#4渲染页面跳转页面成功就渲染不成功就跳转到非法页面)
    - [4.3.2 Interceptor(拦截器)：可以拦截你整个链路上的各个步骤](#432-interceptor拦截器可以拦截你整个链路上的各个步骤)
        - [1）preHandle：用于处理前，一般在preHandle里判断权限](#1prehandle用于处理前一般在prehandle里判断权限)
        - [2）postHandle：用于处理完后，一般在PostHandle里设置一些数据](#2posthandle用于处理完后一般在posthandle里设置一些数据)
        - [3）afterCompletion：用于全体结束后， 一般用于收尾工作](#3aftercompletion用于全体结束后-一般用于收尾工作)
        - [代码演示](#代码演示)
            - [1）创建一个拦截器PassportInterceptor：每次访问之前，拦截器先插进来先找这个用户](#1创建一个拦截器passportinterceptor每次访问之前拦截器先插进来先找这个用户)
            - [2）在model里创建一个HostHolder 类，用于存放这一次访问里面的用户是谁 【不同线程有自己的用户】](#2在model里创建一个hostholder-类用于存放这一次访问里面的用户是谁-不同线程有自己的用户)
            - [3）拦截器写好后要注册要 MVC 里面，创建一个configuration包，创建以一个ToutiaoWebConfiguration类继承 WebMvcConfigurerAdapter，实现它的addInterceptors方法](#3拦截器写好后要注册要-mvc-里面创建一个configuration包创建以一个toutiaowebconfiguration类继承-webmvcconfigureradapter实现它的addinterceptors方法)
    - [4.4 可以有多个拦截器：比如某个页面要登陆了才能访问，未登录就跳转到首页去【只针对/setting开头的页面】【不算项目内容】](#44-可以有多个拦截器比如某个页面要登陆了才能访问未登录就跳转到首页去只针对setting开头的页面不算项目内容)
    - [4.5 为了用户数据安全，我们要做的事：](#45-为了用户数据安全我们要做的事)
        - [1.HTTPS注册页：网页访问http是明文的，https是加密的](#1https注册页网页访问http是明文的https是加密的)
        - [2.公钥加密私钥解密，支付宝h5页面的支付密码加密：用户打开页面时，服务器下发一个公钥，你会用这个公钥对数据加密，加密后再提交到服务器，只有服务器又私钥可以解密](#2公钥加密私钥解密支付宝h5页面的支付密码加密用户打开页面时服务器下发一个公钥你会用这个公钥对数据加密加密后再提交到服务器只有服务器又私钥可以解密)
        - [3.用户密码salt防止破解（CSDN，网易邮箱未加密密码泄漏）](#3用户密码salt防止破解csdn网易邮箱未加密密码泄漏)
        - [4.token有效期：保证这个token不会被别人占用](#4token有效期保证这个token不会被别人占用)
        - [5.单一平台的单点登陆，登陆IP异常检验：别人登陆你就下线，你再登陆一下，会显示上次登陆的IP区域](#5单一平台的单点登陆登陆ip异常检验别人登陆你就下线你再登陆一下会显示上次登陆的ip区域)
        - [6.用户状态的权限判断【用拦截器做的】](#6用户状态的权限判断用拦截器做的)
        - [7.添加验证码机制，防止爆破和批量注册：比如修改密码，一般手机验证码4位，写个程序一下子发0-9999，那肯定就有一个对的，所以输入一次或几次失败后，你要做校验](#7添加验证码机制防止爆破和批量注册比如修改密码一般手机验证码4位写个程序一下子发0-9999那肯定就有一个对的所以输入一次或几次失败后你要做校验)
    - [4.6 AJAX：异步数据交互](#46-ajax异步数据交互)
        - [优点：](#优点)
            - [1.页面不刷新，只是局部刷新](#1页面不刷新只是局部刷新)
            - [2.用户体验更好](#2用户体验更好)
            - [3.传输数据更少](#3传输数据更少)
            - [4.APP/网站通用，因为就是一些数据](#4app网站通用因为就是一些数据)
            - [扩展：有统一的数据格式：{code:0,msg:'',data:''}](#扩展有统一的数据格式code0msgdata)
    - [4.7 Spring Boot DevTools：方便开发](#47-spring-boot-devtools方便开发)
        - [1）动态加载更新的class【比如修改了userService，你只需要（右键recompile）userService即可】：不是整体重启，只是这个类重启](#1动态加载更新的class比如修改了userservice你只需要右键recompileuserservice即可不是整体重启只是这个类重启)
        - [2）编译加载修改的静态文件【比如html文件，修改后只需要重新编译该文件即可（右键recompile）】](#2编译加载修改的静态文件比如html文件修改后只需要重新编译该文件即可右键recompile)
    - [附录：](#附录)
        - [1）StringUtils：是比较好用的一个类org.apache.commons.lang.StringUtils; 可以直接判断字符串，比如StringUtils.isBlank(password）](#1stringutils是比较好用的一个类orgapachecommonslangstringutils-可以直接判断字符串比如stringutilsisblankpassword)
        - [2) fastjson：阿里巴巴的一个用于json的工具，下载地址https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-devtools/1.3.5.RELEASE【你所需要的各种库，这里都有，你直接导入就好：在 pom.xml 中】](#2-fastjson阿里巴巴的一个用于json的工具下载地址httpsmvnrepositorycomartifactorgspringframeworkbootspring-boot-devtools135release你所需要的各种库这里都有你直接导入就好在-pomxml-中)
- [第五周 资讯发布、图片上传、资讯首页](#第五周-资讯发布图片上传资讯首页)
    - [5.1 图片上传到云](#51-图片上传到云)
        - [5.1.1 Fiddler：设置了一个代理，我访问某个网站，实际通过它，让它帮我访问，于是它就捕获了所有的数据。用于web编程查看http。](#511-fiddler设置了一个代理我访问某个网站实际通过它让它帮我访问于是它就捕获了所有的数据用于web编程查看http)
        - [5.1.2 代码步骤：定义一个post的入口，spring会帮你把上传的数据那部分包装成为一个file，然后你判断是不是一张图片，是就存储它](#512-代码步骤定义一个post的入口spring会帮你把上传的数据那部分包装成为一个file然后你判断是不是一张图片是就存储它)
            - [1） 为方便，先加一个NewsController,用于上传图片  【客户端图片传上来后我保存在本地】](#1-为方便先加一个newscontroller用于上传图片--客户端图片传上来后我保存在本地)
        - [2）newsService要增加一个保存图片的方法 saveImage](#2newsservice要增加一个保存图片的方法-saveimage)
        - [附录：](#附录-1)
    - [5.2 图片的展示](#52-图片的展示)
        - [5.2.1 代码-在 NewsController 中【客户端输入图片的链接，你解析它，返回图片】](#521-代码-在-newscontroller-中客户端输入图片的链接你解析它返回图片)
    - [5.3 七牛云服务集成：冗余备份，多台机器统一访问，CDN缓存同步，云实时缩图](#53-七牛云服务集成冗余备份多台机器统一访问cdn缓存同步云实时缩图)
        - [5.3.1 到了网站开发后面，静态（图片这些）和页面是分离的，图片没有数据库这些【因为它只是一些文件，需要读取这些】，一般存放在云中](#531-到了网站开发后面静态图片这些和页面是分离的图片没有数据库这些因为它只是一些文件需要读取这些一般存放在云中)
        - [5.3.2 【好处】：](#532-好处)
            - [1）这些服务器能专门做一些缓存，对静态的优化；网站发布就只需要发布你动态的内容即可](#1这些服务器能专门做一些缓存对静态的优化网站发布就只需要发布你动态的内容即可)
            - [2）前端的文件和后端不一样，在前端，如果全国都在访问的话，是需要加缓存的，即CDN,CDN把静态分发给各个CDN节点,那么你获得静态文件的速度会更快](#2前端的文件和后端不一样在前端如果全国都在访问的话是需要加缓存的即cdncdn把静态分发给各个cdn节点那么你获得静态文件的速度会更快)
            - [3) 为什么用云存储：一张图片可能在不同的地方要用，小图片，中图片，大图片，以前是把这张图片裁擦成这些大小，都存起来，坏处：有些图片上传上来根本没人用。所以现在用实时缩图服务，不会影响服务器存储的空间，合理应对产品需求随时要改这些，用的时候才对图片进行调整](#3-为什么用云存储一张图片可能在不同的地方要用小图片中图片大图片以前是把这张图片裁擦成这些大小都存起来坏处有些图片上传上来根本没人用所以现在用实时缩图服务不会影响服务器存储的空间合理应对产品需求随时要改这些用的时候才对图片进行调整)
        - [5.3.2 代码步骤：](#532-代码步骤)
    - [5.4 资讯发布 【在数据库要用utf8，在application.properties里也要指定utf8，防止中文出错】](#54-资讯发布-在数据库要用utf8在applicationproperties里也要指定utf8防止中文出错)
- [第六周 资讯详情页、评论中心、站内信](#第六周-资讯详情页评论中心站内信)
    - [6.1 通用的新模块开发流程总体步骤都是一样的：](#61-通用的新模块开发流程总体步骤都是一样的)
        - [1) 从数据库设计开始 Database Column](#1-从数据库设计开始-database-column)
        - [2）Model：模型定义，和数据库相匹配](#2model模型定义和数据库相匹配)
        - [3）DAO：数据读取](#3dao数据读取)
        - [4）Service：服务包装](#4service服务包装)
        - [5) Controller：业务入口](#5-controller业务入口)
        - [6) Test](#6-test)
    - [6.2 资讯详情页（代码演示）](#62-资讯详情页代码演示)
        - [6.2.1 步骤：](#621-步骤)
            - [1）一般先找到前端给你的页面，分析这个页面上有多少东西](#1一般先找到前端给你的页面分析这个页面上有多少东西)
            - [2） 到 NewsController 层里面把这些数据传给模板，再对模板文件做一些替换](#2-到-newscontroller-层里面把这些数据传给模板再对模板文件做一些替换)
    - [6.3评论中心（代码演示）](#63评论中心代码演示)
        - [6.3.1 代码一：展示已有评论的步骤：](#631-代码一展示已有评论的步骤)
            - [1）确定这个服务存放在数据库里面需要哪些表，字段是哪些。--自己在数据库里创建，或者放在.sql文件中，然后创建一个测试](#1确定这个服务存放在数据库里面需要哪些表字段是哪些--自己在数据库里创建或者放在sql文件中然后创建一个测试)
            - [2）Model：定义模型，和数据库相匹配--Comment,不需要注解](#2model定义模型和数据库相匹配--comment不需要注解)
            - [3）DAO：定义数据库的访问模型 -@Mapper](#3dao定义数据库的访问模型--mapper)
            - [4）Service：服务包装，去调用DAO层，做一些复杂些的业务](#4service服务包装去调用dao层做一些复杂些的业务)
            - [5）Controller：业务入口：添加评论，删除评论](#5controller业务入口添加评论删除评论)
            - [6）测试一下 ：Fn+delete：从左往右删除](#6测试一下-fndelete从左往右删除)
        - [6.3.2 增加评论的代码](#632-增加评论的代码)
        - [6.3.3 删除评论](#633-删除评论)
    - [6.4 消息中心（代码演示）](#64-消息中心代码演示)
        - [6.4.1 代码一：消息详情页](#641-代码一消息详情页)
            - [1）确定这个服务存放在数据库里面需要哪些表，字段是哪些。--自己在数据库里创建，或者放在.sql文件中，然后创建一个测试](#1确定这个服务存放在数据库里面需要哪些表字段是哪些--自己在数据库里创建或者放在sql文件中然后创建一个测试-1)
            - [2）Model：定义模型，和数据库相匹配--Message,不需要注解](#2model定义模型和数据库相匹配--message不需要注解)
            - [3）DAO：定义数据库的访问模型 -@Mapper ————MessageDAO](#3dao定义数据库的访问模型--mapper-messagedao)
            - [4）Service：服务包装，去调用DAO层，做一些复杂些的业务 ————MessageService](#4service服务包装去调用dao层做一些复杂些的业务-messageservice)
            - [5）Controller：业务入口：增加消息，显示消息详情页--MessageController](#5controller业务入口增加消息显示消息详情页--messagecontroller)
        - [6.4.2 代码二：会话页【你与所有人的会话都显示最新的一条作为代表】](#642-代码二会话页你与所有人的会话都显示最新的一条作为代表)
- [第七周 Redis入门以及redis实现赞踩功能](#第七周-redis入门以及redis实现赞踩功能)
    - [总结：](#总结)
    - [7.1 Redis简介:Redis 是key-value数据库，支持主从同步，数据存储在内存，性能卓越。](#71-redis简介redis-是key-value数据库支持主从同步数据存储在内存性能卓越)
        - [7.1.1 操作](#711-操作)
            - [1）连接redis：redis-cli.exe【cmd】](#1连接redisredis-cliexecmd)
            - [2）配置文件：redis.windows.conf](#2配置文件rediswindowsconf)
            - [3）redis 有两种备份（存储）机制：RDB 、 AOF](#3redis-有两种备份存储机制rdb--aof)
                - [RDB：如果我有100条数据，那么我每隔一段时间我就保存这100条数据一次 【相当于只保存最后的结果】](#rdb如果我有100条数据那么我每隔一段时间我就保存这100条数据一次-相当于只保存最后的结果)
                - [AOF：我把对数据的操作【命令】保存下来 【相当于保存过程，通过这个过程我可以得到最后的结果】](#aof我把对数据的操作命令保存下来-相当于保存过程通过这个过程我可以得到最后的结果)
    - [7.2 赞踩实现：通过集合](#72-赞踩实现通过集合)
        - [7.2.1 Redis 在牛客中的应用](#721-redis-在牛客中的应用)
            - [1）PV ：这些浏览数这些都是在redis中存储的，然后再慢慢同步到额外的字段](#1pv-这些浏览数这些都是在redis中存储的然后再慢慢同步到额外的字段)
            - [2）点赞 ：你点一个赞，我就会把userid放到帖子的集合里面，再点赞，就从集合删掉，我只需要知道集合里有多少数据既可以了](#2点赞-你点一个赞我就会把userid放到帖子的集合里面再点赞就从集合删掉我只需要知道集合里有多少数据既可以了)
            - [3）关注 ： 我关注谁，谁关注我，这些都是一个个的集合，删除关注就从集合中拿出来](#3关注--我关注谁谁关注我这些都是一个个的集合删除关注就从集合中拿出来)
            - [4）排行榜 ：所有排行榜都是用的redis的 Sorted set ,即优先队列集合，比如登陆排行榜，你每登陆一次，我就把你的数字加1](#4排行榜-所有排行榜都是用的redis的-sorted-set-即优先队列集合比如登陆排行榜你每登陆一次我就把你的数字加1)
            - [5）验证码 ：setex 设置过期时间](#5验证码-setex-设置过期时间)
            - [6）缓存 ： 在底层MySQL数据库上做一个缓存，你所有的数据都是放在redis上的，也会做一个缓存的时间，当用户更新的时候，就把缓存失效掉，每次优先从缓存里取，缓存没有再去数据库，这样就可以大大降低数据库的压力](#6缓存--在底层mysql数据库上做一个缓存你所有的数据都是放在redis上的也会做一个缓存的时间当用户更新的时候就把缓存失效掉每次优先从缓存里取缓存没有再去数据库这样就可以大大降低数据库的压力)
            - [7）异步队列 ：我给你点了一个赞，你就会收到一条消息谁谁谁给你点了一个赞，这些都是异步队列，其实就是 list 结构，发生什么事情，就抛一个事件到 list ，然后有一个额外的线程去处理](#7异步队列-我给你点了一个赞你就会收到一条消息谁谁谁给你点了一个赞这些都是异步队列其实就是-list-结构发生什么事情就抛一个事件到-list-然后有一个额外的线程去处理)
            - [8）判题队列 ： 大家都在提交代码编程，我不可能实时同步的处理，所以放在一个队列里，哪台机器有空闲的时候再去跑这个代码编译](#8判题队列--大家都在提交代码编程我不可能实时同步的处理所以放在一个队列里哪台机器有空闲的时候再去跑这个代码编译)
        - [7.2.2 赞功能的实现](#722-赞功能的实现)
            - [1）Redis数据库没有 model、DAO 这些概念，所以直接在service里定义功能即可](#1redis数据库没有-modeldao-这些概念所以直接在service里定义功能即可)
                - [1.1 我们采用连接池 JedisPool ,所以需要对jedis 的操作进行包装，做一个包装类JedisAdapter在util下,数据不再通过DAO，而是 JedisAdapter 来读取操作](#11-我们采用连接池-jedispool-所以需要对jedis-的操作进行包装做一个包装类jedisadapter在util下数据不再通过dao而是-jedisadapter-来读取操作)
                - [1.2 在 service 下定义一个 LikeService](#12-在-service-下定义一个-likeservice)
                - [1.3 定义入口 LikeController](#13-定义入口-likecontroller)
                - [1.4 注意事项：一个资讯可能对应有 pv、赞、踩这些不同的集合，所以集合的命名要有规范，一般我们把业务作为集合的前缀](#14-注意事项一个资讯可能对应有-pv赞踩这些不同的集合所以集合的命名要有规范一般我们把业务作为集合的前缀)
                - [1.5 注意事项：要修改首页的显示，在home.html里，根据你点的喜欢不喜欢进行加量【不同显示】,](#15-注意事项要修改首页的显示在homehtml里根据你点的喜欢不喜欢进行加量不同显示)
                - [1.6 注意事项：同时在HomeController里也要修改一下，把vo.like 传进去](#16-注意事项同时在homecontroller里也要修改一下把volike-传进去)
                - [1.7 注意事项：在NewsController也要改一下，把like 传进去，因此detail也要修改【detail的修改和home一样】](#17-注意事项在newscontroller也要改一下把like-传进去因此detail也要修改detail的修改和home一样)
- [第八周 异步设计和站内邮件通知系统](#第八周-异步设计和站内邮件通知系统)
    - [一、异步队列](#一异步队列)
        - [1.1 通过队列来实现异步结构](#11-通过队列来实现异步结构)
            - [1.1.1 流程：](#111-流程)
            - [1.1.2 异步的好处：](#112-异步的好处)
        - [1.2 代码实现：在LikeController 里面把like事件用 EventProducer 发（lpush）到阻塞队列,在EventConsumer 里从阻塞队列不断阻塞（brpop）取出事件,处理它](#12-代码实现在likecontroller-里面把like事件用-eventproducer-发lpush到阻塞队列在eventconsumer-里从阻塞队列不断阻塞brpop取出事件处理它)
            - [1.把事件放进队列，取出来，涉及到队列的序列化【event 序列化存进队列，取出来时再反序列化】](#1把事件放进队列取出来涉及到队列的序列化event-序列化存进队列取出来时再反序列化)
            - [2、先创一个异步包 async 用于存放异步相关的代码](#2先创一个异步包-async-用于存放异步相关的代码)
                - [1）EventType：表示刚刚发生了什么事件，事件是什么类型【是一个枚举类】](#1eventtype表示刚刚发生了什么事件事件是什么类型是一个枚举类)
                - [2）EventModel：事件发生时现场的数据信息都打包在这个 EventModel里面【事件的数据】](#2eventmodel事件发生时现场的数据信息都打包在这个-eventmodel里面事件的数据)
                - [3) EventHandle 接口：可以处理各种各样的事情，比如LikeHandle 处理踩赞相关的事情 需要实现这个接口](#3-eventhandle-接口可以处理各种各样的事情比如likehandle-处理踩赞相关的事情-需要实现这个接口)
                - [3）EventProducer：用于把事件发给阻塞队列，即把事件序列化后放到 Redis 的一个队列里面](#3eventproducer用于把事件发给阻塞队列即把事件序列化后放到-redis-的一个队列里面)
                - [4）EventConsumer：去取队列里面的事件，取出来后反序列化成 事件发生时的现场（EventModel）,根据事件去找到它对应的 Handler ，把事件处理掉](#4eventconsumer去取队列里面的事件取出来后反序列化成-事件发生时的现场eventmodel根据事件去找到它对应的-handler-把事件处理掉)
            - [3、在LikeController 里试验，赞一下，把赞事件加入队列处理](#3在likecontroller-里试验赞一下把赞事件加入队列处理)
    - [二、站内邮件通知系统：写一个异常登陆通知](#二站内邮件通知系统写一个异常登陆通知)
        - [1）在 LoginController 里通过 EventProducer 把登陆这个事件发给队列](#1在-logincontroller-里通过-eventproducer-把登陆这个事件发给队列)
        - [2）处理队列，如果登陆有异常就发送邮件和站内信【没有写登陆异常算法，所以一登录就会发邮件和站内信】【**邮件是模板才能个性化】](#2处理队列如果登陆有异常就发送邮件和站内信没有写登陆异常算法所以一登录就会发邮件和站内信邮件是模板才能个性化)
- [第九周 多种资讯排序算法以及多线程讲解](#第九周-多种资讯排序算法以及多线程讲解)
    - [一、资讯排序](#一资讯排序)
        - [1.1 通用排序：根据某个值来排序，order by](#11-通用排序根据某个值来排序order-by)
            - [1.按单位时间内的交互数，del.icio.us按1小时内收藏排行来排序：参与的人越多，认为它越火](#1按单位时间内的交互数delicious按1小时内收藏排行来排序参与的人越多认为它越火)
            - [2.按总交互数，按总点赞数来排序：可能这个话题比较有争议，所以数据很大，然而并没有随着时间的推移沉下去](#2按总交互数按总点赞数来排序可能这个话题比较有争议所以数据很大然而并没有随着时间的推移沉下去)
            - [3.按评论数加权：评论比赞有价值，](#3按评论数加权评论比赞有价值)
            - [4.按时间排序：时间最新放最前面](#4按时间排序时间最新放最前面)
        - [1.2代码实现：以log 得方式大幅度降低大流量导致得数据的波动、用指数来提升权重](#12代码实现以log-得方式大幅度降低大流量导致得数据的波动用指数来提升权重)
        - [1.3 Hacker News 的排序：//一个资讯网站：核心就是通过一个公式算一个分数](#13-hacker-news-的排序一个资讯网站核心就是通过一个公式算一个分数)
        - [1.3 Reddit  //也是分享资讯的一个网站 ：核心也是通过一个资讯算一个分数 【时间是最重要的权重，以log 得方式大幅度降低大流量导致的点赞数，适合流量大的新闻类排序】](#13-reddit--也是分享资讯的一个网站-核心也是通过一个资讯算一个分数-时间是最重要的权重以log-得方式大幅度降低大流量导致的点赞数适合流量大的新闻类排序)
        - [1.4 StackOverflow //一个问答网站](#14-stackoverflow-一个问答网站)
        - [1.5 IMDB  //一个电影评分网站 分为两拨：总的平均值和当前电影投票的平均值](#15-imdb--一个电影评分网站-分为两拨总的平均值和当前电影投票的平均值)
    - [二、多线程：怎么通过多线程处理任务](#二多线程怎么通过多线程处理任务)
        - [2.1 多线程的优势和挑战](#21-多线程的优势和挑战)
            - [1） 优势：可以充分利用多处理器；可以异步处理任务](#1-优势可以充分利用多处理器可以异步处理任务)
            - [2） 挑战：数据会被多个线程访问，有安全性问题；不活跃的线程也会占用内存资源； 可能导致死锁](#2-挑战数据会被多个线程访问有安全性问题不活跃的线程也会占用内存资源-可能导致死锁)
        - [2.2 Thread ,Synchronized](#22-thread-synchronized)
        - [2.3 BlockingQueue,AtomicInteger](#23-blockingqueueatomicinteger)
        - [2.4 ThreadLocal,Executor,Future](#24-threadlocalexecutorfuture)
- [第十周 JavaWeb项目测试和部署，课程总结回顾](#第十周-javaweb项目测试和部署课程总结回顾)
    - [一、单元测试JUnit简介：测试某个模块或者细小的接口是否正常](#一单元测试junit简介测试某个模块或者细小的接口是否正常)
        - [1.1 测试四部曲：【代码演示：验证点赞是否有效】【每个service都写一个对应的serviceTest】](#11-测试四部曲代码演示验证点赞是否有效每个service都写一个对应的servicetest)
            - [1）初始化数据](#1初始化数据)
            - [2）执行要测试的业务：有正确测试，异常测试，压力测试【疯狂地调用】等](#2执行要测试的业务有正确测试异常测试压力测试疯狂地调用等)
            - [3）验证测试数据](#3验证测试数据)
            - [4）清理数据](#4清理数据)
    - [二、打包步骤：【用maven把项目打成war包】](#二打包步骤用maven把项目打成war包)
    - [三、部署:【把war包拷贝到tomcat的webapps下】//【未弄linux下的部署】](#三部署把war包拷贝到tomcat的webapps下未弄linux下的部署)
        - [1）War包](#1war包)
        - [2）Tomcat服务器](#2tomcat服务器)
            - [2.1)window下的部署：](#21window下的部署)
            - [2.2）Linux下的部署](#22linux下的部署)
    - [四、总结](#四总结)
        - [1.开发工具Git，IntilliJ](#1开发工具gitintillij)
        - [2.Spring Boot，Velocity](#2spring-bootvelocity)
            - [一、做网站从宏观上来看，分为几个层次,](#一做网站从宏观上来看分为几个层次)
            - [二、还讲了interceptor【拦截器】：](#二还讲了interceptor拦截器)
        - [三、还讲了面向切面编程 @Aspect](#三还讲了面向切面编程-aspect)
        - [四、还讲了redis 【JedisAdapter】](#四还讲了redis-jedisadapter)
        - [五、还讲了异步框架](#五还讲了异步框架)
        - [六、还做了云存储，用的七牛云](#六还做了云存储用的七牛云)
        - [七、登陆注册，用了拦截器，拦截器这里的核心是hostHolder  ***](#七登陆注册用了拦截器拦截器这里的核心是hostholder--)
        - [7.发邮件](#7发邮件)
        - [8.单元测试/部署](#8单元测试部署)
    - [五、扩展](#五扩展)
    - [六、面试怎么讲](#六面试怎么讲)
- [附录：问题](#附录问题)

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
            * application.properties：配置文件，Spring容器？【Spring Boot使用了一个全局的配置文件application.properties，放在src/main/resources目录下。Sping Boot的全局配置文件的作用是对一些默认配置的配置值进行修改。】
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

### 2.4 代码演示：Request/Response/Session【HttpServletReques、HttpServletResponse、HttpSession】
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
* //会自动把http请求信息包装成request，返回信息包装成response这两个变量
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

## 2.7 IOC：控制反转/依赖注入：思想：把它的实现放在一个地方，用到它的时候用最简单的方式把它注入进来
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
    @Before("execution(* com.nowcode.toutiao.controller.*Controller.*(..))")
    public void beforeMethod(JoinPoint joinPoint) {  //把交互的这个口做了一个包装，叫切点
        StringBuilder sb = new StringBuilder();
        for (Object arg : joinPoint.getArgs()) {
            sb.append("arg:" + arg.toString() + "|");
        }

        sb.append(joinPoint.getTarget().getClass().getName());  //记录切点所在的类
        logger.info("before method: " + sb.toString());
    }

    //在执行这些函数之后要执行afterMethod
    @After("execution(* com.nowcode.toutiao.controller.*Controller.*(..))")
    public void afterMethod(JoinPoint joinPoint) {
        logger.info("after method: "+joinPoint.getTarget().getClass().getName());
    }
}

```

# 第三周 数据库交互mybatis集成
## 总结构：controller网页入口、DAO数据库层、service、model模型【和数据库里面都是一一匹配的】：controller调用service【你想读取还是做什么操作都在这里】，service去调用 DAO【定义了对数据库的操作，比如增删改查等】

* 利用mybatis这框架从数据库读取数据，并在页面上显示：把每天的资讯在页面上分成小块，数据的来源是数据库，用户也是从数据库中读取过来的，做一个最基本的数据库的读取和velocity前端的展示连起来
* * mybatis框架就是为了把数据库的表和你的model做一个映射，你有一张user表，一般就有一个user的model，方便你读取
## 3.1 数据库的设计【四张表：user表、站内信（Message）、资讯（News）、评论（Comment）】
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

## 3.5 ViewObject【视图对象】和DateTool【自带的时间的工具】
### 3.5.1  ViewObject:方便传递任何数据到Velocity，给模板提供数据；实质就是一个map,把字段和对象对应起来
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
### 4.1.1 注册注意事项：【用户名和密码】
#### 1. 用户名合法性检测（长度（数据库字段长度是有限制的），敏感词（比如管理员等），重复，特殊字符（比如颜文字（对网站的展示有影响）、HTML代码等））
#### 2. 密码长度要求（以及大小写等，后台会对强度进行判定）
#### 3. 密码salt加密，密码强度检测（md5库）
* 把密码用md5加密，直接放在数据库是有危险的，因为有很多md5破解网站，输入md5值，会得到密码【原理：这些网站把互联网上常用的密码或者已经泄露的密码做md5的密码，所以就可以用md5去查密码】
* 如果你对密码进行加salt，然后再md5加密，那么肯定就查不到了，因为肯定没人以前会用这样的密码，
#### 4. 用户邮件/短信激活【防止机器人注册很多账号】
### 4.1.2 代码 步骤：
#### 1) 在开发过程中，我们会对不同的业务做不同的Controller，比如登陆注册，我们会再做一个LoginController
```java

@Controller
public class LoginController {
    private static final Logger logger = (Logger) LoggerFactory.getLogger(LoginController.class);
    @Autowired
    UserService userService;

    //注册只要你给我提交用户名和密码即可，我将调用UserService的 register 方法【此方法里调用了userDAO.addUser(user)】，将用户增加到数据库
    @RequestMapping(path={"/reg/"},method = {RequestMethod.GET,RequestMethod.POST})
    @ResponseBody
    public String reg(Model model, @RequestParam("username") String username,
                      @RequestParam("password") String password,
                      @RequestParam(value = "remenber",defaultValue = "0") int rememberme) {//即是否记住登录
        try {
            // 1）******* 将用户增加到数据库
            Map<String ,Object>  map = userService.register(username,password); 
            if(map.isEmpty()){ //map为空，说明注册成功，****** 以json的形式返回
                //return ToutiaoUtil.getJSONString(0,map); 
                //当然也可以选择这样返回
                return ToutiaoUtil.getJSONString(0,"注册成功");
            }else {
                return ToutiaoUtil.getJSONString(1,map);
            }

        }catch (Exception e){
            logger.error("注册异常"+e.getMessage());
            return ToutiaoUtil.getJSONString(1,"注册异常");
        }
    }


}

```
#### 2）注册对数据库来说就是要往里面加一个人
* 请求来了从controller进入，然后调用service的某个方法【所以此处我们要在UserService里面增加 register 方法，并在此方法里调用  userDAO.addUser(user); // 往数据库加入用户】，数据库操作是定义在DAO中的（select、update、insert（addUser）等等）【我们之前已经定义过了，所以直接用就好，还要加一个selectByName方法】
    * UserDAO
```java
    //UserDAO
    //根据名字来选，以此判断用户是否已经注册过了
    @Select({"select" ,SELECT_FIELDS,"from ",TABLE_NAME, "where name=#{name}"})
    User selectByName(String name);
```
* UserService
```java
package com.nowcode.toutiao.service;


import com.nowcode.toutiao.dao.UserDAO;
import com.nowcode.toutiao.model.User;
import com.nowcode.toutiao.util.ToutiaoUtil;
import org.apache.commons.lang.StringUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.HashMap;
import java.util.Map;
import java.util.Random;
import java.util.UUID;

@Service
public class UserService {
    @Autowired
    private UserDAO userDAO;

    public Map<String,Object> register(String username, String password){
        Map<String,Object> map = new HashMap<>();

        // 1）先进行一些判断，再决定要不要加，然后把信息返回到前台（controller）
        //前端也会做这些验证，但后端也要做，因为很多人攻击并不是通过网页来的，而是直接post等
        if (StringUtils.isBlank(username)){  //StringUtils是比较好用的一个类org.apache.commons.lang.StringUtils;
            map.put("msgname","用户名不能为空");   //告诉用户不能为空
            return map;  //你是空，不能走下去，直接返回
        }
        if (StringUtils.isBlank(password)){
            map.put("msgpsw","密码不能为空");   //告诉用户不能为空
            return map;
        }

        User user = userDAO.selectByName(username); //在数据库中查找
        if (user != null){
            map.put("msgname","用户名已经被注册");
            return map;
        }

        // 2）通过了你的验证，用户名、密码这些都符合要求，就注册
        user = new User();
        user.setName(username);
        //会生成唯一的UUID，取前5位作为salt
        user.setSalt(UUID.randomUUID().toString().substring(0,5));
        user.setHeadUrl(String.format("http://images.nowcoder.com/head/%dm.png",new Random().nextInt(1000)));//随机给一个头像
        user.setPassword(ToutiaoUtil.MD5(password + user.getSalt()));  //密码加上盐再用md5加密，这种工具类的，你可以专门写一个util工具类来

        userDAO.addUser(user); //******** 往数据库加入用户

        //登录，一般网站都是注册了马上登陆

        return map;  //返回状态，正常加入里面应该是null
    }

    public User getUser(int id){
        return userDAO.selectById(id);
    }

}

```
* 自己写的工具类 
```java
package com.nowcode.toutiao.util;

import com.alibaba.fastjson.JSONObject;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.security.MessageDigest;
import java.util.Map;

public class ToutiaoUtil {
    private static final Logger logger = LoggerFactory.getLogger(ToutiaoUtil.class);


    //用于数据怎么展示给前端看，返回json串，用alibaba.fastjson.
    //告诉用户是否成功，0是正确，1是错误
    public static String getJSONString(int code ){
        JSONObject json = new JSONObject();
        json.put("code", code);
        return json.toJSONString();
    }

    //告诉用户是否成功，0是正确，1是错误,把map里面的每个字段都写在json里面
    public static String getJSONString(int code , Map<String,Object> map ){
        JSONObject json = new JSONObject();
        json.put("code", code);
        for(Map.Entry<String,Object> entry: map.entrySet()){
            json.put(entry.getKey(),entry.getValue());
        }
        return json.toJSONString();
    }

    //我还可以返回一些消息
    public static String getJSONString(int code, String msg) {
        JSONObject json = new JSONObject();
        json.put("code", code);
        json.put("msg", msg);
        return json.toJSONString();
    }



    //用于加密
    public static String MD5(String key) {
        char hexDigits[] = {
                '0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'A', 'B', 'C', 'D', 'E', 'F'
        };
        try {
            byte[] btInput = key.getBytes();
            // 获得MD5摘要算法的 MessageDigest 对象
            MessageDigest mdInst = MessageDigest.getInstance("MD5");
            // 使用指定的字节更新摘要
            mdInst.update(btInput);
            // 获得密文
            byte[] md = mdInst.digest();
            // 把密文转换成十六进制的字符串形式
            int j = md.length;
            char str[] = new char[j * 2];
            int k = 0;
            for (int i = 0; i < j; i++) {
                byte byte0 = md[i];
                str[k++] = hexDigits[byte0 >>> 4 & 0xf];
                str[k++] = hexDigits[byte0 & 0xf];
            }
            return new String(str);
        } catch (Exception e) {
            logger.error("生成MD5失败", e);
            return null;
        }
    }
}

```
## 4.2 登陆/登出（浏览器演示）
### 4.2.1 登陆：
#### 1.服务器密码校验/三方校验回调，token登记
#### 1.1 服务器验证密码正确后，会生成一个token关联userid,下发token给客户端
#### 1.2 客户端存储token(app存储本地，浏览器存储cookie),每次和服务器进行交互的时候就带上这个token，服务器就知道你是谁了
#### 2.服务端/客户端token有效期设置（记住登陆）
#### 注:token可以是sessionid【那么你浏览器不会有变化，只要你没关，都是这个sessionid】，或者是cookie里的一个key
### 4.2.2 代码步骤：
#### 1）登录就是把你的用户名，密码这些进行判断验证
#### 2）验证成功了，就要给用户下发一个ticket【token】
##### 2.1）在这里要数据库新创建一个 login_ticket 表【我这里的建法是在通过InitDatabaseTests，在测试的时候建的，一般我们写一个DAO都会测试一下有没有问题】 
```java

@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(classes = ToutiaoApplication.class)
@WebAppConfiguration
@Sql({"/init-schema.sql"}) //在执行用例前先执行这份文件，创建数据库
public class InitDatabaseTests {

	@Autowired
	UserDAO userDAO;  //


	@Autowired
	NewsDAO newsDAO;

	@Autowired
	LoginTicketDAO loginTicketDAO;

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

			//测试newsDAO,cahru
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
			userDAO.updatePassword(user); //把user的属性值更新到数据库中去\

			//测试ticket
			LoginTicket ticket = new LoginTicket();
			ticket.setStatus(0);
			ticket.setUserId(i+1);
			ticket.setExpired(date);
			ticket.setTicket(String.format("TICKET%d",i+1));
			loginTicketDAO.addTicket(ticket); //插入一条ticket记录

			loginTicketDAO.updateStatus(ticket.getTicket(),2); //更新它的状态

		}
		//在数据库查询id=1的用户的密码,因为这是一个测试用例，所以可以用Assert来帮助
		Assert.assertEquals("newpassword", userDAO.selectById(1).getPassword());
		userDAO.deleteById(1);//在数据库删除id为1的用户
		Assert.assertNull("null",userDAO.selectById(1));

		Assert.assertEquals(1,loginTicketDAO.selectByTicket("TICKET1").getUserId());
		Assert.assertEquals(2,loginTicketDAO.selectByTicket("TICKET1").getStatus());
	}

}


```
##### 2.2）同时对应一个 LoginTicket的 model，
##### 2.3）及LoginTicketDAO【插入ticket、查、更新状态】
```java
package com.nowcode.toutiao.dao;

import com.nowcode.toutiao.model.LoginTicket;
import com.nowcode.toutiao.model.User;
import org.apache.ibatis.annotations.*;

@Mapper  //表示我和数据库中的表是一一匹配的,然后写各种sql语句
public interface LoginTicketDAO {
    String TABLE_NAME="login_ticket";
    String INSERT_FIELDS="user_id,expired,status,ticket";
    String SELECT_FIELDS ="id,"+INSERT_FIELDS;
    @Insert({"insert into",TABLE_NAME,"(",INSERT_FIELDS,")","values(#{userId},#{expired},#{status}," +
            "#{ticket})"})
    int addTicket(LoginTicket loginTicket);  //它自己会在LoginTicket里找这些属性

    @Select({"select" ,SELECT_FIELDS,"from ",TABLE_NAME, "where ticket=#{ticket}"})
    User selectByTicket(String  ticket);

    @Update({"update",TABLE_NAME,"set status=#{status} where ticket=#{ticket}"})
    void updateStatus(@Param("ticket") String ticket,@Param("status") String status);

}

```
##### 2.4) LoginController里写登陆的函数 login 
```java
    //登陆
    @RequestMapping(path={"/login/"},method = {RequestMethod.GET,RequestMethod.POST})
    @ResponseBody
    public String login(Model model, @RequestParam("username") String username,
                      @RequestParam("password") String password,
                      @RequestParam(value = "rember",defaultValue = "0") int rememberme,
                        HttpServletResponse response) {//即是否记住登录
        try {
            Map<String ,Object>  map = userService.login(username,password);
            if(map.containsKey("ticket")){  //登陆成功会下发一个ticket
                Cookie cookie = new Cookie("ticket", map.get("ticket").toString());
                cookie.setPath("/");
                if (rememberme > 0) {
                    cookie.setMaxAge(3600*24*5);
                }
                response.addCookie(cookie);
                return ToutiaoUtil.getJSONString(0,map);
            }else {
                return ToutiaoUtil.getJSONString(1,map);
            }

        }catch (Exception e){
            logger.error("注册异常"+e.getMessage());
            return ToutiaoUtil.getJSONString(1,"注册异常");
        }
    }
```

### 4.2.2 登出：把ticket对应的status设置为1，就表示这个无效了
#### 1） 首先LoginController里进入登出的函数
```java
@Controller
public class LoginController {
    private static final Logger logger = (Logger) LoggerFactory.getLogger(LoginController.class);
    @Autowired
    UserService userService;

    //注册只要你给我提交用户名和密码即可
    @RequestMapping(path={"/reg/"},method = {RequestMethod.GET,RequestMethod.POST})
    @ResponseBody
    public String reg(Model model, @RequestParam("username") String username,
                      @RequestParam("password") String password,
                      @RequestParam(value = "remenber",defaultValue = "0") int rememberme,
                      HttpServletResponse response) {//即是否记住登录
        try {
            Map<String ,Object>  map = userService.register(username,password);
            if(map.containsKey("ticket")){  //注册成功会下发一个ticket ,要把这个ticket放到cookie里面去
                Cookie cookie = new Cookie("ticket",map.get("ticket").toString());
                if(rememberme > 0){
                    cookie.setMaxAge(3600*24*5); //如果用户说要remember，那么时间应该设置长一些,如果不写，则默认浏览器关闭后就没有了
                }
                cookie.setPath("/");  //设置这个cookie的路径是全站有效的
                response.addCookie(cookie);
                return ToutiaoUtil.getJSONString(0,"注册成功");
            }else {
                return ToutiaoUtil.getJSONString(1,map);
            }

        }catch (Exception e){
            logger.error("注册异常"+e.getMessage());
            return ToutiaoUtil.getJSONString(1,"注册异常");
        }
    }


    //登陆
    @RequestMapping(path={"/login/"},method = {RequestMethod.GET,RequestMethod.POST})
    @ResponseBody
    public String login(Model model, @RequestParam("username") String username,
                      @RequestParam("password") String password,
                      @RequestParam(value = "rember",defaultValue = "0") int rememberme,
                        HttpServletResponse response) {//即是否记住登录
        try {
            Map<String ,Object>  map = userService.login(username,password);
            if(map.containsKey("ticket")){  //登陆成功会下发一个ticket
                Cookie cookie = new Cookie("ticket", map.get("ticket").toString());
                cookie.setPath("/");
                if (rememberme > 0) {
                    cookie.setMaxAge(3600*24*5);
                }
                response.addCookie(cookie);
                return ToutiaoUtil.getJSONString(0,map);
            }else {
                return ToutiaoUtil.getJSONString(1,map);
            }

        }catch (Exception e){
            logger.error("注册异常"+e.getMessage());
            return ToutiaoUtil.getJSONString(1,"注册异常");
        }
    }


    //登出
    @RequestMapping(path={"/logout/"},method = {RequestMethod.GET,RequestMethod.POST})
    //要去掉 @ResponseBody，不然返回会被当成字符串
    public String logout(@CookieValue("ticket") String ticket){
        userService.logout(ticket);  //把ticket对应的status过期掉就是登出了，无效了
        return "redirect:/";  //登出后自动跳转到首页
    }
}

```
#### 2） 其次，userService里写一个logout函数
```java
@Service
public class UserService {
    @Autowired
    private UserDAO userDAO;

    @Autowired
    private LoginTicketDAO loginTicketDAO;

    //注册
    public Map<String,Object> register(String username, String password){
        Map<String,Object> map = new HashMap<>();

        // 1）先进行一些判断，再决定要不要加，然后把信息返回到前台（controller）
        //前端也会做这些验证，但后端也要做，因为很多人攻击并不是通过网页来的，而是直接post等
        if (StringUtils.isBlank(username)){  //StringUtils是比较好用的一个类org.apache.commons.lang.StringUtils;
            map.put("msgname","用户名不能为空");   //告诉用户不能为空
            return map;  //你是空，不能走下去，直接返回
        }
        if (StringUtils.isBlank(password)){
            map.put("msgpsw","密码不能为空");   //告诉用户不能为空
            return map;
        }

        User user = userDAO.selectByName(username); //在数据库中查找
        if (user != null){
            map.put("msgname","用户名已经被注册");
            return map;
        }

        // 2）通过了你的验证，用户名、密码这些都符合要求，就注册
        user = new User();
        user.setName(username);
        //会生成唯一的UUID，取前5位作为salt
        user.setSalt(UUID.randomUUID().toString().substring(0,5));
        user.setHeadUrl(String.format("http://images.nowcoder.com/head/%dm.png",new Random().nextInt(1000)));//随机给一个头像
        user.setPassword(ToutiaoUtil.MD5(password + user.getSalt()));  //密码加上盐再用md5加密，这种工具类的，你可以专门写一个util工具类来

        userDAO.addUser(user); //******** 往数据库加入用户

        //登录，一般网站都是注册了马上登陆
        String ticket = addLoginTicket(user.getId()); //给这个user下发一个ticket
        map.put("ticket",ticket);

        return map;  //返回状态，正常加入里面应该是null
    }

    //登陆
    public Map<String,Object> login(String username, String password){
        Map<String,Object> map = new HashMap<>();

        // 1）登录就是把你的用户名，密码这些进行判断验证
        if (StringUtils.isBlank(username)){
            map.put("msgname","用户名不能为空");   //告诉用户不能为空
            return map;
        }
        if (StringUtils.isBlank(password)){
            map.put("msgpsw","密码不能为空");   //告诉用户不能为空
            return map;
        }

        User user = userDAO.selectByName(username); //在数据库中查找
        if (user == null){
            map.put("msgname","用户名不存在"); //*** 差别
            return map;
        }
        //用户存在，就开始验证密码
        if(!ToutiaoUtil.MD5(password+user.getSalt()).equals(user.getPassword())){//不等于说明这个密码有问题
            map.put("msgpwd","密码不正确");
            return map;
        }

        // 2) 验证成功了，就要给用户下发一个ticket【token】
        String ticket = addLoginTicket(user.getId()); //给这个user配套一个ticket
        map.put("ticket",ticket);

        return map;  //返回状态，正常加入里面应该是null
    }

    private String addLoginTicket(int userId){
        LoginTicket ticket = new LoginTicket();
        ticket.setUserId(userId);
        Date date = new Date();
        date.setTime(date.getTime() + 1000*3600*24); //24小时后
        ticket.setExpired(date);
        ticket.setStatus(0);
        ticket.setTicket(UUID.randomUUID().toString().replaceAll("-",""));//UUID可能有—，所以替换掉就成为了纯字符
        loginTicketDAO.addTicket(ticket);
        return ticket.getTicket();
    }

    //登出
    public void logout(String ticket){
        loginTicketDAO.updateStatus(ticket,1);  //把状态改为1 ，说明过期了
    }


    public User getUser(int id){
        return userDAO.selectById(id);
    }

}

```



## 4.3 浏览
### 4.3.1 整体步骤：
#### 1.客户端：带token【也就是ticket】的HTTP请求
#### 2.服务端：【用户是否登陆、用户有无权限】
##### 1).根据token获取用户id
##### 2).根据用户id获取用户的具体信息
##### 3).用户和页面访问权限处理
##### 4).渲染页面/跳转页面【成功就渲染、不成功就跳转到非法页面】
## 4.3.2 Interceptor(拦截器)：可以拦截你整个链路上的各个步骤
### 1）preHandle：用于处理前，一般在preHandle里判断权限
### 2）postHandle：用于处理完后，一般在PostHandle里设置一些数据
### 3）afterCompletion：用于全体结束后， 一般用于收尾工作

```java
public class PassportInterceptorimplements HandlerInterceptor{
//处理前，一般在preHandle里判断权限
boolean preHandle(HttpServletRequestvar1, HttpServletResponsevar2,Object var3) throws Exception;
//处理完后，一般在PostHandle里设置一些数据
void postHandle(HttpServletRequestvar1, HttpServletResponsevar2, Object var3,ModelAndView var4) throws Exception;
//全体结束
void afterCompletion(HttpServletRequestvar1, HttpServletResponsevar2,Object var3, Exception var4) throws Exception;
}
```

### 代码演示
#### 1）创建一个拦截器PassportInterceptor：每次访问之前，拦截器先插进来先找这个用户
* 根据用户传入的 ticket 来处理,创建一个Interceptor包，再建一个 PassportInterceptor 类。
```java
@Component
public class PassportInterceptor implements HandlerInterceptor {

    @Autowired
    private LoginTicketDAO loginTicketDAO;

    @Autowired
    private UserDAO userDAO;

    @Autowired
    private HostHolder hostHolder;

    @Override
    public boolean preHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o) throws Exception {
        String ticket = null;
        // 遍历传入的cookie里面是否有ticket字段
        if(httpServletRequest.getCookies() != null){
            for(Cookie cookie: httpServletRequest.getCookies()){
                if(cookie.getName().equals("ticket")){
                    ticket = cookie.getValue();
                    break;
                }
            }
        }

        if(ticket !=null){ //说明找到了
            LoginTicket loginTicket = loginTicketDAO.selectByTicket(ticket);
            // 1）判断这个ticket是否是真的存在，有效期过期没，以及这个ticket是否还有效
            if (loginTicket == null || loginTicket.getExpired().before(new Date()) || loginTicket.getStatus() !=0){
                return true; //如果返回false，那么后面的postHandle这些都不会执行了，所以要返回true
            }

            // 2）通过上方的验证，那么我知道你是谁了,我就要把你记录下来，
            // ****存在本地线程里面，因为我要随时知道当前调用我这个线程的人是谁
            User user = userDAO.selectById(loginTicket.getUserId());
            hostHolder.setUser(user);  //把用户临时存起来，存在线程里 ThreadLocal

        }
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, ModelAndView modelAndView) throws Exception {
        //3）好处：这是在渲染之前，我把用户存进来，那么我在模板上就能直接用 user
        if(modelAndView != null && hostHolder.getUser()!= null){
            modelAndView.addObject("user",hostHolder.getUser());
        }
    }

    @Override
    public void afterCompletion(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) throws Exception {
        hostHolder.clear();   //做收尾工作，把这个线程对应的用户清理掉
    }
}

```

#### 2）在model里创建一个HostHolder 类，用于存放这一次访问里面的用户是谁 【不同线程有自己的用户】
```java
@Component
public class HostHolder {  //用于存当前的用户是谁

    //每条线程存取它自己的东西【因为有很多人在访问，但只有一个component，所以应该存在自己的线程里】
    private static ThreadLocal<User> users = new ThreadLocal<User>();

    public User getUser(){
        users.get();
    } //提取出来

    public void setUser(User user){
        users.set(user);
    }  //存进来

    public void clear(){
        users.remove();  //把存的这个用户删除掉
    }
}

```
* modelAndView.addObject("user",hostHolder.getUser());之后就能在模板上直接用user.我的所有html页面上都能直接用这个变量
```html
<!--如果这个用户存在，我就显示用户的名字，如果不存在，我就显示登陆-->
       <ul class="nav navbar-nav navbar-right">
                <li class=""><a href="http://nowcoder.com/explore">发现</a></li>

                #if ($user)
                <li class="js-login"><a href="javascript:void(0);">$!{user.name}</a></li>
                #else
                <li class="js-login"><a href="javascript:void(0);">登陆</a></li>
                #end
            </ul>
```
#### 3）拦截器写好后要注册要 MVC 里面，创建一个configuration包，创建以一个ToutiaoWebConfiguration类继承 WebMvcConfigurerAdapter，实现它的addInterceptors方法
```java
//你可以继承某些类，能帮你初始化某些配置
@Component
public class ToutiaoWebConfiguration extends WebMvcConfigurerAdapter {
    @Autowired
    PassportInterceptor passportInterceptor;

    @Autowired
    LoginRequiredInterceptor loginRequiredInterceptor;


    @Override
    public void addInterceptors(InterceptorRegistry registry){ //注册拦截器
        registry.addInterceptor(passportInterceptor);  //这个拦截器用于拦截看看用户是谁，用于全局所有的页面
        //用于拦截看看它访问的页面我是否符合要求,且这个拦截器只处理/setting开头的页面
        registry.addInterceptor(loginRequiredInterceptor).addPathPatterns("/setting*");
        super.addInterceptors(registry);
    }
}

```

## 4.4 可以有多个拦截器：比如某个页面要登陆了才能访问，未登录就跳转到首页去【只针对/setting开头的页面】【不算项目内容】
* 再弄一个拦截器，然后这些页面全部都要做一个登陆的判断
```java
@Component
public class LoginRequiredInterceptor implements HandlerInterceptor {
    @Autowired
    private HostHolder hostHolder;

    @Override
    public boolean preHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o) throws Exception {
       if(hostHolder.getUser() == null){ //说明没登录，那你就没权限访问/setting开头的页面
           httpServletResponse.sendRedirect("/?pop=1");//这是写的前端，会跳出首页登陆框
           return false;  //返回false，那么后面的postHandle这些都不会执行了
       }
       return true;
    }

    @Override
    public void postHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, ModelAndView modelAndView) throws Exception {

    }

    @Override
    public void afterCompletion(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) throws Exception {

    }
}

```

* 在HomeController中
```java
@Controller
public class HomeController {
    @Autowired
    NewsService newsService;

    @Autowired
    UserService userService;

    @Autowired
    HostHolder hostHolder;

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


    //用于获得用户那一页的模板
    @RequestMapping(path={"/user/{userId}"},method = {RequestMethod.GET,RequestMethod.POST})
    public String userIndex(Model model, @PathVariable("userId" ) int userId,
                            @RequestParam(value = "pop",defaultValue = "0") int pop){ //pop默认为0，不跳出登陆页面
        //把Vos传到home.html
        model.addAttribute("vos",getNews(userId,0,10));// 这样，到了home.html就可以通过news字段访问news，通过user字段访问user
        model.addAttribute("pop",pop);
        return "home";

    }
}
```
## 4.5 为了用户数据安全，我们要做的事：
### 1.HTTPS注册页：网页访问http是明文的，https是加密的
### 2.公钥加密私钥解密，支付宝h5页面的支付密码加密：用户打开页面时，服务器下发一个公钥，你会用这个公钥对数据加密，加密后再提交到服务器，只有服务器又私钥可以解密
### 3.用户密码salt防止破解（CSDN，网易邮箱未加密密码泄漏）
### 4.token有效期：保证这个token不会被别人占用
### 5.单一平台的单点登陆，登陆IP异常检验：别人登陆你就下线，你再登陆一下，会显示上次登陆的IP区域
### 6.用户状态的权限判断【用拦截器做的】
### 7.添加验证码机制，防止爆破和批量注册：比如修改密码，一般手机验证码4位，写个程序一下子发0-9999，那肯定就有一个对的，所以输入一次或几次失败后，你要做校验

## 4.6 AJAX：异步数据交互
### 优点：
#### 1.页面不刷新，只是局部刷新
#### 2.用户体验更好
#### 3.传输数据更少
#### 4.APP/网站通用，因为就是一些数据
#### 扩展：有统一的数据格式：{code:0,msg:'',data:''}
* 例子：牛客投递登录框，点赞登录框
## 4.7 Spring Boot DevTools：方便开发
* 如果不用的话，你改了一个小地方，要重启才可以，有了这个就不用
* 在pom.xml中加入：
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <version>1.3.5.RELEASE</version>
</dependency>

```
### 1）动态加载更新的class【比如修改了userService，你只需要（右键recompile）userService即可】：不是整体重启，只是这个类重启
### 2）编译加载修改的静态文件【比如html文件，修改后只需要重新编译该文件即可（右键recompile）】

## 附录：
### 1）StringUtils：是比较好用的一个类org.apache.commons.lang.StringUtils; 可以直接判断字符串，比如StringUtils.isBlank(password）
### 2) fastjson：阿里巴巴的一个用于json的工具，下载地址https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-devtools/1.3.5.RELEASE【你所需要的各种库，这里都有，你直接导入就好：在 pom.xml 中】
```xml
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>fastjson</artifactId>
			<version>1.2.13</version>
		</dependency>
```

# 第五周 资讯发布、图片上传、资讯首页
## 5.1 图片上传到云
### 5.1.1 Fiddler：设置了一个代理，我访问某个网站，实际通过它，让它帮我访问，于是它就捕获了所有的数据。用于web编程查看http。
* 通过filter可以设置监控哪个网站
### 5.1.2 代码步骤：定义一个post的入口，spring会帮你把上传的数据那部分包装成为一个file，然后你判断是不是一张图片，是就存储它
#### 1） 为方便，先加一个NewsController,用于上传图片  【客户端图片传上来后我保存在本地】
```java
@Controller
public class NewsController {
    private static final Logger logger = LoggerFactory.getLogger(NewsController.class);

    @Autowired
    private NewsService newsService;

    @RequestMapping(path = {"/uploadImage/"}, method = {RequestMethod.POST})
    @ResponseBody
    public String uploadImage(@RequestParam("file") MultipartFile file){ //"file"这个是请求传上来的参数，就是那张图片，以二进制流的形式给file
        //把这个图片保存到本地
        try{
            String fileUrl = newsService.saveImage(file); //***** 保存图片
            if(fileUrl != null){
                return ToutiaoUtil.getJSONString(1,"上传图片失败");
            }
            return ToutiaoUtil.getJSONString(0,fileUrl); //图片已经存在本地了，可以通过这个链接访问这张图片

        }catch (Exception e){
            logger.error("上传图片失败" + e.getMessage());
            return ToutiaoUtil.getJSONString(1,"上传失败");
        }
    }
}

```
### 2）newsService要增加一个保存图片的方法 saveImage
```java
@Service
public class NewsService {
    @Autowired
    private NewsDAO newsDAO;

    //执行你在NewsDAO里定义的方法，将最新的资讯读取出来
    public List<News> getLatesNews(int userId,int offser,int limit){
        return newsDAO.selectByUserIdAndOffset(userId,offser,limit);
    }


    //把图片保存到本地，如果成功就返回地址
    public String saveImage(MultipartFile file) throws IOException{
        // 1) 判断这是不是一张图片，通过判断文件后缀名来判断
        int dotPos = file.getOriginalFilename().lastIndexOf(".");  //'.'所在的位置
        if(dotPos < 0 ){
            return null;   //文件没有后缀名，不是图片
        }
        String fileExt = file.getOriginalFilename().substring(dotPos+1); //得到文件的扩展名
        if(!ToutiaoUtil.isFileAllowed(fileExt)){ //判断后缀名是否符合
            return null;
        }

        // 2) 格式都符合了，那么就把图片放到本地的某个目录C:/Users/Administrator/Desktop/upload/
        String fileName = UUID.randomUUID().toString().replaceAll("-","")+"."+fileExt;  //随机给这个图片一个名字，防止一些图片名字不合法
        Files.copy(file.getInputStream(), new File(ToutiaoUtil.IMAGE_DIR + fileName).toPath(),
                StandardCopyOption.REPLACE_EXISTING);//把传入的二进制流复制到我的目标位置，如果之前存在就覆盖它
        return ToutiaoUtil.TOUTIAO_DOMAIN + "image?name=" +fileName;  //这个是访问这张图片的链接，给前端用的
    }

}
```
### 附录：
* http的post请求，是通过header里面的boundary=--------------------------285921431169007254259877来区分你一次性上传的多个图片【字符是随机生成的】
```
POST http://127.0.0.1:8080/uploadImage/ HTTP/1.1
cache-control: no-cache
Postman-Token: b7884754-5277-473b-aa6f-32620c544a71
User-Agent: PostmanRuntime/7.4.0
Accept: */*
Host: 127.0.0.1:8080
accept-encoding: gzip, deflate
content-type: multipart/form-data; boundary=--------------------------285921431169007254259877
content-length: 36332
Connection: keep-alive

----------------------------285921431169007254259877
Content-Disposition: form-data; name="file"; filename="2.jpg"
Content-Type: image/jpeg

     JFIF         C     		

----------------------------285921431169007254259877

```
* ping 的作用是测试你到某一个ip之间的网络是否通畅。

## 5.2 图片的展示 
### 5.2.1 代码-在 NewsController 中【客户端输入图片的链接，你解析它，返回图片】
```java
    //用于客户端请求图片【客户端传过来图片的链接，我解析它】http://127.0.0.1:8080/image?name=b8bd7fce8ce542d69a774b366e682dc1.jpg
    @RequestMapping(path = {"/image"},method = {RequestMethod.GET})
    @ResponseBody //因为图片是二进制流，所以我们要有responde
    public void getImage(@RequestParam("name") String imageName,
                         HttpServletResponse response){
        try {
            response.setContentType("image/jpeg");
            StreamUtils.copy( new FileInputStream(ToutiaoUtil.IMAGE_DIR + imageName),
                    response.getOutputStream()); //根据文件名把该文件通过二进制流的形式读取到response中
        }catch (Exception e){
            logger.error("读取图片错误"+ e.getMessage());
        }


    }

```

## 5.3 七牛云服务集成：冗余备份，多台机器统一访问，CDN缓存同步，云实时缩图
### 5.3.1 到了网站开发后面，静态（图片这些）和页面是分离的，图片没有数据库这些【因为它只是一些文件，需要读取这些】，一般存放在云中
### 5.3.2 【好处】：
#### 1）这些服务器能专门做一些缓存，对静态的优化；网站发布就只需要发布你动态的内容即可
#### 2）前端的文件和后端不一样，在前端，如果全国都在访问的话，是需要加缓存的，即CDN,CDN把静态分发给各个CDN节点,那么你获得静态文件的速度会更快
* CDN的全称是Content Delivery Network，即内容分发网络。CDN的基本原理是广泛采用各种缓存服务器，将这些缓存服务器分布到用户访问相对集中的地区或网络中，在用户访问网站时，利用全局负载技术将用户的访问指向距离最近的工作正常的缓存服务器上，由缓存服务器直接响应用户请求。那么你获得静态文件的速度会更快
#### 3) 为什么用云存储：一张图片可能在不同的地方要用，小图片，中图片，大图片，以前是把这张图片裁擦成这些大小，都存起来，坏处：有些图片上传上来根本没人用。所以现在用实时缩图服务，不会影响服务器存储的空间，合理应对产品需求随时要改这些，用的时候才对图片进行调整
### 5.3.2 代码步骤：
* 1）把七牛导入pom.xml，因为七牛是一个独立的服务，所以最好建一个QiNiuService，那么以后只要接口一样，你换成阿里云这些也可以。然后从七牛网站上拷贝代码
```java
@Service
public class QiniuService {
    private static final Logger logger = LoggerFactory.getLogger(QiniuService.class);

    //构造一个带指定Zone对象的配置类 zone2()表示华南地区【我自己选的华南，在七牛上】
    Configuration cfg = new Configuration(Zone.zone2());
    //设置好账号的ACCESS_KEY和SECRET_KEY
    String ACCESS_KEY = "UzUnZKh6-vUr6WkdEqlfWfZdSeUVgt9cI5xEMK-V";
    String SECRET_KEY = "BjfNYpzE5r28Zb1mVnJsgI4_pMqZhxYAlsya2V3d";
    //要上传的空间
    String bucketname = "nowcoder";

    //密钥配置
    Auth auth = Auth.create(ACCESS_KEY, SECRET_KEY);
    //创建上传对象
    UploadManager uploadManager = new UploadManager(cfg);

    private static String QINIU_IMAGE_DOMAIN = "http://pih7ef369.bkt.clouddn.com/"; //要加'/'，这是七牛云你存放对象nowcoder的外链接

    //简单上传，使用默认策略，只需要设置上传的空间名就可以了，返回图片的链接
    public String getUpToken() {
        return auth.uploadToken(bucketname);
    }

   //**** 1）把图片上传到七牛云，并返回图片名字
    public String saveImage(MultipartFile file) throws IOException {
        try {
            int dotPos = file.getOriginalFilename().lastIndexOf(".");
            if (dotPos < 0) {
                return null;
            }
            String fileExt = file.getOriginalFilename().substring(dotPos + 1).toLowerCase();
            if (!ToutiaoUtil.isFileAllowed(fileExt)) {
                return null;
            }

            String fileName = UUID.randomUUID().toString().replaceAll("-", "") + "." + fileExt;
            //调用put方法上传,file就是文件，fileName就是我们要保存的文件名
            Response res = uploadManager.put(file.getBytes(), fileName, getUpToken());
            //打印返回的信息
            if (res.isOK() && res.isJson()) {
                return QINIU_IMAGE_DOMAIN + JSONObject.parseObject(res.bodyString()).get("key"); //七牛的返回是一个哈希值和图片名字【key】
            } else {
                logger.error("七牛异常:" + res.bodyString());
                return null;
            }
        } catch (QiniuException e) {
            // 请求失败时打印的异常的信息
            logger.error("七牛异常:" + e.getMessage());
            return null;
        }
    }

}
```
* 2）然后在NewsController 里调用它即可
```java
@Controller
public class NewsController {
    private static final Logger logger = LoggerFactory.getLogger(NewsController.class);

    @Autowired
    private NewsService newsService;

    @Autowired
    private QiniuService qiniuService;

    //用于客户端上传图片，我保存它【到本地或者到七牛云】并返回它的链接：比如： http://127.0.0.1:8080/image?name=b8bd7fce8ce542d69a774b366e682dc1.jpg【本地】
    //http://pih7ef369.bkt.clouddn.com/49aedd99672e4fc19b79bddca368e4f8.jpg【七牛云】
    @RequestMapping(path = {"/uploadImage/"}, method = {RequestMethod.POST})
    @ResponseBody
    public String uploadImage(@RequestParam("file") MultipartFile file){ //"file"这个是请求传上来的参数，就是那张图片，以二进制流的形式给file
        //把这个图片保存到本地
        try{
           // String fileUrl = newsService.saveImage(file); //***** 保存图片到本地
            String fileUrl = qiniuService.saveImage(file);
            if(fileUrl == null){
                return ToutiaoUtil.getJSONString(1,"上传图片失败");
            }
            return ToutiaoUtil.getJSONString(0,fileUrl); //图片已经存在本地了，可以通过这个链接访问这张图片

        }catch (Exception e){
            logger.error("上传图片失败" + e.getMessage());
            return ToutiaoUtil.getJSONString(1,"上传失败");
        }
    }

    //用于客户端请求图片【客户端传过来图片的链接，我解析它】http://127.0.0.1:8080/image?name=b8bd7fce8ce542d69a774b366e682dc1.jpg
    @RequestMapping(path = {"/image"},method = {RequestMethod.GET})
    @ResponseBody //因为图片是二进制流，所以我们要有responde
    public void getImage(@RequestParam("name") String imageName,
                         HttpServletResponse response){
        try {
            response.setContentType("image/jpeg");
            StreamUtils.copy( new FileInputStream(ToutiaoUtil.IMAGE_DIR + imageName),
                    response.getOutputStream()); //根据文件名把该文件通过二进制流的形式读取到response中
        }catch (Exception e){
            logger.error("读取图片错误"+ e.getMessage());
        }


    }


}

```
## 5.4 资讯发布 【在数据库要用utf8，在application.properties里也要指定utf8，防止中文出错】
* 记得把script换过,在NewsController 增加一个方法
```java
    //上传资讯
    @RequestMapping(path = {"/user/addNews/"},method = {RequestMethod.POST})
    @ResponseBody
    //一条资讯有图片，标题，a链接【谁发布的？】,就是把它放进数据库
    public String addNews(@RequestParam("image") String image,
                          @RequestParam("title") String title,
                          @RequestParam("link") String link){
        try {
            News news = new News();
            news.setImage(image);
            news.setCreatedDate(new Date());
            news.setTitle(title);
            news.setLink(link);
            if(hostHolder.getUser() != null){ //当前线程有用户，即用户是登陆状态
                news.setUserId(hostHolder.getUser().getId());
            }else {
                //没登录我就说你是一个匿名用户【id = 0,你自己定】
                news.setUserId(0);
            }
            newsService.addNews(news);
            return ToutiaoUtil.getJSONString(0); //返回前端告诉它成功
        }catch (Exception e){
            logger.error("添加资讯错误"+e.getMessage());
            return ToutiaoUtil.getJSONString(1,"发布失败"); //告诉前台这条消息
        }
    }


```


# 第六周 资讯详情页、评论中心、站内信

## 6.1 通用的新模块开发流程总体步骤都是一样的：
### 1) 从数据库设计开始 Database Column
### 2）Model：模型定义，和数据库相匹配
### 3）DAO：数据读取
### 4）Service：服务包装
### 5) Controller：业务入口
### 6) Test


## 6.2 资讯详情页（代码演示）
### 6.2.1 步骤：
#### 1）一般先找到前端给你的页面，分析这个页面上有多少东西
#### 2） 到 NewsController 层里面把这些数据传给模板，再对模板文件做一些替换
```java
    //资讯入口，即资讯详情的入口，返回资讯的详情页,newsId,消息的id
    //实际我们只需要把资讯的信息取出来加入model,通过Model传值给模板
    @RequestMapping(path = {"/news/{newsId}"},method = {RequestMethod.GET})  //在home.html中，资讯名称的链接即是这个 href="/news/$!{vo.news.id}">$!{vo.news.title}</a>
    public String newsDetail(@PathVariable("newsId") int newsId,Model model){
        News news = newsService.getById(newsId);
        if(news != null){
            //评论等
        }
        model.addAttribute("news",news);  //像页面传输对象，然后再去detail.html文件修改替换一下
        model.addAttribute("owner",userService.getUser(news.getUserId()));
        return "detail"; //返回html页面
    }

```


## 6.3评论中心（代码演示）
* 我们认为这是一个统一的评论服务，覆盖所有的实体评论：因为评论有很多，针对news的，user的，针对评论的评论，所以其中的字段不能仅仅设置为newId这些，而应该用entity_type，entity_id组合起来表示：
    * 比如这一条是某条评论的评论，那么这两个的内容就是comment，commentId【它评论的是一条评论，是哪条评论】
### 6.3.1 代码一：展示已有评论的步骤：
#### 1）确定这个服务存放在数据库里面需要哪些表，字段是哪些。--自己在数据库里创建，或者放在.sql文件中，然后创建一个测试
* id  ：评论的id
* content
* entity_id  :newsId/commentId
* entity_type :news/comment【评论的评论】
* created_date
* user_id
```java
@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(classes = ToutiaoApplication.class)
@WebAppConfiguration
@Sql({"/init-schema.sql"}) //在执行用例前先执行这份文件，创建数据库
public class InitDatabaseTests {}
```

#### 2）Model：定义模型，和数据库相匹配--Comment,不需要注解
#### 3）DAO：定义数据库的访问模型 -@Mapper
```java
@Mapper
public interface CommentDAO {
    String TABLE_NAME = "comment";
    String INSERT_FIELDS = " user_id,content,created_date,entity_id,entity_type,status ";
    String SELECT_FIELDS = " id, " + INSERT_FIELDS;

    //@Select(....)注解的作用就是告诉mybatis框架,执行括号内的sql语句
    //增加评论，失败的话会返回0
    @Insert({"insert into ", TABLE_NAME, "(", INSERT_FIELDS,
            ") Values (#{userId},#{content},#{createdDate},#{entityId},#{entityType},#{status})"})
    int addComment(Comment comment);

    //选取评论，按最新的顺序
    @Select({"select ",SELECT_FIELDS," from ",TABLE_NAME,
            " where entity_id=#{entityId} and entity_type=#{entityType} order by id desc"})
    List<Comment> selectByEntity(@Param("entityId") int entityId,@Param("entityType") int entityType);

    //每条资讯/评论 有多少条评论
    //4.在方法参数的前面写上@Param("参数名"),表示给参数命名,名称就是括号中的内容
    //public Student select(@Param("aaaa") String name,@Param("bbbb")int class_id);
    //给入参 String name 命名为aaaa,然后sql语句....where  s_name= #{aaaa} 中就可以根据aaaa得到参数值了
    //就我自己来看，貌似也直接可以int entityId，不用@Param("entityId")
    @Select({"select count(id) from ",TABLE_NAME,
            " where entity_id=#{entityId} and entity_type=#{entityType} order by id desc"})
    int getCommentCount(@Param("entityId") int entityId,@Param("entityType") int entityType);

}
```
#### 4）Service：服务包装，去调用DAO层，做一些复杂些的业务
```java

@Service
public class CommentService {
    @Autowired
    private CommentDAO commentDAO;

    //根据传入的某条资讯/评论，来得到它们对应的评论
    public List<Comment> getCommentByEntity(int entityId,int entityType){
        return commentDAO.selectByEntity(entityId,entityType);
    }


    //增加评论
    public int addComment(Comment comment){
        return commentDAO.addComment(comment);
    }

    //根据传入的某条资讯/评论，来得到它们对应的评论的数量
    public int getCommentCount(int entityId,int entityType){
        return commentDAO.getCommentCount(entityId,entityType);
    }
}

```
#### 5）Controller：业务入口：添加评论，删除评论
* 在 NewsController
```java
@Controller
public class NewsController {

    //省略。。。。。

    //*****资讯入口，即资讯详情的入口，返回资讯的详情页,newsId,消息的id
    //实际我们只需要把资讯的信息取出来加入model,通过Model传值给模板
    @RequestMapping(path = {"/news/{newsId}"},method = {RequestMethod.GET})  //在home.html中，资讯名称的链接即是这个 href="/news/$!{vo.news.id}">$!{vo.news.title}</a>
    public String newsDetail(@PathVariable("newsId") int newsId,Model model){
        News news = newsService.getById(newsId);
        if(news != null){ //这个详情页应该有评论，所以写在这里
            //取出这条资讯的所有评论
            List<Comment> comments = commentService.getCommentByEntity(news.getId(),EntityType.ENTITY_NEWS);
            //因为页面上除了资讯、评论、还应该有评论对应的人的头像，所以做一个ViewObject的list,方便模板显示
            List<ViewObject> commentVOs = new ArrayList<>();
            for(Comment comment: comments){ //一条评论对应一个vo，那么既可以在模板里通过vo.comment,vo.user这些字段来获取信息
                ViewObject vo = new ViewObject();  //里面只有一个hashmap及其存取方法
                vo.set("comment",comment);
                vo.set("user",userService.getUser(comment.getUserId()));
                commentVOs.add(vo);  //commentVOs包含了我们取出的所有评论，方便模板使用
            }
            model.addAttribute("comments",commentVOs);  //把这个传递给模板，模板才能用【遍历comments,对每个vo得到vo.comment,vo.user】【见下文】
        }
        model.addAttribute("news",news);  //像页面传输对象，然后再去detail.html文件修改替换一下
        model.addAttribute("owner",userService.getUser(news.getUserId()));
        return "detail"; //返回html页面
    }

}

```

* 在detail.html中评论的显示 model.addAttribute("comments",commentVOs); 应用
```html
            <div id="comments" class="comments">
                #foreach($commentvo in $comments)
                <div class="media">
                    <a class="media-left" href="/user/{commentvo.user.id}">
                        <img src="$!{commentvo.user.headUrl}">
                    </a>
                    <div class="media-body">
                        <h4 class="media-heading"> <small class="date">$date.format('yyyy-MM-dd HH:mm:ss',$!{commentvo.comment.createdDate})</small></h4>
                        <div>$!{commentvo.comment.content}</div>
                    </div>
                </div>
                #end
            </div>
```
#### 6）测试一下 ：Fn+delete：从左往右删除

### 6.3.2 增加评论的代码
* 在 NewsController 里
```java

    //增加评论
    @RequestMapping(path = {"/addComment"},method = {RequestMethod.POST})
    public String addComment(@RequestParam("newsId") int newsId,@RequestParam("content") String content){//这条评论是属于哪条资讯
        try {
            Comment comment = new Comment();
            comment.setUserId(hostHolder.getUser().getId());
            comment.setContent(content);
            comment.setEntityId(newsId);
            comment.setEntityType(EntityType.ENTITY_NEWS); //实际上这个也应该由对方客户穿进来，因为我们实际上只有这一种选择，所以就没做
            comment.setCreatedDate(new Date());
            comment.setStatus(0);

            commentService.addComment(comment); //把评论加入数据库
            //更新 news 里面的评论数量
            int count = commentService.getCommentCount(comment.getEntityId(),comment.getEntityType());
            newsService.updateCommentCount(comment.getEntityId(),count);
            //下一节课会将它异步化

        }catch (Exception e){
            logger.error("增加评论失败"+ e.getMessage());
        }
        return "redirect:/news/" + String.valueOf(newsId);  //增加评论后就返回这个页面，刷新

    }

```

### 6.3.3 删除评论


## 6.4 消息中心（代码演示）
* 包括两个页面：所有消息的列表【和哪些人聊天】即会话列表，和某个人聊天的消息详情页
### 6.4.1 代码一：消息详情页
#### 1）确定这个服务存放在数据库里面需要哪些表，字段是哪些。--自己在数据库里创建，或者放在.sql文件中，然后创建一个测试
* id
* from_id
* to_id
* content
* created_date
* has_read
* conversation_id  //把会话页面和消息详情页页面连起来了。from_id.to_id


#### 2）Model：定义模型，和数据库相匹配--Message,不需要注解
#### 3）DAO：定义数据库的访问模型 -@Mapper ————MessageDAO
```java
@Mapper
public interface MessageDAO {
    String TABLE_NAME = " message ";
    String INSERT_FIELDS = " from_id, to_id, content, has_read, conversation_id, created_date ";
    String SELECT_FIELDS = " id, " + INSERT_FIELDS;

    //插入一条评论
    @Insert({"insert into ", TABLE_NAME, "(", INSERT_FIELDS,
            ") values (#{fromId},#{toId},#{content},#{hasRead},#{conversationId},#{createdDate})"})
    int addMessage(Message message);

    //分页显示消息详情
    @Select({"select ", SELECT_FIELDS, " from ", TABLE_NAME, " where conversation_id=#{conversationId} order by id desc limit #{offset},#{limit}"})
    List<Message> getConversationDetail(@Param("conversationId") String conversationId, @Param("offset") int offset, @Param("limit") int limit);
}
```
#### 4）Service：服务包装，去调用DAO层，做一些复杂些的业务 ————MessageService
```java
@Service
public class MessageService {
    @Autowired
    MessageDAO messageDAO;

    public int adddMessage(Message message){
        return messageDAO.addMessage(message);
    }

    //得到多少条消息【分页显示】
    public List<Message> getConversationDetail(String conversationId,int offset,int limit){
        return messageDAO.getConversationDetail(conversationId,offset,limit);
    }
}

```
#### 5）Controller：业务入口：增加消息，显示消息详情页--MessageController

```java
@Controller
public class MessageController {

    private static final Logger logger = LoggerFactory.getLogger(NewsController.class);

    @Autowired
    private MessageService messageService;

    @Autowired
    private UserService userService;

    //增加消息
    @RequestMapping(path = {"/msg/addMessage"},method = {RequestMethod.POST})
    @ResponseBody
    public String addMessage(@RequestParam("fromId") int fromId,
                             @RequestParam("toId") int toId,
                             @RequestParam("content") String content){//这条评论是属于哪条资讯
        try {
            Message msg = new Message();
            msg.setContent(content);
            msg.setFromId(fromId);
            msg.setToId(toId);
            msg.setCreatedDate(new Date());
            msg.setConversationId(fromId < toId ? String.format("%d_%d",fromId,toId):String.format("%d_%d",toId,toId));
            messageService.adddMessage(msg);
            return ToutiaoUtil.getJSONString(msg.getId());
        }catch (Exception e){
            logger.error("增加消息失败"+ e.getMessage());
            return ToutiaoUtil.getJSONString(1,"插入评论失败");
        }

    }

    //取得一个会话的详情，多条消息
    @RequestMapping(path = {"/msg/detail"},method = {RequestMethod.GET})
    public String conversationDetail(Model model, @Param("conversationId") String conversationId){
        try {
            List<Message>  conversationList= messageService.getConversationDetail(conversationId,0,10);
            List<ViewObject> messages = new ArrayList<>();
            for (Message msg: conversationList){
                ViewObject vo = new ViewObject();
                vo.set("message",msg);
                User user = userService.getUser(msg.getFromId());
                if(user == null){
                    continue;
                }
                vo.set("headUrl",user.getHeadUrl());
                vo.set("userId",user.getId());
                messages.add(vo);
            }
            model.addAttribute("messages",messages);
        }catch (Exception e){
            logger.error("获取详情消息失败"+e.getMessage());
        }
        return "letterDetail";
    }

}
```

### 6.4.2 代码二：会话页【你与所有人的会话都显示最新的一条作为代表】
```java
    //一个人的会话页，包含与多个人的会话
    @RequestMapping(path = {"/msg/list"},method = {RequestMethod.GET})
    public String conversationDetail(Model model) {
        try {
            int localUserId = hostHolder.getUser().getId();
            List<ViewObject> conversations = new ArrayList<>();
            List<Message> conversationList = messageService.getConversationList(localUserId,0,10);
            for (Message msg: conversationList){
                ViewObject vo = new ViewObject();
                vo.set("conversation",msg);
                int targetId = msg.getFromId() == localUserId ? msg.getToId():msg.getFromId(); //知道对方是发信人还是收信人
                User user = userService.getUser(targetId); //得到对方的信息
                if(user != null){  //对方要存在才行，比如我的id 为1的用户是不存在的
                    vo.set("headUrl", user.getHeadUrl());
                    vo.set("userName", user.getName());
                    vo.set("targetId", targetId);
                    vo.set("totalCount", msg.getId());
                    vo.set("unreadCount",messageService.getConversationUnReadCount(localUserId,msg.getConversationId())); //获得未读条数
                    conversations.add(vo);
                }
            }
            model.addAttribute("conversations",conversations);

        }catch (Exception e){
            logger.error("获取站内信列表失败"+e.getMessage());
        }
        return "letter";
    }

```

# 第七周 Redis入门以及redis实现赞踩功能
## 总结：
* redis 数据结构：
    * List：双向列表，适用于最新列表，关注列表 
        * 【lpush、lpop、blpop、lindex、lrange、lrem、linsert、lset、rpush】
    * Set：适用于无顺序的集合，点赞点踩，抽奖，已读，共同好友
        * sdiff、smembers、sinter、scard
    * SortedSet：排行榜，优先队列
        * zadd、zscore、zrange、zcount、zrank、zrevrank
    * Hash：对象属性，不定长属性数
        * hset、hget、hgetAll、hexists、hkeys、hvals
    * KV：单一数值，验证码，PV，缓存
        * set、setex、incr
## 7.1 Redis简介:Redis 是key-value数据库，支持主从同步，数据存储在内存，性能卓越。
* 哈希的概念，给我一个key，我给你返回一个值。官方网址：http://redis.io/
* Redis是一个开源（BSD许可），内存存储的数据结构服务器，可用作数据库，高速缓存和消息队列代理。它支持字符串、哈希表、列表、集合、有序集合，位图，hyperloglogs等数据类型。内置复制、Lua脚本、LRU收回、事务以及不同级别磁盘持久化功能，同时通过Redis Sentinel提供高可用，通过Redis Cluster提供自动分区。
### 7.1.1 操作
#### 1）连接redis：redis-cli.exe【cmd】
```
C:\Windows\system32>redis-cli
127.0.0.1:6379> //默认端口
```
* 我们可以在命令行操作，如果用Java操作，需要第三方类库，在redis里就是各种各样的clients，找到java的，我们选择用jedis,它对redis的命令这些都进行了包装，只需要在maven里面包含
```xml

<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
    <type>jar</type>
    <scope>compile</scope>
</dependency>
```
#### 2）配置文件：redis.windows.conf
```
bind 127.0.0.1 #绑定的地址，只有本地访问；如果外网要想访问，可以改成0.0.0.0，开放所有ip访问【应该设置密码，默认是没设置的】
port 6379      #默认监听6379端口

# redis是一个数据库，数据默认保存在安装目录下的 dump.rdb
# 这里是保存策略，设置条件来触发它的持久化的进程
save 900 1    # 900s以后如果有至少 1 个key 改变了，就保存一次
save 300 10   # 300s以后如果有至少 10 个key 改变了，就保存一次
save 60 10000 #


dbfilename dump.rdb  #存储的文件
```
#### 3）redis 有两种备份（存储）机制：RDB 、 AOF
##### RDB：如果我有100条数据，那么我每隔一段时间我就保存这100条数据一次 【相当于只保存最后的结果】
##### AOF：我把对数据的操作【命令】保存下来 【相当于保存过程，通过这个过程我可以得到最后的结果】
```java
package com.nowcode.toutiao.util;


import redis.clients.jedis.BinaryClient;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;
import redis.clients.jedis.Tuple;

//用于演示jedis
public class JedisAdapter {

    //用于演示，帮助打印
    public static void print(int index,Object object){
        System.out.println(String.format("%d,%s",index,object));
    }

    public static void main(String[] args) {
        Jedis jedis = new Jedis(); //得到Jedis 的句柄，默认连接的是本机的6379端口 //里面有很多构造器，你可以知道host，端口这些
        jedis.flushAll(); //把所有数据库都清空

        //这些函数就对应redis里支持的所有的命令，key,value能是各种类型
        //jedis.blpop(); //brock,这是一个阻塞式的pop【同步】

        //1）只有一个值，把值放进去 类似 Map<> map = new HashMap<>();
        jedis.set("hello","world");  //key--value
        print(1,jedis.get("hello"));
        jedis.rename("hello","newhello"); //对 key 改名字
        print(2,jedis.get("newhello"));
        jedis.setex("hello2",15,"world");  //你可以在设置一个值时候同时设置一个过期时间【15秒】；比如验证码这些

        //2）
        //pv：网页访问人数100人。如果来一个人就增加，会卡死；所以高并发场景，对pv这些正常应该先存起来【比如存在redis里或内存】，定期更新到数据库
        jedis.set("pv","100");
        jedis.incr("pv");  //增加1，比Java里面取出来增加再放进去更方便
        jedis.incrBy("pv",5); //一次性增加5
        print(2,jedis.get("pv"));

        //list 列表操作，是在表头插入，后插入的在前面
        String listName = "list";
        for(int i= 0;i<10;i++){
            jedis.lpush(listName,"a"+String.valueOf(i));
        }
        print(3,jedis.lrange(listName,0,12));
        print(4,jedis.llen(listName));  //链表的长度
        print(5,jedis.lpop(listName));  //弹出一个数据
        print(6,jedis.llen(listName));
        print(7,jedis.lindex(listName,3)); //找第3个位置是什么
        //插入某个位置，插在 a4 的后面
        print(8,jedis.linsert(listName,BinaryClient.LIST_POSITION.AFTER,"a4","xx"));
        print(9,jedis.linsert(listName,BinaryClient.LIST_POSITION.BEFORE,"a4","bb"));
        print(10,jedis.lrange(listName,0,13));

        //3)类似 hashMap，通过一个key，关联一个 Map ,应用于属性不定，你可以随时给这个用户加属性
        String userKey = "user12";
        jedis.hset(userKey,"name","jim"); //key-field-value
        jedis.hset(userKey,"age","12");
        jedis.hset(userKey,"phone","12423534534");

        print(12,jedis.hget(userKey,"name")); //取出这个人的name字段对应的值
        print(13,jedis.hgetAll(userKey));  //取出与这个人相关的所有属性及对应值
        jedis.hdel(userKey,"phone"); //删除这个属性
        print(13,jedis.hgetAll(userKey));
        print(14,jedis.hkeys(userKey));  //得到这个对象所有的属性
        print(15,jedis.hvals(userKey));  //得到这个对象所有的属性值
        print(16,jedis.hexists(userKey,"name")); //判断这个对象是否存在某个属性
        jedis.hsetnx(userKey,"school","zju"); //只有不存在的时候才设置，有的话不会更新
        print(17,jedis.hkeys(userKey));

        //4) 集合的概念，set,以s 开头，可以做集合类的操作
        String likeKeys1 = "newsLike1";
        String likeKeys2 = "newsLike2";
        for (int i=0;i<10; i++){
            jedis.sadd(likeKeys1,String.valueOf(i));    //加入likeKeys1集合
            jedis.sadd(likeKeys2,String.valueOf(i*2));
        }
        print(20,jedis.smembers(likeKeys1)); //集合里面的全部元素
        print(21,jedis.smembers(likeKeys2));
        print(22,jedis.sinter(likeKeys1,likeKeys2)); //两个集合求交，用于找两个人的共同好友
        print(23,jedis.sunion(likeKeys1,likeKeys2)); //求并
        print(24,jedis.sdiff(likeKeys1,likeKeys2)); // 求不同【求异】，我有你没有的
        print(25,jedis.sismember(likeKeys1,"5")); // 用于判断5是不是存在，应用于你是否点过赞，没点过我才加
        jedis.srem(likeKeys1,"5"); //删除某个集合的某个元素
        print(26,jedis.smembers(likeKeys1));
        print(27,jedis.scard(likeKeys1));  //用于查看这个集合有多少个值
        jedis.smove(likeKeys2,likeKeys1,"14"); //将 14 从 likeKey2 移动到 likeKey1
        print(28,jedis.smembers(likeKeys1));


        // 5) Sorted Set 优先队列【从低到高】，以 z 开头;  这个能非常方便的用于排行榜
        String rankKey = "rankKey";
        jedis.zadd(rankKey,15,"jim");  //出了添加value,还要给予它一个分值
        jedis.zadd(rankKey,13,"ben");
        jedis.zadd(rankKey,90,"lucy");
        jedis.zadd(rankKey,80,"mei");
        jedis.zadd(rankKey,60,"lee");
        print(30,jedis.zcard(rankKey));  //这个队列有多少个值
        print(31,jedis.zcount(rankKey,61,100)); //用于查看在61-100区间内有多少人
        print(32,jedis.zscore(rankKey,"lucy")); //用于查看某个人的分值
        jedis.zincrby(rankKey,2,"lucy");  //把lucy的成绩提高2分
        jedis.zincrby(rankKey,2,"luc");   //给不存在的人提分,相当于让这个人只有2分
        print(33,jedis.zcount(rankKey,0,100));
        print(34, jedis.zrange(rankKey,0,3));  //顺序从小到大，得到第0名到第三名，luc 2分排在最前面
        print(35,jedis.zrevrange(rankKey,0,3)); //**** 顺序从大到小

        for (Tuple tuple: jedis.zrangeByScoreWithScores(rankKey,"0","100")){ //**** tuple是元组，相当于 Map 的entity
            print(36,tuple.getElement() + ":"+tuple.getScore());
        }

        print(37,jedis.zrank(rankKey,"ben")); //ben 是第几名
        print(38,jedis.zrevrank(rankKey,"ben"));


        JedisPool pool = new JedisPool();  //连接池，默认是 8 条线程，类似线程池
        for (int i=0; i<100 ;i++){
            Jedis j = pool.getResource();  //从池子里取一个资源
            j.get("a");
            System.out.println("POOL"+i);
            j.close();  //必须关闭，资源才会还回去
        }

    }
}

```
## 7.2 赞踩实现：通过集合
### 7.2.1 Redis 在牛客中的应用
#### 1）PV ：这些浏览数这些都是在redis中存储的，然后再慢慢同步到额外的字段
#### 2）点赞 ：你点一个赞，我就会把userid放到帖子的集合里面，再点赞，就从集合删掉，我只需要知道集合里有多少数据既可以了
#### 3）关注 ： 我关注谁，谁关注我，这些都是一个个的集合，删除关注就从集合中拿出来
#### 4）排行榜 ：所有排行榜都是用的redis的 Sorted set ,即优先队列集合，比如登陆排行榜，你每登陆一次，我就把你的数字加1
#### 5）验证码 ：setex 设置过期时间
#### 6）缓存 ： 在底层MySQL数据库上做一个缓存，你所有的数据都是放在redis上的，也会做一个缓存的时间，当用户更新的时候，就把缓存失效掉，每次优先从缓存里取，缓存没有再去数据库，这样就可以大大降低数据库的压力
#### 7）异步队列 ：我给你点了一个赞，你就会收到一条消息谁谁谁给你点了一个赞，这些都是异步队列，其实就是 list 结构，发生什么事情，就抛一个事件到 list ，然后有一个额外的线程去处理
#### 8）判题队列 ： 大家都在提交代码编程，我不可能实时同步的处理，所以放在一个队列里，哪台机器有空闲的时候再去跑这个代码编译

### 7.2.2 赞功能的实现
#### 1）Redis数据库没有 model、DAO 这些概念，所以直接在service里定义功能即可
##### 1.1 我们采用连接池 JedisPool ,所以需要对jedis 的操作进行包装，做一个包装类JedisAdapter在util下,数据不再通过DAO，而是 JedisAdapter 来读取操作
```java
package com.nowcode.toutiao.util;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.InitializingBean;
import org.springframework.stereotype.Component;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;

//这个类包装了 Jedis 的功能，所以是个工具类？
@Component  //或者@Service也可以
public class JedisAdapter implements InitializingBean {  //为了让它初始化的时候就把pool初始化好，我们继承一个接口
    //使用指定类初始化日志对象，在日志输出的时候，可以打印出日志信息所在类
    private static Logger logger = LoggerFactory.getLogger(JedisAdapter.class);

    private JedisPool pool = null;   //服务一起来的时候我们就定义一个连接池pool

    @Override
    public void afterPropertiesSet() throws Exception {  //初始化的时候把pool初始化好
        pool = new JedisPool("localhost",6379);
    }

    //获取一个连接池资源去执行操作
    public Jedis getJedis(){
        return  pool.getResource();
    }

    //***以下是对集合方法的包装，因为我们用到了pool,所以不能直接调用方法，在调用方法前需要先获得一个连接池资源

    // * sadd方法的包装，一个对象放入集合key
    public long sadd(String key,String value){
        Jedis jedis = null;
        try {
            jedis = pool.getResource();  // 1）取得一个资源
            return jedis.sadd(key,value); // 2) 执行操作，加入集合
        }catch (Exception e){
            logger.error("sadd发生异常"+e.getMessage());
            return 0;
        }finally {  //无论如何都要管资源
            if (jedis != null){
                jedis.close();  // 3）关闭此资源，不然因为连接池默认只有8个资源，你不还，别人就无法用
            }
        }
    }

    // * srem方法的包装，删除某对象
    public long srem(String key,String value){
        Jedis jedis = null;
        try {
            jedis = pool.getResource();  // 1）取得一个资源
            return jedis.srem(key,value); // 2) 执行操作，删除对象
        }catch (Exception e){
            logger.error("srem发生异常"+e.getMessage());
            return 0;
        }finally {  //无论如何都要管资源
            if (jedis != null){
                jedis.close();  // 3）关闭此资源，不然因为连接池默认只有8个资源，你不还，别人就无法用
            }
        }
    }

    // * sismember方法的包装,判断自己是否在集合内
    public boolean sismenmber(String key,String value){
        Jedis jedis = null;
        try {
            jedis = pool.getResource();  // 1）取得一个资源
            return jedis.sismember(key,value); // 2) 执行操作，删除对象
        }catch (Exception e){
            logger.error("sismember发生异常"+e.getMessage());
            return false;
        }finally {  //无论如何都要管资源
            if (jedis != null){
                jedis.close();  // 3）关闭此资源，不然因为连接池默认只有8个资源，你不还，别人就无法用
            }
        }
    }

    // * scard方法的包装，集合中有多少人
    public long scard(String key){
        Jedis jedis = null;
        try {
            jedis = pool.getResource();  // 1）取得一个资源
            return jedis.scard(key); // 2) 执行操作，删除对象
        }catch (Exception e){
            logger.error("scard发生异常"+e.getMessage());
            return 0;
        }finally {  //无论如何都要管资源
            if (jedis != null){
                jedis.close();  // 3）关闭此资源，不然因为连接池默认只有8个资源，你不还，别人就无法用
            }
        }
    }
}

```

##### 1.2 在 service 下定义一个 LikeService
```java
@Service  //赞服务
public class LikeService {

    @Autowired
    JedisAdapter jedisAdapter;

    // 如果喜欢返回1，不喜欢返回-1，否则返回0
    //函数一：判断某个用户对某个资讯/评论 是否喜欢
    public int getLikeStatus(int userId, int entityType,int entityId){
        String likeKey = RedisKeyUtil.getLikeKey(entityType,entityId); //得到这条资讯的赞集合
        if(jedisAdapter.sismenmber(likeKey,String.valueOf(userId))){//判断这个用户是否喜欢这条资讯
            return 1;
        }
        String dislikeKey = RedisKeyUtil.getDisLikeKey(entityType,entityId);
        if(jedisAdapter.sismenmber(dislikeKey,String.valueOf(userId))){//判断这个用户是否厌恶这条资讯
            return -1;
        }
        return 0; //说明不喜欢也不讨厌
    }

    //函数二、用户赞了这个资源，就把他添加进这个资源的赞集合
    public long like(int userId,int entityType, int entityId){
        String likeKey = RedisKeyUtil.getLikeKey(entityType,entityId);
        jedisAdapter.sadd(likeKey,String.valueOf(userId));  //把用户加入该资源的赞集合

        //因为喜欢和不喜欢是相背的，所以用户喜欢，就要把用户从踩集合中取消掉
        String dislikeKey = RedisKeyUtil.getDisLikeKey(entityType,entityId);
        jedisAdapter.srem(dislikeKey,String.valueOf(userId));

        return jedisAdapter.scard(likeKey);//返回当前有多少人赞了,不能直接+1，因为这是多线程的，所以要用查的
    }

    //函数三、用户踩了这个资源，就把他添加进这个资源的踩集合
    public long disLike(int userId,int entityType, int entityId){
        String likeKey = RedisKeyUtil.getLikeKey(entityType,entityId);
        jedisAdapter.srem(likeKey,String.valueOf(userId));  //把用户从赞集合中删除

        //将用户加入踩集合
        String dislikeKey = RedisKeyUtil.getDisLikeKey(entityType,entityId);
        jedisAdapter.sadd(dislikeKey,String.valueOf(userId));

        return jedisAdapter.scard(dislikeKey);//返回当前有多少人踩了
    }
}

```
##### 1.3 定义入口 LikeController
```java
@Controller  //赞了，踩了，就把这些数据读取出来
public class LikeController {

    @Autowired
    HostHolder hostHolder;

    @Autowired
    LikeService likeService;

    @Autowired
    NewsService newsService;

    //赞
    @RequestMapping(path={"/like"},method = {RequestMethod.GET,RequestMethod.POST})
    @ResponseBody
    public String like(@RequestParam("newsId") int newsId){
        int userId = hostHolder.getUser().getId();
        long likeCount = likeService.like(userId,EntityType.ENTITY_NEWS,newsId);  //把这个用户加入该资源的赞集合
        newsService.updateLikeCount(newsId,(int) likeCount); //还要更新news表里的字段
        return ToutiaoUtil.getJSONString(0,String.valueOf(likeCount)); //返回给前端
    }

    //踩
    @RequestMapping(path={"/dislike"},method = {RequestMethod.GET,RequestMethod.POST})
    @ResponseBody
    public String dislike(@RequestParam("newsId") int newsId){
        int userId = hostHolder.getUser().getId();
        long dislikeCount = likeService.disLike(userId,EntityType.ENTITY_NEWS,newsId);  //把这个用户加入该资源的赞集合
        return ToutiaoUtil.getJSONString(0,String.valueOf(dislikeCount)); //返回给前端
    }

}
```
##### 1.4 注意事项：一个资讯可能对应有 pv、赞、踩这些不同的集合，所以集合的命名要有规范，一般我们把业务作为集合的前缀
```java
package com.nowcode.toutiao.util;


//根据规范生成集合的名字，以业务作为集合名称的前缀，每个集合全局唯一
public class RedisKeyUtil {
    private static String SPLIT = ":";
    private static String BIZ_LIKE="LIKE";
    private static String BIZ_DISLIKE="DISLIKE";

    //每条资讯/评论 关联的赞集合【存放赞它的人】是不同的
    public static String getLikeKey(int entityType,int entityId){
        return BIZ_LIKE + SPLIT + String.valueOf(entityType) + String.valueOf(entityId);
    }

    //每条资讯/评论 关联的踩集合【存放踩它的人】是不同的
    public static String getDisLikeKey(int entityType,int entityId){
        return BIZ_DISLIKE + SPLIT + String.valueOf(entityType) + String.valueOf(entityId);
    }

}

```
##### 1.5 注意事项：要修改首页的显示，在home.html里，根据你点的喜欢不喜欢进行加量【不同显示】,
```html
<div class="votebar">
                                #if ($vo.like > 0)
                                <button class="click-like up pressed" data-id="$!{vo.news.id}" title="赞同"><i class="vote-arrow"></i><span class="count">$!{vo.news.likeCount}</span></button>
                                #else
                                <button class="click-like up" data-id="$!{vo.news.id}" title="赞同"><i class="vote-arrow"></i><span class="count">$!{vo.news.likeCount}</span></button>
                                #end
                                #if($vo.like < 0)
                                <button class="click-dislike down pressed" data-id="$!{vo.news.id}" title="反对"><i class="vote-arrow"></i></button>
                                #else
                                <button class="click-dislike down" data-id="$!{vo.news.id}" title="反对"><i class="vote-arrow"></i></button>
                                #end
                            </div>
```
##### 1.6 注意事项：同时在HomeController里也要修改一下，把vo.like 传进去
```java
    private List<ViewObject> getNews(int userId, int offset, int limmit){
        //取前10条新闻数据
        List<News> newsList= newsService.getLatesNews(userId,offset,limmit);
        int localUserId = hostHolder.getUser() != null?hostHolder.getUser().getId():0;
        List<ViewObject> vos = new ArrayList<>();
        for (News news: newsList){
            ViewObject vo=new ViewObject();
            vo.set("news",news);//vo就是一个map,通过“news”字段就可以得到news
            vo.set("user",userService.getUser(news.getUserId())); //这条资讯对应的作者

            if (localUserId != 0){ //*****说明是登陆状态
                //通过like 就能知道这条资讯关于我这个用户的状态，喜欢还是不喜欢
                vo.set("like",likeService.getLikeStatus(localUserId,EntityType.ENTITY_NEWS,news.getId()));
            }else {
                vo.set("like",0); //没登录我们就默认是0；
            }
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

```
##### 1.7 注意事项：在NewsController也要改一下，把like 传进去，因此detail也要修改【detail的修改和home一样】
```java
    //资讯入口，即资讯详情的入口，返回资讯的详情页,newsId,消息的id
    //实际我们只需要把资讯的信息取出来加入model,通过Model传值给模板
    @RequestMapping(path = {"/news/{newsId}"},method = {RequestMethod.GET})  //在home.html中，资讯名称的链接即是这个 href="/news/$!{vo.news.id}">$!{vo.news.title}</a>
    public String newsDetail(@PathVariable("newsId") int newsId,Model model){
        News news = newsService.getById(newsId);
        if(news != null){ //这个详情页应该有评论，所以写在这里
            //取出这条资讯的所有评论
            int localUserId = hostHolder.getUser() != null?hostHolder.getUser().getId():0;
            if (localUserId != 0){ //*****说明是登陆状态
                //通过like 就能知道这条资讯关于我这个用户的状态，喜欢还是不喜欢
                model.addAttribute("like",likeService.getLikeStatus(localUserId,EntityType.ENTITY_NEWS,news.getId()));  //把like 传进去
            }else {
                model.addAttribute("like",0); //没登录我们就默认是0；
            }
            List<Comment> comments = commentService.getCommentByEntity(news.getId(),EntityType.ENTITY_NEWS);
            //因为页面上除了资讯、评论、还应该有评论对应的人的头像，所以做一个ViewObject的list,方便模板显示
            List<ViewObject> commentVOs = new ArrayList<>();
            for(Comment comment: comments){ //一条评论对应一个vo，那么既可以在模板里通过vo.comment,vo.user这些字段来获取信息
                ViewObject vo = new ViewObject();  //里面只有一个hashmap及其存取方法
                vo.set("comment",comment);
                vo.set("user",userService.getUser(comment.getUserId()));
                commentVOs.add(vo);  //commentVOs包含了我们取出的所有评论，方便模板使用
            }
            model.addAttribute("comments",commentVOs);  //把这个传递给模板，模板才能用【遍历comments,对每个vo得到vo.comment,vo.user】
        }
        model.addAttribute("news",news);  //像页面传输对象，然后再去detail.html文件修改替换一下
        model.addAttribute("owner",userService.getUser(news.getUserId()));
        return "detail"; //返回html页面
    }

```

# 第八周 异步设计和站内邮件通知系统
## 一、异步队列
* 网站上的事情牵一发而动全身。
    * 比如用户给你点了一个赞，那么要考虑成就值要不要增加，我要不要收到一个站内通知，或者其他事情。看起来只是点了一个赞，实际后台要做很多事情。
    * 比如登陆后，网站还要检查你的IP这些有没有问题，然后给用户发邮件这些。
    * 现在系统越来越大，杂碎的事情很多，如果每段代码里面都要做一些相似的事情，那么代码就很冗余。【比如点赞，评论或者其他事情都会导致成就值增加，那么成就值增加这行代码每个地方都调的化很麻烦】
* 综上，所以大型的系统到了最后都是服务化和异步化的：
    * 服务化：我要做很多事情，然后我把每个模块，在以前的系统里面可能就是一个service、一个类，把它们单独出来做成一个工程，再把相关的接口暴露出来供给所有的业务部门用，这就叫服务化。单独去维护这个服务，提供接口。
        * 一个大系统里面，一些主要的核心模块就逐步切出来，切出来后再独立成工程，工程完后再服务化，就可以暴露一些公共的接口来给其他部门所用。比如用户交给一个团队，数据库交给一个团队
    * 异步化：异步化把一些复杂的业务处理流程切开，把那些需要及时反馈的数据直接返回，对那些可以滞后更新的数据慢慢地在后台更新
        * 用户点了赞，你需要迅速地把点赞结果返回去，至于点赞引起的一些成就值增加等等，这些事情暂时都不管，我只需要告诉别人这件事发生了，再通过一个异步化的框架处理掉它们。
### 1.1 通过队列来实现异步结构
![](https://github.com/zhaojing5340126/interview/blob/master/picture/%E5%BC%82%E6%AD%A5%E9%98%9F%E5%88%97.png?raw=true)
#### 1.1.1 流程：
* Biz：业务部门，发送事件到事件统一的构造点
* Event Producer：事件构造点，发送事件到队列
* 队列：用于存储发过来的事件。可以单向队列【先进先出】或者优先队列【redis里有sorted set】
* 右边则是通过消费线程把这些事件处理掉：得到事件后去找到与事件相关联的Handler【哪些在关注这个事件】去处理

#### 1.1.2 异步的好处：
* 即使系统全部挂掉，也只是意味着这些数据的更新及相关联的操作都是延后而已，重启服务器后只要队列还在，继续慢慢处理即可。

### 1.2 代码实现：在LikeController 里面把like事件用 EventProducer 发（lpush）到阻塞队列,在EventConsumer 里从阻塞队列不断阻塞（brpop）取出事件,处理它
#### 1.把事件放进队列，取出来，涉及到队列的序列化【event 序列化存进队列，取出来时再反序列化】
* 在JedisAdapter 里增加两个函数setObject 和 getObject，我可以序列化保存对象，也可以取出对象
```java
   //set 方法的包装
    public void set(String key, String value) {
        Jedis jedis = null;
        try {
            jedis = pool.getResource();
            jedis.set(key, value);
        } catch (Exception e) {
            logger.error("发生异常" + e.getMessage());
        } finally {
            if (jedis != null) {
                jedis.close();
            }
        }
    }

    //get方法的包装
    public String get(String key) {
        Jedis jedis = null;
        try {
            jedis = pool.getResource();
            return jedis.get(key);
        } catch (Exception e) {
            logger.error("发生异常" + e.getMessage());
            return null;
        } finally {
            if (jedis != null) {
                jedis.close();
            }
        }
    }
    
    //往队列里增加一个对象，对象以json 串的方式存储
    public void setObject(String key,Object obj){
        set(key, JSON.toJSONString(obj));  //set是redis 里面的set
    }

    //从队列取出一个对象,通过key取对象，并且传入这是一个什么对象
    public <T> T getObject(String key, Class<T> clazz ){
        String value = get(key);  // 通过key 取出对象
        if(value != null){
            return JSON.parseObject(value,clazz);  //反序列化返回正确类型的对象
        }
        return null;
    }
```
#### 2、先创一个异步包 async 用于存放异步相关的代码
##### 1）EventType：表示刚刚发生了什么事件，事件是什么类型【是一个枚举类】
```java
public enum EventType {
    LIKE(0),
    COMMENT(1),
    LOGIN(2),
    MAIL(3);   //事件的类型
    
    private int value;
    EventType(int value){
        this.value=value;
    }
    public int getValue(){
        return value;
    }
}
```
##### 2）EventModel：事件发生时现场的数据信息都打包在这个 EventModel里面【事件的数据】
```java
package com.nowcode.toutiao.async;

import java.util.HashMap;
import java.util.Map;

//即放进队列里的事件本身 【保存了事件发生时的现场】
public class EventModel {
    //比如赞事件，type：类型是LIKE
    // actorId：当前线程用户触发了该事件
    // entityType 和 entityId 表示触发对象是资讯
    // entityOwnerId;  触发对象的拥有者是谁，即资讯的拥有者是谁
    private EventType type; //刚刚发生的事件的类型是什么：LIKE(0),COMMENT(1),LOGIN(2),MAIL(3);
    private int actorId;  //谁触发了事件
    private int entityType;
    private int entityId;   //entityType 和 entityId 表示触发对象是什么
    private int entityOwnerId;  //触发对象的拥有者是谁
    //用于保存触发现场数据
    private Map<String,String> exts = new HashMap<>();

    // 1）构造函数
    public EventModel(EventType type){
        this.type = type;
    }
    public EventModel(){  //从JSON串 反序列化构造出事件需要默认的构造函数

    }

    // 2）获取和存放现场数据
    public String getExt(String key){
        return exts.get(key);   //获取现场数据
    }

    public EventModel setExt(String key,String value){
        exts.put(key,value);  //存放现场数据
        return this;     //****** 为了能更快设置数据（可以不断调用set）
        // eventProducer.fireEvent(new EventModel(EventType.LIKE).setActorId(hostHolder.getUser().getId())
        // .setEntityId(newsId).setEntityType(EntityType.ENTITY_NEWS)
        // .setEntityOwnerId(news.getUserId()));
    }

    // 3） 设置其他信息
    public EventType getType() {
        return type;
    }

    public EventModel setType(EventType type) {
        this.type = type;
        return this;
    }

    public int getActorId() {
        return actorId;
    }

    public EventModel setActorId(int actorId) {
        this.actorId = actorId;
        return this;
    }

    public int getEntityType() {
        return entityType;
    }

    public EventModel setEntityType(int entityType) {
        this.entityType = entityType;
        return this;
    }

    public int getEntityId() {
        return entityId;
    }

    public EventModel setEntityId(int entityId) {
        this.entityId = entityId;
        return this;
    }

    public int getEntityOwnerId() {
        return entityOwnerId;
    }

    public EventModel setEntityOwnerId(int entityOwnerId) {
        this.entityOwnerId = entityOwnerId;
        return this;
    }

    public Map<String, String> getExts() {
        return exts;
    }

    public EventModel setExts(Map<String, String> exts) {
        this.exts = exts;
        return this;
    }
}


```
##### 3) EventHandle 接口：可以处理各种各样的事情，比如LikeHandle 处理踩赞相关的事情 需要实现这个接口
* 3.1）事件处理的接口
```java
public interface EventHandle {
    void doHandle(EventModel model);  //用于处理model 这个具体的事件
    List<EventType> getSupportEventType();  //用于 得知我要关注哪些类型的的事件，只要这些事件发生，我都要处理一下
}

```
* 3.2）事件处理的实现，先创建一个handler包，用于存放各种事件的处理 ,比如LikeHandler
    * 不同的事件对应不同的处理，比如我们可以对Like事件，我们会给被点赞的资讯的拥有者发一个站内信，告诉他谁点赞了 【LikeHandler 来实现这件事】
```java

@Component
public class LikeHandler implements EventHandle {
    @Autowired
    UserService userService;

    @ Autowired
    MessageService messageService;

    @Override
    //处理事件的代码，给被赞的资讯的拥有者发一个站内信
    public void doHandle(EventModel model)
    {
        Message message = new Message();
        User user = userService.getUser(model.getActorId());  //哪个用户赞了
        message.setFromId(3);  //谁发出的这条资讯，这应该是系统
        message.setToId(model.getEntityOwnerId());
        message.setContent("用户"+user.getName()
                +"赞了你的资讯：http://127.0.0.1:8080/news/"+model.getEntityId());
        message.setCreatedDate(new Date());
        messageService.adddMessage(message);

    }

    @Override
    public List<EventType> getSupportEventType() {
        return Arrays.asList(EventType.LIKE);  //likeHandler 只关心点赞，只用于处理点赞
    }
}

```


##### 3）EventProducer：用于把事件发给阻塞队列，即把事件序列化后放到 Redis 的一个队列里面
```java

@Service   //因为要供给各种各样的业务来用，发送事件到阻塞队列
public class EventProducer {
    @Autowired
    JedisAdapter jedisAdapter;  //用的jedis 的阻塞队列

    //发送一个事件到阻塞队列
    public boolean fireEvent(EventModel model){
        try{
            String json = JSONObject.toJSONString(model);  // 1）把事件序列化
            String key = RedisKeyUtil.getEventQueueKey();  // 2) 通过工具类得到你的阻塞队列的名字【“EVENT"】
            jedisAdapter.lpush(key,json);   //放到队列里去
            return true;
        }catch (Exception e){
            return false;
        }

    }
}

```
##### 4）EventConsumer：去取队列里面的事件，取出来后反序列化成 事件发生时的现场（EventModel）,根据事件去找到它对应的 Handler ，把事件处理掉
```java
@Service //用于处理掉队列里的事件，是消费出口
public class EventConsumer implements InitializingBean ,ApplicationContextAware {
    @Autowired
    JedisAdapter jedisAdapter;

    private static final Logger logger = LoggerFactory.getLogger(EventConsumer.class);
    private Map<EventType, List<EventHandle>> config = new HashMap<>(); //对某个事件，找到所有需要对它进行处理的Handler
    private ApplicationContext applicationContext; //应用上下文

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        this.applicationContext = applicationContext;
    }

    @Override
    public void afterPropertiesSet() throws Exception {
        // 一 、初始化：需要一个映射表 config，来了一个事件，我要用哪些handler 来处理 ，
        // 在Spring 一启动后就把这些初始化好，步骤如下：
        // 1) 找出spring中所有实现了EventHandle 的类
        Map<String,EventHandle> beans = applicationContext.getBeansOfType(EventHandle.class);
        if (beans != null) {
            // 2) 遍历这些handler，找到它支持的type；比如LIKE(0),COMMENT(1),LOGIN(2),MAIL(3);
            for (Map.Entry<String, EventHandle> entry : beans.entrySet()) {
                List<EventType> eventTypes = entry.getValue().getSupportEventType();
                // 3） 对它支持的每一个事件类型，把自己注册进去，即让每个事件对应上处理它的 Handler
                for (EventType type : eventTypes) {
                    if (!config.containsKey(type)) {
                        config.put(type, new ArrayList<EventHandle>());
                    }
                    //把自己注册了进去，即把handler 加入事件对应的 handler 链表中
                    config.get(type).add(entry.getValue());
                }
            }
        }

        // 二、开一个线程去不断处理事件（从阻塞队列获取事件，然后根据事件知道它需要被哪些 Handler处理）
        Thread thread = new Thread(new Runnable() {
            @Override
            public void run() {
                while (true){  //不断处理事件
                    String key = RedisKeyUtil.getEventQueueKey();  //从哪个队列取事件，队列名称
                    // 1）取得事件：lpush - brpop 阻塞队列，有事件我就处理，没有事件就阻塞等着,0表示一直等待
                    List<String> events = jedisAdapter.brpop(0,key);
                    // 2） 处理事件
                    for(String message:events){
                        if(message.equals(key)){ //list 的第一个是阻塞队列名字
                            continue;
                        }
                        // 2.1 反序列化得到事件
                        EventModel eventModel = JSON.parseObject(message,EventModel.class); 
                        // 如果这个事件没有被注册过，说明我们没有定义它的处理方法
                        if(!config.containsKey(eventModel.getType())){
                            logger.error("不能识别的事件");
                            continue;
                        }
                        // 2.2 根据事件类型 遍历需要处理它的handler 来处理它
                        for (EventHandle handle : config.get(eventModel.getType())){
                            handle.doHandle(eventModel);
                        }
                    }
                }
            }
        });
        thread.start();
    }
}

```

#### 3、在LikeController 里试验，赞一下，把赞事件加入队列处理
```java
    //赞
    @RequestMapping(path={"/like"},method = {RequestMethod.GET,RequestMethod.POST})
    @ResponseBody
    public String like(@RequestParam("newsId") int newsId){
        int userId = hostHolder.getUser().getId();
        long likeCount = likeService.like(userId,EntityType.ENTITY_NEWS,newsId);  //把这个用户加入该资源的赞集合
        newsService.updateLikeCount(newsId,(int)likeCount); //还要更新news表里的字段
 
        //异步处理由赞引发的操作，把赞事件加入队列
        News news = newsService.getById(newsId);
        eventProducer.fireEvent(new EventModel(EventType.LIKE)  //因为return 了this,所以这么方便
                .setActorId(hostHolder.getUser().getId())   //此线程的用户触发了这个事件
                .setEntityId(newsId)   //触发的对象:是哪个资讯
                .setEntityType(EntityType.ENTITY_NEWS)
                .setEntityOwnerId(news.getUserId())); //资讯的拥有者是谁
        return ToutiaoUtil.getJSONString(0,String.valueOf(likeCount)); //返回给前端
    }
```





## 二、站内邮件通知系统：写一个异常登陆通知 
* 综：在pom中引入java扩展的库javax.mail，
    * 设置发邮件的地址、用户名  【相当于你这个网站的的邮箱就是这个】
    * 真正发邮件的时候把发件人、目标地址、邮件标题、邮件内容设置上，最重要的是邮件内容需要通过模板引擎去渲染【这样就可以实现个性化】
### 1）在 LoginController 里通过 EventProducer 把登陆这个事件发给队列
```java
    //登陆
    @RequestMapping(path={"/login/"},method = {RequestMethod.GET,RequestMethod.POST})
    @ResponseBody
    public String login(Model model, @RequestParam("username") String username,
                      @RequestParam("password") String password,
                      @RequestParam(value = "rember",defaultValue = "0") int rememberme,
                        HttpServletResponse response) {//即是否记住登录
        try {
            Map<String ,Object>  map = userService.login(username,password);
            if(map.containsKey("ticket")){  //登陆成功会下发一个ticket
                Cookie cookie = new Cookie("ticket", map.get("ticket").toString());
                cookie.setPath("/");  //这个cookie是全站有效的
                if (rememberme > 0) {
                    cookie.setMaxAge(3600*24*5);  //有效为5天，如果不写就默认是浏览器关闭就没有了
                }
                response.addCookie(cookie);

                //上面登陆的事都处理完了，我们就发一个事件到异步队列里，让那些需要处理登陆的Handler去处理
                eventProducer.fireEvent(new EventModel(EventType.LOGIN)
                        .setActorId((int) map.get("userId"))  //在UserService.Login里有： map.put("userId", user.getId());
                        .setExt("username", username).setExt("email", "21715086@zju.edu.cn")); //由我的qq邮箱发到这个邮箱

                return ToutiaoUtil.getJSONString(0,map);
            }else {
                return ToutiaoUtil.getJSONString(1,map);
            }

        }catch (Exception e){
            logger.error("登陆异常"+e.getMessage());
            return ToutiaoUtil.getJSONString(1,"登陆异常");
        }
    }
```

### 2）处理队列，如果登陆有异常就发送邮件和站内信【没有写登陆异常算法，所以一登录就会发邮件和站内信】【**邮件是模板才能个性化】
* 2.1) 在pom.xml中引入邮件
```html 
<dependency>
    <groupId>javax.mail</groupId>
    <artifactId>mail</artifactId>
    <version>1.4.7</version>
</dependency>
```
* 2.2 ) 邮件是一个独立的功能，所以写一个Service来发邮件【JavaMailSenderImpl】：邮件有模板，这样才能针对每个人发，发的时候替换掉对应字段，那就成了个性化的邮件
```java
@Service  //专门用于发邮件
public class MailSender implements InitializingBean {
    private  static final Logger logger = LoggerFactory.getLogger(MailSender.class);
    private JavaMailSenderImpl mailSender;

    @Autowired
    private VelocityEngine velocityEngine;

    @Override  //在bean初始化的时候要设置的属性
    public void afterPropertiesSet() throws Exception {
        mailSender = new JavaMailSenderImpl();
        mailSender.setUsername("zhaojing9563@qq.com");  //发送方的邮箱和密码
        mailSender.setPassword("mtpsatvppdgmbijj");
        mailSender.setHost("smtp.qq.com");   //QQ邮箱的发送方服务器地址
        mailSender.setPort(465);     //QQ邮箱的发送方服务器的端口号
        mailSender.setProtocol("smtps");  //协议是smtp+ssl
        mailSender.setDefaultEncoding("utf8");
        Properties javaMailProperties = new Properties();
        javaMailProperties.put("mail.smtp.ssl.enable",true);  //属性设置进来
        mailSender.setJavaMailProperties(javaMailProperties);  //这样mailSender的属性就全部设置好了
    }


    //发送邮件，邮件是以html 形式
    public boolean sendWithHTMLTemplate(String to,String subject,
                                        String template,
                                        Map<String,Object> model){//model是传入的一些参数
        try {
            String nick = MimeUtility.encodeText("赵静");  //邮件是谁发的（它的昵称）
            InternetAddress from = new InternetAddress(nick + "<zhaojing9563@qq.com>");  //发件人是谁
            MimeMessage mimeMessage = mailSender.createMimeMessage();  //构造一封邮件
            MimeMessageHelper mimeMessageHelper = new MimeMessageHelper(mimeMessage);
            String result = VelocityEngineUtils    //*******邮件有模板，这样才能针对每个人发，发的时候替换掉对应字段
                    .mergeTemplateIntoString(velocityEngine,template,"UTF-8",model);
            mimeMessageHelper.setTo(to);  //邮件去哪里
            mimeMessageHelper.setFrom(from);  //从哪里来
            mimeMessageHelper.setSubject(subject); //标题是什么
            mimeMessageHelper.setText(result,true);  //内容是什么，【即result这份邮件模板】
            mailSender.send(mimeMessage);  //发送
            return true;
        }catch (Exception e){
            logger.error("发送邮件失败"+e.getMessage());
            return false;
        }
    }
}

```
* 2.3) LoginController里面会会发送一个登陆事件到阻塞队列
```java
@Service   //因为要供给各种各样的业务来用，发送事件到阻塞队列
public class EventProducer {
    @Autowired
    JedisAdapter jedisAdapter;  //用的jedis 的阻塞队列

    //发送一个事件到阻塞队列
    public boolean fireEvent(EventModel model){
        try{
            String json = JSONObject.toJSONString(model);  // 1）把事件序列化
            String key = RedisKeyUtil.getEventQueueKey();  // 2) 通过工具类得到你的阻塞队列的名字【“EVENT"】
            jedisAdapter.lpush(key,json);   //放到队列里去
            return true;
        }catch (Exception e){
            return false;
        }

    }
}
```
* 2.4 ) 然后你的 EventConsumer 从队列里取得事件，让需要处理这件事的 Handle 去处理，比如 LoginExceptionHandler 
```java
@Service //用于处理掉队列里的事件，是消费出口
public class EventConsumer implements InitializingBean ,ApplicationContextAware {
    @Autowired
    JedisAdapter jedisAdapter;

    private static final Logger logger = LoggerFactory.getLogger(EventConsumer.class);
    private Map<EventType, List<EventHandle>> config = new HashMap<>(); //对某个事件，找到所有需要对它进行处理的Handler
    private ApplicationContext applicationContext; //应用上下文

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        this.applicationContext = applicationContext;
    }

    @Override
    public void afterPropertiesSet() throws Exception {
        // 一 、初始化：需要一个映射表 config，来了一个事件，我要用哪些handler 来处理 ，
        // 在Spring 一启动后就把这些初始化好，步骤如下：
        // 1) 找出spring中所有实现了EventHandle 的类
        Map<String,EventHandle> beans = applicationContext.getBeansOfType(EventHandle.class);
        if (beans != null) {
            // 2) 遍历这些handler，找到它支持的type；比如LIKE(0),COMMENT(1),LOGIN(2),MAIL(3);
            for (Map.Entry<String, EventHandle> entry : beans.entrySet()) {
                List<EventType> eventTypes = entry.getValue().getSupportEventType();
                // 3） 对它支持的每一个事件类型，把自己注册进去，即让每个事件对应上处理它的 Handler
                for (EventType type : eventTypes) {
                    if (!config.containsKey(type)) {
                        config.put(type, new ArrayList<EventHandle>());
                    }
                    //把自己注册了进去，即把handler 加入事件对应的 handler 链表中
                    config.get(type).add(entry.getValue());
                }
            }
        }

        // 二、开一个线程去不断处理事件（从阻塞队列获取事件，然后根据事件知道它需要被哪些 Handler处理）
        Thread thread = new Thread(new Runnable() {
            @Override
            public void run() {
                while (true){  //不断处理事件
                    String key = RedisKeyUtil.getEventQueueKey();  //从哪个队列取事件，队列名称
                    // 1）取得事件：lpush - brpop 阻塞队列，有事件我就处理，没有事件就阻塞等着,0表示一直等待
                    List<String> events = jedisAdapter.brpop(0,key);
                    // 2） 处理事件
                    for(String message:events){
                        if(message.equals(key)){ //list 的第一个是阻塞队列名字
                            continue;
                        }
                        // 2.1 反序列化得到事件
                        EventModel eventModel = JSON.parseObject(message,EventModel.class);
                        // 如果这个事件没有被注册过，说明我们没有定义它的处理方法
                        if(!config.containsKey(eventModel.getType())){
                            logger.error("不能识别的事件");
                            continue;
                        }
                        // 2.2 根据事件类型 遍历需要处理它的handler 来处理它
                        for (EventHandle handle : config.get(eventModel.getType())){
                            handle.doHandle(eventModel);
                        }
                    }
                }
            }
        });
        thread.start();
    }
}

```
* 2.5) 比如 LoginExceptionHandler
```java
@Component
public class LoginExceptionHandler implements EventHandle {
    @Autowired
    MessageService messageService;

    @Autowired
    MailSender mailSender;    //********即上面那个发送邮件的服务

    @Override
    public void doHandle(EventModel model) {
        // 1）判断是否由异常登陆，
        // 2）如果有异常登陆，发站内信
        Message message = new Message();
        message.setToId(model.getActorId());
        message.setContent("你上次的登陆Ip异常");
        message.setFromId(3);  //用户3是系统
        message.setCreatedDate(new Date());
        messageService.adddMessage(message);

        // 2）如果有异常登陆，给你发一封邮件
        Map<String ,Object> map = new HashMap<>();  //传进去方便模板使用
        map.put("username",model.getExt("username"));
        mailSender.sendWithHTMLTemplate(model.getExt("email"), //发给谁
                "登录异常",  //邮件的标题
                "mails/welcome.html", //邮件使用的模板
                map);
    }

    @Override
    public List<EventType> getSupportEventType() {
            return Arrays.asList(EventType.LOGIN); // 1) 我要关注的是登陆这件事
    }
}
```












# 第九周 多种资讯排序算法以及多线程讲解
## 一、资讯排序
### 1.1 通用排序：根据某个值来排序，order by
#### 1.按单位时间内的交互数，del.icio.us按1小时内收藏排行来排序：参与的人越多，认为它越火
#### 2.按总交互数，按总点赞数来排序：可能这个话题比较有争议，所以数据很大，然而并没有随着时间的推移沉下去
#### 3.按评论数加权：评论比赞有价值，
#### 4.按时间排序：时间最新放最前面
### 1.2代码实现：以log 得方式大幅度降低大流量导致得数据的波动、用指数来提升权重
* 1）找出需要参与排序的元素，比如用户，资讯等
* 2）利用redis的优先队列，把元素和它算出来的分值放进去
* 3）数据的更新：可以用一条线程定时的去触发计算更新【优化：只排序最近几天【或者前几页】的数据或者只重新计算改变过的数据】
### 1.3 Hacker News 的排序：//一个资讯网站：核心就是通过一个公式算一个分数
* 对每个资讯都算了一个值，根据点赞数，发布时间来排序，因为是资讯网站，所以注重时间，对时间加了重力加速度，提升了时间的权重，你如果注重点赞数，也可以提升点赞数的权重【用指数来提升权重********】
![](https://github.com/zhaojing5340126/interview/blob/master/picture/hacker.png?raw=true)

### 1.3 Reddit  //也是分享资讯的一个网站 ：核心也是通过一个资讯算一个分数 【时间是最重要的权重，以log 得方式大幅度降低大流量导致的点赞数，适合流量大的新闻类排序】
* 依据时间差来做后面的一个参数，在依据投票的赞和踩的差来表明后面的时间差对你来说是正相关还是负相关，踩的多，后面就是负数，分值降低【时间越近降得越快】，赞的多，则时间越近，升得越多
![](https://github.com/zhaojing5340126/interview/blob/master/picture/re.png?raw=true)
* 1) t<sub>s</sub> = A -B ;【A：发帖时间，B：网站创建时间； 二者的差值】
* 2）x = U - D ;【U：赞，D：踩； 踩和赞的差值：秒差】
* 3）y ：
    * x>0：表明这是一个好帖子，y=1
    * x=0: 不好不差：         y=0
    * x<0：这是一个不好的帖子  y=-1
* 4) z：赞和踩的差
* 5）得到分数 f(t<sub>s</sub>,y,z)
    * 时间越新，如果y为正，你的分值就越高，如果y为负，你的分值就越低，基本线性
    * log<sub>10</sub> z ,用于平滑【以log 得方式大幅度降低大流量导致得数据的波动**********】，对数你要显示价值，你得很大才可以
        * 86400（一天的秒数）/45000=1.92 一天权重调整【赞就是增加1.92，踩就是减少1.92】，10<sup>1.92</sup>=83 投票差要涨83倍才能和前一天的权重等价【45000用于调整时间带来的影响】
        * 且投票差前10【log<sub>10</sub> 10 =1 】和接下来的100【log<sub>10</sub> 100 =2 】等权；认为前面10个赞更有价值，后面可能存在跟随点赞
### 1.4 StackOverflow //一个问答网站
![](https://github.com/zhaojing5340126/interview/blob/master/picture/stackOverFlow.png?raw=true)
### 1.5 IMDB  //一个电影评分网站 分为两拨：总的平均值和当前电影投票的平均值
* 加权排名(WR) = (v ÷(v+m)) ×R + (m ÷(v+m)) × C
    * R = 某电影投票平均分
    * v = 有效投票人数 ：有效投票人数越多，左边就逐步偏向于1，电影得分就基本越来越相信这些投票人，而人数越少，左边偏向于0，右边偏向于1，电影得分倾向于平均值C
        * 你这个人投过很多票了，我就认为你是认真看电影的人，是有效的，防止水军
    * m = 最低投票人数，1250
    * C = 所有电影平均值
* 投票人数越多，越偏向于用户打分值，防止冷门电影小数人高分导致的高分

## 二、多线程：怎么通过多线程处理任务
### 2.1 多线程的优势和挑战
#### 1） 优势：可以充分利用多处理器；可以异步处理任务
#### 2） 挑战：数据会被多个线程访问，有安全性问题；不活跃的线程也会占用内存资源； 可能导致死锁
### 2.2 Thread ,Synchronized
* 实现线程的方式：
    * 1.extends Thread，重载run()方法
    * 2.implements Runnable()，实现run()方法
* Synchronized－内置锁
    * 1.放在方法上会锁住所有synchronized方法
    * 2.synchronized(obj) 锁住相关的代码段【要获取obj对象的锁】
### 2.3 BlockingQueue,AtomicInteger
* BlockingQueue 同步队列
    * put / take 插入和取出【阻塞】
### 2.4 ThreadLocal,Executor,Future
* ThreadLocal
    * 1.线程局部变量。即使是一个static成员，每个线程访问的变量是不同的。
    * 2.常见于web中存储当前用户到一个静态工具类中，在线程的任何地方都可以访问到当前线程的用户。
    * 3.参考HostHolder.java里的users
* Executor
    * 1.提供一个运行任务的框架。
    * 2.将任务和如何运行任务解耦。
    * 3.常用于提供线程池或定时任务服务
* Future 
    * 1.返回异步结果
    * 2.阻塞等待返回结果【异步等待返回结果】
    * 3.timeout
    * 4.获取线程中的Exception
```java
public static void testFuture(){
        ExecutorService service =Executors.newSingleThreadExecutor();
        Future<Integer> future = service.submit(new Callable<Integer>() {  //用callable才有返回值
            @Override
            public Integer call() throws Exception {
                Thread.sleep(1000);
                return 1;
            }
        });

        service.shutdown();
        try {
            System.out.println(future.get());  //卡着一直等它的返回值
        }catch (Exception e){
            e.printStackTrace();
        }
    }

```


# 第十周 JavaWeb项目测试和部署，课程总结回顾
## 一、单元测试JUnit简介：测试某个模块或者细小的接口是否正常
*  Junit 测试也是程序员测试，即所谓的白盒测试，它需要程序员知道被测试的代码如何完成功能，以及完成什么样的功能
*  Junit 能很好地简化单元测试，写一点测一点，在编写以后的代码中如果发现问题可以较快的追踪到问题的原因，减小回归错误的纠错难度。
*  Junit 的几种类似于 @Test 的注解：
    * 1.@Test: 测试方法
        * a)(expected=XXException.class)如果程序的异常和XXException.class一样，则测试通过
        * b)(timeout=100)如果程序的执行能在100毫秒之内完成，则测试通过
    * 2.@Ignore: 被忽略的测试方法：加上之后，暂时不运行此段代码
    * 3.@Before: 每一个测试方法之前运行
    * 4.@After: 每一个测试方法之后运行
    * 5.@BeforeClass: 方法必须必须要是静态方法（static 声明），所有测试开始之前运行，注意区分before，是所有测试方法
    * 6.@AfterClass: 方法必须要是静态方法（static 声明），所有测试结束之后运行，注意区分 @After
* 编写测试类的原则：　
    * 1）测试方法上必须使用@Test进行修饰
    * 2）测试方法必须使用public void 进行修饰，不能带任何的参数
    * 3）新建一个源代码目录来存放我们的测试代码，即将测试代码和项目业务代码分开
    * 4）测试类所在的包名应该和被测试类所在的包名保持一致
    * 5）测试单元中的每个方法必须可以独立测试，测试方法间不能有任何的依赖
    * 6）测试类使用Test作为类名的后缀（不是必须）
    * 7）测试方法使用test作为方法名的前缀（不是必须）
### 1.1 测试四部曲：【代码演示：验证点赞是否有效】【每个service都写一个对应的serviceTest】
* 先创建一个 LikeServiceTest：运行顺序是：beforeClass【只运行一次】——before【每次测试开始前都运行】——test1———after——before————test2——after——afterclass
#### 1）初始化数据
#### 2）执行要测试的业务：有正确测试，异常测试，压力测试【疯狂地调用】等
#### 3）验证测试数据
#### 4）清理数据
```java

@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(classes = ToutiaoApplication.class)
public class LikeServiceTest {
    @Autowired
    LikeService likeService;

    @Test  // 2）用于测试数据,正确测试
    public void testLike1(){
        likeService.like(123,1,1);  //用户123喜欢资讯1
        //断言，确认这个必须是这样，不然就报错
        Assert.assertEquals(likeService.getLikeStatus(123,1,1),1);
        System.out.println("testLike1");
    }
    @Test   //2）用于测试数据,正确测试
    public void testLike2(){
        likeService.disLike(123,1,1);
        Assert.assertEquals(1,likeService.getLikeStatus(123,1,1));
        System.out.println("testLike2");
    }

    @Test(expected=IllegalArgumentException.class)  //正常情况是要抛某个异常的。【把那个异常的类注释一下】
    public void exceptionTest(){
        throw new IllegalArgumentException("xxxx");
    }

    @Before  // 1）用于初始化数据，在每一个测试用例开始前都会跑一次
    public void serUp(){
        System.out.println("serUp");
    }

    @After  // 4） 用于清理数据
    public void tearDown(){
        System.out.println("tearDown");
    }

    @BeforeClass  //在跑所有的测试用例前只跑一次,而Before 是每跑一次测试用例就会跑一次
    public static void beforeClass(){
        System.out.println("beforeClass");
    }

    @AfterClass
    public static void afterClass(){
        System.out.println("afterClass");
    }
}

```
## 二、打包步骤：【用maven把项目打成war包】
* 1.application 继承SpringBootServletInitializer
* 2.pom.xml 里面的打包jar改为war
* 3.mvn package -Dmaven.test.skip=true  ：打包好的war 会在项目的target目录下：toutiao-0.0.1-SNAPSHOT.war
    * 1)自己装一个maven: https://jingyan.baidu.com/article/3065b3b6a00792becef8a46c.html
    * 2)在cmd 下面转到项目的目录下
    * 3）mvn package：打包，会跑测试用例
    * 4）mvn package -Dmaven.test.skip=true  //不跑测试用例
* 4.去除多余的main函数
原来的样子
```java
@SpringBootApplication
public class ToutiaoApplication {
	public static void main(String[] args) {
		SpringApplication.run(ToutiaoApplication.class, args);
	}
}	
```
现在的样子
```java
@SpringBootApplication
public class ToutiaoApplication extends SpringBootServletInitializer {
	@Override  //重写这个方法
	protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
		return builder.sources(ToutiaoApplication.class);
	}

	public static void main(String[] args) {
		SpringApplication.run(ToutiaoApplication.class, args);
	}
}
```
## 三、部署:【把war包拷贝到tomcat的webapps下】//【未弄linux下的部署】
### 1）War包
* War包一般是在进行Web开发时，通常是一个网站Project下的所有源码的集合，里面包含前台HTML/CSS/JS的代码，也包含Java的代码。
* 当开发人员在自己的开发机器上调试所有代码并通过后，为了交给测试人员测试和未来进行产品发布，都需要将开发人员的源码打包成War进行发布。

### 2）Tomcat服务器
* Tomcat服务器是一个免费的开放源代码的Web应用服务器，属于轻量级应用服务器，在中小型系统和并发访问用户不是很多的场合下被普遍使用，是开发和调试JSP程序的首选，最新的Servlet和JSP规范总是能在Tomcat中得到体现。安装：https://www.cnblogs.com/huangwentian/p/7542280.html
#### 2.1)window下的部署：
* War包【我们改名为ROOT就可以直接通过http://localhost:8080来访问【因为tomcat的root是根目录】，不改名字的化就需要通过http://localhost:8080/projectName来访问】可以放在Tomcat下的webapps目录下，随着tomcat服务器的启动【bin/startup.bat，启动Tomcat服务器】，它可以自动被解压 
* 可以将tomcat的默认端口改为7777【原本是8080，但这个和我们开发用的端口一样冲突了，所以改一下】，此后就通过：http://localhost:7777来访问
```xml
  <Connector port="7777" protocol="HTTP/1.1" 
               connectionTimeout="20000"
               redirectPort="8443" />
```
#### 2.2）Linux下的部署
![](https://github.com/zhaojing5340126/interview/blob/master/picture/nginx.png?raw=true)

* 1) 服务器安装Nginx
    * apt-get install nginx mysql-server libmysqlclient-dev maven redis
* 2）打开Nginx 的配置文件：/etc/nginx/sites-enabled/c2
    * 一个访问来，并不是直接访问tomcat,而是访问 Nginx 【一个具有负载均衡的web server：把进来的流量平均分给后台的机器，也可以做反向代理、域名指定等，用一台机器同时服务N个域名】
```
server {
listen 80;  //服务器监听在80端口
server_name c2.nowcoder.com;  //如果访问的域名是c2.nowcoder.com，就反向代理到http://127.0.0.1:8080这个端口【分给后台的机器】
location / {
proxy_pass http://127.0.0.1:8080;
}
}
```
* 注意事项：
    *  启动WEB：tomcat目录增加执行权限。chmod +x startup.sh
    * 配置Java：update-alternatives –config java  【tomcat这些有版本要求，1.8的，你用1.6的java可能就跑不起来】
    * Redis链接修改


## 四、总结
### 1.开发工具Git，IntilliJ
### 2.Spring Boot，Velocity
#### 一、做网站从宏观上来看，分为几个层次,
* http请求过来后：
    * controller：所有的入口：指定了网页访问的地址，指定了参数，http的方法（get,post还是其他）
        * 它会调用底层的service
    * service：对业务进行一些包装：比如取数据等等，这些又依赖于底层的DAO层
    * DAO（数据库层）：我们用的是Mybatis：可以通过注解的方式写各种sql语句，直接对数据库进行操作【并且直接和对象里面的数据做一些匹配】
        * mybatis的配置十分简单，就一个mybatis-config.xml 【直接从官网抄】
        * 在application.properties里 指定一下数据源，指定一下访问的用户名和密码，指定一下mybatis的配置文件【为mybatis-config.xml】
        * 除了用代码注解的方式，还可以用xml的方式：此时要注意namespace不要写错了，要和DAO的包名一样
* 数据从DAO层返回到controller层，有两种展示模式：
    * ResponseBody：直接就是一个JSON串
    * 可以返回一个模板 【这个时候没有 ResponseBody】
        * 模板可以做for循环，属性这些的调用，还可以定义宏，调用方法等 【通过controller里面的数据传递到模板，模板进行解析，页面就显示出来了】
#### 二、还讲了interceptor【拦截器】：
* 对完整的http请求：
    * preHandle：请求开始之前要判断一下做什么事情：知道用户是谁，这个用户是否合法
    * postHandle：在渲染之前把用户传给模板【在请求已经执行好了，渲染之前】
    * afterCompletion：在整个http请求结束以后执行【做清理工作】
### 三、还讲了面向切面编程 @Aspect
* 可以切入到所有的业务里面去
    * @Before：在执行哪些方法时都要先执行我这个方法
    * @After：在方法执行完后调用这个方法
### 四、还讲了redis 【JedisAdapter】
* Redis是一个Key-Value（K-V）系统的数据库，通过各种命令来读取或者设置他的数据
    * 支持简单的get和set，数值操作（incr,decrBy,）  【maybe是变量的概念？】
    * 有列表List的概念：可以加/减数据，筛选一部分数据
    * 哈希的概念：插入，读取【maybe是哈希表的概念？】
    * 有集合的概念：对集合加/减 元素，对集合求并、求交、判断集合是否有一些元素【用于点赞】
    * 有排序集合、即优先队列的概念：可以给每一个key设置一个分值，于是之后可以取范围、逆着取范围，以及根据分数取对应的值【用于各种各样的排行榜】*****
* redis有两种连接方式：
    * 一个连接
    * 连接池：记得每次从中取一个资源，用完以后一定要记得把它关闭返回给数据库
### 五、还讲了异步框架
* 我要执行一些事件，但我并不是马上执行，而是通过一个队列，比如登陆后我还想异步做一些事情
    * 在controller里面，通过一个producer来发送一个事件到队列
    * 我的EventConsumer会在起来的时候把各种事件都注册好，然后死循环地取事件，一旦取到事件，再根据一开始就注册的的所有的handler对这个事件进行处理
### 六、还做了云存储，用的七牛云
* 把一张图片【网页传上来的MutipartFile 】直接传到七牛云里，调用七牛云的接口返回记录的是七牛云给我们的URL【所以很快，不需要存到我们本地】
### 七、登陆注册，用了拦截器，拦截器这里的核心是hostHolder  ***
* 在拦截器里给每一条线程都保存了当前用户，【这个用户从你每次访问的cookie来的，cookie怎么知道你是谁，因为在每次登陆的时候都通过 LoginTicket 存储了数据】
### 7.发邮件
* 用的javax.mail模块，我们发的邮件要是高端邮件，个性化的邮件，而不是纯文本【用了VelocityEngineUtils】
* 核心代码：在初始化的时候把邮件服务器，邮件账号，邮件密码等参数【标准邮件SMTP协议要指定的】
    * 发邮件的时候指定好发送给谁，把邮件模板传进来就好

### 8.单元测试/部署
## 五、扩展
* 1) 产品功能扩展
    * 1.用户注册，邮箱激活流程
    * 2.站内信互发【现在的站内信还是系统发起的，我们可以做成人与人之间的】
    * 3.首页滚动到底部自动加载更多
    * 4.管理员后台管理
    * 5.运营推荐置顶
    * 6.广告推广（smzdm.com）
    * 7.SNS关注，个性化首页
* 2) 技术深度扩展
    * 1.自定义排序：做一个资讯的排序
    * 2.缩图服务：现在用的缩图
    * 3.爬虫自动填充资讯：
    * 4.个性化推荐
## 六、面试怎么讲
* myBatis和数据库：需要深入了解，mybatis是怎么和数据库关联起来的，把JDBC这些这些都包装好了，只是把最核心的sql语句部分暴露出来
* MVC，前后端分离，模板：后端怎么数据暴露给前端，前端的数据怎么展现的
* 单元测试：面试官会觉得你有大局观
* 云SDK接入：只是用了，接触知识面宽泛一些
//* AJAX：后端不太有关，
* Spring框架***：我们主要用的是SpringMVC里面的东西【它是怎么解析各种参数、控制反转和依赖注入，各种对象是怎么初始化的，上下文是怎么构造的*，以及spring里的面向切面编程，拦截器，为什么有这些东西，它是怎么暴露留好这些接口，保证我框架设计未来使用者可以方便扩展实现各种各样的需求；控制反转是怎么初始化对象的，怎么把对象间的依赖关系构建好的；】
* Git工具





# 附录：问题
* 1、这里面的@CookieValue是什么意思，为什么
    * 输入是http://127.0.0.1:8080/response?key=nowcodeid&value=54时才会返回Nowcoderid from cookie: 54
    

```java
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
2、分享的时候会填link,这个link有什么用呢，在detail.html中是点title就跳转到news.link吗？  实际点title跳转的是这样：http://127.0.0.1:8080/news/10，那么link有什么用
```html
     <div class="content" data-url="http://nowcoder.com/posts/5l3hjr">
                      <div class="content-img">
                          <img src="$!{news.image}" alt="">
                      </div>
                      <div class="content-main">
                          <h3 class="title">
                              <!--通过资讯的标题点进去资讯的详情页？-->
                              <a target="_blank" rel="external nofollow" href="$!{news.link}">$!{news.title}</a>
                          </h3>
                          <div class="meta">
                              $!{news.link}
                              <span>
                                  <i class="fa icon-comment"></i> $!{news.commentCount}
                              </span>
                          </div>
                      </div>
                  </div>
```
```java
public String addNews(@RequestParam("image") String image,
                          @RequestParam("title") String title,
                          @RequestParam("link") String link)  
```

3、在登陆框跳出来后，点击注册可以正常运行出用户已经登陆的页面，但点击登陆，还是原来那个页面，没反应
* 因为你登陆入口写错了
3.1 第七章无法跳出登陆框？
* 解决办法，在header.html中加入 home.js
4、不管是图片还是评论输入中文都无法识别，显示？
5、news表中没有 dislikeCount字段，那么，这是怎么回事？
* 因为我只需要在likeCount字段操作就可以了，不喜欢，就从喜欢集合中减少他，在不喜欢集合中加上他
```java
   @RequestMapping(path = {"/like"}, method = {RequestMethod.GET, RequestMethod.POST})
    @ResponseBody
    public String like(@Param("newId") int newsId) {
        long likeCount = likeService.like(hostHolder.getUser().getId(), EntityType.ENTITY_NEWS, newsId);
        // 更新喜欢数
        News news = newsService.getById(newsId);
        newsService.updateLikeCount(newsId, (int) likeCount);  //？****
        eventProducer.fireEvent(new EventModel(EventType.LIKE)
                .setEntityOwnerId(news.getUserId())
                .setActorId(hostHolder.getUser().getId()).setEntityId(newsId));
        return ToutiaoUtil.getJSONString(0, String.valueOf(likeCount));
    }

    @RequestMapping(path = {"/dislike"}, method = {RequestMethod.GET, RequestMethod.POST})
    @ResponseBody
    public String dislike(@Param("newId") int newsId) {
        long likeCount = likeService.disLike(hostHolder.getUser().getId(), EntityType.ENTITY_NEWS, newsId);
        // 更新喜欢数
        newsService.updateLikeCount(newsId, (int) likeCount); //？*****news没有踩字段
        return ToutiaoUtil.getJSONString(0, String.valueOf(likeCount));
    }
```