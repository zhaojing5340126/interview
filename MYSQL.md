

<!-- TOC -->

- [一、day1-3：MySQL连接、登录、安装、升级](#一day1-3mysql连接登录安装升级)
    - [1、连接和登录和安装位置介绍 **](#1连接和登录和安装位置介绍-)
        - [1.1 启动MySQL：/etc/init.d/mysql.server start 【etc里面一般是配置文件】](#11-启动mysqletcinitdmysqlserver-start-etc里面一般是配置文件)
        - [1.2 登录MySQL：mysql -uroot -p(密码)](#12-登录mysqlmysql--uroot--p密码)
        - [1.3 退出MySQL：exit](#13-退出mysqlexit)
        - [1.4 关闭MySQL：/etc/init.d/mysql.server stop](#14-关闭mysqletcinitdmysqlserver-stop)
        - [1.5 一些安装信息介绍](#15-一些安装信息介绍)
            - [1.5.1 安装位置：/usr/local/](#151-安装位置usrlocal)
            - [1.5.2 可执行文件：/usr/local/mysql/bin，比如mysql_safe](#152-可执行文件usrlocalmysqlbin比如mysql_safe)
            - [1.5.3 配置文件my.cnf：MySQL数据库会根据配置文件的参数来启动数据库实例](#153-配置文件mycnfmysql数据库会根据配置文件的参数来启动数据库实例)
            - [1.5.4 datadir参数：指定了数据库数据所在的路径，一般我们会将数据与数据库分离](#154-datadir参数指定了数据库数据所在的路径一般我们会将数据与数据库分离)
        - [附录](#附录)
            - [1）/usr/local/mysql/bin/mysqld_safe &  //&后台启动mysql](#1usrlocalmysqlbinmysqld_safe---后台启动mysql)
            - [2）ps -ef | grep mysqld //查看当前所有的进程](#2ps--ef--grep-mysqld-查看当前所有的进程)
            - [3）mysql --help | grep my.cnf  //查看MySQL数据库启动实例时会在哪些位置查找配置文件](#3mysql---help--grep-mycnf--查看mysql数据库启动实例时会在哪些位置查找配置文件)
    - [2、安装和升级](#2安装和升级)
        - [2.1 MySQL下载【推荐`Linux-Generic`版本】](#21-mysql下载推荐linux-generic版本)
        - [2.2 MySQL安装](#22-mysql安装)
        - [2.3 MySQL升级](#23-mysql升级)
    - [3.附录](#3附录)
        - [3.1 配置文件my.cnf讲解](#31-配置文件mycnf讲解)
        - [3.2 几种登录方式（本地socket连接【方法二、四】或TCP/IP远程连接【方法三】）](#32-几种登录方式本地socket连接方法二四或tcpip远程连接方法三)
        - [3.3. 免密码登录](#33-免密码登录)
        - [3.4. MySQL 参数介绍和设置](#34-mysql-参数介绍和设置)
        - [附录](#附录-1)
            - [1）mysql> show variables; //显示当前mysql的所有参数，且无隐藏参数](#1mysql-show-variables-显示当前mysql的所有参数且无隐藏参数)
            - [2）set autocommit = 0;  //当前会话生效](#2set-autocommit--0--当前会话生效)
            - [3）set global slow_query_log = off; //因为slow_query_log是全局参数](#3set-global-slow_query_log--off-因为slow_query_log是全局参数)
- [二、day4：权限、Role模拟、workbench、MySQL体系结构](#二day4权限role模拟workbenchmysql体系结构)
    - [1、权限管理](#1权限管理)
        - [1.1 “’用户‘ + @ + ’IP‘”=完整的用户标识](#11-用户----ip完整的用户标识)
        - [1.2 系统表权限信息与常用权限](#12-系统表权限信息与常用权限)
            - [1.2.1 系统表权限信息:](#121-系统表权限信息)
                - [1）用户名和IP是否允许](#1用户名和ip是否允许)
                - [2）查看mysql.user表 【查看全局所有库的权限】](#2查看mysqluser表-查看全局所有库的权限)
                - [3）查看mysql.db表 【查看指定库的权限】](#3查看mysqldb表-查看指定库的权限)
                - [4）查看mysql.table_priv表 【查看指定表的权限】](#4查看mysqltable_priv表-查看指定表的权限)
                - [5）查看mysql.column_priv表 【查看指定列的权限】](#5查看mysqlcolumn_priv表-查看指定列的权限)
            - [1.2.2 常用权限：](#122-常用权限)
                - [1）SQL语句：SELECT、INSERT、UPDATE、DELETE、INDEX](#1sql语句selectinsertupdatedeleteindex)
                - [2）存储过程：CREATE ROUTINE、ALTER ROUTINE、EXECUTE、TRIGGER](#2存储过程create-routinealter-routineexecutetrigger)
                - [3）管理权限：SUPER、RELOAD、SHOW DATABASE、SHUTDOWN](#3管理权限superreloadshow-databaseshutdown)
            - [1.2.3 可选资源:](#123-可选资源)
        - [1.3. 创建用户（create user ...）及授予权限（grant...to...）](#13-创建用户create-user-及授予权限grantto)
        - [1.4 撤销权限(revoke...from...只删除权限，drop user ...删除用户)](#14-撤销权限revokefrom只删除权限drop-user-删除用户)
        - [1.5 查看某一个用户的权限（show grants for 'perf'@'127.0.0.1';）](#15-查看某一个用户的权限show-grants-for-perf127001)
        - [1.6 mysql库主要负责存储数据库的用户、权限设置、关键字等mysql自己需要使用的控制和管理信息](#16-mysql库主要负责存储数据库的用户权限设置关键字等mysql自己需要使用的控制和管理信息)
        - [1.7 information_schema](#17-information_schema)
        - [1.8 附录](#18-附录)
            - [1）desc [tablename]; #可以查看表的结构信息](#1desc-tablename-可以查看表的结构信息)
            - [2）create user 'pref'@'127.0.0.1' identified by '123'; #创建用户](#2create-user-pref127001-identified-by-123-创建用户)
            - [3）grant select on sys.* to 'perf'@'127.0.0.1';  #授予权限](#3grant-select-on-sys-to-perf127001--授予权限)
            - [4）revoke select on sys.* from 'perf'@'127.0.0.1'； #删除权限，不删除用户](#4revoke-select-on-sys-from-perf127001-删除权限不删除用户)
            - [5）drop user 'perf'@'127.0.0.1'; #删除用户](#5drop-user-perf127001-删除用户)
            - [6）show grants for 'perf'@'127.0.0.1';  #查看用户权限](#6show-grants-for-perf127001--查看用户权限)
            - [7）select user()；#查看用户](#7select-user查看用户)
            - [8）show databases; #查看数据库](#8show-databases-查看数据库)
            - [9）use sys; #使用sys数据库](#9use-sys-使用sys数据库)
    - [2、Role模拟](#2role模拟)
        - [2.1 角色(Role)：可以用来批量管理用户，同一个角色下的用户，拥有相同的权限。](#21-角色role可以用来批量管理用户同一个角色下的用户拥有相同的权限)
        - [2.2 附录](#22-附录)
            - [1） grant proxy on 'junior_dba'@'127.0.0.1' to 'tom'@'127.0.0.1';  #将junior_dba的权限映射(map)到tom，proxy是代理的意思](#1-grant-proxy-on-junior_dba127001-to-tom127001--将junior_dba的权限映射map到tomproxy是代理的意思)
            - [2） grant select on *.* to 'junior_dba'@'127.0.0.1'; # 给junior_dba（模拟的Role）赋予实际权限](#2-grant-select-on--to-junior_dba127001--给junior_dba模拟的role赋予实际权限)
            - [3） show grants for 'junior_dba'@'127.0.0.1';    #查看权限](#3-show-grants-for-junior_dba127001----查看权限)
            - [4） select * from mysql.proxies_priv;    #查看 proxies_priv的权限](#4-select--from-mysqlproxies_priv----查看-proxies_priv的权限)
    - [3. Workbench与Utilities介绍【略】](#3-workbench与utilities介绍略)
    - [4. MySQL体系结构](#4-mysql体系结构)
        - [4.1 数据库与数据库实例](#41-数据库与数据库实例)
            - [4.1.1 数据库【物理操作系统文件或其他形式文件类型集合，是一个或者一组二进制文件】](#411-数据库物理操作系统文件或其他形式文件类型集合是一个或者一组二进制文件)
            - [4.1.2 数据库实例【用来操作数据库文件的，数据库实例和数据库是一一对应的】](#412-数据库实例用来操作数据库文件的数据库实例和数据库是一一对应的)
        - [4.2 MySQL体系结构：单进程多线程、插件式存储引擎架构、存储引擎的对象是表](#42-mysql体系结构单进程多线程插件式存储引擎架构存储引擎的对象是表)
            - [4.2.1 体系结构图](#421-体系结构图)
            - [4.2.2 单进程多线程结构【MySQL数据库实例在系统上的表现就是一个进程】](#422-单进程多线程结构mysql数据库实例在系统上的表现就是一个进程)
            - [4.2.3 存储引擎【存储引擎的对象是表】](#423-存储引擎存储引擎的对象是表)
            - [4.2.4.逻辑存储结构](#424逻辑存储结构)
        - [4.3 MySQL物理存储结构【主要文件：数据库配置文件、表结构定义文件、错误文件、慢查询日志、通用日志】](#43-mysql物理存储结构主要文件数据库配置文件表结构定义文件错误文件慢查询日志通用日志)
            - [4.3.1 MySQL配置文件my.cnf](#431-mysql配置文件mycnf)
            - [4.3.2 表结构的组成](#432-表结构的组成)
                - [1）.frm：表结构定义文件（二进制）](#1frm表结构定义文件二进制)
                - [2）.MYI：索引文件](#2myi索引文件)
                - [3）.MYD：数据文件](#3myd数据文件)
            - [4.3.3 错误日志文件（用于定位错误）——err.log](#433-错误日志文件用于定位错误errlog)
            - [4.3.4 慢查询日志文件（用于优化查询）——slow.log](#434-慢查询日志文件用于优化查询slowlog)
            - [5、通用日志与审计【进行全面记录， 可用于审计`Audit`，审计插件 MariaDB Audit 】](#5通用日志与审计进行全面记录-可用于审计audit审计插件-mariadb-audit-)
        - [4.4 附录](#44-附录)
            - [1）mysqlfrm --diagnostic /data/mysql_data/aaa/.a.frm  #可将frm文件转成create table的语句](#1mysqlfrm---diagnostic-datamysql_dataaaaafrm--可将frm文件转成create-table的语句)
            - [2）mysql> select version(); #查看mysql版本](#2mysql-select-version-查看mysql版本)
            - [3）set global slow_query_log = 1;  #slow_query_log可以在线打开](#3set-global-slow_query_log--1--slow_query_log可以在线打开)
            - [4）[root@localhost mysql_data]# tail -f slow.log #查看慢查询日志](#4rootlocalhost-mysql_data-tail--f-slowlog-查看慢查询日志)
            - [5）[root@localhost mysql_data]# mysqldumpslow  slow.log #格式化慢查询日志，相同类型的归为一条](#5rootlocalhost-mysql_data-mysqldumpslow--slowlog-格式化慢查询日志相同类型的归为一条)
- [三、day5： 存储引擎](#三day5-存储引擎)
    - [1、存储引擎的概念【用来处理数据库的相关CRUD操作（create、read、update、delete）】](#1存储引擎的概念用来处理数据库的相关crud操作createreadupdatedelete)
    - [2、各种存储引擎介绍](#2各种存储引擎介绍)
        - [2.1、MyISAM](#21myisam)
        - [2.2 Memory存储引擎](#22-memory存储引擎)
        - [2.3 CSV存储引擎](#23-csv存储引擎)
        - [2.4 Federated存储引擎](#24-federated存储引擎)
        - [2. Federated 语法](#2-federated-语法)
    - [5. 多实例安装与federated测试【未】](#5-多实例安装与federated测试未)
    - [附录](#附录-2)
            - [1) show engines; //显示所有存储引擎](#1-show-engines-显示所有存储引擎)
- [八、day8：MySQL数据类型](#八day8mysql数据类型)
    - [1、 INT类型](#1-int类型)
        - [1.1 INT类型分类:TINYINT,SMALLINT,MEDIUMINT,INT,BIGINT](#11-int类型分类tinyintsmallintmediumintintbigint)
        - [1.2 INT类型的使用](#12-int类型的使用)
            - [1.2.1 自增长ID ：推荐使用BIGINT](#121-自增长id-推荐使用bigint)
            - [1.2.2 unsigned or signed：推荐使用signed](#122-unsigned-or-signed推荐使用signed)
        - [1.2.3 INT(N) 中`N`是显示宽度，不表示存储的数字的长度的上限。](#123-intn-中n是显示宽度不表示存储的数字的长度的上限)
        - [1.2.4 AUTO_INCREMENT：自增、每张表一个、必须是索引的一部分](#124-auto_increment自增每张表一个必须是索引的一部分)
        - [1.2.5 附录](#125-附录)
            - [1) create  table  test(a int(3) zerofill);  //显示宽度N=3](#1-create--table--testa-int3-zerofill--显示宽度n3)
            - [2) insert into test values(1); //插入](#2-insert-into-test-values1-插入)
            - [3）select * from test; //显示表列](#3select--from-test-显示表列)
    - [2. 数字类型](#2-数字类型)
        - [2.1 分类：float、double、decimal（精确性非常高，财务系统必须使用）](#21-分类floatdoubledecimal精确性非常高财务系统必须使用)
    - [3. 字符串类型](#3-字符串类型)
        - [3.1 字符串类型：定长字符CHAR(N),变长字符VARCHAR(N)、BLOB、TEXT、其他](#31-字符串类型定长字符charn变长字符varcharnblobtext其他)
        - [3.2 CHAR(10)：可存储10~（10*字符集最大长度w）字节](#32-char10可存储1010字符集最大长度w字节)
        - [3.3 varchar(N)数据存储大小以输入数据的字节的实际长度为准，超过N就截去](#33-varcharn数据存储大小以输入数据的字节的实际长度为准超过n就截去)
        - [3.4 插入的数据尾部带空格时:char忽略空格，varchar认同空格](#34-插入的数据尾部带空格时char忽略空格varchar认同空格)
        - [3.5 在BLOB和TEXT上创建索引时，必须指定索引前缀的长度](#35-在blob和text上创建索引时必须指定索引前缀的长度)
        - [3.6 附录](#36-附录)
            - [1）mysql> select a, length(a) from test_char;  //](#1mysql-select-a-lengtha-from-test_char--)
            - [2）select a, hex(a) from test_char;    //十六进制显示](#2select-a-hexa-from-test_char----十六进制显示)
            - [3）create table text(a int primary key, b text, key(b(64))); //64为索引前缀长度](#3create-table-texta-int-primary-key-b-text-keyb64-64为索引前缀长度)
    - [4. 字符集](#4-字符集)
        - [4.1 常见字符集：utf8mb4(推荐),utf8,gbk,gb18030](#41-常见字符集utf8mb4推荐utf8gbkgb18030)
        - [4.2 字符集的指定：可以在my.cnf、创建数据库、创建表、创建列的时候进行指定](#42-字符集的指定可以在mycnf创建数据库创建表创建列的时候进行指定)
        - [4.2 排序规则collation，默认ci（case insensitive），即不区分大小写](#42-排序规则collation默认cicase-insensitive即不区分大小写)
            - [4.2.1 ci的意义：当有用户叫Tom，你不希望再来一个tom](#421-ci的意义当有用户叫tom你不希望再来一个tom)
            - [4.2.2 修改默认的collation：当前会话有效](#422-修改默认的collation当前会话有效)
        - [4.3 附录](#43-附录)
            - [1）my.cnf：character_set_server=utf8b4](#1mycnfcharacter_set_serverutf8b4)
            - [2）show character set; //显示所有字符集](#2show-character-set-显示所有字符集)
            - [3）create database aaa charset gbk；//指定字符集](#3create-database-aaa-charset-gbk指定字符集)
            - [4) set names utf8mb4 collate utf8mb4_bin;  //修改默认排序规则，当前会话有效](#4-set-names-utf8mb4-collate-utf8mb4_bin--修改默认排序规则当前会话有效)
    - [5. 集合类型ENUM、SET](#5-集合类型enumset)
        - [5.1 通过sql_mode参数可以用户约束检查：强烈建议设置为严格模式](#51-通过sql_mode参数可以用户约束检查强烈建议设置为严格模式)
        - [5.2 附录](#52-附录)
            - [1）create table test_col (sex enum('male', 'female'))](#1create-table-test_col-sex-enummale-female)
            - [2) set sql_mode='strict_trans_tables';  //设置为严格模式](#2-set-sql_modestrict_trans_tables--设置为严格模式)
    - [6. 日期类型](#6-日期类型)
        - [6.1 TIMESTAMP(带时区)、`DATETIME`、DATE、YEAR、TIME](#61-timestamp带时区datetimedateyeartime)
        - [6.2 附录](#62-附录)
            - [1）create table test_time(a timestamp, b datetime);](#1create-table-test_timea-timestamp-b-datetime)
            - [2）insert into test_time values (now(), now()); //插入当前时间](#2insert-into-test_time-values-now-now-插入当前时间)
            - [3）select @@time_zone; //显示电脑时区](#3select-time_zone-显示电脑时区)
            - [4）set time_zone='+00:00'; //设置时区](#4set-time_zone0000-设置时区)
- [九、 day9：JSON数据类型——5.7支持](#九-day9json数据类型57支持)
    - [1. JSON：是一种轻量级的数据交换语言，并且是独立于语言的文本格式。 Key : Value 格式](#1-json是一种轻量级的数据交换语言并且是独立于语言的文本格式-key--value-格式)
    - [2.结构化：二维表结构（行和列），使用SQL语句进行操作](#2结构化二维表结构行和列使用sql语句进行操作)
    - [3.非结构化：使用Key-Value格式定义数据，无结构定义；Value可以嵌套Key-Value格式的数据；使用JSON进行实现](#3非结构化使用key-value格式定义数据无结构定义value可以嵌套key-value格式的数据使用json进行实现)
    - [4. JSON操作示例](#4-json操作示例)
        - [4.1 JSON入门](#41-json入门)
    - [4.2 JSON函数](#42-json函数)
        - [4.4附录](#44附录)
            - [4.4.1 create table user ( uid int auto_increment, data json，primary key(uid) ); //创建带json字段的表](#441-create-table-user--uid-int-auto_increment-data-jsonprimary-keyuid--创建带json字段的表)
            - [4.4.2 insert into user values (null[-- 自增长数据，可以插入null], '{ "name":"tom","age":18, "address":"SZ" }');//插入json数据](#442-insert-into-user-values-null---自增长数据可以插入null--nametomage18-addresssz-插入json数据)
            - [4.4.3 drop table if exists User; //如果存在就删除](#443-drop-table-if-exists-user-如果存在就删除)
            - [4.4.4 ALTER TABLE User ADD COLUMN address2 VARCHAR(512) NOT NULL; //***SQL想新增列需要ALTER TABLE](#444-alter-table-user-add-column-address2-varchar512-not-null-sql想新增列需要alter-table)
            - [4.4.5 truncate table UserJson;](#445-truncate-table-userjson)
            - [4.4.6 select json_object("name", "jery", "email", "jery@163.com", "age",33); //json_object 将list(K-V对)封装成json格式](#446-select-json_objectname-jery-email-jery163com-age33-json_object-将listk-v对封装成json格式)
            - [4.4.7 SELECT uid,JSON_EXTRACT(data,'$.address2') from UserJson; //使用json_extract提取数据](#447-select-uidjson_extractdataaddress2-from-userjson-使用json_extract提取数据)
            - [4.4.8 UPDATE UserJson set data = json_insert(data,"$.address2","HangZhou ...") where uid = 1; //json_insert 插入数据,在address2这个字段](#448-update-userjson-set-data--json_insertdataaddress2hangzhou--where-uid--1-json_insert-插入数据在address2这个字段)
            - [4.4.9 select json_merge(JSON_EXTRACT(data,'$.address') ,JSON_EXTRACT(data,'$.address2')) //json_merge from UserJson;合并数据并返回。原数据不受影响](#449-select-json_mergejson_extractdataaddress-json_extractdataaddress2-json_merge-from-userjson合并数据并返回原数据不受影响)
            - [4.4.10 UPDATE UserJson set data = json_array_append(data,"$.address",JSON_EXTRACT(data,'$.address2')) where JSON_EXTRACT(data,'$.address2') IS NOT NULL AND uid >0; //json_array_append 追加数据](#4410-update-userjson-set-data--json_array_appenddataaddressjson_extractdataaddress2-where-json_extractdataaddress2-is-not-null-and-uid-0-json_array_append-追加数据)
            - [4.4.11 UPDATE UserJson set data = JSON_REMOVE(data,'$.address2') where uid>0; //json_remove 从json记录中删除数据](#4411-update-userjson-set-data--json_removedataaddress2-where-uid0-json_remove-从json记录中删除数据)
- [十、day10：Employees 临时表的创建、外键约束](#十day10employees-临时表的创建外键约束)
    - [1.`Employees`数据库是一个用于学习和测试的数据库，大约`160MB`，`4百万条记录`](#1employees数据库是一个用于学习和测试的数据库大约160mb4百万条记录)
    - [2. 表(TABLE)](#2-表table)
        - [2.1. 表的介绍](#21-表的介绍)
            - [1） 表是关系数据库的核心，表=关系](#1-表是关系数据库的核心表关系)
            - [2） 表是记录的集合：集合是无序的](#2-表是记录的集合集合是无序的)
            - [3） 二维表格模型易于人的理解](#3-二维表格模型易于人的理解)
            - [4） MySQL默认存储引擎都是基于行(记录)存储](#4-mysql默认存储引擎都是基于行记录存储)
            - [5） 每行记录都是基于列进行组织的](#5-每行记录都是基于列进行组织的)
        - [2.2 临时表 create temporary table temp_a(a int);](#22-临时表-create-temporary-table-temp_aa-int)
            - [1) 临时表是`SESSION`级别的, 当前用户logout或者其他用户登录上来，是无法看到这张表的](#1-临时表是session级别的-当前用户logout或者其他用户登录上来是无法看到这张表的)
            - [2) 当临时表和普通表同名时，当前用户只能看到同名的临时表**](#2-当临时表和普通表同名时当前用户只能看到同名的临时表)
            - [3) 创建表时带上`if not exists`进行表的存在性检查；同时建议在临时表的表名前面加上统一的词头：防止创建和普通表同名的临时表](#3-创建表时带上if-not-exists进行表的存在性检查同时建议在临时表的表名前面加上统一的词头防止创建和普通表同名的临时表)
            - [4) 临时表的主要的作用：是给当前登录的用户存储临时数据或者临时结果的。不要和SQL优化器在排序过程中内部帮你创建的临时表相混淆。](#4-临时表的主要的作用是给当前登录的用户存储临时数据或者临时结果的不要和sql优化器在排序过程中内部帮你创建的临时表相混淆)
            - [5) 临时表默认存储引擎是 InnoDB [ 5.7以后]](#5-临时表默认存储引擎是-innodb--57以后)
            - [6) 临时表存储位置：MySQL5.7.9 把临时`表结构`放在`tmpdir`，而数据`表数据`放在`datadir`](#6-临时表存储位置mysql579-把临时表结构放在tmpdir而数据表数据放在datadir)
        - [2.3 查看表结构](#23-查看表结构)
        - [1) show create table test_1\G   -- 表结构](#1-show-create-table-test_1\g------表结构)
        - [2) desc test_1\G  -- 表的描述,描述二维表信息](#2-desc-test_1\g-----表的描述描述二维表信息)
        - [3) show table status like "test_1"\G    -- 看表结构的元数据信息](#3-show-table-status-like-test_1\g-------看表结构的元数据信息)
        - [2.4  ALTER TABLE ：当表记录很大的时候，`alter table`会很耗时，影响性能](#24--alter-table-当表记录很大的时候alter-table会很耗时影响性能)
            - [2.5 alter table test_1 add column b char(10);  -- 添加列 b](#25-alter-table-test_1-add-column-b-char10-----添加列-b)
            - [2.6 alter table test_1 drop column b;  -- 删除列 b](#26-alter-table-test_1-drop-column-b-----删除列-b)
    - [3 外键约束：可以让数据进行一致性更新，但是会有一定的`性能损耗`，线上业务使用不多。通常下述级联更新和删除都是由应用层业务逻辑进行判断并实现，而非使用外键](#3-外键约束可以让数据进行一致性更新但是会有一定的性能损耗线上业务使用不多通常下述级联更新和删除都是由应用层业务逻辑进行判断并实现而非使用外键)
        - [3.1 外键约束三种形式：【合理模式：删除父表，子表置空；更新父表，子表做级联操作】（on delete set null on update cascade）](#31-外键约束三种形式合理模式删除父表子表置空更新父表子表做级联操作on-delete-set-null-on-update-cascade)
            - [1） 严格模式(默认)：restrict,父表不能删除或更新一个已经被子表数据引用的记录](#1-严格模式默认restrict父表不能删除或更新一个已经被子表数据引用的记录)
            - [2)  级联模式：cascade，父表的操作，对应子表关联的数据也跟着操作](#2--级联模式cascade父表的操作对应子表关联的数据也跟着操作)
            - [3） 置空模式：set null，父表被操作后，子表对应的外键字段被置空。](#3-置空模式set-null父表被操作后子表对应的外键字段被置空)
        - [3.2 外键约束的操作【maybe，外键相当于一个链接，存放在子表，链接指向父表】](#32-外键约束的操作maybe外键相当于一个链接存放在子表链接指向父表)
            - [1) create table child (id int, parent_id INT,index(parent_id),foreign key (parent_id) references parent(id) on delete cascade on update cascade ) engine=innodb; //创建外键约束，child表的parent_id和parent表的id关联在一起了，innodb强制外键使用索引](#1-create-table-child-id-int-parent_id-intindexparent_idforeign-key-parent_id-references-parentid-on-delete-cascade-on-update-cascade--engineinnodb-创建外键约束child表的parent_id和parent表的id关联在一起了innodb强制外键使用索引)
            - [2)  update parent set id=100 where id=1;   //级联更新级联删除，父表parent的id变为1，子表child的parent_id也会变为1](#2--update-parent-set-id100-where-id1---级联更新级联删除父表parent的id变为1子表child的parent_id也会变为1)
            - [3）alter table child drop foreign key child_ibfk_1;  -- 删除 之前的外键，外键名字可以通过show create table 看到](#3alter-table-child-drop-foreign-key-child_ibfk_1-----删除-之前的外键外键名字可以通过show-create-table-看到)
            - [4）  alter table child add foreign key(parent_id) references parent(id) on update cascade on delete restrict;  -- 添加外键，使用严格模式](#4--alter-table-child-add-foreign-keyparent_id-references-parentid-on-update-cascade-on-delete-restrict-----添加外键使用严格模式)
- [十一、day11：SQL语法之SELECT](#十一day11sql语法之select)
    - [1. SELECT语法介绍](#1-select语法介绍)
    - [2. LIMIT 和 ORDER BY](#2-limit-和-order-by)
        - [2.1 order by col_name ：`ORDER BY` 是把已经查询好的结果集进行排序， asc ：升序(默认), desc：降序。](#21-order-by-col_name-order-by-是把已经查询好的结果集进行排序-asc-升序默认-desc降序)
            - [1）emp_no 是主键，order by 主键 不会创建临时表的，主键(索引)本身有序](#1emp_no-是主键order-by-主键-不会创建临时表的主键索引本身有序)
            - [2）select * from employees order by emp_no asc limit 5,5;  -- 从第5条 开始取，取5条出来](#2select--from-employees-order-by-emp_no-asc-limit-55-----从第5条-开始取取5条出来)
            - [3）select * from employees where emp_no > 20000 order by emp_no limit 5; --取出5条](#3select--from-employees-where-emp_no--20000-order-by-emp_no-limit-5---取出5条)
    - [3. WHERE：`WHERE`是将查询出来的结果，通过`WHERE`后面的条件，对结果进行过滤](#3-wherewhere是将查询出来的结果通过where后面的条件对结果进行过滤)
        - [3.1 逻辑：and、or、not](#31-逻辑andornot)
            - [1） select * from employees where emp_no > 40000 and hire_date > '1991-01-01' order by emp_no limit 4;  //也可以加双引号“1991-01-01”](#1-select--from-employees-where-emp_no--40000-and-hire_date--1991-01-01-order-by-emp_no-limit-4--也可以加双引号1991-01-01)
            - [2） select * from employees where (emp_no > 40000 and birth_date > '1961-01-01')  or (emp_no > 40000 and hire_date > '1991-01-01') order by emp_no limit 5;](#2-select--from-employees-where-emp_no--40000-and-birth_date--1961-01-01--or-emp_no--40000-and-hire_date--1991-01-01-order-by-emp_no-limit-5)
    - [4. JOIN](#4-join)
        - [4.1. INNER JOIN -内联](#41-inner-join--内联)
            - [1）select * from employees,titles where employees.emp_no = titles.emp_no limit 5;](#1select--from-employeestitles-where-employeesemp_no--titlesemp_no-limit-5)
            - [2）select e.emp_no, concat(last_name,' ', first_name) as emp_name, gender, title from employees as e,titles as t  where e.emp_no = t.emp_no limit 5;   -- 使用别名as，也可以不使用](#2select-eemp_no-concatlast_name--first_name-as-emp_name-gender-title-from-employees-as-etitles-as-t--where-eemp_no--temp_no-limit-5------使用别名as也可以不使用)
            - [3) select e.emp_no, concat(last_name,' ', first_name) as emp_name, gender, title from employees as e join titles as t  on e.emp_no = t.emp_no limit 5;  //inner join...on...](#3-select-eemp_no-concatlast_name--first_name-as-emp_name-gender-title-from-employees-as-e-join-titles-as-t--on-eemp_no--temp_no-limit-5--inner-joinon)
        - [4.2. OUTER JOIN：包括left join 和 right join 【outer是省略了的】， ON 参与outer join的结果的生成，而where只是对结果的一个过滤](#42-outer-join包括left-join-和-right-join-outer是省略了的-on-参与outer-join的结果的生成而where只是对结果的一个过滤)
            - [4.2.1 left join ： 左表 left join 右表 on 条件](#421-left-join--左表-left-join-右表-on-条件)
                - [1) select * from  t1 left join t2 on t1.a = t2.b;  //左表t1全部显示，右表满足这个匹配条件的才显示，不满足则显示null](#1-select--from--t1-left-join-t2-on-t1a--t2b--左表t1全部显示右表满足这个匹配条件的才显示不满足则显示null)
            - [4.2.2  right join ： 左表 right join 右表 on 条件](#422--right-join--左表-right-join-右表-on-条件)
                - [2) select * from  t1 right join t2 on t1.a = t2.b;  //右表t2全部显示，左表满足这个匹配条件的才显示，不满足则显示null](#2-select--from--t1-right-join-t2-on-t1a--t2b--右表t2全部显示左表满足这个匹配条件的才显示不满足则显示null)
            - [4.2.3 题目： 查找在t1表，而不在t2表的数据](#423-题目-查找在t1表而不在t2表的数据)
                - [3）select * from t1 left join t2 on t1.a = t2.b where t2.b is null;  //**判断是否为null用 is](#3select--from-t1-left-join-t2-on-t1a--t2b-where-t2b-is-null--判断是否为null用-is)
            - [4.2.5 题目：查找哪些员工不是经理](#425-题目查找哪些员工不是经理)
                - [4）select e.emp_no,concat(last_name,' ', first_name) as emp_name, gender, d.dept_no from employees as e left join dept_manager as d on e.emp_no = d.emp_no where d.emp_no is null limit 5;](#4select-eemp_noconcatlast_name--first_name-as-emp_name-gender-ddept_no-from-employees-as-e-left-join-dept_manager-as-d-on-eemp_no--demp_no-where-demp_no-is-null-limit-5)
        - [4.3. GROUP BY：分组](#43-group-by分组)
            - [4.3.1 题目：选出部门人数 > 50000](#431-题目选出部门人数--50000)
                - [1） select dept_no, count(dept_no) from dept_emp group by dept_no having count(dept_no) > 50000;  -- 通过 dept_no部门分组，count是计算数量， 如果是对分组的聚合函数做过滤，使用having，用where报语法错误](#1-select-dept_no-countdept_no-from-dept_emp-group-by-dept_no-having-countdept_no--50000-----通过-dept_no部门分组count是计算数量-如果是对分组的聚合函数做过滤使用having用where报语法错误)
- [十二、day12：子查询 INSERT UPDATE DELETE REPLACE](#十二day12子查询-insert-update-delete-replace)
    - [1.子查询：指在一个select语句中嵌套另一个select语句。同时，子查询必须包含括号。](#1子查询指在一个select语句中嵌套另一个select语句同时子查询必须包含括号)
        - [1.1.  ANY / SOME](#11--any--some)
            - [1）select a from t1 where a > any(select a from t2);  //t1表内a列的值 大于 t2表中a列的任意(any)一个值（t1.a > any(t2.a) == true）,则返回t1.a的记录](#1select-a-from-t1-where-a--anyselect-a-from-t2--t1表内a列的值-大于-t2表中a列的任意any一个值t1a--anyt2a--true则返回t1a的记录)
                - [1. `select a from t1` 是外部查询(outer query)](#1-select-a-from-t1-是外部查询outer-query)
                - [2. `(select a from t2)` 是子查询](#2-select-a-from-t2-是子查询)
                - [3.ANY和some是一个意思，必须与一个`比较操作符`一起使用： `=`, `>`, `<`, `>=`, `<=`, `<>` ,其中<>表示不等于](#3any和some是一个意思必须与一个比较操作符一起使用-------其中表示不等于)
        - [1.2. IN：`in`是`ANY`的一种特殊情况：**`"in"`** 的结果等同于  **`"= any"`**](#12-inin是any的一种特殊情况in-的结果等同于---any)
            - [1） select a from t1 where a in (select a from t2);](#1-select-a-from-t1-where-a-in-select-a-from-t2)
        - [1.3. ALL](#13-all)
            - [1）truncate t1;   -- 清空t1](#1truncate-t1------清空t1)
            - [2）select a from t1 where a > all(select a from t2); //`ALL`关键词必须与一个`比较操作符`一起使用，NOT IN 是 <> ALL 的别名](#2select-a-from-t1-where-a--allselect-a-from-t2-all关键词必须与一个比较操作符一起使用not-in-是--all-的别名)
        - [1.4. 子查询的分类](#14-子查询的分类)
            - [1.4.1 独立子查询：不依赖外部查询而运行的子查询](#141-独立子查询不依赖外部查询而运行的子查询)
            - [1.4.2 相关子查询：引用了外部查询列的子查询](#142-相关子查询引用了外部查询列的子查询)
            - [1.4.3.和 null比较，使用is和is not， 而不是 = 和 <>](#143和-null比较使用is和is-not-而不是--和-)
    - [2. INSERT (select 比 values强大)](#2-insert-select-比-values强大)
        - [1）insert into t1 values(2),(3),(-1);  -- 插入多个值，MySQL独有](#1insert-into-t1-values23-1-----插入多个值mysql独有)
        - [2）insert into t3(a) select 8;  -- 指定列a，以下这些方法values都不能用](#2insert-into-t3a-select-8-----指定列a以下这些方法values都不能用)
        - [3) insert into t3 select 8, 9;  -- 不指定列，但是插入值匹配列的个数和类型](#3-insert-into-t3-select-8-9-----不指定列但是插入值匹配列的个数和类型)
        - [4) insert into t3(b) select a from t2;  -- 从t2表中查询数据并插入到t3(a)中，注意指定列](#4-insert-into-t3b-select-a-from-t2-----从t2表中查询数据并插入到t3a中注意指定列)
    - [3. DELETE](#3-delete)
        - [1）delete from t3 where a is null;  -- 根据过滤条件删除](#1delete-from-t3-where-a-is-null-----根据过滤条件删除)
        - [2）delete from t3;   -- 删除整个表](#2delete-from-t3------删除整个表)
    - [4. UPDATE【update...set...】](#4-updateupdateset)
        - [1）update t3 set a=10 where a=1;](#1update-t3-set-a10-where-a1)
        - [2）update t1 join t2 on t1.a = t2.a set t1.a=100;  -- 先得到t1.a=t2.a的结果集， 然后将结果集中的t1.a设置为100](#2update-t1-join-t2-on-t1a--t2a-set-t1a100-----先得到t1at2a的结果集-然后将结果集中的t1a设置为100)
    - [5. REPLACE: replace的原理是：先delete，再insert，没有替换对象时，类似插入效果insert](#5-replace-replace的原理是先delete再insert没有替换对象时类似插入效果insert)
        - [1) replace into t4 values(1, 100); -- 替换该主键对应的值](#1-replace-into-t4-values1-100----替换该主键对应的值)
    - [6. 其他知识点](#6-其他知识点)
        - [6.1 更新有关系的值](#61-更新有关系的值)
        - [6.2 显示行号(RowNumber)](#62-显示行号rownumber)
            - [1) set @rn:=0;  -- 冒号是赋值，产生 SESSION(会话)级别的变量,这是MySQL自定义的变量](#1-set-rn0-----冒号是赋值产生-session会话级别的变量这是mysql自定义的变量)
            - [2）select @rn:=@rn+1 as rownumber, emp_no, gender from employees limit 10;  -- ：= 是赋值的意思](#2select-rnrn1-as-rownumber-emp_no-gender-from-employees-limit-10------是赋值的意思)
- [十三、day013-作业讲解一 Rank 视图 UNION 触发器上](#十三day013-作业讲解一-rank-视图-union-触发器上)
    - [1. 作业讲解](#1-作业讲解)
        - [1）select datediff('1971-01-01', '1970-01-05');  //971-01-01 减去 1970-01-5 为361天](#1select-datediff1971-01-01-1970-01-05--971-01-01-减去-1970-01-5-为361天)
        - [2) select datediff('1971-01-01', '1970-01-05') / 7;  -- 求周数](#2-select-datediff1971-01-01-1970-01-05--7-----求周数)
        - [3) select floor(5.4); //向下取整](#3-select-floor54-向下取整)
        - [4）select round(5.4); //ROUND(X, D) 返回值是对数字X保留到小數点后D位，进行四舍五入，D默认为0；如果D为负数，则保留小数点左边（整数）的位数](#4select-round54-roundx-d-返回值是对数字x保留到小數点后d位进行四舍五入d默认为0如果d为负数则保留小数点左边整数的位数)
        - [5）select adddate('1970-01-05', interval 7 day);   //使用adddate函数，在1970-01-05的基础上，增加7天](#5select-adddate1970-01-05-interval-7-day---使用adddate函数在1970-01-05的基础上增加7天)
    - [2. Rank 排名问题的步骤：](#2-rank-排名问题的步骤)
        - [1）set @prev_value := NULL;  //prev_value可以理解为是临时保存第N-1行的score的变量](#1set-prev_value--null--prev_value可以理解为是临时保存第n-1行的score的变量)
        - [2) set @rank_count := 0;   ///用于存放当前的排名](#2-set-rank_count--0---用于存放当前的排名)
        - [3)  select  id, score,](#3--select--id-score)
        - [case](#case)
        - [when @prev_value = score then @rank_count  // 相等则prev_value不变， 并返回rank_count（第一次为NULL，不会相等，所以跳转到下一个when语句）](#when-prev_value--score-then-rank_count---相等则prev_value不变-并返回rank_count第一次为null不会相等所以跳转到下一个when语句)
        - [when @prev_value := score then @rank_count := @rank_count + 1   // 不等，则第N行的score赋值(:=)给prev_value。且rank_count增加1](#when-prev_value--score-then-rank_count--rank_count--1----不等则第n行的score赋值给prev_value且rank_count增加1)
        - [end as rank_column   -- case 开始的，end结尾](#end-as-rank_column------case-开始的end结尾)
        - [from test_rank order by score desc;](#from-test_rank-order-by-score-desc)
    - [3. 视图：视图在mysql是虚拟表，视图在创建的瞬间，便确定了结构（每次查询视图，实际上还是去查询的原来的表，只是查询的规则是在视图创建时经过定义的。），其作用是，可以对开发人员是透明的，可以隐藏部分关键的列。](#3-视图视图在mysql是虚拟表视图在创建的瞬间便确定了结构每次查询视图实际上还是去查询的原来的表只是查询的规则是在视图创建时经过定义的其作用是可以对开发人员是透明的可以隐藏部分关键的列)
        - [1) create view view_rank as select * from test_rank;  //创建视图view_rank](#1-create-view-view_rank-as-select--from-test_rank--创建视图view_rank)
        - [2) show create table test_rank\G   //视图是以一张表的形式存在的，可通过show tables看到，和真正的表不同的是，这里show出来的是视图的定义](#2-show-create-table-test_rank\g---视图是以一张表的形式存在的可通过show-tables看到和真正的表不同的是这里show出来的是视图的定义)
        - [3）select * from view_rank;       //可以直接查询该视图得结果](#3select--from-view_rank-------可以直接查询该视图得结果)
        - [3.1 视图的算法(`ALGORITHM`)有三种方式：](#31-视图的算法algorithm有三种方式)
            - [1.`UNDEFINED` 默认方式，由MySQL来判断使用下面的哪种算法【我们一般选择默认方式，让MySQL自己判断】](#1undefined-默认方式由mysql来判断使用下面的哪种算法我们一般选择默认方式让mysql自己判断)
            - [2. `MERGE` ： `每次`通过`物理表`查询得到结果，把结果merge(合并)起来返回](#2-merge--每次通过物理表查询得到结果把结果merge合并起来返回)
            - [3.`TEMPTABLE` ： 产生一张`临时表`，把数据放入临时表后，客户端再去临时表取数据（`不会缓存`）](#3temptable--产生一张临时表把数据放入临时表后客户端再去临时表取数据不会缓存)
                - [3.1 `TEMPTABLE 特点`：即使访问条件一样，第二次查询还是会去读取物理表中的内容，并重新生成一张临时表,并不会取缓存之前的表。*（临时表是Memory存储引擎，默认放内存，超过配置大小放磁盘）*](#31-temptable-特点即使访问条件一样第二次查询还是会去读取物理表中的内容并重新生成一张临时表并不会取缓存之前的表临时表是memory存储引擎默认放内存超过配置大小放磁盘)
                - [3.2 当查询有一个较大的结果集时，使用`TEMPTABLE`可以快速的结束对该物理表的访问，从而可以快速释放这张物理表上占用的资源。然后客户端可以对临时表上的数据做一些耗时的操作，而不影响原来的物理表。](#32-当查询有一个较大的结果集时使用temptable可以快速的结束对该物理表的访问从而可以快速释放这张物理表上占用的资源然后客户端可以对临时表上的数据做一些耗时的操作而不影响原来的物理表)
    - [4. UNION：作用是将两个查询的结果集进行合并。](#4-union作用是将两个查询的结果集进行合并)
        - [4.1 UNION必须由`两条或两条以上`的SELECT语句组成，语句之间用关键字`UNION`分隔。](#41-union必须由两条或两条以上的select语句组成语句之间用关键字union分隔)
        - [4.2 UNION中的每个查询必须包含相同的列（`类型相同或可以隐式转换`）、表达式或聚集函数。](#42-union中的每个查询必须包含相同的列类型相同或可以隐式转换表达式或聚集函数)
        - [1）select a, b from t1 union select * from t2;  //使用union会去重，导致性能下降](#1select-a-b-from-t1-union-select--from-t2--使用union会去重导致性能下降)
        - [2）select a, b from t1 union all select * from t2;  //使用 union all 可以不去重， 如果知道数据本身具有唯一性，没有重复，则建议使用`union all`](#2select-a-b-from-t1-union-all-select--from-t2--使用-union-all-可以不去重-如果知道数据本身具有唯一性没有重复则建议使用union-all)
    - [5. 触发器：触发器的对象是`表`，当表上出现`特定的事件`时`触发`该程序的执行](#5-触发器触发器的对象是表当表上出现特定的事件时触发该程序的执行)
        - [5.1 触发器的类型](#51-触发器的类型)
            - [5.1.1 `UPDATE`触发器：用于 update 操作](#511-update触发器用于-update-操作)
            - [5.1.2 `DELETE`触发器：用于delete 操作、replace 操作](#512-delete触发器用于delete-操作replace-操作)
                - [注意：drop，truncate等DDL操作`不会触发`DELETE](#注意droptruncate等ddl操作不会触发delete)
            - [5.1.3 `INSERT`触发器：insert 操作、 load data 操作、replace 操作](#513-insert触发器insert-操作-load-data-操作replace-操作)
        - [5.2 注意：`replace`操作会`触发两次`，一次是`UPDATE`类型的触发器，一次是`INSERT`类型的触发器](#52-注意replace操作会触发两次一次是update类型的触发器一次是insert类型的触发器)
        - [5.3 注意：触发器只触发DML(Data Manipulation Language )操作，不会触发DDL(Data Definition Language)操作（create,drop等操作）](#53-注意触发器只触发dmldata-manipulation-language-操作不会触发ddldata-definition-language操作createdrop等操作)
        - [5.4 触发器总结](#54-触发器总结)
            - [1）触发器对性能有损耗，应当非常慎重使用；](#1触发器对性能有损耗应当非常慎重使用)
            - [2）对于事务表，`触发器执行失败则整个语句回滚`；](#2对于事务表触发器执行失败则整个语句回滚)
            - [3）Row格式主从复制，`触发器不会在从库上执行`；](#3row格式主从复制触发器不会在从库上执行)
            - [4）使用触发器时应防止递归执行；](#4使用触发器时应防止递归执行)
        - [5.5 触发器模拟物化视图](#55-触发器模拟物化视图)
            - [5.5.1 物化视图的概念](#551-物化视图的概念)
                - [1）不是基于基表的虚表](#1不是基于基表的虚表)
                - [2）根据基表实际存在的实表](#2根据基表实际存在的实表)
                - [3）预先计算并保存耗时较多的SQL操作结果（如多表链接(join)或者group by等）](#3预先计算并保存耗时较多的sql操作结果如多表链接join或者group-by等)
        - [5.5 MySQL内建函数演示：ifnull、select into](#55-mysql内建函数演示ifnullselect-into)
            - [1）select ifnull(@test, 100);   // 如果test为NULL，则ifnull返回100,test不为null,则返回test的值](#1select-ifnulltest-100----如果test为null则ifnull返回100test不为null则返回test的值)
            - [2) select * from test_rank_2 where id=1 into @id_1, @score_1;  //选择id=1的记录，将对应的id和score赋值给变量 id_1 和 score_1](#2-select--from-test_rank_2-where-id1-into-id_1-score_1--选择id1的记录将对应的id和score赋值给变量-id_1-和-score_1)
- [十四、day014：存储过程 自定义函数MySQL 执行计划与优化器](#十四day014存储过程-自定义函数mysql-执行计划与优化器)
    - [1. 存储过程](#1-存储过程)
        - [1.1 存储过程介绍:](#11-存储过程介绍)
            - [1) 存储在数据库端的一组SQL语句集；](#1-存储在数据库端的一组sql语句集)
            - [2) 用户可以通过存储过程名和传参多次调用的程序模块；](#2-用户可以通过存储过程名和传参多次调用的程序模块)
        - [1.2 存储过程的特点：](#12-存储过程的特点)
            - [1) 使用灵活，可以使用流控语句、自定义变量等完成复杂的业务逻辑；](#1-使用灵活可以使用流控语句自定义变量等完成复杂的业务逻辑)
            - [2) 提高数据安全性，屏蔽应用程序直接对表的操作，易于进行审计；](#2-提高数据安全性屏蔽应用程序直接对表的操作易于进行审计)
            - [3) 减少网络传输；//因为只需要调用就好](#3-减少网络传输因为只需要调用就好)
            - [4) 提高代码维护的复杂度，实际使用需要结合业务评估；](#4-提高代码维护的复杂度实际使用需要结合业务评估)
    - [2. 自定义函数](#2-自定义函数)
        - [2.1 自定义函数和存储过程很类似，但是必须要有返回值；](#21-自定义函数和存储过程很类似但是必须要有返回值)
        - [2.2 与内置的函数(sum(), max()等)使用方法类似: select fun(val);](#22-与内置的函数sum-max等使用方法类似-select-funval)
        - [2.3 自定义函数可能在遍历每条记录中使用；](#23-自定义函数可能在遍历每条记录中使用)
- [十五、day015-索引 B+树 上](#十五day015-索引-b树-上)
    - [三. B树/B+树](#三-b树b树)
        - [1. B树的定义](#1-b树的定义)
        - [2. B+树的定义](#2-b树的定义)
        - [3. B+树的操作](#3-b树的操作)
        - [3. B+树的扇出(fan out)](#3-b树的扇出fan-out)
        - [4. B+树存储数据举例](#4-b树存储数据举例)
    - [四. MySQL索引](#四-mysql索引)
        - [1. MySQL 创建索引](#1-mysql-创建索引)
        - [2. MySQL 查看索引](#2-mysql-查看索引)
        - [3. Cardinality（基数）](#3-cardinality基数)
        - [4. 复合索引](#4-复合索引)
    - [五. information_schema（一）](#五-information_schema一)
    - [6. EXPLAIN： explain是解释SQL语句的执行计划，即显示该SQL语句怎么执行的，但不会真的去执行。也可以使用desc，等价于explain](#6-explain-explain是解释sql语句的执行计划即显示该sql语句怎么执行的但不会真的去执行也可以使用desc等价于explain)
        - [6.1 Explain输出介绍](#61-explain输出介绍)
            - [（1）. id：执行计划的id标志](#1-id执行计划的id标志)
            - [（2）. select_type：SELECT的类型](#2-select_typeselect的类型)
                - [比如：SIMPLE ：简单SELECT(不使用UNION或子查询等)](#比如simple-简单select不使用union或子查询等)
            - [（3）. table：通常是用户操作的用户表](#3-table通常是用户操作的用户表)
            - [（4）. type](#4-type)
            - [（5）. extra](#5-extra)
- [十八、day018-磁盘](#十八day018-磁盘)
    - [1. iostat](#1-iostat)
    - [2. 磁盘【MySQL配SSD】](#2-磁盘mysql配ssd)
        - [1. 磁盘的访问模式](#1-磁盘的访问模式)
        - [3. 磁盘的分类](#3-磁盘的分类)
        - [4. 提升IOPS性能的手段【因为很早之前还没有SSD】](#4-提升iops性能的手段因为很早之前还没有ssd)
        - [5. RAID类别](#5-raid类别)
        - [6. RAID卡的构造](#6-raid卡的构造)
        - [7. 文件系统和操作系统](#7-文件系统和操作系统)

<!-- /TOC -->

# 一、day1-3：MySQL连接、登录、安装、升级
## 1、连接和登录和安装位置介绍 **
### 1.1 启动MySQL：/etc/init.d/mysql.server start 【etc里面一般是配置文件】
### 1.2 登录MySQL：mysql -uroot -p(密码)
### 1.3 退出MySQL：exit
### 1.4 关闭MySQL：/etc/init.d/mysql.server stop
### 1.5 一些安装信息介绍
#### 1.5.1 安装位置：/usr/local/
#### 1.5.2 可执行文件：/usr/local/mysql/bin，比如mysql_safe
```mysql
./mysqld_safe &     //&后台启动mysql 
ps -ef | grep mysqld    //查看当前所有的进程，grep是文本搜索工具
```
#### 1.5.3 配置文件my.cnf：MySQL数据库会根据配置文件的参数来启动数据库实例
* 启动实例时，MySQL数据库会根据配置文件的参数来启动数据库实例，如果有几个配置文件都有同一个参数，MySQL会以读取到的最后一个配置文件中的参数为准。【若没有配置文件，会按照编译时默认的参数设置启动实例】
#### 1.5.4 datadir参数：指定了数据库数据所在的路径，一般我们会将数据与数据库分离
* 指定了数据库数据所在的路径，默认/usr/local/mysql/data，但一般我们会将数据与数据库分离，用户可以直接修改datadir参数，也可以使用该路径，只是该路径是一个链接而已。
     * 并且必须保证其权限，只有mysql用户和组可以访问，通常MySQL数据库访问权限为mysql：mysql
```mysql
mysql --help | grep my.cnf 

//查看MySQL数据库启动实例时会在哪些位置查找配置文件，/etc/my.cnf  /etc/mysql/my.cnf  /usr/local/mysql/etc/my.cnf  ~/.my.cnf   【我自己的后面三个位置都不存在，配置文件在/etc/my.cnf】
```

### 附录

#### 1）/usr/local/mysql/bin/mysqld_safe &  //&后台启动mysql 

#### 2）ps -ef | grep mysqld //查看当前所有的进程

#### 3）mysql --help | grep my.cnf  //查看MySQL数据库启动实例时会在哪些位置查找配置文件

## 2、安装和升级
### 2.1 MySQL下载【推荐`Linux-Generic`版本】
1. 推荐下载`Linux-Generic`版本
2. `Source Code`版本主要作用是为了让开发人员研究源码使用，自己编译对性能提升不明显

>*下载地址：*
[MySQL Community Server 5.7.9 Linux Generic x86-64bit](http://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.9-linux-glibc2.5-x86_64.tar.gz)<br>
[MySQL Community Server 5.6.27 Linux Generic x86-64bit](http://dev.mysql.com/get/Downloads/MySQL-5.6/mysql-5.6.27-linux-glibc2.5-x86_64.tar.gz)<br>

-----

### 2.2 MySQL安装
1. **安装通用步骤：**
    * 解压缩`mysql-VERSION-linux-glibc2.5-x86_64.tar.gz`
    * 打开`INSTALL_BINARY` 文件，按照`shell>`开头的步骤进行操作
    * 将`export PATH=/安装路径/mysql/bin:$PATH`添加到`/etc/profile`
    * `chkconfig mysqld on`或者`chkconfig mysqld.server on`视你的环境而定，详细步骤如下

2. **MySQL 5.7.X 安装**
    ```bash
    shell> groupadd mysql 									 #创建用户组
    shell> useradd -r -g mysql mysql  						 #创建用户
    shell> cd /usr/local									#我们把数据库安装在此目录下
    shell> tar zxvf /path/to/mysql-VERSION-OS.tar.gz		#解压缩包
    shell> ln -s full-path-to-mysql-VERSION-OS mysql		#创建软连接用mysql链接解压的包
    shell> cd mysql											#转到软连接mysql下
    shell> mkdir mysql-files
    shell> chmod 770 mysql-files
    shell> chown -R mysql .									#设定目录访问权限，用mysql_install_db																#创建MySQL授权表初始化，并设mysql,root																#帐号访问权限
    shell> chgrp -R mysql .
    shell> bin/mysqld --initialize --user=mysql 		#该步骤中会产生零时
                                      #root@localhost密码
                                      #需要自己记录下来
    shell> bin/mysql_ssl_rsa_setup          
    shell> chown -R root .
    shell> chown -R mysql data mysql-files
    shell> bin/mysqld_safe --user=mysql &
    # Next command is optional
    shell> cp support-files/mysql.server /etc/init.d/mysql.server		
    #设置后以后就用/etc/init.d/mysql.server启动
    ```

3. **验证安装**
    * `data`目录在安装之前是空目录，安装完成后应该有`ibXXX`等文件
    * 安装过程中输出的信息中，不应该含有`ERROR`信息，错误信息`默认`会写入到`主机名.err`的文件中
    * 通过`bin/mysql`命令（*5.7.X含有零时密码*）可以正常登录

4.**MySQL启动**
    * `mysqld_safe --user=mysql &` 即可启动，`mysqld_safe`是一个守护`mysqld`进程的脚本程序，旨在`mysqld`意外停止时，可以重启`mysqld`进程
    * 也可以通过`INSTALL_BINARRY`中的的步骤，使用`/etc/init.d/mysql.server  start`进行启动（启动脚本以你复制的实际名字为准，通常改名为`mysqld`,即`/etc/init.d/mysqld start`）

### 2.3 MySQL升级
**1. 环境说明：**
一般说来，MySQL数据库的二进制数据文件data和我们MySQL应用程序安装的位置是分开的。仅仅通过`my.cnf`中的配置项`datadir`告诉MySQL，数据库的数据存在`datadir`这个目录下。当程序和数据分离以后，方便我们对数据库应用程序做版本的升级或者回退。

**2. 安装位置：**
* MySQL安装目录：//实质就是把解压后的安装包放在/usr/local/下
    - **MySQL 5.6.27:** /usr/local/mysql-5.6.27-linux-glibc2.5-x86_64
    - **MySQL 5.7.9 :** /usr/local/mysql-5.7.9-linux-glibc2.5-x86_64
         
* datadir目录：
    - /data/mysq_data/

* 初始环境：
 
    ```bash
    shell> ll | grep mysql
    lrwxrwxrwx   1 root root    34 Nov 16 13:40 mysql -> mysql-5.6.27-linux-glibc2.5-x86_64
    drwxr-xr-x  13 root mysql  4096 Nov 16 13:37 mysql-5.6.27-linux-glibc2.5-x86_64
    drwxr-xr-x   9 7161 wheel 4096 Oct 12 00:29 mysql-5.7.9-linux-glibc2.5-x86_64
    
    shell> ll /data/mysql_data/
    total 13540
    -rw-rw---- 1 mysql mysql    65468 Nov 16 13:50 bin.000001
    -rw-rw---- 1 mysql mysql  1176237 Nov 16 13:50 bin.000002
    -rw-rw---- 1 mysql mysql       26 Nov 16 13:50 bin.index
    -rw-rw---- 1 mysql mysql     6882 Nov 16 13:50 error.log
    -rw-rw---- 1 mysql mysql      865 Nov 16 13:50 ib_buffer_pool
    -rw-rw---- 1 mysql mysql 12582912 Nov 16 13:50 ibdata1
    drwx------ 2 mysql mysql     4096 Nov 16 13:50 mysql
    drwx------ 2 mysql mysql     4096 Nov 16 13:50 performance_schema
    drwx------ 2 mysql mysql     4096 Nov 16 13:49 test
    ```

**3. 版本升级**
```bash
shell> /etc/init.d/mysqld stop  #安全的停止数据库的运行
shell> cd /usr/local/
shell> unlink mysql    #实质就是改变了软连接
shell> ln -s mysql-5.7.9-linux-glibc2.5-x86_64 mysql 
        #此时，MySQL的应用程序版本已经升级完成
        #/etc/init.d/mysqld
        #/etc/profile中PATH增加的/usr/local/mysql/bin
        #都不需要做任何的改变，即可将当前系统的mysql版本升级完成
        #注意：此时只是应用程序升级完成，系统表仍然还是5.6的版本
        

shell> cd /usr/local/mysql
shell> chown root.mysql . -R
#5.7.x -> 5.6.X 降级存在问题，这里暂且注释掉
#shell> cp /data/mysql_data/mysql /你的备份路径/mysql_5_6_27.backup -r
       #该步骤将mysql5.6.27版本的系统表进行了备份，以便将来可以回退
       
shell> /etc/init.d/mysqld start 
#此时 /etc/init.d/mysqld start  # 可以启动
#     且可以使用 mysql -u root -p （原密码） 进入数据库
#     show databases;存在test表，而没有sys表（数据的二进制文件兼容）
#     但是如果去看error.log会发现好多的WARNNING
#     所以，这个时候我们要去 upgrade 去升级

       
shell> mysql_upgrade -p -s  
        #参数 -s 一定要加,表示只更新系统表，-s: upgrade-system-tables
        #如果不加-s,则会把所有库的表以5.7.9的方式重建，线上千万别这样操作
        #因为数据库二进制文件是兼容的，无需升级
        #什么时候不需要-s ? 当一些老的版本的存储格式需要新的特性，
        #                 来提升性能时，不加-s
        #即使通过slave进行升级，也推荐使用该方式升级，速度比较快
   
Enter password: 

    
shell> mysql -u root -p
Enter password: 


mysql> show databases;

+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |  # 这个就是升级后的系统库，如果回退，将备份的拷贝回来覆盖即可
| performance_schema |
| sys                |  # 5.7 新的sys库
| test               |  # 5.6 中的test库
+--------------------+
```
> `5.1.X`、`5.5.X` 、`5.6.X` 是可以直接通过该方式升级到`5.7.X`。`5.0.X`未知，需要测试

**注意：**
如果原来数据二进制文件保存在`/usr/local/`mysql-5.6.27`-linux-glibc2.5-x86_64/data`目录下,在升级之前，要么将该目录的数据拷贝到新的你指定的data目录（比如`/usr/local/`mysql-5.7.9`-linux-glibc2.5-x86_64/data` ），要么修改`my.cnf`，将`datadir`指向`/usr/local/mysql-5.6.27-linux-glibc2.5-x86_64/data`，总之一定要确保`my.cnf`中的数据位置和你实际的数据位置是一致的，不管是默认的也好，还是你`datadir`指定的也好


------

## 3.附录
### 3.1 配置文件my.cnf讲解
**1. 配置文件`my.cnf`**   
    
    ```bash
    [client]
    user=david
    password=88888888
    
    [mysqld]
    ########basic settings########
    server-id = 11 
    port = 3306
    user = mysql
    bind_address = 10.166.224.32   #根据实际情况修改,用于监听某个TCP/IP连接，绑定一个IP
    autocommit = 0   #5.6.X安装时，需要注释掉，安装完成后再打开
    character_set_server=utf8mb4
    skip_name_resolve = 1
    max_connections = 800
    max_connect_errors = 1000
    datadir = /data/mysql_data      #数据存放地址，根据实际情况修改,建议和程序分离存放
    transaction_isolation = READ-COMMITTED
    explicit_defaults_for_timestamp = 1
    join_buffer_size = 134217728
    tmp_table_size = 67108864
    tmpdir = /tmp
    max_allowed_packet = 16777216
    sql_mode = "STRICT_TRANS_TABLES,NO_ENGINE_SUBSTITUTION,NO_ZERO_DATE,NO_ZERO_IN_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER"
    interactive_timeout = 1800
    wait_timeout = 1800
    read_buffer_size = 16777216
    read_rnd_buffer_size = 33554432
    sort_buffer_size = 33554432
    ########log settings########
    log_error = error.log
    slow_query_log = 1
    slow_query_log_file = slow.log
    log_queries_not_using_indexes = 1
    log_slow_admin_statements = 1
    log_slow_slave_statements = 1
    log_throttle_queries_not_using_indexes = 10
    expire_logs_days = 90
    long_query_time = 2
    min_examined_row_limit = 100
    ########replication settings########
    master_info_repository = TABLE
    relay_log_info_repository = TABLE
    log_bin = bin.log
    sync_binlog = 1
    gtid_mode = on
    enforce_gtid_consistency = 1
    log_slave_updates
    binlog_format = row 
    relay_log = relay.log
    relay_log_recovery = 1
    binlog_gtid_simple_recovery = 1
    slave_skip_errors = ddl_exist_errors
    ########innodb settings########
    innodb_page_size = 8192
    innodb_buffer_pool_size = 6G    #根据实际情况修改
    innodb_buffer_pool_instances = 8
    innodb_buffer_pool_load_at_startup = 1
    innodb_buffer_pool_dump_at_shutdown = 1
    innodb_lru_scan_depth = 2000
    innodb_lock_wait_timeout = 5
    innodb_io_capacity = 4000
    innodb_io_capacity_max = 8000
    innodb_flush_method = O_DIRECT
    innodb_file_format = Barracuda
    innodb_file_format_max = Barracuda
    innodb_log_group_home_dir = /redolog/  #根据实际情况修改
    innodb_undo_directory = /undolog/      #根据实际情况修改
    innodb_undo_logs = 128
    innodb_undo_tablespaces = 3
    innodb_flush_neighbors = 1
    innodb_log_file_size = 4G               #根据实际情况修改
    innodb_log_buffer_size = 16777216
    innodb_purge_threads = 4
    innodb_large_prefix = 1
    innodb_thread_concurrency = 64
    innodb_print_all_deadlocks = 1
    innodb_strict_mode = 1
    innodb_sort_buffer_size = 67108864 
    ########semi sync replication settings########
    plugin_dir=/usr/local/mysql/lib/plugin      #根据实际情况修改
    plugin_load = "rpl_semi_sync_master=semisync_master.so;rpl_semi_sync_slave=semisync_slave.so"
    loose_rpl_semi_sync_master_enabled = 1
    loose_rpl_semi_sync_slave_enabled = 1
    loose_rpl_semi_sync_master_timeout = 5000
    
    [mysqld-5.7]
    innodb_buffer_pool_dump_pct = 40
    innodb_page_cleaners = 4
    innodb_undo_log_truncate = 1
    innodb_max_undo_log_size = 2G
    innodb_purge_rseg_truncate_frequency = 128
    binlog_gtid_simple_recovery=1
    log_timestamps=system
    transaction_write_set_extraction=MURMUR32
    show_compatibility_56=on
    ```


**2. 几个重要的参数配置和说明**
*  `nodb_log_file_size = 4G` ：线上环境推荐用4G，以前5.5和5.1等版本之所以官方给的值很小，是因为太大后有bug，现在bug已经修复
    
* `innodb_undo_logs = 128`和`innodb_undo_tablespaces = 3`建议在安装之前就确定好该值，后续修改比较麻烦
* `[mysqld]`，`[mysqld-5.7]`这种tag表明了下面的配置在什么版本下才生效,`[mysqld]`下均生效
* `autocommit`,这个参数在5.5.X以后才有，安装5.6.X的时候要注意先把该参数注释掉，等安装完成后，再行打开, 5.7.X无需预先注释
* `datadir`, `innodb_log_group_home_dir`, `innodb_undo_directory`一定要注意他的权限是 `mysql:mysql`

**3、my.cnf顺序问题**
* 使用`mysqld --help -vv | grep my.cnf `查看mysql的配置文件读取顺序
* 后读取的`my.cnf`中的配置，如果有相同项，会覆盖之前的配置
* 使用`--defaults-files`可指定配置文件
    
### 3.2 几种登录方式（本地socket连接【方法二、四】或TCP/IP远程连接【方法三】）
* 方式三 `mysql -h127.0.0.1 -uroot -p`
    - 127.0.0.1：本机地址（本机服务器），需要通过网卡传输，使用TCP/IP连接，用户为`'root'@'127.0.0.1'`
        
* 方式四 `mysql -hlocalhost -uroot -p` 
    - localhost:本地服务器，不需网卡传输。该方式等价与【方式二】，且和【方式三】属于两个不同的“用户”
* 方式二 `mysql -S /tmp/mysql.sock -uroot -p` 
    - 适用于在安装MySQL主机上进行本地登录
* 方式一 `mysql -p`
    - 默认使用root用户, 可使用`select user();`查看当前用户


    


### 3.3. 免密码登录
* 方式一 `my.cnf`增加`[client]`标签   
    ```bash   
    [client]   
    user="root"  
    password="你的密码"  
    ```
    
    ```bash
    #单对定义不同的客户端
    [mysql] # 这个是给/usr/loca/mysql/bin/mysql 使用的
    user=root
    password="你的密码"
    
    [mysqladmin] # 这个是给/usr/local/mysql/bin/mysqladmin使用的
    user=root
    password="你的密码"
    ```

    **每个不同的客户端需要定义不同的标签，使用`[client]`可以统一**
    
* 方式二  `login-path`
    
    ```bash
    shell> mysql_config_editor set -G vm1 -S /tmp/mysql.sock -u root -p
    Enter password [输入root的密码]
    
    shell> mysql_config_editor print --all
    [vm1]
    user=root
    password=*****
    socket=/tmp/mysql.sock
    
    #login
    shell> mysql --login-path=vm1 # 这样登录就不需要密码，且文件二进制存储 ,位置是 ~/.mylogin.cnf
    ```
    **该方式相对安全。如果server被黑了，该二进制文件还是会被破解**
        
* 方式三 `~/.my.cnf`, 自己当前家目录
    ```bash
    #Filename: ~/.my.cnf
    [client]
    user="root"
    password="你的密码"
    ```

-----
    
### 3.4. MySQL 参数介绍和设置
**1. 参数的分类**
* 全局参数：GLOBAL
    - 可修改参数
    - 不可修改参数
* 会话参数：SESSION
    - 可修改参数
    - 不可修改参数
        
> 1: 用户可在线修改`非只读参数`，`只读参数`只能预先在配置文件中进行设置，通过重启数据库实例,方可生效。  

> 2: 所有的在线修改过的参数(GLOBAL/SESSION)，在重启后，都会丢失，不会写如`my.cnf`，无法将修改进行持久化

> 3: 有些参数，即存在于`GLOBAL`又存在于`SESSION`, 比如`autocommit` (PS：MySQL默认是提交的)


**2. 查看参数**
 
```bash
mysql> show variables; # 显示当前mysql的所有参数，且无隐藏参数
mysql> show variables like "max_%"; #查以max_开头的变量
```
**3. 设置参数**
* 设置全局(GLOBAL)参数
    ```bash
    mysql> set global slow_query_log = off; #不加global，会提示错误
                                            #slow_query_log是全局参数

    mysql> set slow_query_log = off;  # 下面就报错了，默认是会话参数
    ERROR 1229 (HY000): Variable 'slow_query_log' is a GLOBAL variable and should be set with SET GLOBAL
    ```

* 设置会话(SESSION)参数
    
    ```bash
    mysql> set autocommit = 0;  # 当前会话生效
    # 或者
    mysql> set session autocommit = 0;  # 当前会话生效
    ```
    `autocommit`同样在`GLOBAL`中, 也有同样的参数
    ```bash
    mysql> set global autocommit = 1; #当前实例，全局生效
    ```
    **注意：如果这个时候/etc/init.d/mysqld restart, 则全局的autocommit的值会变成默认值，或者依赖于my.cnf的设置值。**
    
    执行的效果如下：
    ```bash
    mysql> show variables like "slow%"; # 原值为ON

    +---------------------+----------+
    | Variable_name       | Value    |
    +---------------------+----------+
    | slow_launch_time    | 2        |
    | slow_query_log      | OFF      |
    | slow_query_log_file | slow.log |
    +---------------------+----------+
    
    mysql> select @@session.autocommit; # 等价于 slect @@autocomit;
    +----------------------+
    | @@session.autocommit |
    +----------------------+
    |                    0 |
    +----------------------+

    mysql> select @@global.autocommit;       
    +---------------------+
    | @@global.autocommit |
    +---------------------+
    |                   1 |
    +---------------------+

    ```

### 附录
#### 1）mysql> show variables; //显示当前mysql的所有参数，且无隐藏参数
#### 2）set autocommit = 0;  //当前会话生效
#### 3）set global slow_query_log = off; //因为slow_query_log是全局参数
-----



# 二、day4：权限、Role模拟、workbench、MySQL体系结构
## 1、权限管理
### 1.1 “’用户‘ + @ + ’IP‘”=完整的用户标识
>`bob@127.0.0.1` 和 `bob@loalhost` 以及 `bob@192.168.1.100` 这三个其实是`不同`的 **用户标识** 

### 1.2 系统表权限信息与常用权限

#### 1.2.1 系统表权限信息:
##### 1）用户名和IP是否允许
##### 2）查看mysql.user表 【查看全局所有库的权限】
##### 3）查看mysql.db表 【查看指定库的权限】
##### 4）查看mysql.table_priv表 【查看指定表的权限】
##### 5）查看mysql.column_priv表 【查看指定列的权限】

***tips**: mysql> desc [tablename]; 可以查看表的结构信息；*
#### 1.2.2 常用权限：
##### 1）SQL语句：SELECT、INSERT、UPDATE、DELETE、INDEX
##### 2）存储过程：CREATE ROUTINE、ALTER ROUTINE、EXECUTE、TRIGGER
##### 3）管理权限：SUPER、RELOAD、SHOW DATABASE、SHUTDOWN


#### 1.2.3 可选资源:
- MAX_QUERIES_PER_HOUR *count*
- MAX_UPDATES_PER_HOUR *count*
- MAX_CONNECTIONS_PER_HOUR *count*
- MAX_USER_CONNECTIONS *count*

***tips:**只能精确到小时，对于部分场景不适用，可以考虑中间件方式*
  
### 1.3. 创建用户（create user ...）及授予权限（grant...to...）
```sql
mysql> create user 'pref'@'127.0.0.1' identified by '123';
mysql> grant select on sys.* to 'perf'@'127.0.0.1';
```

### 1.4 撤销权限(revoke...from...只删除权限，drop user ...删除用户)
* `revoke` 关键字，该关键字只删除用户权限，不删除用户
* `revoke` 语法同`grant`一致, 从`grant ... to` 变为`revoke ... from`
* **删除用户 drop user**
```sql
mysql> drop user 'perf'@'127.0.0.1';
```

### 1.5 查看某一个用户的权限（show grants for 'perf'@'127.0.0.1';）
```sql
mysql> show grants for 'perf'@'127.0.0.1';   
+-----------------------------------------------+
| Grants for perf@127.0.0.1                     |
+-----------------------------------------------+
| GRANT USAGE ON *.* TO 'perf'@'127.0.0.1'      | -- USAGE表示用户可以登录
| GRANT SELECT ON `sys`.* TO 'perf'@'127.0.0.1' | -- 对sys库的所有表有select权限
+-----------------------------------------------+
```
### 1.6 mysql库主要负责存储数据库的用户、权限设置、关键字等mysql自己需要使用的控制和管理信息
```sql
mysql> select * from mysql.user where user='perf'\G
*************************** 1. row ***************************
                  Host: 127.0.0.1
                  User: perf
           Select_priv: N   ---由于perf用户是对sys库有权限，所以这里（USER）全是N
           Insert_priv: N
           Update_priv: N
           Delete_priv: N
           Create_priv: N
             Drop_priv: N
           Reload_priv: N
         Shutdown_priv: N
          Process_priv: N
             File_priv: N
            Grant_priv: N
       References_priv: N
            Index_priv: N
            Alter_priv: N
          Show_db_priv: N
            Super_priv: N
 Create_tmp_table_priv: N
      Lock_tables_priv: N
          Execute_priv: N
       Repl_slave_priv: N
      Repl_client_priv: N
      Create_view_priv: N
        Show_view_priv: N
   Create_routine_priv: N
    Alter_routine_priv: N
      Create_user_priv: N
            Event_priv: N
          Trigger_priv: N
Create_tablespace_priv: N
              ssl_type: 
            ssl_cipher: 
           x509_issuer: 
          x509_subject: 
         max_questions: 0
           max_updates: 0
       max_connections: 0
  max_user_connections: 0
                plugin: mysql_native_password
 authentication_string: *23AE809DDACAF96AF0FD78ED04B6A265E05AA257
      password_expired: N
 password_last_changed: 2015-11-18 12:20:13
     password_lifetime: NULL
        account_locked: N    -- 如果这里为Y，该用户就无法使用了
1 row in set (0.00 sec)

------------------------我是分割线-----------------------------

mysql> select * from mysql.db where user='perf'\G    
*************************** 1. row ***************************
                 Host: 127.0.0.1
                   Db: sys  -- sys 数据库
                 User: perf
          Select_priv: Y    -- 有select权限，和我们赋予perf的权限一致
          Insert_priv: N
          Update_priv: N
          Delete_priv: N
          Create_priv: N
            Drop_priv: N
           Grant_priv: N
      References_priv: N
           Index_priv: N
           Alter_priv: N
Create_tmp_table_priv: N
     Lock_tables_priv: N
     Create_view_priv: N
       Show_view_priv: N
  Create_routine_priv: N
   Alter_routine_priv: N
         Execute_priv: N
           Event_priv: N
         Trigger_priv: N
1 row in set (0.00 sec)
```
>**注意：**
**`不建议`使用`INSERT`或者`GRANT`对元数据表进行修改，来达到修改权限的目的**

### 1.7 information_schema

```sql
mysql> select user();
+----------------+
| user()         |
+----------------+
| perf@127.0.0.1 |
+----------------+
1 row in set (0.00 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |  -- 这是一个特殊的数据库，我们虽然可以看见，但其实没有权限
| sys                |
+--------------------+
2 rows in set (0.00 sec)

mysql> use information_schema;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> select * from  INNODB_CMP;
ERROR 1227 (42000): Access denied; you need (at least one of) the PROCESS privilege(s) for this operation
```
### 1.8 附录
#### 1）desc [tablename]; #可以查看表的结构信息
#### 2）create user 'pref'@'127.0.0.1' identified by '123'; #创建用户
#### 3）grant select on sys.* to 'perf'@'127.0.0.1';  #授予权限
#### 4）revoke select on sys.* from 'perf'@'127.0.0.1'； #删除权限，不删除用户
#### 5）drop user 'perf'@'127.0.0.1'; #删除用户
#### 6）show grants for 'perf'@'127.0.0.1';  #查看用户权限 
#### 7）select user()；#查看用户
#### 8）show databases; #查看数据库
#### 9）use sys; #使用sys数据库


## 2、Role模拟
### 2.1 角色(Role)：可以用来批量管理用户，同一个角色下的用户，拥有相同的权限。
`MySQL5.7.X`以后可以模拟角色(Role)的功能
```sql
mysql> create user 'junior_dba'@'127.0.0.1';  
-- 相当于定于一个角色(Role),但这只是个普通的用户，名字比较有(Role)的感觉，有点类似用户组，都可以设置密码identified by 
mysql> create user 'tom'@'127.0.0.1';         -- 用户1

mysql> create user 'jim'@'127.0.0.1';         -- 用户2

mysql> grant proxy on 'junior_dba'@'127.0.0.1' to 'tom'@'127.0.0.1';  
-- 将junior_dba的权限映射(map)到tom，proxy是代理的意思

mysql> grant proxy on 'junior_dba'@'127.0.0.1' to 'jim'@'127.0.0.1';  
-- 然后映射(map)给jim

mysql> grant select on *.* to 'junior_dba'@'127.0.0.1';  
-- 给junior_dba（模拟的Role）赋予实际权限

mysql> show grants for 'junior_dba'@'127.0.0.1';        
-- 查看 junior_dba的权限
+-------------------------------------------------+
| Grants for junior_dba@127.0.0.1                 |
+-------------------------------------------------+
| GRANT SELECT ON *.* TO 'junior_dba'@'127.0.0.1' |
+-------------------------------------------------+


mysql> show grants for 'jim'@'127.0.0.1';              
 -- 查看jim的权限
+--------------------------------------------------------------+
| Grants for jim@127.0.0.1                                     |
+--------------------------------------------------------------+
| GRANT USAGE ON *.* TO 'jim'@'127.0.0.1'                      |
| GRANT PROXY ON 'junior_dba'@'127.0.0.1' TO 'jim'@'127.0.0.1' |
+--------------------------------------------------------------+

mysql> show grants for 'tom'@'127.0.0.1';              
 -- 查看tom的权限 
+--------------------------------------------------------------+
| Grants for tom@127.0.0.1                                     |
+--------------------------------------------------------------+
| GRANT USAGE ON *.* TO 'tom'@'127.0.0.1'                      |
| GRANT PROXY ON 'junior_dba'@'127.0.0.1' TO 'tom'@'127.0.0.1' |
+--------------------------------------------------------------+

mysql> select * from mysql.proxies_priv;    
--  查看 proxies_priv的权限
+-----------+------+--------------+--------------+------------+----------------------+---------------------+
| Host      | User | Proxied_host | Proxied_user | With_grant | Grantor              | Timestamp           |
+-----------+------+--------------+--------------+------------+----------------------+---------------------+
| localhost | root |              |              |          1 | boot@connecting host | 0000-00-00 00:00:00 |
| 127.0.0.1 | tom  | 127.0.0.1    | junior_dba   |          0 | root@localhost       | 0000-00-00 00:00:00 |
| 127.0.0.1 | jim  | 127.0.0.1    | junior_dba   |          0 | root@localhost       | 0000-00-00 00:00:00 |
+-----------+------+--------------+--------------+------------+----------------------+---------------------+

```
>**`mysql.proxies_priv`仅仅是对Role的`模拟`，和Oracle的角色还是有所不同.官方称呼为`Role like`**

### 2.2 附录
#### 1） grant proxy on 'junior_dba'@'127.0.0.1' to 'tom'@'127.0.0.1';  #将junior_dba的权限映射(map)到tom，proxy是代理的意思
#### 2） grant select on *.* to 'junior_dba'@'127.0.0.1'; # 给junior_dba（模拟的Role）赋予实际权限
#### 3） show grants for 'junior_dba'@'127.0.0.1';    #查看权限
#### 4） select * from mysql.proxies_priv;    #查看 proxies_priv的权限
-----

## 3. Workbench与Utilities介绍【略】

## 4. MySQL体系结构 
### 4.1 数据库与数据库实例
#### 4.1.1 数据库【物理操作系统文件或其他形式文件类型集合，是一个或者一组二进制文件】

* **数据库：** 物理操作系统文件或其他形式文件类型集合，（数据库文件）是一个或者一组二进制文件。在MySQL中，数据库文件可以是frm、MYD、MYI、ibd结尾的文件。当使用NDB引擎时，数据库文件可能不是操作系统上的文件，而是存放在内存中的文件，但是定义仍然不变。

#### 4.1.2 数据库实例【用来操作数据库文件的，数据库实例和数据库是一一对应的】

* **数据库实例：**用来操作数据库文件的，由数据库后台进程/线程以及一个共享区域组成(程序的概念)，共享内存可以被运行的后台进程/线程所共享。【用户对数据库的任何操作都是在数据库实例下进行的】

**`注意：MySQL中，数据库实例和数据库是一一对应的。【多实例：一个数据库对应多个实例，mysql通常不支持，oracle支持】`**

### 4.2 MySQL体系结构：单进程多线程、插件式存储引擎架构、存储引擎的对象是表


#### 4.2.1 体系结构图

* 由图可知，ＭｙＳＱＬ由以下几部分组成：
    * 连接池组件
    * 管理服务和工具组件
    * ＳＱＬ接口组件
    * 查询分析器组件
    * 优化器组件
    * 缓冲（Cache）组件
    * 插件式存储引擎   【MySQL的核心】
        * 存储引擎是基于表的，而不是数据库。【其好处是，每个存储引擎都有各自的特点，能根据具体的应用建立不同的存储引擎表。】
        * MySQL插件式的存储引擎架构提供了一系列标准的管理和服务支持，这些标准与存储引擎本身无关，可能是每个数据库系统本身都必须的，比如SQL分析器和优化器等，而存储引擎是底层物理结构的实现。
    * 物理文件<br>

![](https://github.com/zhaojing5340126/interview/blob/master/picture/mysql%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84.jpg?raw=true)<br>

#### 4.2.2 单进程多线程结构【MySQL数据库实例在系统上的表现就是一个进程】
    - （多进程程序，进程间通信开销大于多线程）。MySQL数据库实例在系统上的表现就是一个进程。


#### 4.2.3 存储引擎【存储引擎的对象是表】
    - 可以理解成文件系统，例如FAT32, NTFS, EXT4。 一个表是一个分区，引擎就是分区的文件系统
    - `存储引擎的对象就是表`
    - show tables; 可以看到每个表对应的是上面引擎(Engine)
    - 除了特殊情况，我们现在就只考虑`INNODB`




#### 4.2.4.逻辑存储结构
- **一个数据库对应一个schema**
- **一个数据库对应一个文件夹**
- **一个表对应一组文件**

    |MySQL逻辑存储结构|
    |:---------------:|
    |instance|
    |database|  
    |schema|
    |table|
    |view|

```
                                                 |--> table1 --- | view1 |
MySQL Instance -----> Database ----> Schema ---> |--> table2 --- | view2 |
                                                 |--> table3 --- | View3 |
```


>**注意：**
MySQL中一个`Database`对应一个`Schema`,之所以要有这个`schema`, 是为了兼容其他数据库
`information_schema`数据库不是文件夹，存在于内存中，在启动时创建

### 4.3 MySQL物理存储结构【主要文件：数据库配置文件、表结构定义文件、错误文件、慢查询日志、通用日志】
#### 4.3.1 MySQL配置文件my.cnf
* `datadir`
        - 存储数据二进制文件的路径
#### 4.3.2 表结构的组成
##### 1）.frm：表结构定义文件（二进制）
##### 2）.MYI：索引文件
##### 3）.MYD：数据文件
- 可以用`hexdump -c XXX.frm`查看二进制文件(意义不大)
- `show create table tablename;`
- **用mysqlfrm命令查看.frm文件 (utilities工具包)**

```bash
    shell> mysqlfrm --diagnostic /data/mysql_data/aaa/.a.frm  #可将frm文件转成create table的语句
```
    

#### 4.3.3 错误日志文件（用于定位错误）——err.log
* `log_err`
        - 建议配置成统一的名字`err.log`，方便定位错误

#### 4.3.4 慢查询日志文件（用于优化查询）——slow.log
* **将运行超过某一个时间`阈值`的SQL语句记录到文件，用于优化查询**
    - MySQL < 5.1 ：以秒为单位
    - MySQL >= 5.1 : 以毫秒为单位
    - MySQL >= 5.5 : 可以将慢查询日志记录到表
    - MySQL >= 5.6 : 以更细的粒度记录慢查询
    - MySQL >= 5.7 : 增加时区支持
* `slow_query_log_file`
        - 建议配置成统一的名字`slow.log`



* **4.1、慢查询相关参数**
    * **`slow_query_log`**
        - 是否开启慢查询日志                
    
    * **`slow_query_log_file`**
        - 慢查询日志文件名, 在`my.cnf`我们已经定义为slow.log，默认是 *机器名-slow.log*          
    
    * **`long_query_time`**
        - 制定慢查询阈值, 单位是秒，且当版本 `>=5.5.X`，支持毫秒。例如`0.5`即为`500ms`
        - `大于`该值，不包括值本身。例如该值为2，则执行时间正好`等于`2的SQL语句`不会记录`

    * **`log_queries_not_using_indexes`**
        - 将没有使用索引的SQL记录到慢查询日志 
        - 如果一开始因为数据少，查表快，耗时的SQL语句没被记录，当数据量大时，该SQL可能会执行很长时间
        - 需要测试阶段就要发现问题，减小上线后出现问题的概率

    * **`log_throttle_queries_not_using_indexes`**
        - 限制每分钟内，在慢查询日志中，去记录没有使用索引的SQL语句的次数；版本需要`>=5.6.X`
        - 因为没有使用索引的SQL可能会短时间重复执行，为了避免日志快速增大，限制每分钟的记录次数
        
    * **`min_examined_row_limit`**
        - 扫描记录少于该值的SQL不记录到慢查询日志 
        - 结合去记录没有使用索引的SQL语句的例子，有可能存在某一个表，数据量维持在百行左右，且没有建立索引。这种表即使不建立索引，查询也很快，扫描记录很小，如果确定有这种表，则可以通过此参数设置，将这个SQL不记录到慢查询日志。
    
    * **`log_slow_admin_statements`**
        - 记录超时的管理操作SQL到慢查询日志，比如ALTER/ANALYZE TABLE，否则这些操作不会被记录

    * **`log_output`**
         - 慢查询日志的格式，文件或者表，[FILE | TABLE | NONE]，默认是FILE（slow.log）；版本`>=5.5`
        - 如果设置为TABLE，则记录的到`mysql.slow_log`(在mysql这个数据库下)

    * **`log_slow_slave_statements`**
        - 在从服务器上开启慢查询日志

    * **`log_timestamps`**
        - 写入时区信息。可根据需求记录UTC时间或者服务器本地系统时间


* **4.2 慢查询日志实践**

    * 设置慢查询记录的相关参数
```sql
--
-- 终端A
--
-- 注意做实验以前，先把my.cnf中的 slow_query_log = 0, 同时将min_examined_row_limit = 100 进行注释
--
mysql> select version();
+-----------+
| version() |
+-----------+
| 5.7.9-log |
+-----------+


mysql> show variables like "slow_query_log"； -- 为了测试，特地在my.cnf中关闭了该选项
+----------------+-------+
| Variable_name  | Value |
+----------------+-------+
| slow_query_log | OFF   |
+----------------+-------+


mysql> set global slow_query_log = 1;         -- slow_query_log可以在线打开


mysql> show variables like "slow_query_log";  -- 已经打开
+----------------+-------+
| Variable_name  | Value |
+----------------+-------+
| slow_query_log | ON    |
+----------------+-------+


mysql> show variables like "long_query_time";
+-----------------+----------+
| Variable_name   | Value    |
+-----------------+----------+
| long_query_time | 2.000000 |   -- my.cnf 中该值设置为2秒
+-----------------+----------+


mysql> show variables like "min_ex%";  -- my.cnf 中已经关闭注释，所以这里为0
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| min_examined_row_limit | 0     |
+------------------------+-------+

```

* 查看慢查询日志
```bash
#
#终端B
#
[root@localhost mysql_data]# tail -f slow.log 
/usr/local/mysql/bin/mysqld, Version: 5.7.9-log (MySQL Community Server (GPL)). started with:
Tcp port: 3306  Unix socket: (null)
Time                 Id Command    Argument  #测试没有任何慢查询日志信息
```

* 进行模拟耗时操作
```sql
--
-- 终端A
--
mysql> select sleep(4);
+----------+
| sleep(4) |
+----------+
|        0 |
+----------+

```

* 最终产生慢查询日志
```bash
#
#终端B
#
[root@localhost mysql_data]# tail -f slow.log 
/usr/local/mysql/bin/mysqld, Version: 5.7.9-log (MySQL Community Server (GPL)). started with:
Tcp port: 3306  Unix socket: (null)
Time                 Id Command    Argument  #测试没有任何慢查询日志信息
# Time: 2015-11-21T07:18:10.741663+08:00
# User@Host: root[root] @ localhost []  Id:     2
# Query_time: 4.000333  Lock_time: 0.000000 Rows_sent: 1  Rows_examined: 0 

#这个就是min_examined_row_limit
#设置的意义。如my.cnf中设置该值为100
#则这条语句因为Rows_examined < 100,而不会被记录
SET timestamp=1448061490;
select sleep(4);
```

> **注意**
如果在终端A中`set global min_examined_row_limit = 100;`, 然后执行`select sleep(5);`，会发现该记录仍然被记录到慢查询日志中。原因是因为`set global min_examined_row_limit`设置的是全局变量，此次会话不生效。

>**但是我们上面`set global slow_query_log = 1；`却是在线生效的，这点有所不通**


* mysqldumpslow
    - 格式化慢查询日志，相同类型的归为一条
```bash
[root@localhost mysql_data]# mysqldumpslow  slow.log

Reading mysql slow query log from slow.log
Count: 2  Time=0.00s (0s)  Lock=0.00s (0s)  Rows=0.0 (0), 0users@0hosts
  Time: N-N-21T07:N:N.N+N:N
  # User@Host: root[root] @ localhost []  Id:     N
  # Query_time: N.N  Lock_time: N.N Rows_sent: N  Rows_examined: N
  SET timestamp=N;
  select sleep(N)

Count: 1  Time=0.00s (0s)  Lock=0.00s (0s)  Rows=0.0 (0), 0users@0hosts
  # Time: N-N-21T07:N:N.N+N:N
  # User@Host: root[root] @ localhost []  Id:     N
  # Query_time: N.N  Lock_time: N.N Rows_sent: N  Rows_examined: N
  SET timestamp=N;
  select sleep(N)
  
#######################################################################

[root@localhost mysql_data]# mysqldumpslow  --help
Usage: mysqldumpslow [ OPTS... ] [ LOGS... ]

Parse and summarize the MySQL slow query log. Options are

  --verbose    verbose
  --debug      debug
  --help       write this text to standard output

  -v           verbose
  -d           debug
  -s ORDER     what to sort by (al, at, ar, c, l, r, t), 'at' is default #根据以下某个信息来排序
                al: average lock time
                ar: average rows sent
                at: average query time
                 c: count
                 l: lock time
                 r: rows sent
                 t: query time  
  -r           reverse the sort order (largest last instead of first)  # 逆序输出
  -t NUM       just show the top n queries      # TOP(n)参数
  -a           don't abstract all numbers to N and strings to 'S'
  -n NUM       abstract numbers with at least n digits within names
  -g PATTERN   grep: only consider stmts that include this string
  -h HOSTNAME  hostname of db server for *-slow.log filename (can be wildcard),
               default is '*', i.e. match all
  -i NAME      name of server instance (if using mysql.server startup script)
  -l           don't subtract lock time from total time
```

> 如果在线上操作，不需要`mysqldumpslow`去扫整个`slow.log`， 可以去` tail -n 10000 slow.log > last_10000_slow.log`(*10000这个数字根据实际情况进行调整*),然后进行`mysqldumpslow last_10000_slow.log`

* 慢查询日志存入表
```sql
--
-- 在my.cnf 中增加 log_output = TABLE，打开slow_query_log选项，然后重启数据库实例
--
mysql> show variables like "log_output%";
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| log_output    | TABLE |
+---------------+-------+


mysql> show variables like "slow_query_log";
+----------------+-------+
| Variable_name  | Value |
+----------------+-------+
| slow_query_log | ON    |
+----------------+-------+


mysql> select * from mysql.slow_log;
+----------------------------+---------------------------+-----------------+-----------------+-----------+---------------+----+----------------+-----------+-----------+-----------------+-----------+
| start_time                 | user_host                 | query_time      | lock_time       | rows_sent | rows_examined | db | last_insert_id | insert_id | server_id | sql_text        | thread_id |
+----------------------------+---------------------------+-----------------+-----------------+-----------+---------------+----+----------------+-----------+-----------+-----------------+-----------+
| 2015-11-20 19:50:28.574677 | root[root] @ localhost [] | 00:00:04.000306 | 00:00:00.000000 |         1 |             0 |    |              0 |         0 |        11 | select sleep(4) |         3 |
+----------------------------+---------------------------+-----------------+-----------------+-----------+---------------+----+----------------+-----------+-----------+-----------------+-----------+
1 row in set (0.00 sec)

mysql> show create table mysql.slow_log;
--
-- 表结构输出省略
-- 关键一句如下：
--
ENGINE=CSV DEFAULT CHARSET=utf8 COMMENT='Slow log'  -- ENGINE=CSV 这里使用的是CSV的引擎,性能较差

-- 建议将slow_log表的存储引擎改成MyISAM
mysql> alter table mysql.slow_log engine = myisam;
ERROR 1580 (HY000): You cannot 'ALTER' a log table if logging is enabled  '-- 提示我正在记录日志中，不能转换

mysql> set global slow_query_log = 0;    -- 先停止记录日志


mysql> alter table mysql.slow_log engine = myisam;   -- 然后转换表的引擎


mysql> set global slow_query_log = 1;     -- 再开启记录日志


mysql> show create table mysql.slow_log;
--
-- 表结构输出省略
-- 关键一句如下：
--
ENGINE=MyISAM DEFAULT CHARSET=utf8 COMMENT='Slow log'  -- ENGINE 变成了MyISAM
```
> 使用`TABLE`的优势在于方便查询，但是记住当在备份的时候，不要备份慢查询日志的表，避免备份过大。
使用`FILE`也可以，需要定时清除该文件，避免单文件过大。

-----

#### 5、通用日志与审计【进行全面记录， 可用于审计`Audit`，审计插件 MariaDB Audit 】
* 当需要查找某条特定SQL语句，且该SQL语句执行较快，无法记录到slow_log中时，可以开启通用日志`generic_log`,进行全面记录， 可用于审计`Audit`
* 通用日志会记录所有操作，性能下降明显。所以如果需要审计，需要`Audit Plugin`（插件）
* 审计插件**
    * MariaDB Audit 插件
        - MySQL社区版本目前没有提供Audit的功能，企业版本提供了该功能。MariaDB 提供了开源的Audit插件，且MySQL也能使用。
### 4.4 附录
#### 1）mysqlfrm --diagnostic /data/mysql_data/aaa/.a.frm  #可将frm文件转成create table的语句
#### 2）mysql> select version(); #查看mysql版本
#### 3）set global slow_query_log = 1;  #slow_query_log可以在线打开
#### 4）[root@localhost mysql_data]# tail -f slow.log #查看慢查询日志
#### 5）[root@localhost mysql_data]# mysqldumpslow  slow.log #格式化慢查询日志，相同类型的归为一条


# 三、day5： 存储引擎

## 1、存储引擎的概念【用来处理数据库的相关CRUD操作（create、read、update、delete）】
>* 用来处理数据库的相关CRUD操作（create、read、update、delete），以下是mysql支持的存储引擎
```sql
mysql> show engines;
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| Engine             | Support | Comment                                                        | Transactions | XA   | Savepoints |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| MyISAM             | YES     | MyISAM storage engine                                          | NO           | NO   | NO         |
| CSV                | YES     | CSV storage engine                                             | NO           | NO   | NO         |
| PERFORMANCE_SCHEMA | YES     | Performance Schema                                             | NO           | NO   | NO         |
| BLACKHOLE          | YES     | /dev/null storage engine (anything you write to it disappears) | NO           | NO   | NO         |
| MRG_MYISAM         | YES     | Collection of identical MyISAM tables                          | NO           | NO   | NO         |
| InnoDB             | DEFAULT | Supports transactions, row-level locking, and foreign keys     | YES          | YES  | YES        |
| ARCHIVE            | YES     | Archive storage engine                                         | NO           | NO   | NO         |
| MEMORY             | YES     | Hash based, stored in memory, useful for temporary tables      | NO           | NO   | NO         |
| FEDERATED          | NO      | Federated MySQL storage engine                                 | NULL         | NULL | NULL       |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
9 rows in set (0.00 sec)
```
* MySQL存储引擎
    * 官方存储引擎
        - MyISAM
         - `InnoDB`  -- 推荐；其他引擎已经体停止维护和开发
        - Memory
         - Federated
        - CSV
        - Archive
    * 第三方存储引擎
        - TokuDB  -- 开源，适合插入密集型
        - InfoBright  -- 商业，开源版本有数据量限制。属于列存储，面向OLAP场景
        - Spider
>第三方存储引擎在特定场合下比较适合，除此之外，都应该使用InnoDB

## 2、各种存储引擎介绍
### 2.1、MyISAM
* MySQL5.1版本之前的默认存储引擎
* 堆表数据结构
* 表锁设计
* 支持数据静态压缩

* 不支持事务
* 数据容易丢失
* 索引容易损坏
* 唯一优点
    - 数据文件可以直接拷贝到另一台服务器使用

2.1.1. MyISAM还在使用的原因
- 历史原因，需要逐步替换
- 部分如User，DB等系统表(MyISAM引擎)，可以直接拷贝，比较方便，文件以`MY`开头的基本都是MyISAM的表
- 性能好，或者存储小`不是`MyISAM的优点，也不是存在的原因

2.1.2 MyISAM文件组成
- `frm` 表结构文件
- `MYI` 索引文件
- `MYD` 数据文件
    - 数据文件是堆表数据结构，堆是无序数据的集合
    - `MYI`中的叶子节点，指向`MYD`中的数据页
    - 当数据移动到页外时，需要修改对应指针

2.1.3. myisamchk
- **`myisamchk`通过扫描MYD文件来重建MYI文件；如果MYD文件中某条记录有问题，将跳过该记录**

-----

### 2.2 Memory存储引擎
2.2.1. Memory介绍
* 全内存存储的引擎
* 数据库重启后数据丢失
* 支持哈希索引
* 不支持事物

2.2.2 Memory特性
* **`千万不要用Memory存储引擎去做缓存(Cache)`, 性能上不及Redis和Memcahced**
* Memory`不能禁用`，当涉及内部排序操作的临时表时，使用该存储引擎
    - `max_heap_table_size`决定使用内存的大小，默认时`16M`
        - 无论该表使用的什么引擎，只要使用到临时表，或者指定Memory，都受参数影响
    - 当上面设置的内存放不下数据时，(>=5.6)转为MyISAM,(>=5.7)转为InnoDB
        - 注意磁盘上临时路径空间的大小(`tmpdir`)
    - 内存使用为会话(SESSION)级别，当心内核OOM
* 支持哈希索引，且仅支持等值查询

```sql
mysql> show global status like "%tmp%tables";
+-------------------------+-------+
| Variable_name           | Value |
+-------------------------+-------+
| Created_tmp_disk_tables | 0     |   -- 内存放不下，转成磁盘存储的数量,如果过大，考虑增大内存参数
| Created_tmp_tables      | 4     |   -- 创建临时表的数量
+-------------------------+-------+
2 rows in set (0.00 sec)

mysql> show variables like 'tmpdir';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| tmpdir        | /tmp  |  -- memory转成磁盘存储的路径
+---------------+-------+
1 row in set (0.00 sec)

mysql> show create table User\G
*************************** 1. row ***************************
Table: User
Create Table: CREATE TABLE `User` (
`id` int(11) NOT NULL,
`name` varchar(128) DEFAULT NULL,
PRIMARY KEY (`id`),
KEY `name` (`name`) USING HASH  -- 对这个字段使用USING HASH,创建hash索引
) ENGINE=MEMORY DEFAULT CHARSET=latin1
1 row in set (0.00 sec)
```

2.2.3 Memory的物理特性
* 内存不会一次性分配最大空间，而是随着使用逐步增到到最大值
* 通过链表管理空闲空间
* 使用固定长度存储数据
* 不支持BLOB和TEXT类型
* 可以创建自增主键

----

### 2.3 CSV存储引擎
2.3.1. CSV介绍
* CSV - Comma-Separated Values，使用逗号分隔
* 不支持特殊字符
* CSV是一种标准文件格式
* 文件以纯文本形式存储表格数据
* 使用广泛

2.3.2 CSV文件组成(general_log、slow_log等就是用的CSV引擎)
* `frm` 表结构
* `CSV` 数据文件
* `CSM` 元数据信息
    

2.3.3 CSV特性
* MySQL CSV存储引擎运行时，即创建CSV`文件`
* 通过MySQL标准接口来查看和修改CSV文件（其实Excel也可以打开）
* 无需将CSV文件导入到数据库，只需创建相同字段的表结构，拷贝CSV文件即可
* CSV存储引擎表每个字段`必须是NOT NULL`属性

----

### 2.4 Federated存储引擎
2.4.1. Federated介绍
* 允许本地访问远程MySQL数据库中表的数据
* 本地不存储任何数据文件
* 类似Oracle中的DBLink
* Federated存储引擎默认不开启, 需要在`my.cnf`的`[mysqld]`标签下添加 `federated`
* MySQL的Federated不支持异构数据库访问，MariaDB中的`FederatedX`支持

### 2. Federated 语法
`scheme://user_name[:password]@host_name[:port_num]/db_name/tbl_name`

`CONNECTION='mysql://username:password@hostname:port/database/tablename'`

```sql
--
-- 例子
--
CREATE TABLE `T1` (
`A` VARCHAR(100),
UNIQUE KEY (`A` (30))
) ENGINE=FEDERATED
CONNECTION='MYSQL://david:123@127.0.0.1:3306/TEST/T1';
```

-----

## 5. 多实例安装与federated测试【未】
* 1.  多实例介绍
    * 一台服务器上安装多个MySQL数据库实例
    * 可以充分利用服务器的硬件资源
    * 通过mysqld_multi进行管理<br>

![多实例安装与federated测试](https://github.com/zhaojing5340126/interview/blob/master/mysql%E9%99%84%E4%BB%B6/MySQL%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0(Day006).md)

## 附录
#### 1) show engines; //显示所有存储引擎

# 八、day8：MySQL数据类型
## 1、 INT类型
### 1.1 INT类型分类:TINYINT,SMALLINT,MEDIUMINT,INT,BIGINT
* **TINYINT**
    - 存储空间 ： 1 字节
    - 取值范围 
        - 有符号(signed) ：  [-128, 127]
        - 无符号(unsigned) ：[0, 255]
        
* **SMALLINT**
    - 存储空间 ： 2 字节
    - 取值范围 
        - 有符号(signed) ：  [-32768, 32767]
        - 无符号(unsigned) ：[0, 65535]
        
* **MEDIUMINT**
    - 存储空间 ： 3 字节
    - 取值范围 
        - 有符号(signed) ：  [-8388608, 8388607]
        - 无符号(unsigned) ：[0, 16777215]
        
* **INT**
    - 存储空间 ： 4 字节
    - 取值范围 
        - 有符号(signed) ：  [-2147483648, 2147483647]
        - 无符号(unsigned) ：[0, 4294967295]
        
* **BIGINT**
    - 存储空间 ： 8 字节
    - 取值范围 
        - 有符号(signed) ：  [-9223372036854775808, 9223372036854775807]
        - 无符号(unsigned) ：[0, 18446744073709551615]

### 1.2 INT类型的使用
#### 1.2.1 自增长ID ：推荐使用BIGINT
    - `推荐`使用`BIGINT`，而不是INT；

#### 1.2.2 unsigned or signed：推荐使用signed
- 根据实际情况使用，一般情况下推荐`默认`的`sigend`
- unsigned 的注意事项：出现负数溢出

    >一般情况下使用`int`时，推荐有符号数`(signed)`， 使用无符号数只是比原来多一倍的取值，数量级上没有改变。

    >如果需要取值范围很大，直接选择用`BIGINT`
 
### 1.2.3 INT(N) 中`N`是显示宽度，不表示存储的数字的长度的上限。

* **int(N) 和 zerofill**
    - int(**N**)中的`N`是显示宽度，`不表示`存储的数字的`长度`的上限。
    - `zerofill`表示当存储的数字`长度 < N`时，用`数字0`填充左边，直至补满长度`N`
    - 当存储数字的长度`超过N时`，按照`实际存储`的数字显示
    - int(N)中的`N`和`zerofill`配合才有意义，且仅仅是显示的时候才有意义，和实际存储没有关系，不会去截取数字的长度。
    
```sql
mysql> create  table  test_int_n(a int(3) zerofill);  -- 显示宽度N=3

mysql> insert into test_int_n values(1);

mysql> select * from test_int_n;
+------+
| a    |
+------+
|  001 |   -- 不满 N=3时，左边用0填充
+------+

mysql> insert into test_int_n values(1111);

mysql> select * from test_int_n;
+------+
| a    |
+------+
|  001 |
| 1111 |  -- 超过N=3的长度时，是什么数字，显示什么数字
+------+

mysql> select a, HEX(a) from test_int_n\G
*************************** 1. row ***************************
     a: 001
HEX(a): 1    -- 实际存储的还是1
*************************** 2. row ***************************
     a: 1111
HEX(a): 457  -- 1111对应的16进制就是457
2 rows in set (0.00 sec)
```

### 1.2.4 AUTO_INCREMENT：自增、每张表一个、必须是索引的一部分
* 自增
* 每张表一个
* 必须是索引的一部分
* 插入0和插入NULL的效果是一样的，都是代表自增

> `AUTO_INCREMENT`是实例启动时，取当前表的最大值，然后 +1 即为下次自增的值。`（MAX + 1)`

> **TIPS:**
`insert into tablename select NULL;` 等价于 `insert into tablename values (NULL);`

```sql
mysql> create table test_auto_increment(a int auto_increment);
ERROR 1075 (42000): Incorrect table definition; there can be only one auto column and it must be defined as a key
-- 没有指定为key，报错了

mysql> create table test_auto_increment(a int auto_increment primary key);  -- 指定为key后有效
Query OK, 0 rows affected (0.11 sec)

mysql> insert into test_auto_increment values(NULL);  -- 插入NULL值
Query OK, 1 row affected (0.03 sec)

mysql> select * from test_auto_increment;
+---+
| a |
+---+
| 1 |  -- 插入NULL值，便可以让其自增，且默认从1开始
+---+
1 row in set (0.00 sec)

mysql> insert into test_auto_increment values(0);  -- 插入 0
Query OK, 1 row affected (0.03 sec)

mysql> select * from test_auto_increment;
+---+
| a |
+---+
| 1 |
| 2 |  -- 插入 0 ，自增长为2
+---+
2 rows in set (0.00 sec)

mysql> insert into test_auto_increment values(-1);  -- 插入 -1
Query OK, 1 row affected (0.02 sec)

mysql> select * from test_auto_increment;
+----+
| a  |
+----+
| -1 |   -- 刚刚插入的-1
|  1 |
|  2 |
+----+
3 rows in set (0.00 sec)

mysql> insert into test_auto_increment values(NULL);  -- 继续插入NULL
Query OK, 1 row affected (0.02 sec)

mysql> select * from test_auto_increment;
+----+
| a  |
+----+
| -1 |
|  1 |
|  2 |
|  3 |  -- 刚刚插入NULL， 自增为3
+----+
4 rows in set (0.00 sec)

mysql> insert into test_auto_increment values('0'); -- 插入字符0
Query OK, 1 row affected (0.04 sec)

mysql> select * from test_auto_increment;
+----+
| a  |
+----+
| -1 |
|  1 |
|  2 |
|  3 |
|  4 |  -- 插入字符'0' 后， 自增长为4
+----+
5 rows in set (0.00 sec)

mysql> update test_auto_increment set a = 0 where a = -1;  -- 更新为0
Query OK, 1 row affected (0.03 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from test_auto_increment;
+---+
| a |
+---+
| 0 |  -- 原来的 -1 更新为0
| 1 |
| 2 |
| 3 |
| 4 |
+---+
5 rows in set (0.00 sec)

--
--  数字 0 这个值比较特殊， 插入0和插入NULL的效果是一样的，都是代表自增
--

-----

mysql> insert into test_auto_increment values(99); -- 插入99
Query OK, 1 row affected (0.02 sec)

mysql> select * from test_auto_increment;
+-----+
| a   |
+-----+
|   0 |
|   1 |
|   2 |
|   3 |
|   4 |
|   5 |
|  99 |  -- 刚刚插入的 99
| 100 |
| 101 |
+-----+
9 rows in set (0.00 sec)
```
-----
### 1.2.5 附录
#### 1) create  table  test(a int(3) zerofill);  //显示宽度N=3
#### 2) insert into test values(1); //插入
#### 3）select * from test; //显示表列

## 2. 数字类型
### 2.1 分类：float、double、decimal（精确性非常高，财务系统必须使用）
* 单精度类型：FLOAT
    - 存储空间：4 字节
    - 精确性：低
    
* 双精度类型：DOUBLE
    - 占用空间：8 字节
    - 精确性：低，比FLOAT高
    
* 高精度类型：DECIMAL
    - 占用空间：变长
    - 精确性：非常高

**注意：财务系统`必须使用DECIMAL`**
    
-----    
    
## 3. 字符串类型

### 3.1 字符串类型：定长字符CHAR(N),变长字符VARCHAR(N)、BLOB、TEXT、其他

| 类型 | 说明 | N的含义 |  是否有字符集 | 最大长度 |
|:------:|:------:|:---------:|:------------:|:---:|
| `CHAR(N)` | 定长字符 | 字符 | 是 | 255 | 
| `VARCHAR(N)` | 变长字符 | 字符 | 是 | 16384 | 
| BINARY(N) | 定长二进制字节 | 字节 | 否 | 255 | 
| VARBINARY(N) | 变长二进制字节 | 字节 | 否 | 16384 | 
| TINYBLOB(N) | 二进制大对象 | 字节 | 否 | 256 | 
| BLOB(N) | 二进制大对象 | 字节 | 否 | 16K | 
| MEDIUMBLOB(N) | 二进制大对象 | 字节 | 否 | 16M | 
| LONGBLOB(N) | 二进制大对象 | 字节 | 否 | 4G | 
| TINYTEXT(N) | 大对象 | 字节 | 是 | 256 | 
| TEXT(N) | 大对象 | 字节 | 是 | 16K | 
| MEDIUMTEXT(N) | 大对象 | 字节 | 是 | 16M | 
| LONGTEXT(N) | 大对象 | 字节 | 是 | 4G | 

    
### 3.2 CHAR(10)：可存储10~（10*字符集最大长度w）字节
* **char(N)**
    - 假设当前table的字符集的`最大长度`为`W`, 则`char(N)`的最大存储空间为 `(N * W)Byte`;假设使用`UTF-8`，则char(10)可以最小存储10个字节的字符，最大存储30个字节的字符，其实是另一种意义上的`varchar`
    - 当存储的字符数`小于N`时，尾部使用`空格`填充，并且填充最小字节的空格
    
```sql
mysql> create table test_char(a char(10));

mysql> insert into test_char values('abc');

mysql> insert into test_char values('你好吗');

mysql> insert into test_char values('大家好ab');

mysql> insert into test_char values('大家ab好');

mysql> insert into test_char values('大家ab好吗');

mysql> select a, length(a) from test_char;
+----------------+-----------+
| a              | length(a) |
+----------------+-----------+
| abc            |         3 |
| 你好吗         |         9 |
| 大家好ab       |        11 |
| 大家ab好       |        11 |
| 大家ab好吗     |        14 |
+----------------+-----------+

mysql> select a, hex(a) from test_char;
+----------------+------------------------------+
| a              | hex(a)                       |
+----------------+------------------------------+
| abc            | 616263                       |    -- 注意这里，以及下面的16进制值，一会可以对比
| 你好吗         | E4BDA0E5A5BDE59097           |
| 大家好ab       | E5A4A7E5AEB6E5A5BD6162       |
| 大家ab好       | E5A4A7E5AEB66162E5A5BD       |
| 大家ab好吗     | E5A4A7E5AEB66162E5A5BDE59097 |
+----------------+------------------------------+

mysql> select hex(' ');
+----------+
| hex(' ') |
+----------+
| 20       |   -- 注意 空格 空格对应的16进制数字是 20
+----------+
1 row in set (0.00 sec)
```

### 3.3 varchar(N)数据存储大小以输入数据的字节的实际长度为准，超过N就截去
```sql
mysql> create table test_varchar(a varchar(10));

mysql> insert into test_varchar values('abc');

mysql> insert into test_varchar values('你好吗');

mysql> insert into test_varchar values('大家好ab');

mysql> insert into test_varchar values('大家ab好');

mysql> insert into test_varchar values('大家ab好吗');

mysql> select a, hex(a) from test_varchar;
+----------------+------------------------------+
| a              | hex(a)                       |
+----------------+------------------------------+
| abc            | 616263                       |
| 你好吗         | E4BDA0E5A5BDE59097           |
| 大家好ab       | E5A4A7E5AEB6E5A5BD6162       |
| 大家ab好       | E5A4A7E5AEB66162E5A5BD       |
| 大家ab好吗     | E5A4A7E5AEB66162E5A5BDE59097 |
+----------------+------------------------------+

mysql> select a, length(a) from test_varchar;
+----------------+-----------+
| a              | length(a) |
+----------------+-----------+
| abc            |         3 |
| 你好吗         |         9 |
| 大家好ab       |        11 |
| 大家ab好       |        11 |
| 大家ab好吗     |        14 |
+----------------+-----------+
5 rows in set (0.00 sec)
```

### 3.4 插入的数据尾部带空格时:char忽略空格，varchar认同空格

```sql
mysql> insert into test_char values('好好好   ');  -- 后面有3个空格
Query OK, 1 row affected (0.03 sec)

mysql> insert into test_varchar values('好好好   '); -- 后面有3个空格
Query OK, 1 row affected (0.02 sec)

-- 
-- test_char 表
--
mysql> select a, length(a) from test_char;   
+----------------+-----------+
| a              | length(a) |
+----------------+-----------+
| abc            |         3 |
| 你好吗         |         9 |
| 大家好ab       |        11 |
| 大家ab好       |        11 |
| 大家ab好吗     |        14 |
| 好好好         |         9 |  -- 只有9个字节
+----------------+-----------+
6 rows in set (0.00 sec)

mysql> select a, hex(a) from test_char;
+----------------+------------------------------+
| a              | hex(a)                       |
+----------------+------------------------------+
| abc            | 616263                       |
| 你好吗         | E4BDA0E5A5BDE59097           |
| 大家好ab       | E5A4A7E5AEB6E5A5BD6162       |
| 大家ab好       | E5A4A7E5AEB66162E5A5BD       |
| 大家ab好吗     | E5A4A7E5AEB66162E5A5BDE59097 |
| 好好好         | E5A5BDE5A5BDE5A5BD           | -- 无填充空格
+----------------+------------------------------+
6 rows in set (0.00 sec)


--
-- test_varchar表
--
mysql> select a, length(a) from test_varchar;
+----------------+-----------+
| a              | length(a) |
+----------------+-----------+
| abc            |         3 |
| 你好吗         |         9 |
| 大家好ab       |        11 |
| 大家ab好       |        11 |
| 大家ab好吗     |        14 |
| 好好好         |        12 |  -- (好好好)9个字节 +  3个字节的空格
+----------------+-----------+
7 rows in set (0.00 sec)

mysql> select a, hex(a) from test_varchar;      
+----------------+------------------------------+
| a              | hex(a)                       |
+----------------+------------------------------+
| abc            | 616263                       |
| 你好吗         | E4BDA0E5A5BDE59097           |
| 大家好ab       | E5A4A7E5AEB6E5A5BD6162       |
| 大家ab好       | E5A4A7E5AEB66162E5A5BD       |
| 大家ab好吗     | E5A4A7E5AEB66162E5A5BDE59097 |
| 好好好         | E5A5BDE5A5BDE5A5BD202020     |  -- 后面有20 20 20 ，表示3个自己的空格
+----------------+------------------------------+
7 rows in set (0.00 sec)
```


>上面的现象无法用统一的规则进行表述，但是[官方文档](http://dev.mysql.com/doc/refman/5.7/en/innodb-physical-record.html)给出的解释是，这样的安排是为了避免索引页的碎片 

### 3.5 在BLOB和TEXT上创建索引时，必须指定索引前缀的长度
* 在BLOB和TEXT上创建索引时，必须指定索引前缀的长度
* BLOB和TEXT列不能有默认值
* BLOB和TEXT列排序时只使用该列的前max_sort_length个字节
* 【注意】：不建议在MySQL中存储大型的二进制数据，比如歌曲，视频
```sql
mysql> create table test_text(a int primary key, b text, key(b));
ERROR 1170 (42000): BLOB/TEXT column 'b' used in key specification without a key length

mysql> create table test_text(a int primary key, b text, key(b(64)));
```

```sql
mysql> select @@max_sort_length;
+-------------------+
| @@max_sort_length |
+-------------------+
|              1024 |
+-------------------+
1 row in set (0.00 sec)
```
-----
### 3.6 附录
#### 1）mysql> select a, length(a) from test_char;  //
#### 2）select a, hex(a) from test_char;    //十六进制显示
#### 3）create table text(a int primary key, b text, key(b(64))); //64为索引前缀长度

## 4. 字符集
### 4.1 常见字符集：utf8mb4(推荐),utf8,gbk,gb18030 
* utf8：最长3字节
* utf8mb4：utf8 + mobile端字符【推荐】，扩展了utf8
* gbk：表示的字符有限
* gb18030：最长4个字节

```sql
mysql> show character set;
+----------+---------------------------------+---------------------+--------+
| Charset  | Description                     | Default collation   | Maxlen |
+----------+---------------------------------+---------------------+--------+
| big5     | Big5 Traditional Chinese        | big5_chinese_ci     |      2 |
| dec8     | DEC West European               | dec8_swedish_ci     |      1 |
| cp850    | DOS West European               | cp850_general_ci    |      1 |
| hp8      | HP West European                | hp8_english_ci      |      1 |
| koi8r    | KOI8-R Relcom Russian           | koi8r_general_ci    |      1 |
| latin1   | cp1252 West European            | latin1_swedish_ci   |      1 |
| latin2   | ISO 8859-2 Central European     | latin2_general_ci   |      1 |
| swe7     | 7bit Swedish                    | swe7_swedish_ci     |      1 |
| ascii    | US ASCII                        | ascii_general_ci    |      1 |
| ujis     | EUC-JP Japanese                 | ujis_japanese_ci    |      3 |
| sjis     | Shift-JIS Japanese              | sjis_japanese_ci    |      2 |
| hebrew   | ISO 8859-8 Hebrew               | hebrew_general_ci   |      1 |
| tis620   | TIS620 Thai                     | tis620_thai_ci      |      1 |
| euckr    | EUC-KR Korean                   | euckr_korean_ci     |      2 |
| koi8u    | KOI8-U Ukrainian                | koi8u_general_ci    |      1 |
| gb2312   | GB2312 Simplified Chinese       | gb2312_chinese_ci   |      2 |
| greek    | ISO 8859-7 Greek                | greek_general_ci    |      1 |
| cp1250   | Windows Central European        | cp1250_general_ci   |      1 |
| gbk      | GBK Simplified Chinese          | gbk_chinese_ci      |      2 | -- gbk，表示的字符有限
| latin5   | ISO 8859-9 Turkish              | latin5_turkish_ci   |      1 |
| armscii8 | ARMSCII-8 Armenian              | armscii8_general_ci |      1 |
| utf8     | UTF-8 Unicode                   | utf8_general_ci     |      3 | -- utf8，最长3字节
| ucs2     | UCS-2 Unicode                   | ucs2_general_ci     |      2 |
| cp866    | DOS Russian                     | cp866_general_ci    |      1 |
| keybcs2  | DOS Kamenicky Czech-Slovak      | keybcs2_general_ci  |      1 |
| macce    | Mac Central European            | macce_general_ci    |      1 |
| macroman | Mac West European               | macroman_general_ci |      1 |
| cp852    | DOS Central European            | cp852_general_ci    |      1 |
| latin7   | ISO 8859-13 Baltic              | latin7_general_ci   |      1 |
| utf8mb4  | UTF-8 Unicode                   | utf8mb4_general_ci  |      4 | -- utf8 + mobile端字符
| cp1251   | Windows Cyrillic                | cp1251_general_ci   |      1 |
| utf16    | UTF-16 Unicode                  | utf16_general_ci    |      4 |
| utf16le  | UTF-16LE Unicode                | utf16le_general_ci  |      4 |
| cp1256   | Windows Arabic                  | cp1256_general_ci   |      1 |
| cp1257   | Windows Baltic                  | cp1257_general_ci   |      1 |
| utf32    | UTF-32 Unicode                  | utf32_general_ci    |      4 |
| binary   | Binary pseudo charset           | binary              |      1 |
| geostd8  | GEOSTD8 Georgian                | geostd8_general_ci  |      1 |
| cp932    | SJIS for Windows Japanese       | cp932_japanese_ci   |      2 |
| eucjpms  | UJIS for Windows Japanese       | eucjpms_japanese_ci |      3 |
| gb18030  | China National Standard GB18030 | gb18030_chinese_ci  |      4 | -- gb18030,最长4个字节
+----------+---------------------------------+---------------------+--------+
41 rows in set (0.00 sec)
```
### 4.2 字符集的指定：可以在my.cnf、创建数据库、创建表、创建列的时候进行指定
* my.cnf：character_set_server=utf8mb4
* create database aaa charset gbk;
### 4.2 排序规则collation，默认ci（case insensitive），即不区分大小写
#### 4.2.1 ci的意义：当有用户叫Tom，你不希望再来一个tom
* collation的含义是指排序规则，`ci（case insensitive）`结尾的排序集是不区分大小写的
    * 应用场景：从业务的角度上看，可以很好理解，比如创建一个用户叫做Tom，你是不希望再创建一个叫做tom的用户的

#### 4.2.2 修改默认的collation：当前会话有效
```sql
mysql> set names utf8mb4 collate utf8mb4_bin;  -- 当前会话有效
Query OK, 0 rows affected (0.00 sec)

mysql> select 'a' = 'A';
+-----------+
| 'a' = 'A' |
+-----------+
|         0 |
+-----------+
1 row in set (0.00 sec)
```
### 4.3 附录
#### 1）my.cnf：character_set_server=utf8b4
#### 2）show character set; //显示所有字符集
#### 3）create database aaa charset gbk；//指定字符集
#### 4) set names utf8mb4 collate utf8mb4_bin;  //修改默认排序规则，当前会话有效
-----

## 5. 集合类型ENUM、SET
### 5.1 通过sql_mode参数可以用户约束检查：强烈建议设置为严格模式
* 集合类型ENUM 和 SET
* ENUM类型最多允许65536个值
* SET类型最多允许64个值
* 通过sql_mode参数可以用户约束检查：my.cnf里有
    * 不是你预定的集合内的值会警告，设置为严格模式则会直接报错

```sql
mysql> create table test_col (
    -> user varchar(10),
    -> sex enum('male', 'female')  -- 虽然写的是字符串，单其实存储的整型，效率还是可以的
    -> );
    
mysql> insert into test_col values ("tom", "male");
Query OK, 1 row affected (0.02 sec)

mysql> insert into test_col values ("tom", "xmale");  -- 不是male 和 female
Query OK, 1 row affected, 1 warning (0.03 sec)  -- 有warning

mysql> set sql_mode='strict_trans_tables';  -- 设置为严格模式
Query OK, 0 rows affected, 2 warnings (0.00 sec)

mysql> insert into test_col values ("tom", "xmale");
ERROR 1265 (01000): Data truncated for column 'sex' at row 1
```
>强烈建议新业务上都设置成严格模式
### 5.2 附录
#### 1）create table test_col (sex enum('male', 'female')) 
#### 2) set sql_mode='strict_trans_tables';  //设置为严格模式

----

## 6. 日期类型
### 6.1 TIMESTAMP(带时区)、`DATETIME`、DATE、YEAR、TIME

| 日期类型 | 占用空间 | 表示范围 |
|----------|----------|----------|
| `DATETIME` | 8 | 1000-01-01 00:00:00 ~ 9999-12-31 23:59:59 |
| DATE| 3 | 1000-01-01 ~ 9999-12-31 |
| `TIMESTAMP` | 4 | 1970-01-01 00:00:00UTC ~ 2038-01-19 03:14:07UTC |
| YEAR | 1 | YEAR(2):1970-2070, YEAR(4):1901-2155 |
| TIME | 3 | -838:59:59 ~ 838:59:59 |

>TIMESTAMP 带时区功能

```sql
mysql> create table test_time(a timestamp, b datetime);
Query OK, 0 rows affected (0.12 sec)

mysql> insert into test_time values (now(), now());
Query OK, 1 row affected (0.03 sec)

mysql> select * from test_time;
+---------------------+---------------------+
| a                   | b                   |
+---------------------+---------------------+
| 2015-11-28 10:00:39 | 2015-11-28 10:00:39 |
+---------------------+---------------------+
1 row in set (0.00 sec)

mysql> select @@time_zone; 
+-------------+
| @@time_zone |
+-------------+
| SYSTEM      |
+-------------+
1 row in set (0.00 sec)

mysql> set time_zone='+00:00'; //设置时区
Query OK, 0 rows affected (0.00 sec)

mysql> select @@time_zone;
+-------------+
| @@time_zone |
+-------------+
| +00:00      |
+-------------+
1 row in set (0.00 sec)

mysql> select * from test_time;
+---------------------+---------------------+
| a                   | b                   |
+---------------------+---------------------+
| 2015-11-28 2:00:39 | 2015-11-28 10:00:39  |  -- 时区的差别体现出来了
+---------------------+---------------------+
1 row in set (0.00 sec)
```
### 6.2 附录
#### 1）create table test_time(a timestamp, b datetime);
#### 2）insert into test_time values (now(), now()); //插入当前时间
#### 3）select @@time_zone; //显示电脑时区
#### 4）set time_zone='+00:00'; //设置时区



# 九、 day9：JSON数据类型——5.7支持

## 1. JSON：是一种轻量级的数据交换语言，并且是独立于语言的文本格式。 Key : Value 格式

* JSON（JavaScript Object Notation）是一种轻量级的数据交换语言，并且是独立于语言的文本格式。一些NoSQL数据库选择JSON作为其数据存储格式，比如：MongoDB、CouchDB等。MySQL5.7.x开始支持JSON数据类型。

* JSON VS BLOB
    * JSON
        * JSON数据可以做有效性检查;
        * JSON使得查询性能提升;
        * JSON支持部分属性索引，通过虚拟列的功能可以对JSON中的部分数据进行索引;
    * BLOB
        * BLOB类型无法在数据库层做约束性检查;
        * BLOB进行查询，需要遍历所有字符串;
        * BLOB做只能做指定长度的索引;
    * 5.7之前，只能把JSON当作BLOB进行存储。数据库层面无法对JSON数据做一些操作，只能由应用程序处理。

## 2.结构化：二维表结构（行和列），使用SQL语句进行操作
```mysql
//SQL创建User表

create table user (
    id bigint not null auto_increment,
    user_name varchar(10),
    age int,
    primary key(id)
);
```
## 3.非结构化：使用Key-Value格式定义数据，无结构定义；Value可以嵌套Key-Value格式的数据；使用JSON进行实现
```mysql
//JSON定义的User表

db.user.insert({
    user_name:"tom",
    age:30
})

db.createCollection("user")
```
## 4. JSON操作示例

### 4.1 JSON入门
```mysql
//创建带json字段的表
mysql> create table user (
    -> uid int auto_increment,
    -> data json,
    -> primary key(uid)
    -> );

//插入json数据
mysql> insert into user values (
    -> null,  -- 自增长数据，可以插入null
    -> '{
    '> "name":"tom",
    '> "age":18,
    '> "address":"SZ"
    '> }'
    -> );

mysql> insert into user values (
    -> null,
    -> '{
    '> "name":"jim",
    '> "age":28,
    '> "mail":"jim@163.com"
    '> }'
    -> );

mysql> insert into user values ( null, "can you insert it?");  -- 无法插入，因为是JSON类型
ERROR 3140 (22032): Invalid JSON text: "Invalid value." at position 0 in value (or column) can you insert it?.  -- 这短话有单引号，但是渲染有问题，所以这里去掉了

mysql> select * from user;
+-----+---------------------------------------------------+
| uid | data                                              |
+-----+---------------------------------------------------+
|   1 | {"age": 18, "name": "tom", "address": "SZ"}       |  -- 这个json中有address字段
|   2 | {"age": 28, "mail": "jim@163.com", "name": "jim"} |  -- 这个json中有mail字段
+-----+---------------------------------------------------+
2 rows in set (0.00 sec)
```


## 4.2 JSON函数
```mysql
drop table if exists User; //如果存在就删除

CREATE TABLE User (     //SQL创建表
    uid BIGINT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(32) NOT NULL,
    email VARCHAR(256) NOT NULL,
    address VARCHAR(512) NOT NULL,
    UNIQUE KEY (name),
    UNIQUE KEY (email)
);

INSERT INTO User VALUES (NULL,'David','david@gmail','Shanghai ...');    //SQL插入数据
INSERT INTO User VALUES (NULL,'Amy','amy@gmail','Beijing ...');
INSERT INTO User VALUES (NULL,'Tom','tom@gmail','Guangzhou ...');

SELECT * FROM User;      //显示数据

ALTER TABLE User ADD COLUMN address2 VARCHAR(512) NOT NULL; //***SQL想新增列需要ALTER TABLE
ALTER TABLE User ADD COLUMN passport VARCHAR(64) NOT NULL;

DROP TABLE IF EXISTS UserJson;

CREATE TABLE UserJson(      //创建带有json列的表
	uid BIGINT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    data JSON
);

truncate table UserJson;

insert into UserJson 
SELECT 
    uid,JSON_OBJECT('name',name,'email',email,'address',address) AS data    //json_object 将list(K-V对)封装成json格式
FROM
    User;

//json_object 将list(K-V对)封装成json格式
mysql> select json_object("name", "jery", "email", "jery@163.com", "age",33);
+----------------------------------------------------------------+
| json_object("name", "jery", "email", "jery@163.com", "age",33) |
+----------------------------------------------------------------+
| {"age": 33, "name": "jery", "email": "jery@163.com"}           |  -- 封装成了K-V对
+----------------------------------------------------------------+
    
SELECT * FROM UserJson; 

SELECT uid,JSON_EXTRACT(data,'$.address2') from UserJson; //使用json_extract提取数据

//使用json_extract提取数据
mysql> select json_extract('[10, 20, [30, 40]]', '$[1]');                  
+--------------------------------------------+
| json_extract('[10, 20, [30, 40]]', '$[1]') |
+--------------------------------------------+
| 20                                         |  -- 从list中抽取 下标 为1的元素（下标从0开始）
+--------------------------------------------+

    
UPDATE UserJson 
set data = json_insert(data,"$.address2","HangZhou ...")  //json_insert 插入数据,在address2这个字段
where uid = 1;

SELECT JSON_EXTRACT(data,'$.address[1]') from UserJson;

select json_merge(JSON_EXTRACT(data,'$.address') ,JSON_EXTRACT(data,'$.address2')) //json_merge 合并数据并返回。注意：原数据不受影响
from UserJson;

//json_merge 合并数据并返回。注意：原数据不受影响
mysql> select json_merge('{"name": "x"}', '{"id": 47}');    -- 原来有两个JSON             
+-------------------------------------------+
| json_merge('{"name": "x"}', '{"id": 47}') |
+-------------------------------------------+
| {"id": 47, "name": "x"}                   |  -- 合并多个JSON
+-------------------------------------------+

begin;
UPDATE UserJson
set data = json_array_append(data,"$.address",JSON_EXTRACT(data,'$.address2'))
where JSON_EXTRACT(data,'$.address2') IS NOT NULL AND uid >0;

//json_array_append 追加数据
//json_append 在5.7.9 中重命名为 json_array_append
mysql> set @j = '["a", ["b", "c"], "d"]';     -- 下标为1的元素中只有["b", "c"]

mysql> select json_array_append(@j, '$[1]', 1);                       
+----------------------------------+
| json_array_append(@j, '$[1]', 1) |
+----------------------------------+
| ["a", ["b", "c", 1], "d"]        |    --  现在插入了 数字 1
+----------------------------------+


select JSON_EXTRACT(data,'$.address') from UserJson;

UPDATE UserJson
set data = JSON_REMOVE(data,'$.address2')
where uid>0;

//json_remove 从json记录中删除数据
mysql> set @j = '["a", ["b", "c"], "d"]';   

mysql> select json_remove(@j, '$[1]');
+-------------------------+
| json_remove(@j, '$[1]') |
+-------------------------+
| ["a", "d"]              |  -- 删除了下标为1的元素["b", "c"]
+-------------------------+

commit;
```
### 4.4附录

#### 4.4.1 create table user ( uid int auto_increment, data json，primary key(uid) ); //创建带json字段的表
#### 4.4.2 insert into user values (null[-- 自增长数据，可以插入null], '{ "name":"tom","age":18, "address":"SZ" }');//插入json数据
#### 4.4.3 drop table if exists User; //如果存在就删除
#### 4.4.4 ALTER TABLE User ADD COLUMN address2 VARCHAR(512) NOT NULL; //***SQL想新增列需要ALTER TABLE
#### 4.4.5 truncate table UserJson;
#### 4.4.6 select json_object("name", "jery", "email", "jery@163.com", "age",33); //json_object 将list(K-V对)封装成json格式
#### 4.4.7 SELECT uid,JSON_EXTRACT(data,'$.address2') from UserJson; //使用json_extract提取数据
#### 4.4.8 UPDATE UserJson set data = json_insert(data,"$.address2","HangZhou ...") where uid = 1; //json_insert 插入数据,在address2这个字段
#### 4.4.9 select json_merge(JSON_EXTRACT(data,'$.address') ,JSON_EXTRACT(data,'$.address2')) //json_merge from UserJson;合并数据并返回。原数据不受影响
#### 4.4.10 UPDATE UserJson set data = json_array_append(data,"$.address",JSON_EXTRACT(data,'$.address2')) where JSON_EXTRACT(data,'$.address2') IS NOT NULL AND uid >0; //json_array_append 追加数据

#### 4.4.11 UPDATE UserJson set data = JSON_REMOVE(data,'$.address2') where uid>0; //json_remove 从json记录中删除数据


# 十、day10：Employees 临时表的创建、外键约束
## 1.`Employees`数据库是一个用于学习和测试的数据库，大约`160MB`，`4百万条记录`

## 2. 表(TABLE)
### 2.1. 表的介绍
#### 1） 表是关系数据库的核心，表=关系
#### 2） 表是记录的集合：集合是无序的
```sql
select * from table_name limit 1;  
```
集合是无序的，上面的SQL语句的意思是 **从表(集合)中`随机`选出一条数据，结果是不确定的**, 不能简单的认为是取出第一条数据。

```sql
select * from table_name order by col_name limit 1;
```
只有通过`order by`排序之后取出的数据，才是确定的。
#### 3） 二维表格模型易于人的理解
#### 4） MySQL默认存储引擎都是基于行(记录)存储
#### 5） 每行记录都是基于列进行组织的

### 2.2 临时表 create temporary table temp_a(a int);
#### 1) 临时表是`SESSION`级别的, 当前用户logout或者其他用户登录上来，是无法看到这张表的
#### 2) 当临时表和普通表同名时，当前用户只能看到同名的临时表**
#### 3) 创建表时带上`if not exists`进行表的存在性检查；同时建议在临时表的表名前面加上统一的词头：防止创建和普通表同名的临时表
#### 4) 临时表的主要的作用：是给当前登录的用户存储临时数据或者临时结果的。不要和SQL优化器在排序过程中内部帮你创建的临时表相混淆。


```sql
mysql> create temporary table temp_a(a int);

mysql> show  create table temp_a\G
*************************** 1. row ***************************
       Table: temp_a
Create Table: CREATE TEMPORARY TABLE `temp_a` (
  `a` int(11) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4   -- 使用的是innodb 
1 row in set (0.00 sec)

mysql> show processlist\G  //查看有几个mysql进程在执行
*************************** 1. row ***************************
     Id: 10  -- 当前ID 是 10
   User: root
   Host: localhost
     db: burn_test
Command: Query
   Time: 0
  State: starting
   Info: show processlist   -- 当前终端执行
*************************** 2. row ***************************
     Id: 12
   User: root
   Host: localhost
     db: NULL
Command: Sleep
   Time: 328
  State: 
   Info: NULL
*************************** 3. row ***************************
     Id: 13
   User: root
   Host: localhost
     db: burn_test
Command: Sleep
   Time: 16
  State: 
   Info: NULL
3 rows in set (0.00 sec)


mysql> create temporary table if not exists table_name  (a int);  -- 使用if not exists进行判断
```


#### 5) 临时表默认存储引擎是 InnoDB [ 5.7以后]
#### 6) 临时表存储位置：MySQL5.7.9 把临时`表结构`放在`tmpdir`，而数据`表数据`放在`datadir`
```bash
#
# MySQL 5.7
#
mysql> system ls -l /tmp  # 使用system 可以解析执行linux shell命令
total 20
drwxr-xr-x. 4 mysql mysql 4096 Dec  2 10:06 mysql_data
srwxrwxrwx. 1 mysql mysql    0 Dec  2 21:20 mysql.sock_56
srwxrwxrwx. 1 mysql mysql    0 Dec  2 20:51 mysql.sock_57
-rw-------. 1 mysql mysql    5 Dec  2 20:51 mysql.sock_57.lock
-rw-r-----. 1 mysql mysql 8554 Dec  2 22:04 #sqlf18_a_0.frm  -- temp_1 的表结构

mysql> system ls -l /data/mysql_data/5.7/ | grep ib
-rw-r-----. 1 mysql mysql       879 Dec  2 20:47 ib_buffer_pool
-rw-r-----. 1 mysql mysql  12582912 Dec  2 22:21 ibdata1
-rw-r-----. 1 mysql mysql 134217728 Dec  2 22:20 ib_logfile0
-rw-r-----. 1 mysql mysql 134217728 Dec  2 21:33 ib_logfile1
-rw-r-----. 1 mysql mysql  12582912 Dec  2 22:33 ibtmp1  -- 这个是我们的表结构对应的数据

mysql> show variables like "innodb_temp%";
+----------------------------+-----------------------+
| Variable_name              | Value                 |
+----------------------------+-----------------------+
| innodb_temp_data_file_path | ibtmp1:12M:autoextend |
+----------------------------+-----------------------+
1 row in set (0.00 sec)

```


### 2.3 查看表结构
### 1) show create table test_1\G   -- 表结构
### 2) desc test_1\G  -- 表的描述,描述二维表信息
### 3) show table status like "test_1"\G    -- 看表结构的元数据信息
 
```sql
mysql> show create table test_1\G   -- 表结构
*************************** 1. row ***************************
       Table: test_1
Create Table: CREATE TABLE `test_1` (
  `a` int(11) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4
1 row in set (0.00 sec)

mysql> desc test_1\G  -- 表的描述,描述二维表信息
*************************** 1. row ***************************
  Field: a
   Type: int(11)
   Null: YES
    Key: 
Default: NULLdes
  Extra: 
1 row in set (0.00 sec)

mysql> show table status like "test_1"\G    -- 看表结构的元数据信息
*************************** 1. row ***************************
           Name: test_1
         Engine: InnoDB
        Version: 10
     Row_format: Dynamic
           Rows: 2
 Avg_row_length: 4096
    Data_length: 8192
Max_data_length: 0
   Index_length: 0
      Data_free: 0
 Auto_increment: NULL
    Create_time: 2015-12-02 22:20:19
    Update_time: 2015-12-02 22:20:44
     Check_time: NULL
      Collation: utf8mb4_general_ci
       Checksum: NULL
 Create_options: 
        Comment: 
1 row in set (0.00 sec)

```

### 2.4  ALTER TABLE ：当表记录很大的时候，`alter table`会很耗时，影响性能
#### 2.5 alter table test_1 add column b char(10);  -- 添加列 b
#### 2.6 alter table test_1 drop column b;  -- 删除列 b
```sql
mysql> alter table test_1 add column b char(10);  -- 添加列 b

mysql> select * from test_1;
+------+------+
| a    | b    |
+------+------+
|   23 | NULL |
|   24 | NULL |
+------+------+


mysql> alter table test_1 drop column b;  -- 删除列 b

mysql> select * from test_1;
+------+
| a    |
+------+
|   23 |
|   24 |
+------+

```
>注意，当表记录很大的时候，`alter table`会很耗时，影响性能
-----

## 3 外键约束：可以让数据进行一致性更新，但是会有一定的`性能损耗`，线上业务使用不多。通常下述级联更新和删除都是由应用层业务逻辑进行判断并实现，而非使用外键
### 3.1 外键约束三种形式：【合理模式：删除父表，子表置空；更新父表，子表做级联操作】（on delete set null on update cascade）
#### 1） 严格模式(默认)：restrict,父表不能删除或更新一个已经被子表数据引用的记录
#### 2)  级联模式：cascade，父表的操作，对应子表关联的数据也跟着操作
#### 3） 置空模式：set null，父表被操作后，子表对应的外键字段被置空。
### 3.2 外键约束的操作【maybe，外键相当于一个链接，存放在子表，链接指向父表】
#### 1) create table child (id int, parent_id INT,index(parent_id),foreign key (parent_id) references parent(id) on delete cascade on update cascade ) engine=innodb; //创建外键约束，child表的parent_id和parent表的id关联在一起了，innodb强制外键使用索引
#### 2)  update parent set id=100 where id=1;   //级联更新级联删除，父表parent的id变为1，子表child的parent_id也会变为1
#### 3）alter table child drop foreign key child_ibfk_1;  -- 删除 之前的外键，外键名字可以通过show create table 看到
#### 4）  alter table child add foreign key(parent_id) references parent(id) on update cascade on delete restrict;  -- 添加外键，使用严格模式

```sql

mysql> create table parent (
    ->     id int not null,
    ->     primary key (id)
    -> ) engine=innodb;

mysql> create table child (
    ->     id int, 
    ->     parent_id INT,
    ->     index(parent_id),
    ->     foreign key (parent_id) 
    ->         references parent(id)
    ->         on delete cascade on update cascade  -- 比官网例子增加 update cascade 
    -> ) engine=innodb;


mysql> insert into child values(1,1);  -- 我们插入一条数据，id=1，parent_id=1
ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails (`burn_test`.`child`, CONSTRAINT `child_ibfk_1` FOREIGN KEY (`parent_id`) REFERENCES `parent` (`id`) ON DELETE CASCADE)  
-- 直接报错了，因为此时parent表中没有任何记录

mysql> insert into parent values(1);   -- 现在parent中插入记录
Query OK, 1 row affected (0.03 sec)

mysql> insert into child values(1,1);  -- 然后在child中插入记录，且parent_id是在parent中存在的
Query OK, 1 row affected (0.02 sec)

mysql> insert into child values(1,2);  -- 插入parent_id=2的记录，报错。因为此时parent_id=2的记录不存在
ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails (`burn_test`.`child`, CONSTRAINT `child_ibfk_1` FOREIGN KEY (`parent_id`) REFERENCES `parent` (`id`) ON DELETE CASCADE)

mysql> select * from  child;     
+------+-----------+
| id   | parent_id |
+------+-----------+
|    1 |         1 |  -- parent_id = 1
+------+-----------+
1 row in set (0.00 sec)

mysql> select * from  parent;
+----+
| id |
+----+
|  1 |  -- 根据表结构的定义（Foreign_key），这个值就是 child表中的id
+----+
1 row in set (0.00 sec)

mysql> update parent set id=100 where id=1;       

mysql> select * from parent;
+-----+
| id  |
+-----+
| 100 |  -- 已经设置成了100
+-----+

mysql> select * from child;
+------+-----------+
| id   | parent_id |
+------+-----------+
|    1 |       100 |  -- 自动变化，这是on update cascade的作用，联级更新，parent更新，child也跟着更新
+------+-----------+


mysql> delete from parent where id=100;  -- 删除这条记录

mysql> select * from parent;  -- id=100的记录已经被删除了
Empty set (0.00 sec)

mysql> select * from child;  -- id=1，parent_id=100的记录跟着被删除了。on delete cascade的作用
Empty set (0.00 sec)

mysql> alter table child drop foreign key child_ibfk_1;  -- 删除 之前的外键，外键名字可以通过show create table 看到
Query OK, 0 rows affected (0.07 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> alter table child add foreign key(parent_id) 
    -> references parent(id) on update cascade on delete restrict;  -- 使用严格模式

mysql> insert into parent values(50);

mysql> insert into child values(3,50); 

mysql> insert into child values(3,51);  -- 和之前一样会提示错误，因为父表不存在51
ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails (`burn_test`.`child`, CONSTRAINT `child_ibfk_1` FOREIGN KEY (`parent_id`) REFERENCES `parent` (`id`) ON UPDATE CASCADE)

mysql> delete from parent where id=50;  -- 删除失败了，因为是restrict模式：父表不能删除或更新一个已经被子表数据引用的记录
ERROR 1451 (23000): Cannot delete or update a parent row: a foreign key constraint fails (`burn_test`.`child`, CONSTRAINT `child_ibfk_1` FOREIGN KEY (`parent_id`) REFERENCES `parent` (`id`) ON UPDATE CASCADE)

-- 注意，delete 后面说明都不写表示 no action == restrict
```

# 十一、day11：SQL语法之SELECT
## 1. SELECT语法介绍

>[SELECT语法官方文档](http://dev.mysql.com/doc/refman/5.7/en/select.html)

```sql
SELECT
-- -------------------------不推荐使用--------------------------
    [ALL | DISTINCT | DISTINCTROW ]
      [HIGH_PRIORITY]
      [MAX_STATEMENT_TIME = N]
      [STRAIGHT_JOIN]
      [SQL_SMALL_RESULT] [SQL_BIG_RESULT] [SQL_BUFFER_RESULT]
      [SQL_CACHE | SQL_NO_CACHE] [SQL_CALC_FOUND_ROWS]
-- -------------------------------------------------------------
    select_expr [, select_expr ...]
    [FROM table_references  
      [PARTITION partition_list]
    [WHERE where_condition] 
    [GROUP BY {col_name | expr | position}  
      [ASC | DESC], ... [WITH ROLLUP]] 
    [HAVING where_condition] 
    [ORDER BY {col_name | expr | position} 
      [ASC | DESC], ...]
    [LIMIT {[offset,] row_count | row_count OFFSET offset}]  
    [PROCEDURE procedure_name(argument_list)]
    [INTO OUTFILE 'file_name'
        [CHARACTER SET charset_name]
        export_options
      | INTO DUMPFILE 'file_name'
      | INTO var_name [, var_name]]
    [FOR UPDATE | LOCK IN SHARE MODE]]
```

## 2. LIMIT 和 ORDER BY
### 2.1 order by col_name ：`ORDER BY` 是把已经查询好的结果集进行排序， asc ：升序(默认), desc：降序。
#### 1）emp_no 是主键，order by 主键 不会创建临时表的，主键(索引)本身有序
#### 2）select * from employees order by emp_no asc limit 5,5;  -- 从第5条 开始取，取5条出来
#### 3）select * from employees where emp_no > 20000 order by emp_no limit 5; --取出5条
```sql
mysql> select * from employees limit 1;  -- 从employees中 随机 取出一条数据，结果是不确定的

-- order by col_name 根据某列的值进行排序
-- asc ： 升序(default)
-- desc： 降序

mysql> show create table employees\G
*************************** 1. row ***************************
       Table: employees
Create Table: CREATE TABLE `employees` (
  `emp_no` int(11) NOT NULL,
  `birth_date` date NOT NULL,
  `first_name` varchar(14) NOT NULL,
  `last_name` varchar(16) NOT NULL,
  `gender` enum('M','F') NOT NULL,
  `hire_date` date NOT NULL,
  PRIMARY KEY (`emp_no`)    -- emp_no 是主键，order by 主键 不会创建临时表的，主键(索引)本身有序
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4
1 row in set (0.00 sec)

mysql> select * from employees order by emp_no asc limit 5,5;  -- limit start, offset
                                                               -- 从第5条 开始取，取5条出来
+--------+------------+------------+-----------+--------+------------+
| emp_no | birth_date | first_name | last_name | gender | hire_date  |
+--------+------------+------------+-----------+--------+------------+
|  10006 | 1953-04-20 | Anneke     | Preusig   | F      | 1989-06-02 |
|  10007 | 1957-05-23 | Tzvetan    | Zielinski | F      | 1989-02-10 |
|  10008 | 1958-02-19 | Saniya     | Kalloufi  | M      | 1994-09-15 |
|  10009 | 1952-04-19 | Sumant     | Peac      | F      | 1985-02-18 |
|  10010 | 1963-06-01 | Duangkaew  | Piveteau  | F      | 1989-08-24 |
+--------+------------+------------+-----------+--------+------------+
5 rows in set (0.00 sec)

-- 以上这个语法有一种分页的效果，但是会随着start的增加，性能会下降，因为会扫描表(从 1 到 start)

-- 相对比较推荐的方法
mysql> select * from employees where emp_no > 20000 order by emp_no limit 5;
+--------+------------+------------+-----------+--------+------------+
| emp_no | birth_date | first_name | last_name | gender | hire_date  |
+--------+------------+------------+-----------+--------+------------+
|  20001 | 1962-05-16 | Atreye     | Eppinger  | M      | 1990-04-18 |
|  20002 | 1955-12-25 | Jaber      | Brender   | M      | 1988-01-26 |
|  20003 | 1953-04-11 | Munehiko   | Coors     | F      | 1991-02-07 |
|  20004 | 1952-03-07 | Radoslaw   | Pfau      | M      | 1995-11-24 |
|  20005 | 1956-02-20 | Licheng    | Przulj    | M      | 1992-07-17 |
+--------+------------+------------+-----------+--------+------------+
5 rows in set (0.00 sec)
-- （当然推荐把热数据放cache里,比如Redis）
```
>`ORDER BY` 是把已经查询好的结果集进行排序

## 3. WHERE：`WHERE`是将查询出来的结果，通过`WHERE`后面的条件，对结果进行过滤
### 3.1 逻辑：and、or、not
#### 1） select * from employees where emp_no > 40000 and hire_date > '1991-01-01' order by emp_no limit 4;  //也可以加双引号“1991-01-01”
#### 2） select * from employees where (emp_no > 40000 and birth_date > '1961-01-01')  or (emp_no > 40000 and hire_date > '1991-01-01') order by emp_no limit 5;
```sql
mysql> select * from employees  
    -> where emp_no > 40000
    -> and hire_date > '1991-01-01'   -- 可以用 and 进行 逻辑与 
    -> order by emp_no limit 4;
+--------+------------+------------+------------+--------+------------+
| emp_no | birth_date | first_name | last_name  | gender | hire_date  |
+--------+------------+------------+------------+--------+------------+
|  40003 | 1960-01-26 | Jacopo     | Marshall   | F      | 1991-09-30 |
|  40005 | 1961-02-27 | Zsolt      | Fairtlough | F      | 1991-07-08 |
|  40012 | 1955-02-07 | Chinhyun   | Ozeri      | F      | 1995-08-12 |
|  40015 | 1964-10-08 | Ioana      | Lemarechal | M      | 1997-08-07 |
+--------+------------+------------+------------+--------+------------+


mysql> select * from employees
    -> where (emp_no > 40000 and birth_date > '1961-01-01') -- 使用()明确条件的逻辑规则
    ->    or (emp_no > 40000 and hire_date > '1991-01-01')  -- 可以使用 or 做 逻辑或
    -> order by emp_no limit 5;
+--------+------------+------------+------------+--------+------------+
| emp_no | birth_date | first_name | last_name  | gender | hire_date  |
+--------+------------+------------+------------+--------+------------+
|  40003 | 1960-01-26 | Jacopo     | Marshall   | F      | 1991-09-30 |
|  40005 | 1961-02-27 | Zsolt      | Fairtlough | F      | 1991-07-08 |
|  40006 | 1962-11-07 | Basim      | Panienski  | F      | 1986-12-27 |
|  40012 | 1955-02-07 | Chinhyun   | Ozeri      | F      | 1995-08-12 |
|  40015 | 1964-10-08 | Ioana      | Lemarechal | M      | 1997-08-07 |
+--------+------------+------------+------------+--------+------------+
```

## 4. JOIN
![](https://github.com/zhaojing5340126/interview/blob/master/picture/123.png?raw=true)

### 4.1. INNER JOIN -内联
#### 1）select * from employees,titles where employees.emp_no = titles.emp_no limit 5;
#### 2）select e.emp_no, concat(last_name,' ', first_name) as emp_name, gender, title from employees as e,titles as t  where e.emp_no = t.emp_no limit 5;   -- 使用别名as，也可以不使用
####3) select e.emp_no, concat(last_name,' ', first_name) as emp_name, gender, title from employees as e join titles as t  on e.emp_no = t.emp_no limit 5;  //inner join...on...
```sql
--
-- ANSI SQL 89
-- 关联employees表和titles表
-- 要求是 employees的emp_no 等于 titles的emp_no
--
mysql> select * from employees,titles where employees.emp_no = titles.emp_no limit 5;
+--------+------------+------------+-----------+--------+------------+--------+-----------------+------------+------------+
| emp_no | birth_date | first_name | last_name | gender | hire_date  | emp_no | title           | from_date  | to_date    |
+--------+------------+------------+-----------+--------+------------+--------+-----------------+------------+------------+
|  10001 | 1953-09-02 | Georgi     | Facello   | M      | 1986-06-26 |  10001 | Senior Engineer | 1986-06-26 | 9999-01-01 |
|  10002 | 1964-06-02 | Bezalel    | Simmel    | F      | 1985-11-21 |  10002 | Staff           | 1996-08-03 | 9999-01-01 |
|  10003 | 1959-12-03 | Parto      | Bamford   | M      | 1986-08-28 |  10003 | Senior Engineer | 1995-12-03 | 9999-01-01 |
|  10004 | 1954-05-01 | Chirstian  | Koblick   | M      | 1986-12-01 |  10004 | Engineer        | 1986-12-01 | 1995-12-01 |
|  10004 | 1954-05-01 | Chirstian  | Koblick   | M      | 1986-12-01 |  10004 | Senior Engineer | 1995-12-01 | 9999-01-01 |
+--------+------------+------------+-----------+--------+------------+--------+-----------------+------------+------------+


--
-- 在上面的基础上只显示emp_no，名字，性别和职位名称
--
mysql> select emp_no, concat(last_name,' ', first_name), gender, title 
    -> from employees,titles
    -> where employees.emp_no = titles.emp_no limit 5;
ERROR 1052 (23000): Column 'emp_no' in field list is ambiguous  -- 报错了，原因是emp_no两个表都有

mysql> select employees.emp_no, -- 指定了employees
    -> concat(last_name,' ', first_name), gender, title
    -> from employees,titles
    -> where employees.emp_no = titles.emp_no limit 5;
+--------+-----------------------------------+--------+-----------------+
| emp_no | concat(last_name,' ', first_name) | gender | title           |
+--------+-----------------------------------+--------+-----------------+
|  10001 | Facello Georgi                    | M      | Senior Engineer |
|  10002 | Simmel Bezalel                    | F      | Staff           |
|  10003 | Bamford Parto                     | M      | Senior Engineer |
|  10004 | Koblick Chirstian                 | M      | Engineer        |
|  10004 | Koblick Chirstian                 | M      | Senior Engineer |
+--------+-----------------------------------+--------+-----------------+    


mysql> select e.emp_no,  -- 使用表的别名
    -> concat(last_name,' ', first_name) as emp_name, gender, title  -- 对名字的列取一个别名叫emp_name
    -> from employees as e,titles as t      -- 对表做别名
    -> where e.emp_no = t.emp_no limit 5;   -- 使用报表的别名
+--------+-------------------+--------+-----------------+
| emp_no | emp_name          | gender | title           |
+--------+-------------------+--------+-----------------+
|  10001 | Facello Georgi    | M      | Senior Engineer |
|  10002 | Simmel Bezalel    | F      | Staff           |
|  10003 | Bamford Parto     | M      | Senior Engineer |
|  10004 | Koblick Chirstian | M      | Engineer        |
|  10004 | Koblick Chirstian | M      | Senior Engineer |
+--------+-------------------+--------+-----------------+


--
-- ANSI SQL 92
-- inner join ... on ...语法
--
mysql> select e.emp_no,
    -> concat(last_name,' ', first_name) as emp_name, gender, title
    -> from employees as e join titles as t  -- inner join 可以省略inner关键字
    -> on e.emp_no = t.emp_no limit 5;             -- 配合join使用on
+--------+-------------------+--------+-----------------+
| emp_no | emp_name          | gender | title           |
+--------+-------------------+--------+-----------------+
|  10001 | Facello Georgi    | M      | Senior Engineer |
|  10002 | Simmel Bezalel    | F      | Staff           |
|  10003 | Bamford Parto     | M      | Senior Engineer |
|  10004 | Koblick Chirstian | M      | Engineer        |
|  10004 | Koblick Chirstian | M      | Senior Engineer |
+--------+-------------------+--------+-----------------+


-- 通过explain看两条语句的执行计划，发现是一样的，所以性能上是一样的，只是语法的不同
```

### 4.2. OUTER JOIN：包括left join 和 right join 【outer是省略了的】， ON 参与outer join的结果的生成，而where只是对结果的一个过滤
#### 4.2.1 left join ： 左表 left join 右表 on 条件
##### 1) select * from  t1 left join t2 on t1.a = t2.b;  //左表t1全部显示，右表满足这个匹配条件的才显示，不满足则显示null
#### 4.2.2  right join ： 左表 right join 右表 on 条件
##### 2) select * from  t1 right join t2 on t1.a = t2.b;  //右表t2全部显示，左表满足这个匹配条件的才显示，不满足则显示null
#### 4.2.3 题目： 查找在t1表，而不在t2表的数据
##### 3）select * from t1 left join t2 on t1.a = t2.b where t2.b is null;  //**判断是否为null用 is
#### 4.2.5 题目：查找哪些员工不是经理
##### 4）select e.emp_no,concat(last_name,' ', first_name) as emp_name, gender, d.dept_no from employees as e left join dept_manager as d on e.emp_no = d.emp_no where d.emp_no is null limit 5;

```sql
--
-- 左连接 left join
--

mysql> select * from test_left_join_1;
+------+
| a    |
+------+
|    1 |
|    2 |
+------+


mysql> select * from test_left_join_2;
+------+
| b    |
+------+
|    1 |
+------+


mysql> select * from 
    -> test_left_join_1 as t1 
    -> left join   -- 使用left join
    -> test_left_join_2 as t2 
    -> on t1.a = t2.b;
+------+------+
| a    | b    |
+------+------+
|    1 |    1 |  -- 满足条件的，显示t2中该条记录的值
|    2 | NULL |  -- 不满足条件的，用NULL填充
+------+------+

-- left join ： 左表 left join 右表 on 条件；
--              左表全部显示，右表是匹配表，
--              如果右表的某条记录满足 [on 条件] 匹配，则该记录显示
--              如果右表的某条记录 不 满足 匹配，则该记录显示NULL




--
-- 右连接 right join 
--
mysql> select * from
    -> test_left_join_1 as t1
    -> right join   -- 使用right join
    -> test_left_join_2 as t2
    -> on t1.a = t2.b;
+------+------+
| a    | b    |
+------+------+
|    1 |    1 |   -- 右表（t2）全部显示
+------+------+

-- right join ： 左表 right join 右表 on 条件
--               右表全部显示，左边是匹配表
--               同样和left join，满足则显示，不满足且右表中有值，则填充NULL





-- 题目：查找在t1表，而不在t2表的数据
--
mysql> select * from 
    -> test_left_join_1 as t1 
    -> left join
    -> test_left_join_2 as t2 
    -> on t1.a = t2.b where t2.b is null;
+------+------+
| a    | b    |
+------+------+
|    2 | NULL |  -- 数据1 在t1和t2中都有，所以不显示
+------+------+



-- left join ： left outer join , outer关键字可以省略
-- right join： right outer join , outer 关键字可以省略

-- join无论inner还是outer，列名不需要一样，甚至列的类型也可以不一样，会进行转换。
-- 一般情况下，表设计合理，需要关联的字段类型应该是一样的，这样才有实际意义


--
-- 查找哪些员工不是经理
--
mysql> select e.emp_no,
    -> concat(last_name,' ', first_name) as emp_name, gender, d.dept_no
    -> from employees as e left join dept_manager as d
    -> on e.emp_no = d.emp_no
    -> where d.emp_no is null limit 5;
+--------+-------------------+--------+---------+
| emp_no | emp_name          | gender | dept_no | -- dept_no是dept_manager的字段，一定要有一个dept_manager里的字段，不然哪里来的null
+--------+-------------------+--------+---------+
|  10001 | Facello Georgi    | M      | NULL    |
|  10002 | Simmel Bezalel    | F      | NULL    |
|  10003 | Bamford Parto     | M      | NULL    |
|  10004 | Koblick Chirstian | M      | NULL    |
|  10005 | Maliniak Kyoichi  | M      | NULL    |
+--------+-------------------+--------+---------+



-- 在 inner join中，过滤条件放在where或者on中都是可以的
-- 在 outer join中 条件放在where和on中是不一样的
mysql> select * from 
    -> test_left_join_1 as t1
    -> left join
    -> test_left_join_2 as t2
    -> on t1.a = t2.b
    -> where t2.b is null;
+------+------+
| a    | b    |
+------+------+
|    2 | NULL |
+------+------+
1 row in set (0.00 sec)

mysql> select * from 
    -> test_left_join_1 as t1
    -> left join
    -> test_left_join_2 as t2
    -> on t1.a = t2.b
    -> and t2.b is null;  -- 除了a=b, 还要找到b=null的，但是b里面没有null，所有a全部显示，b全为null
+------+------+
| a    | b    |
+------+------+
|    1 | NULL |
|    2 | NULL |
+------+------+
2 rows in set (0.00 sec)

-- ON 参与outer join的结果的生成，而where只是对结果的一个过滤

```

### 4.3. GROUP BY：分组
#### 4.3.1 题目：选出部门人数 > 50000 
##### 1） select dept_no, count(dept_no) from dept_emp group by dept_no having count(dept_no) > 50000;  -- 通过 dept_no部门分组，count是计算数量， 如果是对分组的聚合函数做过滤，使用having，用where报语法错误
```sql
--
-- 找出同一个部门的员工数量
--
mysql> select dept_no, count(dept_no)  -- count是得到数量，这里就是分组函数
    -> from dept_emp
    -> group by dept_no;  -- 通过 dept_no 分组
+---------+----------------+
| dept_no | count(dept_no) |
+---------+----------------+
| d001    |          20211 |
| d002    |          17346 |
| d003    |          17786 |
| d004    |          73485 |
| d005    |          85707 |
| d006    |          20117 |
| d007    |          52245 |
| d008    |          21126 |
| d009    |          23580 |
+---------+----------------+


--
-- 选出部门人数 > 50000 
-- 
mysql> select dept_no, count(dept_no)
    -> from dept_emp
    -> group by dept_no
    -> having count(dept_no) > 50000;  -- 如果是对分组的聚合函数做过滤，使用having，用where报语法错误
+---------+----------------+
| dept_no | count(dept_no) |
+---------+----------------+
| d004    |          73485 |
| d005    |          85707 |
| d007    |          52245 |
+---------+----------------+

```

# 十二、day12：子查询 INSERT UPDATE DELETE REPLACE

## 1.子查询：指在一个select语句中嵌套另一个select语句。同时，子查询必须包含括号。
### 1.1.  ANY / SOME
#### 1）select a from t1 where a > any(select a from t2);  //t1表内a列的值 大于 t2表中a列的任意(any)一个值（t1.a > any(t2.a) == true）,则返回t1.a的记录
##### 1. `select a from t1` 是外部查询(outer query)
##### 2. `(select a from t2)` 是子查询
##### 3.ANY和some是一个意思，必须与一个`比较操作符`一起使用： `=`, `>`, `<`, `>=`, `<=`, `<>` ,其中<>表示不等于

* 如果外部查询的列的结果和子查询的列的结果比较得到为True的话，则返回比较值为True的（外查询）的记录

```sql

mysql> insert into t2 values(12),(13),(5);  //赋值可以这样赋值


mysql> select a from t1;
+------+
| a    |
+------+
|   10 |
|    4 |
+------+


mysql> select * from t2;
+------+
| a    |
+------+
|   12 |   
|   13 |  
|    5 |  
+------+

mysql> select a from t1  
    -> where a > any
    -> (select a from t2); -- 返回(12，13，4)
                           -- t1中a列的值，只要大于(12,13,4)中任意一值
                           -- 即t1.a > t2.a为True，则返回对应的t1.a
+------+
| a    |
+------+
|   10 | 
+------+
1 row in set (0.00 sec)

-- 这个查询可以解释为，t1表内a列的值 大于 t2表中a列的任意(any)一个值（t1.a > any(t2.a) == true）,则返回t1.a的记录
```


### 1.2. IN：`in`是`ANY`的一种特殊情况：**`"in"`** 的结果等同于  **`"= any"`**
#### 1） select a from t1 where a in (select a from t2);

```sql

mysql> select a from t1 where a in (select a from t2); -- in的结果等同于 =any 的结果
+------+
| a    |
+------+
|    5 |
+------+

```


### 1.3. ALL
#### 1）truncate t1;   -- 清空t1
#### 2）select a from t1 where a > all(select a from t2); //`ALL`关键词必须与一个`比较操作符`一起使用，NOT IN 是 <> ALL 的别名 
```sql
mysql> truncate t1;   -- 清空t1

mysql> truncate t2;   -- 清空t2

mysql> insert into t1 values(10),(4);

mysql> insert into t2 values(5),(4),(3);  

mysql> select a from t1 where a > all(select a from t2);
+------+
| a    |
+------+
|   10 |  
+------+

```
>`ALL`关键词必须与一个`比较操作符`一起使用
>`NOT IN` 是 `<> ALL`的别名 

### 1.4. 子查询的分类
#### 1.4.1 独立子查询：不依赖外部查询而运行的子查询
    ```sql
    mysql> select a from t1 where a in (1,2,3,4,5);
    +------+
    | a    |
    +------+
    |    4 |  
    +------+

    ```

#### 1.4.2 相关子查询：引用了外部查询列的子查询
    ```sql
    -- 在这个例子中，子查询中使用到了外部的列t1.a 
    mysql> select a from t1 where a in (select * from t2 where t1.a = t2.a);
    +------+
    | a    |
    +------+
    |    4 |
    +------+

    ```
#### 1.4.3.和 null比较，使用is和is not， 而不是 = 和 <>
```sql
--
-- SQL语句一 使用 EXISTS
--
select customerid, companyname 
    from customers as A
    where country = 'Spain' 
        and not exists
            ( select * from orders as B
              where A.customerid = B.customerid );
              
--
-- SQL语句二 使用 IN
--
select customerid, companyname 
    from customers as A
    where country = 'Spain' 
        and customerid not in (select customerid from orders);
              
-----
-- 当结果集合中没有NULL值时，上述两条SQL语句查询的结果是一致的 
-----

--
-- 插入一个NULL值
--
insert into orders(orderid) values (null);

-----
-- SQL语句1 : 返回和之前一致
-- SQL语句2 : 返回为空表，因为子查询返回的结果集中存在NULL值。not in null 永远返回False或者NULL
--            此时 where (country = 'Spain' and (False or NULL)) 为 False OR NULL，条件永远不匹配
-----

--
-- SQL语句2 改写后
--
select customerid, companyname 
    from customers as A
    where country = 'Spain' 
        and customerid not in (select customerid from orders 
                                where customerid is not null);  -- 增加这个过滤条件，使用is not，而不是<>


--
-- 和 null比较，使用is和is not， 而不是 = 和 <>
--
mysql> select null = null; 
+-------------+
| null = null |
+-------------+
|        NULL |
+-------------+


mysql> select null <> null;
+--------------+
| null <> null |
+--------------+
|         NULL |
+--------------+


mysql> select null is null; 
+--------------+
| null is null |
+--------------+
|            1 |  -- 返回 True
+--------------+


mysql> select null is not  null;
+-------------------+
| null is not  null |
+-------------------+
|                 0 |  -- 返回 False
+-------------------+

```

----

## 2. INSERT (select 比 values强大)
### 1）insert into t1 values(2),(3),(-1);  -- 插入多个值，MySQL独有
### 2）insert into t3(a) select 8;  -- 指定列a，以下这些方法values都不能用
### 3) insert into t3 select 8, 9;  -- 不指定列，但是插入值匹配列的个数和类型
### 4) insert into t3(b) select a from t2;  -- 从t2表中查询数据并插入到t3(a)中，注意指定列
```sql
mysql> insert into t1 values(1);  -- 插入一个值

mysql> insert into t1 values(2),(3),(-1);  -- 插入多个值，MySQL独有

mysql> insert into t1 select 8;   -- insert XXX select XXX 语法，MySQ独有

mysql> create table t3 (a int, b int); -- 有多个列

mysql> insert into t3 select 8;  -- 没有指定列，报错
ERROR 1136 (21S01): Column count doesnt match value count at row 1

mysql> insert into t3(a) select 8;  -- 指定列a

mysql> insert into t3 select 8, 9;  -- 不指定列，但是插入值匹配列的个数和类型

mysql> select * from t3;
+------+------+
| a    | b    |
+------+------+
|    8 | NULL |
|    8 |    9 |
+------+------+
2 rows in set (0.00 sec)

mysql> insert into t3(b) select a from t2;  -- 从t2表中查询数据并到t3(a)中，注意指定列


mysql> select * from t3;
+------+------+
| a    | b    |
+------+------+
|    8 | NULL |
|    8 |    9 |
| NULL |    5 |
| NULL |    4 |
| NULL |    3 |
+------+------+
5 rows in set (0.00 sec)

```

-----

## 3. DELETE
### 1）delete from t3 where a is null;  -- 根据过滤条件删除
### 2）delete from t3;   -- 删除整个表
```sql
mysql> delete from t3 where a is null;  -- 根据过滤条件删除

mysql> select * from t3;               
+------+------+
| a    | b    |
+------+------+
|    8 | NULL |
|    8 |    9 |
|    8 | NULL |
|    8 |    9 |
|    8 | NULL |
|    8 |    9 |
|    8 | NULL |
|    8 |    9 |
+------+------+

mysql> delete from t3;   -- 删除整个表


```

-----

## 4. UPDATE【update...set...】
### 1）update t3 set a=10 where a=1;
### 2）update t1 join t2 on t1.a = t2.a set t1.a=100;  -- 先得到t1.a=t2.a的结果集， 然后将结果集中的t1.a设置为100
```sql
mysql> insert into t3 select 1,2;

mysql> select * from t3;
+------+------+
| a    | b    |
+------+------+
|    1 |    2 |
+------+------+

mysql> update t3 set a=10 where a=1;

mysql> select * from t3;
+------+------+
| a    | b    |
+------+------+
|   10 |    2 |
+------+------+

--
-- 关联后更新
--
mysql> select * from t1;
+------+
| a    |
+------+
|   10 |
|    4 |  -- 和t2中的4相等
|    1 |
|    2 |
|    3 |  -- 和t2中的3相等
|   -1 |
|    8 |
+------+

mysql> select * from t2;
+------+
| a    |
+------+
|    5 |
|    4 |  -- 和t1中的4相等
|    3 |  -- 和t1中的3相等
+------+

mysql> update t1 join t2 on t1.a = t2.a set t1.a=100;  -- 先得到t1.a=t2.a的结果集
                                                       -- 然后将结果集中的t1.a设置为100

mysql> select * from t1;
+------+
| a    |
+------+
|   10 |
|  100 |  -- 该行被更新成100
|    1 |
|    2 |
|  100 |  -- 该行被更新成100
|   -1 |
|    8 |
+------+
```

-----

## 5. REPLACE: replace的原理是：先delete，再insert，没有替换对象时，类似插入效果insert
### 1) replace into t4 values(1, 100); -- 替换该主键对应的值  
```sql
mysql> create table t4(a int primary key auto_increment, b int);

mysql> insert into t4 values(NULL, 10);

mysql> insert into t4 values(NULL, 11);

mysql> insert into t4 values(NULL, 12);

mysql> select * from t4;
+---+------+
| a | b    |
+---+------+
| 1 |   10 |
| 2 |   11 |
| 3 |   12 |
+---+------+

mysql> insert into t4 values(1, 100);  -- 报错，说存在重复的主键记录 "1"
ERROR 1062 (23000): Duplicate entry '1' for key 'PRIMARY'

mysql> replace into t4 values(1, 100); -- 替换该主键对应的值  
Query OK, 2 rows affected (0.03 sec)   -- 两行记录受到影响

mysql> select * from t4;
+---+------+
| a | b    |
+---+------+
| 1 |  100 |  -- 已经被替换
| 2 |   11 |
| 3 |   12 |
+---+------+

-- replace的原理是：先delete，在insert
-----

mysql> replace into t4 values(5, 50);  -- 没有替换对象时，类似插入效果
Query OK, 1 row affected (0.03 sec)    -- 只影响1行

mysql> select * from t4;
+---+------+
| a | b    |
+---+------+
| 1 |  100 |
| 2 |   11 |
| 3 |   12 |
| 5 |   50 |  -- 插入了1行
+---+------+
4 rows in set (0.00 sec)

--
-- replace原理更明显的例子 
--

mysql> create table t6 
    -> (a int primary key, 
    -> b int auto_increment,   -- b是auto_increment的int型数据
    -> c int, key(b));
Query OK, 0 rows affected (0.15 sec)

mysql> insert into t6 values(10, NULL, 100),(20,NULL,200);  -- b自增长

mysql> select * from t6;
+----+---+------+
| a  | b | c    |
+----+---+------+
| 10 | 1 |  100 |  -- b为1
| 20 | 2 |  200 |  -- b为2
+----+---+------+

mysql> replace into t6 values(10,NULL,150);  -- 将a=10的替换掉

mysql> select * from t6;
+----+---+------+
| a  | b | c    |
+----+---+------+
| 10 | 3 |  150 |  -- 替换后b从1变成了3，说明是先删除，再插入
| 20 | 2 |  200 |
+----+---+------+

-----

```
-----

## 6. 其他知识点

### 6.1 更新有关系的值

```sql
mysql> create table t5 (a int, b int);

mysql> insert into t5 values(1,1);

mysql> select * from t5;
+------+------+
| a    | b    |
+------+------+
|    1 |    1 |
+------+------+

mysql> update t5 set a=a+1, b=a where a=1;


mysql> select * from t5;
+------+------+
| a    | b    |
+------+------+
|    2 |    2 |  -- SQL Server和Oracle中得到的值是2, 1
+------+------+

```

### 6.2 显示行号(RowNumber)
#### 1) set @rn:=0;  -- 冒号是赋值，产生 SESSION(会话)级别的变量,这是MySQL自定义的变量
#### 2）select @rn:=@rn+1 as rownumber, emp_no, gender from employees limit 10;  -- ：= 是赋值的意思
```sql
--
-- 方法一
--
mysql> use employees ;

mysql> set @rn:=0;  -- 冒号是赋值，产生 SESSION(会话)级别的变量

mysql> select @rn:=@rn+1 as rownumber, emp_no, gender from employees limit 10;  -- ：= 是赋值的意思
+-----------+--------+--------+
| rownumber | emp_no | gender |
+-----------+--------+--------+
|        11 |  10001 | M      |
|        12 |  10002 | F      |
|        13 |  10003 | M      |
|        14 |  10004 | M      |
|        15 |  10005 | M      |
|        16 |  10006 | F      |
|        17 |  10007 | F      |
|        18 |  10008 | M      |
|        19 |  10009 | F      |
|        20 |  10010 | F      |
+-----------+--------+--------+
10 rows in set (0.00 sec)

--
-- 方法二 （推荐）
--
mysql> select @rn1:=@rn1+1 as rownumber, emp_no, gender from employees, (select @rn1:=0) as a limit 10;
+-----------+--------+--------+
| rownumber | emp_no | gender |
+-----------+--------+--------+
|         1 |  10001 | M      |
|         2 |  10002 | F      |
|         3 |  10003 | M      |
|         4 |  10004 | M      |
|         5 |  10005 | M      |
|         6 |  10006 | F      |
|         7 |  10007 | F      |
|         8 |  10008 | M      |
|         9 |  10009 | F      |
|        10 |  10010 | F      |
+-----------+--------+--------+


-- MySQL 自定义变量，根据每一记录进行变化的,冒号是赋值

mysql> select @rn1:=0;
+---------+
| @rn1:=0 |
+---------+
|       0 |  -- 只有一行记录
+---------+

-- 相当于 把 employees 和 (select @rn1:=0)做了笛卡尔积，然后使用@rn1:=@rn + 1，根据每行进行累加


--
-- ":=" 和 "="
--

mysql> set @a:=10;  -- 赋值为10

mysql> select @a;
+------+
| @a   |
+------+
|   10 |
+------+

mysql> select @a=9;  -- 进行比较
+------+
| @a=9 |
+------+
|    0 |  -- 返回为False
+------+

```


# 十三、day013-作业讲解一 Rank 视图 UNION 触发器上

## 1. 作业讲解
### 1）select datediff('1971-01-01', '1970-01-05');  //971-01-01 减去 1970-01-5 为361天
### 2) select datediff('1971-01-01', '1970-01-05') / 7;  -- 求周数
### 3) select floor(5.4); //向下取整
### 4）select round(5.4); //ROUND(X, D) 返回值是对数字X保留到小數点后D位，进行四舍五入，D默认为0；如果D为负数，则保留小数点左边（整数）的位数
### 5）select adddate('1970-01-05', interval 7 day);   //使用adddate函数，在1970-01-05的基础上，增加7天


----

## 2. Rank 排名问题的步骤：
### 1）set @prev_value := NULL;  //prev_value可以理解为是临时保存第N-1行的score的变量
### 2) set @rank_count := 0;   ///用于存放当前的排名
### 3)  select  id, score, 
###     case
###        when @prev_value = score then @rank_count  // 相等则prev_value不变， 并返回rank_count（第一次为NULL，不会相等，所以跳转到下一个when语句）
###        when @prev_value := score then @rank_count := @rank_count + 1   // 不等，则第N行的score赋值(:=)给prev_value。且rank_count增加1
###     end as rank_column   -- case 开始的，end结尾
###     from test_rank order by score desc;


>给出不同的用户的分数，然后根据分数计算排名
```sql
-- case  
--   when [condition_1] then [do_something_1] 
--   when [condition_2] then [do_something_2] 
--   end
-- 语法：如果 condition_1条件满足，则执行 do_something_1 然后就跳出,不会执行condition_2;
--       如果 condition_1条件不满足，则继续执行到 condition_2。以此类推。


mysql> create table test_rank(id int, score int);

mysql> insert into test_rank values(1, 10), (2, 20), (3, 30), (4, 30), (5, 40), (6, 40);


mysql> select * from test_rank;
+------+-------+
| id   | score |
+------+-------+
|    1 |    10 |
|    2 |    20 |
|    3 |    30 |
|    4 |    30 |
|    5 |    40 |
|    6 |    40 |
+------+-------+


mysql> set @prev_value := NULL;  -- 假设比较到第N行，设置一个变量prev_value用于存放第N-1行score的分数
                                 -- 用于比较第N行的score和第N-1行的score
                                 -- prev_value可以理解为 是临时保存第N-1行的score的变量

mysql> set @rank_count := 0;     -- 用于存放当前的排名

mysql> select  id, score, 
    -> case
    ->     when @prev_value = score then @rank_count  
           -- 相等则prev_value不变， 并返回rank_count（第一次为NULL，不会相等，所以跳转到下一个when语句）
    ->     when @prev_value := score then @rank_count := @rank_count + 1 
           -- 不等，则第N行的score赋值(:=)给prev_value。且rank_count增加1
    -> end as rank_column   -- case 开始的，end结尾
    -> from test_rank
    -> order by score desc;
+------+-------+-------------+
| id   | score | rank_column |
+------+-------+-------------+
|    5 |    40 |           1 |
|    6 |    40 |           1 |
|    3 |    30 |           2 |
|    4 |    30 |           2 |
|    2 |    20 |           3 |
|    1 |    10 |           4 |
+------+-------+-------------+


```

-----

## 3. 视图：视图在mysql是虚拟表，视图在创建的瞬间，便确定了结构（每次查询视图，实际上还是去查询的原来的表，只是查询的规则是在视图创建时经过定义的。），其作用是，可以对开发人员是透明的，可以隐藏部分关键的列。
### 1) create view view_rank as select * from test_rank;  //创建视图view_rank
### 2) show create table test_rank\G   //视图是以一张表的形式存在的，可通过show tables看到，和真正的表不同的是，这里show出来的是视图的定义
### 3）select * from view_rank;       //可以直接查询该视图得结果


```sql
--
-- 创建视图
--
mysql> create view view_rank as select * from test_rank;  -- 针对上面的test_rank创建一个视图
-- 也可以对select结果增加条件进行过滤后，再创建视图


mysql> show create table test_rank\G
*************************** 1. row ***************************
       Table: test_rank
Create Table: CREATE TABLE `test_rank` (    -- 得到的是表结构
  `id` int(11) DEFAULT NULL,
  `score` int(11) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4


mysql> show create table view_rank\G  -- 他是以一张表的形式存在的，可通过show tables看到
*************************** 1. row ***************************
                View: view_rank
         Create View: CREATE ALGORITHM=UNDEFINED DEFINER=`root`@`localhost` SQL SECURITY DEFINER VIEW `view_rank` AS select `test_rank`.`id` AS `id`,`test_rank`.`score` AS `score` from `test_rank`
                      -- 和真正的表不同的是，这里show出来的是视图的定义
character_set_client: utf8
collation_connection: utf8_general_ci

mysql> select * from view_rank;  -- 可以直接查询该视图得结果
+------+-------+
| id   | score |
+------+-------+
|    1 |    10 |
|    2 |    20 |
|    3 |    30 |
|    4 |    30 |
|    5 |    40 |
|    6 |    40 |
+------+-------+

-- 视图的作用是，可以对开发人员是透明的，可以隐藏部分关键的列
-- 视图在mysql是虚拟表。根据视图的定义，还是取执行定义中的select语句。


-- 只开放部分列
mysql> create view view_rank_1 as select id from test_rank; -- 只开放id列

mysql> select * from view_rank_1;  -- 即使 select * ，也只能看到id列，具有隐藏原来表中部分列的功能                        
+------+
| id   |
+------+
|    1 |
|    2 |
|    3 |
|    4 |
|    5 |
|    6 |
+------+


-- 不要取用select * from 去创建视图，因为mysql会把*逐个解析成列。
-- 当原来的表结构发生变化时，视图的表结构是不会发生变化的，视图在创建的瞬间，便确定了结构。
-- 比如，当你alter原来的表 增加列(add columns)时,再去查询该视图,新增加的列是不存在的。

mysql> alter table test_rank add column  c int default 0; -- 增加一列名字为c，默认值为0

mysql> select * from test_rank;  -- 查询原表，新的列c出现了
+------+-------+------+
| id   | score | c    |
+------+-------+------+
|    1 |    10 |    0 |
|    2 |    20 |    0 |
|    3 |    30 |    0 |
|    4 |    30 |    0 |
|    5 |    40 |    0 |
|    6 |    40 |    0 |
+------+-------+------+


mysql> select * from view_rank; -- 尽管view_rank用select * 创建，但当时没有列c，所以无法得到c列的值
+------+-------+
| id   | score |
+------+-------+
|    1 |    10 |
|    2 |    20 |
|    3 |    30 |
|    4 |    30 |
|    5 |    40 |
|    6 |    40 |
+------+-------+

-- 注意：mysql中的视图都是虚拟表。不像Oracle可以物化成真实存在的表。
--      每次查询视图，实际上还是去查询的原来的表，只是查询的规则是在视图创建时经过定义的。

```

### 3.1 视图的算法(`ALGORITHM`)有三种方式：
#### 1.`UNDEFINED` 默认方式，由MySQL来判断使用下面的哪种算法【我们一般选择默认方式，让MySQL自己判断】
#### 2. `MERGE` ： `每次`通过`物理表`查询得到结果，把结果merge(合并)起来返回
#### 3.`TEMPTABLE` ： 产生一张`临时表`，把数据放入临时表后，客户端再去临时表取数据（`不会缓存`）
##### 3.1 `TEMPTABLE 特点`：即使访问条件一样，第二次查询还是会去读取物理表中的内容，并重新生成一张临时表,并不会取缓存之前的表。*（临时表是Memory存储引擎，默认放内存，超过配置大小放磁盘）*
##### 3.2 当查询有一个较大的结果集时，使用`TEMPTABLE`可以快速的结束对该物理表的访问，从而可以快速释放这张物理表上占用的资源。然后客户端可以对临时表上的数据做一些耗时的操作，而不影响原来的物理表。

----

## 4. UNION：作用是将两个查询的结果集进行合并。
### 4.1 UNION必须由`两条或两条以上`的SELECT语句组成，语句之间用关键字`UNION`分隔。
### 4.2 UNION中的每个查询必须包含相同的列（`类型相同或可以隐式转换`）、表达式或聚集函数。
### 1）select a, b from t1 union select * from t2;  //使用union会去重，导致性能下降
### 2）select a, b from t1 union all select * from t2;  //使用 union all 可以不去重， 如果知道数据本身具有唯一性，没有重复，则建议使用`union all`

```sql
mysql> select * from test_union_1;
+------+------+
| a    | b    |
+------+------+
|    1 |    2 |
|    3 |    4 |
|    5 |    6 |
|   10 |   20 |  -- test_union_1 中的10, 20
+------+------+

mysql> select * from test_union_2;
+------+------+
| a    | c    |
+------+------+
|   10 |   20 | -- test_union_2 中的10, 20
|   30 |   40 |
|   50 |   60 |  
+------+------+

mysql> select a, b as t from test_union_1
    -> union
    -> select * from test_union_2;
+------+------+
| a    | t    |
+------+------+
|    1 |    2 |
|    3 |    4 |
|    5 |    6 |
|   10 |   20 | -- 只出现了一次 10, 20，union会去重
|   30 |   40 |
|   50 |   60 |
+------+------+

mysql> select a, b as t from test_union_1
    -> union all   -- 使用 union all 可以不去重
    -> select * from test_union_2;
+------+------+
| a    | t    |
+------+------+
|    1 |    2 |
|    3 |    4 |
|    5 |    6 |
|   10 |   20 | -- test_union_1 中的10, 20
|   10 |   20 | -- test_union_2 中的10, 20
|   30 |   40 |
|   50 |   60 |
+------+------+

mysql> select a, b as t from test_union_1 where a > 2 
    -> union
    -> select * from test_union_2 where c > 50;  -- 使用where过滤也可以 
+------+------+
| a    | t    |
+------+------+
|    3 |    4 |
|    5 |    6 |
|   10 |   20 |
|   50 |   60 |
+------+------+
4 rows in set (0.00 sec)
```

> 如果知道数据本身具有唯一性，没有重复，则建议使用`union all`，因为`union`会做`去重操作`，性能会比`union all`要低

----


## 5. 触发器：触发器的对象是`表`，当表上出现`特定的事件`时`触发`该程序的执行
### 5.1 触发器的类型
#### 5.1.1 `UPDATE`触发器：用于 update 操作
#### 5.1.2 `DELETE`触发器：用于delete 操作、replace 操作
##### 注意：drop，truncate等DDL操作`不会触发`DELETE
#### 5.1.3 `INSERT`触发器：insert 操作、 load data 操作、replace 操作
### 5.2 注意：`replace`操作会`触发两次`，一次是`UPDATE`类型的触发器，一次是`INSERT`类型的触发器
### 5.3 注意：触发器只触发DML(Data Manipulation Language )操作，不会触发DDL(Data Definition Language)操作（create,drop等操作）       
* **创建触发器**

    ```sql
    CREATE
        [DEFINER = { user | CURRENT_USER }]
        TRIGGER trigger_name  -- 触发器名字
        trigger_time trigger_event  -- 触发时间和事件
        ON tbl_name FOR EACH ROW    
        [trigger_order]
        trigger_body
    
    trigger_time: { BEFORE | AFTER }   -- 事件之前还是之后触发
    
    trigger_event: { INSERT | UPDATE | DELETE }  -- 三个类型
    
    trigger_order: { FOLLOWS | PRECEDES } other_trigger_name
    ```
    
    ```sql
    mysql> create table test_trigger_1 (
        ->     name varchar(10),
        ->     score int(10),
        -> primary key (name));

        mysql> delimiter //  -- 将语句分隔符定义成 // （原来是';'）
        mysql> create trigger  trg_upd_score  -- 定义触发器名字
            -> before update on test_trigger_1  -- 作用在test_trigger_1 更新(update)之前(before)
            -> for each row  -- 每行
            -> begin  -- 开始定义
            -> if new.score < 0 then   -- 如果新值小于0
            ->     set new.score=0;        -- 则设置成0
            -> elseif new.score > 100 then  -- 如果新值大于100
            ->     set new.score = 100;  -- 则设置成100
            -> end if;  -- begin对应的 结束 
            -> end;//   -- 结束，使用新定义的 '//' 结尾
        Query OK, 0 rows affected (0.03 sec)
    
    mysql> delimiter ;  -- 恢复 ';' 结束符
    
    -- new.col : 表示更新以后的值
    -- old.col : 表示更新以前的值(只读)
    
    mysql> insert into test_trigger_1 values ("tom", 200);  -- 插入新值

    
    mysql> select * from test_trigger_1;
    +------+-------+
    | name | score |
    +------+-------+
    | tom  |   200 |  -- 没改成100，因为定义的是update，而执行的是insert
    +------+-------+
        
    mysql> update test_trigger_1 
        -> set score=300 where name='tom'; -- 改成300
    
    mysql> select * from test_trigger_1;
    +------+-------+
    | name | score |
    +------+-------+
    | tom  |   100 | -- 通过触发器的设置，大于100的值被修改成100
    +------+-------+

    ```


### 5.4 触发器总结
#### 1）触发器对性能有损耗，应当非常慎重使用；
#### 2）对于事务表，`触发器执行失败则整个语句回滚`；
#### 3）Row格式主从复制，`触发器不会在从库上执行`； 
    - 因为从库复制的肯定是主库已经提交的数据，既然已经提交了说明触发器已经被触发过了，所以从库不会执行。
#### 4）使用触发器时应防止递归执行；

    ```sql
    delimiter //
    create trigger trg_test
        before update on 'test_trigger'
        for each row
    begin
        update test_trigger set score=20 where name = old.name;  -- 使用update触发器，却又触发了update操作，循环触发了，形成递归
    end;//
    ```

### 5.5 触发器模拟物化视图
#### 5.5.1 物化视图的概念
##### 1）不是基于基表的虚表
##### 2）根据基表实际存在的实表
##### 3）预先计算并保存耗时较多的SQL操作结果（如多表链接(join)或者group by等）

* **模拟物化视图**

```sql
mysql> create table Orders  
    -> (order_id int unsigned not null auto_increment,
    -> product_name varchar(30) not null,
    -> price decimal(8,2) not null,
    -> amount smallint not null,
    -> primary key(order_id));
 -- 创建Orders表

mysql> insert into Orders values 
    -> (null, 'cpu', 135.5 ,1),
    -> (null, 'memory', 48.2, 3),
    -> (null, 'cpu', 125.6, 3),
    -> (null, 'cpu', 105.3, 4);


mysql> select * from  Orders;
+----------+--------------+--------+--------+
| order_id | product_name | price  | amount |
+----------+--------------+--------+--------+
|        1 | cpu          | 135.50 |      1 |
|        2 | memory       |  48.20 |      3 |
|        3 | cpu          | 125.60 |      3 |
|        4 | cpu          | 105.30 |      4 |
+----------+--------------+--------+--------+


-- 建立一个模拟物化视图的表（即用这张表来模拟物化视图）
mysql> create table Orders_MV 
    -> ( product_name varchar(30) not null,
    ->  price_sum decimal(8,2) not null,
    ->  amount_sum int not null,
    ->  price_avg float not null,
    ->  orders_cnt int not null,
    ->  unique index (product_name));

-- 通过Orders表的数据，将测试数据初始化到Orders_MV表中
mysql> insert into Orders_MV 
    ->     select product_name, sum(price), 
    ->            sum(amount), avg(price), count(*)
    ->     from Orders
    ->     group by product_name;

mysql> select * from Orders_MV;
+--------------+-----------+------------+-----------+------------+
| product_name | price_sum | amount_sum | price_avg | orders_cnt |
+--------------+-----------+------------+-----------+------------+
| cpu          |    366.40 |          8 |   122.133 |          3 |
| memory       |     48.20 |          3 |      48.2 |          1 |
+--------------+-----------+------------+-----------+------------+


-- 在MySQL workbench中输入，比较方便
delimiter //

CREATE TRIGGER tgr_Orders_insert -- 创建触发器为tgr_Orders_insert
	AFTER INSERT ON Orders  -- 触发器是INSERT类型的，且作用于Orders表
	FOR EACH ROW
BEGIN
	SET @old_price_sum := 0;  -- 设置临时存放Orders_MV表(模拟物化视图)的字段的变量
	SET @old_amount_sum := 0;
	SET @old_price_avg := 0;
	SET @old_orders_cnt := 0;
	SELECT   -- select ... into ... 在更新Orders_MV之前，将Orders_MV中对应某个产品的信息写入临时变量 
		IFNULL(price_sum, 0),
		IFNULL(amount_sum, 0),
		IFNULL(price_avg, 0),
		IFNULL(orders_cnt, 0)
	FROM
		Orders_MV
	WHERE
		product_name = NEW.product_name INTO @old_price_sum , @old_amount_sum , @old_price_avg , @old_orders_cnt;

	SET @new_price_sum = @old_price_sum + NEW.price; -- 累加新的值
	SET @new_amount_sum = @old_amount_sum + NEW.amount;
	SET @new_orders_cnt = @old_orders_cnt + 1;
	SET @new_price_avg = @new_price_sum / @new_orders_cnt ;
	
    REPLACE INTO Orders_MV   
			VALUES(NEW.product_name, @new_price_sum,
				   @new_amount_sum, @new_price_avg, @new_orders_cnt );
   -- REPLACE 将对应的物品（唯一索引）的字段值替换new_xxx的值
END;//

delimiter ;


mysql> insert into Orders values (null, 'ssd', 299, 3);


mysql> insert into Orders values (null, 'memory', 47.9, 5);

mysql> select * from Orders_MV;
+--------------+-----------+------------+-----------+------------+
| product_name | price_sum | amount_sum | price_avg | orders_cnt |
+--------------+-----------+------------+-----------+------------+
| cpu          |    366.40 |          8 |   122.133 |          3 |
| memory       |     96.10 |          8 |     48.05 |          2 | -- 数量自动增加了1，价格也发生了变化
| ssd          |    299.00 |          3 |       299 |          1 | -- 新增加的ssd产品
+--------------+-----------+------------+-----------+------------+

```
### 5.5 MySQL内建函数演示：ifnull、select into
#### 1）select ifnull(@test, 100);   // 如果test为NULL，则ifnull返回100,test不为null,则返回test的值
#### 2) select * from test_rank_2 where id=1 into @id_1, @score_1;  //选择id=1的记录，将对应的id和score赋值给变量 id_1 和 score_1

```SQL
--
-- IFNULL MySQL内建函数的演示
--
mysql> select @test;
+-------+
| @test |
+-------+
| NULL  |  -- 当前会话中没有test变量
+-------+

mysql> select ifnull(@test, 100);   -- 如果test为NULL，则ifnull返回100
+--------------------+
| ifnull(@test, 100) |
+--------------------+
| 100                |  -- ifnull函数return的值是100
+--------------------+


mysql> select @test;
+-------+
| @test |
+-------+
| NULL  |  -- 但是test还是NULL
+-------+


mysql> set @test:=200;  -- 给test变量赋值为200

mysql> select ifnull(@test, 100);  -- 再次ifnull判断，此时test不为null，则返回test变量的值
+--------------------+
| ifnull(@test, 100) |
+--------------------+
|                200 |  -- test不为null。返回test的值200
+--------------------+


--
-- select into 用法
--
mysql> select @id_1;
+-------+
| @id_1 |
+-------+
|  NULL | -- 当前变量id_1为null 
+-------+


mysql> select @score_1;
+----------+
| @score_1 |
+----------+
|     NULL |  -- 当前变量score_1为null
+----------+


mysql> select * from test_rank_2;
+------+-------+
| id   | score |
+------+-------+
|    1 |    10 |
|    2 |    20 |
|    3 |    30 |
|    4 |    30 |
|    5 |    40 |
|    6 |    40 |
+------+-------+


mysql> select * from test_rank_2 
    -> where id=1 into @id_1, @score_1;
-- 选择id=1的记录，将对应的id和score赋值给变量 id_1 和 score_1


mysql> select @id_1;
+-------+
| @id_1 |
+-------+
|     1 |
+-------+

mysql> select @score_1;
+----------+
| @score_1 |
+----------+
|       10 |
+----------+

-- 触发器对性能会有影响，相当于在一个事物中插入了其他的事物
```

-----

# 十四、day014：存储过程 自定义函数MySQL 执行计划与优化器


## 1. 存储过程
### 1.1 存储过程介绍:
#### 1) 存储在数据库端的一组SQL语句集；
#### 2) 用户可以通过存储过程名和传参多次调用的程序模块；
### 1.2 存储过程的特点：
#### 1) 使用灵活，可以使用流控语句、自定义变量等完成复杂的业务逻辑；
#### 2) 提高数据安全性，屏蔽应用程序直接对表的操作，易于进行审计；
#### 3) 减少网络传输；//因为只需要调用就好
#### 4) 提高代码维护的复杂度，实际使用需要结合业务评估；

```sql

-- 老师给出的例子， 阶乘
mysql> create table test_proc_1(a int, b int); -- 给一个存放数据的表

mysql> delimiter //
mysql> create procedure proc_test1(in total int, out res int)
    -> begin
    ->     declare i int;
    ->     set i := 1;
    ->     set res := 1;
    ->     if total <= 0 then
    ->         set total := 1;
    ->     end if;
    ->     while i <= total do
    ->         set res := res * i;
    ->         insert into test_proc_1 values(i, res);
    ->         set i := i + 1;
    ->     end while;
    -> end;//

mysql> delimiter ;

mysql> set @res_value := 0;  --set @a=0;  set @a:=0;这两个在赋值的时候无区别，但在判断的时候case，前者表示是否相等，后者是赋值


mysql> call proc_test1(5, @res_value); -- 因为res是out变量，要预先有这个变量，这里上面设置了res_value(实参和形参不必同名)

mysql> select @res_value;
+------------+
| @res_value |
+------------+
|        120 | -- 5的阶乘的结果是120
+------------+

mysql> select * from test_proc_1;
+------+------+
| a    | b    |
+------+------+
|    1 |    1 |
|    2 |    2 |
|    3 |    6 |
|    4 |   24 |
|    5 |  120 |  -- 每次insert的结果
+------+------+
```

-----

## 2. 自定义函数
### 2.1 自定义函数和存储过程很类似，但是必须要有返回值；
### 2.2 与内置的函数(sum(), max()等)使用方法类似: select fun(val);
### 2.3 自定义函数可能在遍历每条记录中使用；

```sql
CREATE
    [DEFINER = { user | CURRENT_USER }]
    FUNCTION sp_name ([func_parameter[,...]])
    RETURNS type  -- 必须有返回值
    [characteristic ...] routine_body
    
func_parameter:
    param_name type

type:
    Any valid MySQL data type

characteristic:
    COMMENT 'string'
  | LANGUAGE SQL
  | [NOT] DETERMINISTIC
  | { CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA }
  | SQL SECURITY { DEFINER | INVOKER }

routine_body:
    Valid SQL routine statement
    
-- 删除
DROP FUNCTION fun_name;
```

```sql
-- 老师给的例子，还是阶乘，用自定义函数的方式

mysql> delimiter //
mysql> 
mysql> create function fun_test_1(total int)
    -> returns int
    -> begin
    ->     declare i int;
    ->     declare res int;
    ->     set i := 1;
    ->     set res := 1;
    ->     if total <= 0 then
    ->         set total := 1;
    ->     end if;
    ->     while i <= total do
    ->         set res := res * i;
    ->         set i := i + 1;
    ->     end while;
    ->     return res;show 
    -> end;//
ERROR 1418 (HY000): This function has none of DETERMINISTIC, NO SQL, or READS SQL DATA in its declaration and binary logging is enabled (you *might* want to use the less safe log_bin_trust_function_creators variable)
-- 报错，提示因为函的声明中没有"DETERMINISTIC, NO SQL, or READS SQL DATA"等关键字 ，需要使用打开参数 log_bin_trust_function_creators

-- 解决方法，set global log_bin_trust_function_creators=1; 开启该选项可能会引起主从服务器不一致
--           或者 增加 上述相应功能的关键字


-- 使用 deterministic 关键字
-- 当你声明一个函数的返回是确定性的，则必须显示的使用deterministic关键字，默认是 no deterministic的
mysql> delimiter //
mysql> create function fun_test_1(total int)
    -> returns int deterministic -- 这个只是告诉MySQL我这个函数是否会改变数据
                                 -- 即使我下面使用了insert，update等DML语句，MySQL不会检查
                                 -- 函数是否会改变数据，完全依赖创建函数的用户去指定的关键字
                                 -- 而非真的是否有修改数据
                                 -- 只是声明，而非约束
    -> begin
    ->     declare i int;
    ->     declare res int;
    ->     set i := 1;
    ->     set res := 1;
    ->     if total <= 0 then
    ->         set total := 1;
    ->     end if;
    ->     while i <= total do
    ->         set res := res * i;
    ->         insert into test_proc_1 values(i, res);  -- 在自定义函数中，同样可以使用sql
                                                        -- 并且该SQL是insert，其实和deterministic违背。
    ->         set i := i + 1;
    ->     end while;
    ->     return res;
    -> end;//seles

mysql> delimiter ;

mysql> truncate table test_proc_1;

mysql> select fun_test_1(6);  -- return了6的阶乘，720
+---------------+
| fun_test_1(6) |
+---------------+
|           720 |
+---------------+

mysql> select * from test_proc_1; 
+------+------+
| a    | b    |
+------+------+
|    1 |    1 |
|    2 |    2 |
|    3 |    6 |
|    4 |   24 |
|    5 |  120 |
|    6 |  720 |  -- 使用了insert语句进行插入阶乘的历史记录
+------+------+


-- 关键字简单说明
-- DETERMINISTIC ： 当给定相同的输入，产生确定的结果
-- NOT DETERMINISTIC ： 默认值，认为产生的结果是不确定的

-- READS SQL DATA  ： 只是读取SQL数据
-- MODIFIES SQL DATA ： 会修改数据
-- NO SQL ： 没有SQL遇见
-- CONTAINS SQL ： 包含SQL语句，但是没有读写语句，理论有select now()等

```

# 十五、day015-索引 B+树 上


## 三. B树/B+树

>**注意:B树和B+树开头的`B`不是Binary，而是`Balance`**

### 1. B树的定义
M 阶 B 树的定义：

* 每个节点最多有M个孩子；
* 除了root节点外，每个非叶子(non-leaf)节点至少含有(M/2)个孩子；
* 如果root节点不为空，则root节点至少要有两个孩子节点；
* 一个非叶子(non-leaf)节点如果含有K个孩子，则包含k-1个keys；
* 所有叶子节点都在同一层；
* B树中的非叶子(non-leaf)节点也包含了数据部分；

### 2. B+树的定义
在B树的基础上，B+树做了如下改进

* 数据只存储在叶子节点上，非叶子节点只保存索引信息；
    - 非叶子节点（索引节点）存储的只是一个Flag，不保存实际数据记录；
    - 索引节点指示该节点的左子树比这个Flag小，而右子树大于等于这个Flag
* 叶子节点本身按照数据的升序排序进行链接(串联起来)；
    - 叶子节点中的数据在`物理存储上是无序`的，仅仅是在`逻辑上有序`（通过指针串在一起）；


### 3. B+树的操作

* **B+树的插入**
B+树的插入必须保证插入后叶子节点中的记录依然排序。

    - 插入操作步骤*（引用自姜老师的书《MySQL技术内幕：InnoDB存储引擎（第2版）》 第5.3.1小节）*
    ![B+ Insert](https://github.com/zhaojing5340126/interview/blob/master/mysql%E9%99%84%E4%BB%B6/MySQL%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0(Day015--Day016)_md/B+_insert.jpg?raw=true)

    >B+树总是会保持平衡。但是为了保持平衡对于新插入的键值可能需要做大量的拆分页（split）操作；部分情况下可以通过B+树的旋转来替代拆分页操作，进而达到平衡效果。

* **B+树的删除**
B+树使用填充因子（fill factor）来控制树的删除变化，50%是填充因子可设的最小值。B+树的删除操作同样必须保证删除后叶子节点中的记录依然排序。与插入不同的是，删除根据填充因子的变化来衡量。
    - 删除操作步骤*（引用自姜老师的书《MySQL技术内幕：InnoDB存储引擎（第2版）》 第5.3.2小节）*
    ![B+ Delete](https://github.com/zhaojing5340126/interview/blob/master/mysql%E9%99%84%E4%BB%B6/MySQL%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0(Day015--Day016)_md/B+_delete.jpg?raw=true)


### 3. B+树的扇出(fan out)
* **B+树图例**
![B+树的例子](https://github.com/zhaojing5340126/interview/blob/master/mysql%E9%99%84%E4%BB%B6/MySQL%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0(Day015--Day016)_md/B+Example.jpg?raw=true)

    * 该 B+ 树高度为 2
    * 每叶子页（LeafPage）4条记录
    * `扇出数为5`
    * 叶子节点(LeafPage)由小到大（有序）串联在一起

>`扇出`是每个索引节点(Non-LeafPage)指向每个叶子节点(LeafPage)的指针
【上层结点指向下一层结点的指针就叫扇出】
>`扇出数` = 索引节点(Non-LeafPage)可存储的最大关键字个数 + 1
>图例中的索引节点(Non-LeafPage)最大可以存放4个关键字，但实际使用了3个；

### 4. B+树存储数据举例
假设B+树中页的大小是16K，每行记录是200Byte大小，求出树的高度为1，2，3，4时，分别可以存储多少条记录。

* 查看数据表中每行记录的平均大小

    ```sql
    mysql> show table status like "%employees%"\G
    *************************** 1. row ***************************
               Name: employees
             Engine: InnoDB
            Version: 10
         Row_format: Dynamic
               Rows: 298124
     Avg_row_length: 51   -- 平均长度为51个字节
        Data_length: 15245312
    Max_data_length: 0
       Index_length: 0
          Data_free: 0
     Auto_increment: NULL
        Create_time: 2015-12-02 21:32:02
        Update_time: NULL
         Check_time: NULL
          Collation: utf8mb4_general_ci
           Checksum: NULL
     Create_options: 
            Comment: 
    1 row in set (0.00 sec)
    ```

* **高度为1**
    - 16K/200B 约等于 80 个记录（数据结构元信息如指针等忽略不计）

* **高度为2**
非叶子节点中存放的仅仅是一个索引信息，包含了`Key`和`Point`指针；`Point`指针在MySQL中固定为`6Byte`。而`Key`我们这里假设为`8Byte`，则单个索引信息即为14个字节，`KeySize = 14Byte`
    - 高度为2，即有一个索引节点（索引页），和N个叶子节点
    - 一个索引节点可以存放 16K / KeySize = 16K / 14B = 1142个索引信息，即有（1142 + 1）个扇出，以及有（1142 + 1）个叶子节点（数据页）  *（可以简化为1000）*
    - 数据记录数 = （16K / KeySize + 1）x (16K / 200B) 约等于 80W 个记录

* **高度为3**
高度为3的B+树，即ROOT节点有1000个扇出，每个扇出又有1000个扇出指向叶子节点。每个节点是80个记录，所以一共有 8000W个记录


* **高度为4**
同高度3一样，高度为4时的记录书为（8000 x 1000）W

>上述的8000W等数据只是一个理论值。因为线上实际使用单个页的记录数字要乘以70%，即第二层需要70% x 70% ，依次类推。【一个页实际只使用70%】

>因此在数据库中，B+树的高度一般都在2～4层，这也就是说查找某一键值的行记录时最多只需要2到4次IO，2～4次的IO意味着查询时间只需0.02～0.04秒（假设IOPS=100，当前SSD可以达到50000IOPS，IOPS：IO per sec:每秒多少次IO）。【有公司把慢查询设置为0.5秒（但还是应该设的稍大一些），即约莫走50次IO，他认为这样的查询没有走索引，是不正常的】

从5.7开始，页的预留大小可以设置了，以减少split操作的概率（因为假如数据要原地更新，占用更多的空间，那么你预留的空间就可以用上了，而不用split，所以原地更新操作很多的话，可以把预留大小设置的很多，比如快递公司，即空间换时间）

-----


## 四. MySQL索引
### 1. MySQL 创建索引
>[ALTER TABLE 方式](http://dev.mysql.com/doc/refman/5.7/en/alter-table.html)
>[CREATE INDEX 方式](http://dev.mysql.com/doc/refman/5.7/en/create-index.html)

```sql
--
-- ALTER TABLE
--
mysql> create table test_index_1(a int, b int , c int);
Query OK, 0 rows affected (0.20 sec)

mysql> show create table test_index_1\G
*************************** 1. row ***************************
       Table: test_index_1
Create Table: CREATE TABLE `test_index_1` (
  `a` int(11) DEFAULT NULL,
  `b` int(11) DEFAULT NULL,
  `c` int(11) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4
1 row in set (0.00 sec)

mysql> insert into test_index_1 values
    -> (1,10,100),(2,20,200),
    -> (3,30,300),(4,40,400);
Query OK, 4 rows affected (0.03 sec)
Records: 4  Duplicates: 0  Warnings: 0

mysql> select * from test_index_1;
+------+------+------+
| a    | b    | c    |
+------+------+------+
|    1 |   10 |  100 |
|    2 |   20 |  200 |
|    3 |   30 |  300 |
|    4 |   40 |  400 |
+------+------+------+
4 rows in set (0.00 sec)

mysql> explain select * from test_index_1 where a=3\G  -- 看执行计划，使用的是扫描整张表的方式
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: test_index_1
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 4
     filtered: 25.00
        Extra: Using where
1 row in set, 1 warning (0.00 sec)

-- 给字段a 增加索引
mysql> alter table test_index_1 add index idx_a (a);  -- 给字段a添加索引。索引名为idx_a
Query OK, 0 rows affected (0.15 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> explain select * from test_index_1 where a=3\G  -- 看执行计划，使用的key为idx_a，走了索引
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: test_index_1
   partitions: NULL
         type: ref
possible_keys: idx_a
          key: idx_a
      key_len: 5
          ref: const
         rows: 1
     filtered: 100.00
        Extra: NULL
1 row in set, 1 warning (0.00 sec)

-- 使用create index
mysql> explain select * from test_index_1 where b=30\G  -- 同样b字段也没有索引
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: test_index_1
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 4
     filtered: 25.00
        Extra: Using where
1 row in set, 1 warning (0.00 sec)

-- 给b字段增加索引
mysql> create index idx_b on test_index_1 (b);
Query OK, 0 rows affected (0.14 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> explain select * from test_index_1 where b=30\G  -- 查看执行计划，使用的key为idx_b，走了索引
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: test_index_1
   partitions: NULL
         type: ref
possible_keys: idx_b
          key: idx_b
      key_len: 5
          ref: const
         rows: 1
     filtered: 100.00
        Extra: NULL
1 row in set, 1 warning (0.00 sec)
```

### 2. MySQL 查看索引
```sql
--
-- 方式一
--
mysql> desc orders;
+-----------------+-------------+------+-----+---------+-------+
| Field           | Type        | Null | Key | Default | Extra |
+-----------------+-------------+------+-----+---------+-------+
| o_orderkey      | int(11)     | NO   | PRI | NULL    |       | -- 索引
| o_custkey       | int(11)     | YES  | MUL | NULL    |       | -- 索引
| o_orderstatus   | char(1)     | YES  |     | NULL    |       |
| o_totalprice    | double      | YES  |     | NULL    |       |
| o_orderDATE     | date        | YES  | MUL | NULL    |       | -- 索引
| o_orderpriority | char(15)    | YES  |     | NULL    |       |
| o_clerk         | char(15)    | YES  |     | NULL    |       |
| o_shippriority  | int(11)     | YES  |     | NULL    |       |
| o_comment       | varchar(79) | YES  |     | NULL    |       |
+-----------------+-------------+------+-----+---------+-------+
9 rows in set (0.00 sec)

--
-- 方式二
--
mysql> show create table orders\G
*************************** 1. row ***************************
       Table: orders
Create Table: CREATE TABLE `orders` (
  `o_orderkey` int(11) NOT NULL,
  `o_custkey` int(11) DEFAULT NULL,
  `o_orderstatus` char(1) DEFAULT NULL,
  `o_totalprice` double DEFAULT NULL,
  `o_orderDATE` date DEFAULT NULL,
  `o_orderpriority` char(15) DEFAULT NULL,
  `o_clerk` char(15) DEFAULT NULL,
  `o_shippriority` int(11) DEFAULT NULL,
  `o_comment` varchar(79) DEFAULT NULL,
  PRIMARY KEY (`o_orderkey`),  -- 索引
  KEY `i_o_orderdate` (`o_orderDATE`), -- 索引
  KEY `i_o_custkey` (`o_custkey`),  -- 索引
  CONSTRAINT `orders_ibfk_1` FOREIGN KEY (`o_custkey`) REFERENCES `customer` (`c_custkey`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1
1 row in set (0.00 sec)


--
-- 方式三
--
mysql> show index from orders\G
*************************** 1. row ***************************
        Table: orders
   Non_unique: 0        -- 表示唯一的 
     Key_name: PRIMARY  -- key的name是primary
 Seq_in_index: 1
  Column_name: o_orderkey
    Collation: A
  Cardinality: 1306748  -- 基数，这个列上不同值的记录数，通过采样来预估出来的
     Sub_part: NULL
       Packed: NULL
         Null: 
   Index_type: BTREE    -- 索引类型是BTree
      Comment: 
Index_comment: 
*************************** 2. row ***************************
        Table: orders
   Non_unique: 1       -- Non_unique为True，表示不唯一
     Key_name: i_o_orderdate
 Seq_in_index: 1
  Column_name: o_orderDATE
    Collation: A
  Cardinality: 2405
     Sub_part: NULL
       Packed: NULL
         Null: YES
   Index_type: BTREE
      Comment: 
Index_comment: 
*************************** 3. row ***************************
        Table: orders
   Non_unique: 1
     Key_name: i_o_custkey
 Seq_in_index: 1
  Column_name: o_custkey
    Collation: A
  Cardinality: 95217
     Sub_part: NULL
       Packed: NULL
         Null: YES
   Index_type: BTREE
      Comment: 
Index_comment: 
3 rows in set (0.00 sec)

mysql>  select count(*) from orders; 
+----------+
| count(*) |
+----------+
|  1500000 |  -- orders中有150W条记录，和Cardinality 是不一致的
+----------+
1 row in set (0.25 sec)
```

### 3. Cardinality（基数）

`Cardinality`表示该索引列上有多少`不同的记录`，这个是一个`预估的值`，是采样得到的（由InnoDB触发，采样20个页，进行预估），该值`越大越好`，即当`Cardinality / RowNumber 越接近1越好`。表示该列是`高选择性的`。高选择性才有必要创建索引。

- 高选择性：身份证 、手机号码、姓名、订单号等
- 低选择性：性别、年龄等

**即该列是否适合创建索引，就看该字段是否具有高选择性**


```sql
mysql>  show create table lineitem\G
*************************** 1. row ***************************
       Table: lineitem
Create Table: CREATE TABLE `lineitem` (
  `l_orderkey` int(11) NOT NULL,
  `l_partkey` int(11) DEFAULT NULL,
  `l_suppkey` int(11) DEFAULT NULL,
  `l_linenumber` int(11) NOT NULL,
  `l_quantity` double DEFAULT NULL,
  `l_extendedprice` double DEFAULT NULL,
  `l_discount` double DEFAULT NULL,
  `l_tax` double DEFAULT NULL,
  `l_returnflag` char(1) DEFAULT NULL,
  `l_linestatus` char(1) DEFAULT NULL,
  `l_shipDATE` date DEFAULT NULL,
  `l_commitDATE` date DEFAULT NULL,
  `l_receiptDATE` date DEFAULT NULL,
  `l_shipinstruct` char(25) DEFAULT NULL,
  `l_shipmode` char(10) DEFAULT NULL,
  `l_comment` varchar(44) DEFAULT NULL,
  PRIMARY KEY (`l_orderkey`,`l_linenumber`),  -- 两个列作为primary
  KEY `i_l_shipdate` (`l_shipDATE`),
  KEY `i_l_suppkey_partkey` (`l_partkey`,`l_suppkey`),
  KEY `i_l_partkey` (`l_partkey`),
  KEY `i_l_suppkey` (`l_suppkey`),
  KEY `i_l_receiptdate` (`l_receiptDATE`),
  KEY `i_l_orderkey` (`l_orderkey`),
  KEY `i_l_orderkey_quantity` (`l_orderkey`,`l_quantity`),
  KEY `i_l_commitdate` (`l_commitDATE`),
  CONSTRAINT `lineitem_ibfk_1` FOREIGN KEY (`l_orderkey`) REFERENCES `orders` (`o_orderkey`),
  CONSTRAINT `lineitem_ibfk_2` FOREIGN KEY (`l_partkey`, `l_suppkey`) REFERENCES `partsupp` (`ps_partkey`, `ps_suppkey`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1
1 row in set (0.00 sec)

mysql>  show index from lineitem\G  -- 省略其他输出，只看PRIMARY
*************************** 1. row ***************************
        Table: lineitem
   Non_unique: 0
     Key_name: PRIMARY
 Seq_in_index: 1  -- 索引中的顺序，该列的顺序为1
  Column_name: l_orderkey
    Collation: A
  Cardinality: 1416486  -- 约140W
     Sub_part: NULL
       Packed: NULL
         Null: 
   Index_type: BTREE
      Comment: 
Index_comment: 
*************************** 2. row ***************************
        Table: lineitem
   Non_unique: 0
     Key_name: PRIMARY
 Seq_in_index: 2  -- 索引中的顺序，该列的顺序为2
  Column_name: l_linenumber
    Collation: A
  Cardinality: 5882116  -- 约580W
     Sub_part: NULL
       Packed: NULL
         Null: 
   Index_type: BTREE
      Comment: 
Index_comment:
```
>**对应当前例子**
第一个索引（Seq_in_index = 1）的`Cardinality`的值表示`当前列（l_orderkey）`的不重复的值，
第二个索引（Seq_in_index = 2）的`Cardinality`的值表示`前两列（l_orderkey）和（l_linenumber）`不重复的值

```sql
--
-- SQL-1
--
mysql> select * from lineitem limit 10;                                                                 
+------------+-----------+-----------+--------------+------------+-----------------+------------+-------+--------------+--------------+------------+--------------+---------------+-------------------+------------+----------------------------------------+
| l_orderkey | l_partkey | l_suppkey | l_linenumber | l_quantity | l_extendedprice | l_discount | l_tax | l_returnflag | l_linestatus | l_shipDATE | l_commitDATE | l_receiptDATE | l_shipinstruct    | l_shipmode | l_comment                              |
+------------+-----------+-----------+--------------+------------+-----------------+------------+-------+--------------+--------------+------------+--------------+---------------+-------------------+------------+----------------------------------------+
|          1 |    155190 |      7706 |            1 |         17 |        21168.23 |       0.04 |  0.02 | N            | O            | 1996-03-13 | 1996-02-12   | 1996-03-22    | DELIVER IN PERSON | TRUCK      | blithely regular ideas caj             |
|          1 |     67310 |      7311 |            2 |         36 |        45983.16 |       0.09 |  0.06 | N            | O            | 1996-04-12 | 1996-02-28   | 1996-04-20    | TAKE BACK RETURN  | MAIL       | slyly bold pinto beans detect s        |
|          1 |     63700 |      3701 |            3 |          8 |         13309.6 |        0.1 |  0.02 | N            | O            | 1996-01-29 | 1996-03-05   | 1996-01-31    | TAKE BACK RETURN  | REG AIR    | deposits wake furiously dogged,        |
|          1 |      2132 |      4633 |            4 |         28 |        28955.64 |       0.09 |  0.06 | N            | O            | 1996-04-21 | 1996-03-30   | 1996-05-16    | NONE              | AIR        | even ideas haggle. even, bold reque    |
|          1 |     24027 |      1534 |            5 |         24 |        22824.48 |        0.1 |  0.04 | N            | O            | 1996-03-30 | 1996-03-14   | 1996-04-01    | NONE              | FOB        | carefully final gr                     |
|          1 |     15635 |       638 |            6 |         32 |        49620.16 |       0.07 |  0.02 | N            | O            | 1996-01-30 | 1996-02-07   | 1996-02-03    | DELIVER IN PERSON | MAIL       | furiously regular accounts haggle bl   |
|          2 |    106170 |      1191 |            1 |         38 |        44694.46 |          0 |  0.05 | N            | O            | 1997-01-28 | 1997-01-14   | 1997-02-02    | TAKE BACK RETURN  | RAIL       | carefully ironic platelets against t   |
|          3 |      4297 |      1798 |            1 |         45 |        54058.05 |       0.06 |     0 | R            | F            | 1994-02-02 | 1994-01-04   | 1994-02-23    | NONE              | AIR        | blithely s                             |
|          3 |     19036 |      6540 |            2 |         49 |        46796.47 |        0.1 |     0 | R            | F            | 1993-11-09 | 1993-12-20   | 1993-11-24    | TAKE BACK RETURN  | RAIL       | final, regular pinto                   |
|          3 |    128449 |      3474 |            3 |         27 |        39890.88 |       0.06 |  0.07 | A            | F            | 1994-01-16 | 1993-11-22   | 1994-01-23    | DELIVER IN PERSON | SHIP       | carefully silent pinto beans boost fur |
+------------+-----------+-----------+--------------+------------+-----------------+------------+-------+--------------+--------------+------------+--------------+---------------+-------------------+------------+----------------------------------------+
10 rows in set (0.00 sec)

--
-- SQL-2
--
mysql> select l_orderkey, l_linenumber from lineitem limit 10; 
+------------+--------------+
| l_orderkey | l_linenumber |
+------------+--------------+
|     721220 |            2 |
|     842980 |            4 |
|     904677 |            1 |
|     990147 |            1 |
|    1054181 |            1 |
|    1111877 |            3 |
|    1332613 |            1 |
|    1552449 |            2 |
|    2167527 |            3 |
|    2184032 |            5 |
+------------+--------------+
10 rows in set (0.00 sec)

--- SQL-1和SQL-2其实都是在没有排序的情况下，取出前10条数据。但是结果不一样

--
-- SQL-3
--
mysql> select l_orderkey, l_linenumber from lineitem order by l_orderkey limit 10;  -- 和上面的sql相比，多了一个order by的操作
+------------+--------------+
| l_orderkey | l_linenumber |
+------------+--------------+
|          1 |            1 | -----
|          1 |            2 | -- 看orderkey为1，对应的linenumber有6条
|          1 |            3 | -- 这就是orderkey的Cardinality仅为140W
|          1 |            4 | -- 而(orderkey + linenumber)就有580W
|          1 |            5 | 
|          1 |            6 | -----
|          2 |            1 |
|          3 |            1 |
|          3 |            2 |
|          3 |            3 |
+------------+--------------+
10 rows in set (0.01 sec)

--- SQL-3 和SQL-2 不同的原因是 他们走了不同的索引
mysql> explain select l_orderkey, l_linenumber from lineitem limit 10\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: lineitem
   partitions: NULL
         type: index
possible_keys: NULL
          key: i_l_shipdate  -- 使用了shipdate进行了索引
      key_len: 4
          ref: NULL
         rows: 5882306
     filtered: 100.00
        Extra: Using index
1 row in set, 1 warning (0.00 sec)


mysql> explain select l_orderkey, l_linenumber from lineitem order by l_orderkey limit 10\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: lineitem
   partitions: NULL
         type: index
possible_keys: NULL
          key: i_l_orderkey  -- 使用了orderkey进行了查询
      key_len: 4
          ref: NULL
         rows: 10
     filtered: 100.00
        Extra: Using index
1 row in set, 1 warning (0.00 sec)

mysql> explain select * from lineitem limit 10\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: lineitem
   partitions: NULL
         type: ALL   -- SQL-1进行了全表扫描
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 5882306
     filtered: 100.00
        Extra: NULL
1 row in set, 1 warning (0.00 sec)

-- 所以，不使用order by取出的结果，可以理解为不是根据主键排序的结果。
```
>`innodb_on_state = off `
在MySQL5.5之前，执行`show create table`操作会触发采样，而5.5之后将该参数off后，需要主动执行`analyze table`才会去采样。采样不会锁表或者锁记录。


### 4. 复合索引

(a,b,c)复合索引快速的原因是B+树已经对索引排序过了，（1，1,1），（1,1，2），（1,2,1）（2,1,1）....  对（a,b,c）排序，但没对（b,c)这些排序，比如（1,1），（1,2），（2,1），（1,1）
```sql
mysql>  show create table lineitem\G
*************************** 1. row ***************************
       Table: lineitem
Create Table: CREATE TABLE `lineitem` (
  `l_orderkey` int(11) NOT NULL,
  `l_partkey` int(11) DEFAULT NULL,
  `l_suppkey` int(11) DEFAULT NULL,
  `l_linenumber` int(11) NOT NULL,
  `l_quantity` double DEFAULT NULL,
  `l_extendedprice` double DEFAULT NULL,
  `l_discount` double DEFAULT NULL,
  `l_tax` double DEFAULT NULL,
  `l_returnflag` char(1) DEFAULT NULL,
  `l_linestatus` char(1) DEFAULT NULL,
  `l_shipDATE` date DEFAULT NULL,
  `l_commitDATE` date DEFAULT NULL,
  `l_receiptDATE` date DEFAULT NULL,
  `l_shipinstruct` char(25) DEFAULT NULL,
  `l_shipmode` char(10) DEFAULT NULL,
  `l_comment` varchar(44) DEFAULT NULL,
  PRIMARY KEY (`l_orderkey`,`l_linenumber`),  -- 两个列作为primary，这个就是复合索引【多个列组成】
  KEY `i_l_shipdate` (`l_shipDATE`),
  KEY `i_l_suppkey_partkey` (`l_partkey`,`l_suppkey`),
  KEY `i_l_partkey` (`l_partkey`),
  KEY `i_l_suppkey` (`l_suppkey`),
  KEY `i_l_receiptdate` (`l_receiptDATE`),
  KEY `i_l_orderkey` (`l_orderkey`),
  KEY `i_l_orderkey_quantity` (`l_orderkey`,`l_quantity`),
  KEY `i_l_commitdate` (`l_commitDATE`),
  CONSTRAINT `lineitem_ibfk_1` FOREIGN KEY (`l_orderkey`) REFERENCES `orders` (`o_orderkey`),
  CONSTRAINT `lineitem_ibfk_2` FOREIGN KEY (`l_partkey`, `l_suppkey`) REFERENCES `partsupp` (`ps_partkey`, `ps_suppkey`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1
1 row in set (0.00 sec)

--
-- 复合索引举例
-- 
mysql> create table test_index_2(a int, b int , c int);
Query OK, 0 rows affected (0.15 sec)

mysql> alter table test_index_2 add index  idx_mul_ab (a, b);        
Query OK, 0 rows affected (0.11 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> insert into test_index_2 values
    -> (1,1,10),
    -> (1,2,9),
    -> (2,1,8),
    -> (2,4,15),
    -> (3,1,6),
    -> (3,2,17);
Query OK, 6 rows affected (0.04 sec)
Records: 6  Duplicates: 0  Warnings: 0

mysql> select * from test_index_2 where a = 1;
+------+------+------+
| a    | b    | c    |
+------+------+------+
|    1 |    1 |   10 |
|    1 |    2 |    9 |
+------+------+------+
2 rows in set (0.00 sec)

mysql> explain select * from test_index_2 where a = 1\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: test_index_2
   partitions: NULL
         type: ref
possible_keys: idx_mul_ab
          key: idx_mul_ab   -- 走了索引
      key_len: 5
          ref: const
         rows: 2
     filtered: 100.00
        Extra: NULL
1 row in set, 1 warning (0.00 sec)

mysql> select * from test_index_2 where a = 1 and b = 2;
+------+------+------+
| a    | b    | c    |
+------+------+------+
|    1 |    2 |    9 |
+------+------+------+
1 row in set (0.00 sec)

mysql> explain select * from test_index_2 where a = 1 and b = 2\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: test_index_2
   partitions: NULL
         type: ref   -- 此时也是走了索引
possible_keys: idx_mul_ab
          key: idx_mul_ab  
      key_len: 10
          ref: const,const
         rows: 1
     filtered: 100.00
        Extra: NULL
1 row in set, 1 warning (0.00 sec)

mysql> explain select * from test_index_2 where b = 2\G  -- 只查询b          
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: test_index_2
   partitions: NULL
         type: ALL  -- 没有使用索引
possible_keys: NULL  
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 6
     filtered: 16.67
        Extra: Using where
1 row in set, 1 warning (0.00 sec)


mysql> explain select * from test_index_2 where a=1 or b = 2\G  -- 使用or，要求结果集是并集，a的扫描+b的扫描，所以没有走索引
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: test_index_2
   partitions: NULL
         type: ALL  -- 没有使用索引，因为b没有索引，所以b是走全表扫描的，既然走扫描，a的值也可以一起过滤
                    -- 就没有必要在去查一次 a 的索引了
possible_keys: idx_mul_ab
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 6
     filtered: 30.56
        Extra: Using where
1 row in set, 1 warning (0.00 sec)

--
-- 特别的例子
--
---- 还是只使用b列去做范围查询，发现是走索引了
---- 注意查询的是 count(*)
mysql> explain select count(*) from test_index_2 where  b >1 and b < 3\G 
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: test_index_2
   partitions: NULL
         type: index  -- 走了索引 
possible_keys: NULL
          key: idx_mul_ab
      key_len: 10
          ref: NULL
         rows: 6
     filtered: 16.67
        Extra: Using where; Using index  -- 覆盖索引
1 row in set, 1 warning (0.00 sec)

-- 因为要求的是count(*), 要求所有的记录的和，
-- 那索引a是包含了全部的记录的，即扫描（a,b)的索引也是可以得到count(*)的

mysql> explain select * from test_index_2 where  b >1 and b < 3\G        
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: test_index_2
   partitions: NULL
         type: ALL  -- 查询 * 就没法使用（a，b）索引了，需要全表扫描b的值。
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 6
     filtered: 16.67
        Extra: Using where
1 row in set, 1 warning (0.00 sec)


mysql> explain select * from test_index_2 where  a = 1 and c = 10\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: test_index_2
   partitions: NULL
         type: ref  -- 也是走索引的，先用走a索引得到结果集，再用c=10去过滤
possible_keys: idx_mul_ab
          key: idx_mul_ab
      key_len: 5
          ref: const
         rows: 2
     filtered: 16.67
        Extra: Using where
1 row in set, 1 warning (0.00 sec)

mysql> explain select * from test_index_2 where  b = 2 and c = 10\G   
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: test_index_2
   partitions: NULL
         type: ALL  -- 而b和c是不行的，(b，c)不是有序的
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 6
     filtered: 16.67
        Extra: Using where
1 row in set, 1 warning (0.00 sec)
```

-----

## 五. information_schema（一）

* 作用：数据字典的作用，表是怎么定义的，列是怎么定义的

```sql
mysql> use information_schema;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed

mysql> show tables;
+---------------------------------------+
| Tables_in_information_schema          |
+---------------------------------------+
| CHARACTER_SETS                        |
| COLLATIONS                            |
| COLLATION_CHARACTER_SET_APPLICABILITY |
| COLUMNS                               |
| COLUMN_PRIVILEGES                     |
| ENGINES                               |
| EVENTS                                |
| FILES                                 |
| GLOBAL_STATUS                         |
| GLOBAL_VARIABLES                      |
| KEY_COLUMN_USAGE                      |
| OPTIMIZER_TRACE                       |
| PARAMETERS                            |
| PARTITIONS                            |
| PLUGINS                               |
| PROCESSLIST                           |
| PROFILING                             |
| REFERENTIAL_CONSTRAINTS               |
| ROUTINES                              |
| SCHEMATA                              |
| SCHEMA_PRIVILEGES                     |
| SESSION_STATUS                        |
| SESSION_VARIABLES                     |
| STATISTICS                            |
| TABLES                                | //存放了所有表的信息，类似于 show table status like ""; 
| TABLESPACES                           |
| TABLE_CONSTRAINTS                     |
| TABLE_PRIVILEGES                      |
| TRIGGERS                              |
| USER_PRIVILEGES                       |
| VIEWS                                 | //表创建的信息，相当于 show create table employees.vRank\G
| INNODB_LOCKS                          |
| INNODB_TRX                            |
| INNODB_SYS_DATAFILES                  |
| INNODB_FT_CONFIG                      |
| INNODB_SYS_VIRTUAL                    |
| INNODB_CMP                            |
| INNODB_FT_BEING_DELETED               |
| INNODB_CMP_RESET                      |
| INNODB_CMP_PER_INDEX                  |
| INNODB_CMPMEM_RESET                   |
| INNODB_FT_DELETED                     |
| INNODB_BUFFER_PAGE_LRU                |
| INNODB_LOCK_WAITS                     |
| INNODB_TEMP_TABLE_INFO                |
| INNODB_SYS_INDEXES                    |
| INNODB_SYS_TABLES                     |
| INNODB_SYS_FIELDS                     |
| INNODB_CMP_PER_INDEX_RESET            |
| INNODB_BUFFER_PAGE                    |
| INNODB_FT_DEFAULT_STOPWORD            |
| INNODB_FT_INDEX_TABLE                 |
| INNODB_FT_INDEX_CACHE                 |
| INNODB_SYS_TABLESPACES                |
| INNODB_METRICS                        |
| INNODB_SYS_FOREIGN_COLS               |
| INNODB_CMPMEM                         |
| INNODB_BUFFER_POOL_STATS              |
| INNODB_SYS_COLUMNS                    |
| INNODB_SYS_FOREIGN                    |
| INNODB_SYS_TABLESTATS                 |
+---------------------------------------+
61 rows in set (0.00 sec)

-- information_schema 数据库相当于一个数据字典。保存了表的元信息。

mysql> select * from key_column_usage limit  3\G  -- 显示了哪个索引使用了哪个列
*************************** 1. row ***************************
           CONSTRAINT_CATALOG: def
            CONSTRAINT_SCHEMA: burn_test
              CONSTRAINT_NAME: PRIMARY
                TABLE_CATALOG: def
                 TABLE_SCHEMA: burn_test
                   TABLE_NAME: Orders -- 表名
                  COLUMN_NAME: order_id  -- 索引的名称
             ORDINAL_POSITION: 1
POSITION_IN_UNIQUE_CONSTRAINT: NULL
      REFERENCED_TABLE_SCHEMA: NULL
        REFERENCED_TABLE_NAME: NULL
       REFERENCED_COLUMN_NAME: NULL
*************************** 2. row ***************************
           CONSTRAINT_CATALOG: def
            CONSTRAINT_SCHEMA: burn_test
              CONSTRAINT_NAME: product_name
                TABLE_CATALOG: def
                 TABLE_SCHEMA: burn_test
                   TABLE_NAME: Orders_MV
                  COLUMN_NAME: product_name
             ORDINAL_POSITION: 1
POSITION_IN_UNIQUE_CONSTRAINT: NULL
      REFERENCED_TABLE_SCHEMA: NULL
        REFERENCED_TABLE_NAME: NULL
       REFERENCED_COLUMN_NAME: NULL
*************************** 3. row ***************************
           CONSTRAINT_CATALOG: def
            CONSTRAINT_SCHEMA: burn_test
              CONSTRAINT_NAME: child_ibfk_1
                TABLE_CATALOG: def
                 TABLE_SCHEMA: burn_test
                   TABLE_NAME: child
                  COLUMN_NAME: parent_id
             ORDINAL_POSITION: 1
POSITION_IN_UNIQUE_CONSTRAINT: 1
      REFERENCED_TABLE_SCHEMA: burn_test
        REFERENCED_TABLE_NAME: parent
       REFERENCED_COLUMN_NAME: id
3 rows in set (0.04 sec)

-- 作业:  
-- 1:查看索引Cardinality 存放的元数据表
-- 2:查看线上数据库索引Cardinality的值，判断索引创建是否合理，写出这条SQL语句
```

-----

## 6. EXPLAIN： explain是解释SQL语句的执行计划，即显示该SQL语句怎么执行的，但不会真的去执行。也可以使用desc，等价于explain

* **参数extended**

```sql
mysql> explain extended  select * from test_index_2 where  b >1 and b < 3\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: test_index_2
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 6
     filtered: 16.67
        Extra: Using where
1 row in set, 2 warnings (0.00 sec)  有 warnings，这里相当于提供一个信息返回

mysql> show warnings\G 
*************************** 1. row ***************************
  Level: Warning
   Code: 1681
Message: 'EXTENDED' is deprecated and will be removed in a future release. -- 即将被弃用
*************************** 2. row ***************************  -- 显示真正的执行语句
  Level: Note
   Code: 1003
Message: /* select#1 */ select `burn_test`.`test_index_2`.`a` AS `a`,`burn_test`.`test_index_2`.`b` AS `b`,`burn_test`.`test_index_2`.`c` AS `c` from `burn_test`.`test_index_2` where ((`burn_test`.`test_index_2`.`b` > 1) and (`burn_test`.`test_index_2`.`b` < 3))
2 rows in set (0.00 sec)
```


* **参数FORMAT**
    - 使用`FORMART=JSON`不仅仅是为了格式化输出效果，还有其他有用的显示信息。
    - 且当5.6版本后，使用`MySQL Workbench`，可以使用`visual Explain`方式显示详细的图示信息。

```sql
mysql> explain format=json  select * from test_index_2 where  b >1 and b < 3\G          
*************************** 1. row ***************************
EXPLAIN: {
  "query_block": {
    "select_id": 1,
    "cost_info": {
      "query_cost": "2.20"  -- 总成本
    },
    "table": {
      "table_name": "test_index_2",
      "access_type": "ALL",
      "rows_examined_per_scan": 6,
      "rows_produced_per_join": 1,
      "filtered": "16.67",
      "cost_info": {
        "read_cost": "2.00",  //读成本
        "eval_cost": "0.20",
        "prefix_cost": "2.20",
        "data_read_per_join": "16"
      },
      "used_columns": [
        "a",
        "b",
        "c"
      ],
      "attached_condition": "((`burn_test`.`test_index_2`.`b` > 1) and (`burn_test`.`test_index_2`.`b` < 3))"
    }
  }
}
```

### 6.1 Explain输出介绍

|  列  | 含义 |
|:----:|:----:|
|id|执行计划的id标志|
|select_type|SELECT的类型|
|table|输出记录的表|
|partitions|符合的分区，[PARTITIONS]|
|type|JOIN的类型|
|possible_keys|优化器可能使用到的索引|
|key|优化器实际选择的索引|
|key_len|使用索引的字节长度|
|ref|进行比较的索引列|
|rows|优化器`预估`的记录数量|
|filtered|根据条件过滤得到的记录的百分比[EXTENDED]|
|extra|额外的显示选项|



#### （1）. id：执行计划的id标志


#### （2）. select_type：SELECT的类型
##### 比如：SIMPLE ：简单SELECT(不使用UNION或子查询等)
| select_type | 含义 |
|:------------:|:-----:|
| SIMPLE | 简单SELECT(不使用UNION或子查询等)  |
| PRIMARY | 最外层的select |
| UNION | UNION中的第二个或后面的SELECT语句 |
| DEPENDENT UNION | UNION中的第二个或后面的SELECT语句，依赖于外面的查询  |
| UNION RESULT | UNION的结果  |
| SUBQUERY | 子查询中的第一个SELECT  |
| DEPENDENT SUBQUERY | 子查询中的第一个SELECT，依赖于外面的查询 |
| DERIVED | 派生表的SELECT(FROM子句的子查询)  |
| MATERIALIZED | 物化子查询 |
| UNCACHEABLE SUBQUERY | 不会被缓存的并且对于外部查询的每行都要重新计算的子查询  |
| UNCACHEABLE UNION | 属于不能被缓存的 UNION中的第二个或后面的SELECT语句 |


* **MATERIALIZED**
    - 产生中间临时表（实体）
    - 临时表自动创建索引并和其他表进行关联，提高性能
    - 和子查询的区别是，优化器将可以进行`MATERIALIZED`的语句自动改写成`join`，并自动创建索引


#### （3）. table：通常是用户操作的用户表
* <unionM, N> UNION得到的结果表
* <derivedN> 排生表，由id=N的语句产生
* <subqueryN> 由子查询物化产生的表，由id=N的语句产生


####（4）. type
>**摘自姜老师的PDF，按照图上箭头的顺序来看，成本（cost）是从小到大**

![TYPE](./Selection_002.png)


####（5）. extra

![Extra](./Selection_003.png)

* Using filesort：可以使用复合索引将filesort进行优化。提高性能
* Using index：比如使用覆盖索引
* Using where: 使用where过滤条件

> Extra的信息是可以作为优化的提示，但是更多的是优化器优化的一种说明

# 十八、day018-磁盘

## 1. iostat

```bash
#
# 安装 iostat
#
shell> yum install sysstat 
# debian 系： apt-get install sysstat

# 使用
shell> iostat -xm 3 # x表示显示扩展统计信息，m表示以兆为单位显示，3表示每隔3秒显示
# 输出如下：
avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.58    0.00    0.33    0.00    0.00   99.08

Device:         rrqm/s   wrqm/s     r/s     w/s    rMB/s    wMB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
sda               0.00     0.00    0.00    0.67     0.00     0.00     8.00     0.00    2.00    0.00    2.00   1.00   0.07
sdb               0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00
```

| CPU属性| 说明|
|:------:|:---:|
|%user|CPU处在用户模式下的时间百分比|
|%nice|CPU处在带NICE值的用户模式下的时间百分比|
|%sys|CPU处在系统模式下的时间百分比|
|%iowait|CPU等待IO完成时间的百分比|
|%steal|管理程序维护另一个虚拟处理器时，虚拟CPU的无意的等待时间的百分比|
|%idle|闲置cpu的百分比|
>**提示：**
如果%iowait的值过高，表示硬盘存在I/O瓶颈;
如果%idle值高，表示CPU较空闲，如果%idle值高但系统响应慢时，有可能是CPU等待分配内存，此时应加大内存容量。
如果%idle值如果`持续`很低，那么系统的CPU处理能力相对较低，表明系统中最需要解决的资源是CPU。

|Device属性 | 说明 |
|:----------:|:----:|
|rrqm/s	    |每秒进行 merge 的读操作数目 |
|wrqm/s	    |每秒进行 merge 的写操作数目|
|r/s	    |每秒完成的读 I/O 设备次数|
|w/s	    |每秒完成的写 I/O 设备次数|
|rsec/s	    |每秒读扇区数|
|wsec/s	    |每秒写扇区数|
|rkB/s	    |每秒读K字节数|
|wkB/s	    |每秒写K字节数|
|avgrq-sz	|平均每次设备I/O操作的数据大小 (扇区)|
|avgqu-sz	|平均I/O队列长度|
|await	    |平均每次设备I/O操作的等待时间 (毫秒)|
|svctm	    |平均每次设备I/O操作的服务时间 (毫秒)|
|%util	    |一秒中有百分之多少的时间用于 I/O 操作，即被io消耗的cpu百分比|
>**提示：**
如果 %util 接近 100%，说明产生的I/O请求太多，I/O系统已经满负荷，该磁盘可能存在瓶颈。
如果 svctm 比较接近 await，说明 I/O 几乎没有等待时间；
如果 await 远大于 svctm，说明I/O队列太长，io响应太慢，则需要进行必要优化。
如果avgqu-sz比较大，也表示有当量io在等待。

-----

## 2. 磁盘【MySQL配SSD】
### 1. 磁盘的访问模式
* 顺序访问
    - 顺序的访问磁盘上的块；
    - 一般经过测试后，得到该值的单位是`MB/s`，表示为磁盘`带宽`，普通硬盘在 50~ 100 MB/s
* 随机访问
    - 随机的访问磁盘上的块
    - 也可以用MB/s进行表示，但是通常使用`IOPS`（每秒处理IO的能力），普通硬盘在 100-200 IOPS

![磁盘的访问模式](./Selection_001.png)

>拷贝文件属于顺序访问，`数据库`中访问数据属于`随机访问`。
数据库对数据的访问做了优化，把随机访问转成顺序访问。


### 3. 磁盘的分类
* HDD【机械磁盘】
    - 盘片通过旋转，磁头进行定位，读取数据；
    - 顺序性较好，随机性较差；
    - 常见转速
        - 笔记本硬盘：5400转/分钟；
        - 桌面硬盘：7200转/分钟；
        - 服务器硬盘：10000转/分钟、15000转/分钟；
        - SATA：120 ~ 150 IOPS
        - SAS ：150 ~ 200 IOPS
    
    >从理论上讲，15000转/分钟，最高是 15000/60 约等于250IOPS
    >由于机械盘片需要旋转，转速太高无法很好的散热

    >如果一个HDD对4K的块做随机访问是0.8MB/s，可通过`0.8 *（1024 / 4）= 200` （(1M/s) /(4k） = 256，每秒能发起多少次4K的请求）或者 `（0.8 * 1000） / 4=200`得到`IOPS`，但是这个值存在部分干扰因素，如cache等

* SSD【固态硬盘】
    - 纯电设备
    - 由FLash Memory组成（闪存）
    - 没有读写磁头
    - MLC闪存颗粒对一般企业的业务够用。目前SLC闪存颗粒价格较贵
    - IOPS高
        - 50000+ IOPS【比机械磁盘高了几百倍】
        - 读写速度非对称 以 [INTEL SSD DC-S3500](http://www.intel.com/content/www/us/en/solid-state-drives/ssd-dc-s3500-spec.html)为例子：
            - Random 4KB3 Reads: Up to 75,000 IOPS 
            - Random 4KB Writes: Up to 11,500 IOPS
            - Random 8KB3 Reads: Up to 47,500 IOPS
            - Random 8KB Writes: Up to 5,500 IOPS
            * 随机写比读要慢
        
        - 当在写过数据的区域再写入数据时，要先擦除老数据，再写入新数据
        - 擦除数据需要擦除整个区域（128K or 256K）一起擦除（自动把部分有用的数据挪到别的区域）
            
            > 对比发现4K性能要优于8K的性能，几乎是2倍的差距，当然16K就更明显，所以当使用SSD时，建议数据库页大小设置成4K或者是8K，`innodb_page_size=8K`）
            > 上线以前，SSD需要经过严格的压力测试（一周时间），确保性能平稳
        
    - Endurance Rating 
        - 表示该SSD的寿命是多少
        - 比如450TBW，表示这个SSD可以反复写入的数据总量是450T（包括插入和更新这些都算写入）
    
    
    - SSD线上参数设置
        - 磁盘调度算法改为Deadline

            ```bash
            echo deadline > /sys/block/sda/queue/scheduler  # deadline适用于数据库，HDD也建议改成Deadline
            ```
            
        - MySQL参数
            - `innodb_log_file_size=4G`  该参数设置的尽可能大
            - `innodb_flush_neighbors=0`
            
            > 性能更平稳，且至少有15%的性能提升
        
    - SSD 品牌推荐
        - Intel
        - FusionIO
        - 宝存
        
    - 不是很建议使用PCI-E的Flash卡（PCI-E插槽的SSD） 
        - 性能过剩
        - 安装比较麻烦

### 4. 提升IOPS性能的手段【因为很早之前还没有SSD】
* 通过 RAID 技术【多块机械硬盘组成一块逻辑盘】
    - 功耗较高
    - IOPS在2000左右

* 通过购买共享存储设备【】
    - 价格非常昂贵
    - 但是比较稳定
    - 底层还是通过RAID实现

* 直接使用SSD，使用SSD才是趋势，机械硬盘会淘汰掉
    - 性能较好的SSD可以达到 `万级别的IOPS`
    - 建议可以用SSD + RAID5，RAID1+0太奢侈

* 前面两者的提升都非常有限,IOSP以千为单位，互联网公司一般不会买共享存储设备，因为太贵

### 5. RAID类别

* RAID0
![RAID0](./130px-RAID_0.svg.png)
    - 速度最快
    - 没有冗余备份

* RAID1
![RAID1](./130px-RAID_1.svg.png)
    - 可靠性高
    - 读取速度理论上等于硬盘数量的倍数
    - 容量等于一个硬盘的容量

* RAID5
![RAID5](./220px-RAID_5.svg.png)
     - 至少要3块硬盘
     - 通过对数据的奇偶检验信息存储到不同的磁盘上，来恢复数据，最多只能坏一块
     - 属于折中方案

* RAID6
![RAID6](./270px-RAID_6.svg.png)
    - 至少是4块硬盘
    - 和RAID5比较，RAID6增加第二个独立的奇偶校验信息，写入速度略受影响
    - 数据可靠性高，可以同时坏两块
    - 由于使用了双校验机制，恢复数据速度较慢

* RAID1+0
![RAID 1+0](./220px-RAID_10.svg.png)
既提供了READ1的可靠性，又提供了READ0的速度，性能安全性和速度较好，如果你用的机械硬盘，建议用这个，SSD则不建议

* RAID5+0
![Alt text](./RAID_50.png)


### 6. RAID卡的构造
* BBU：电池备份装置：
    - Battery Backup Unit
    - 目前几乎所有RAID卡都带BBU
    - 需要电池保证写入的可靠性（在断电后，因为我READ卡有电，可以将RAID卡`内存`中的缓存的数据刷入到磁盘）
    - 电池有充放电时间 (30天左右一个周期，充放电会切换成 Write Through，导致性能下降)
        - 使用`闪存（Flash）`的方式，就不会有充放电性能下降的问题

* RAID卡缓存
    - Write Backup （`强烈建议开启缓存`）
    - Write Through (不使用缓存，直接写入磁盘)
    - 写缓存并非默认开启


* LSI-RAID卡相关命令
    - 查看电量百分比
    
        ```bash
        [root@test_raid ~]# megacli -AdpBbuCmd -GetBbuStatus -aALL |grep "Relative State of Charge"
        Relative State of Charge: 100 %
        ```

    - 查看充电状态
    
        ```bash
        [root@test_raid ~]# megacli -AdpBbuCmd -GetBbuStatus -aALL |grep "Charger Status"
        Charger Status: Complete
        ```
    - 查看缓存策略
    
        ```bash
        [root@test_raid ~]# megacli -LDGetProp -Cache -LALL -a0
        Adapter 0-VD 0(target id: 0): Cache Policy:WriteBack, ReadAdaptive, Direct, No Write Cache if bad BBU
        ```

### 7. 文件系统和操作系统

* 文件系统
    - XFS/EXT4
    - noatime (不更新文件的atime标记，减少系统的IO访问)
    - nobarrier （禁用barrier，可以提高性能，前提是使用write backup和使用BBU）
    
    > mount -o noatime,nobarrier /dev/sda1 /data

* 操作系统
    - 推荐Linux
    - 关闭SWAP


