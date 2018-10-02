

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
- [十一、day11：SQL语法之SELECT](#十一day11sql语法之select)
- [十二、day012-子查询 INSERT UPDATE DELETE REPLACE](#十二day012-子查询-insert-update-delete-replace)
- [十三、day013-作业讲解一 Rank 视图 UNION 触发器上](#十三day013-作业讲解一-rank-视图-union-触发器上)
- [十四、day014-触发器下 存储过程 自定义函数MySQL 执行计划与优化器](#十四day014-触发器下-存储过程-自定义函数mysql-执行计划与优化器)
- [十五、day015-索引 B+树 上](#十五day015-索引-b树-上)
- [十六、day016-索引 B+树 下 Explain 1](#十六day016-索引-b树-下-explain-1)
- [十七、day017-Explain 2【MySQL innodb引擎优化】](#十七day017-explain-2mysql-innodb引擎优化)
- [十八、day018-磁盘](#十八day018-磁盘)
- [十九、day019-磁盘测试](#十九day019-磁盘测试)
- [二十、day020-InnoDB_1 表空间 General](#二十day020-innodb_1-表空间-general)
- [二十一、day021-InnoDB_2 SpaceID.PageNumber 压缩表）](#二十一day021-innodb_2-spaceidpagenumber-压缩表)
- [二十二、day022-InnoDB_3 透明表空间压缩 索引组织表](#二十二day022-innodb_3-透明表空间压缩-索引组织表)
- [二十三、day023-InnoDB_4 页(2) 行记录](#二十三day023-innodb_4-页2-行记录)
- [二十四、day024-InnoDB_5 – heap_number Buffer Pool](#二十四day024-innodb_5--heap_number-buffer-pool)
- [二十五、day025-InnoDB_6 Buffer Pool与压缩页 CheckPoint LSN](#二十五day025-innodb_6-buffer-pool与压缩页-checkpoint-lsn)
- [二十六、day026-InnoDB_7 doublewrite ChangeBuffer AHI FNP【MySQL 索引与innodb锁机制】](#二十六day026-innodb_7-doublewrite-changebuffer-ahi-fnpmysql-索引与innodb锁机制)
- [二十七、day027-Secondary Index](#二十七day027-secondary-index)
- [二十八、day028-join算法锁_1](#二十八day028-join算法锁_1)
- [二十九、day029-锁_2](#二十九day029-锁_2)
- [三十、day030-锁_3](#三十day030-锁_3)
- [三十一、day031-锁_4](#三十一day031-锁_4)
- [三十二、day032-锁_5](#三十二day032-锁_5)
- [三十三、day033-锁_6 事物_1](#三十三day033-锁_6-事物_1)
- [三十四、day034-事务_2  MySQL 性能衡量](#三十四day034-事务_2  mysql-性能衡量)
- [三十五、day035-redo_binlog_xa](#三十五day035-redo_binlog_xa)
- [三十六、day036-undo_sysbench](#三十六day036-undo_sysbench)
- [三十七、day037-tpcc_mysqlslap【MySQL 备份与恢复】](#三十七day037-tpcc_mysqlslapmysql-备份与恢复)
- [三十八、day038-purge死锁举例_MySQL backup备份_1](#三十八day038-purge死锁举例_mysql-backup备份_1)
- [三十九、day039-MySQL backup备份恢复_2【MySQL 复制技术与高可用】](#三十九day039-mysql-backup备份恢复_2mysql-复制技术与高可用)
- [四十、day040-MySQL 备份恢复backup_3_replication_1](#四十day040-mysql-备份恢复backup_3_replication_1)
- [四十一、day041-backup_4-replication_2](#四十一day041-backup_4-replication_2)
- [四十二、day042-replication_3](#四十二day042-replication_3)
- [四十三、day043-replication_4-GTID 1](#四十三day043-replication_4-gtid-1)
- [四十四、day044-replication_5-GTID 2](#四十四day044-replication_5-gtid-2)

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
# 十一、day11：SQL语法之SELECT
# 十二、day012-子查询 INSERT UPDATE DELETE REPLACE
# 十三、day013-作业讲解一 Rank 视图 UNION 触发器上
# 十四、day014-触发器下 存储过程 自定义函数MySQL 执行计划与优化器
# 十五、day015-索引 B+树 上
# 十六、day016-索引 B+树 下 Explain 1
# 十七、day017-Explain 2【MySQL innodb引擎优化】
# 十八、day018-磁盘
# 十九、day019-磁盘测试
# 二十、day020-InnoDB_1 表空间 General
# 二十一、day021-InnoDB_2 SpaceID.PageNumber 压缩表）
# 二十二、day022-InnoDB_3 透明表空间压缩 索引组织表
# 二十三、day023-InnoDB_4 页(2) 行记录
# 二十四、day024-InnoDB_5 – heap_number Buffer Pool
# 二十五、day025-InnoDB_6 Buffer Pool与压缩页 CheckPoint LSN
# 二十六、day026-InnoDB_7 doublewrite ChangeBuffer AHI FNP【MySQL 索引与innodb锁机制】
# 二十七、day027-Secondary Index
# 二十八、day028-join算法锁_1
# 二十九、day029-锁_2
# 三十、day030-锁_3
# 三十一、day031-锁_4
# 三十二、day032-锁_5
# 三十三、day033-锁_6 事物_1
# 三十四、day034-事务_2  MySQL 性能衡量
# 三十五、day035-redo_binlog_xa
# 三十六、day036-undo_sysbench
# 三十七、day037-tpcc_mysqlslap【MySQL 备份与恢复】
# 三十八、day038-purge死锁举例_MySQL backup备份_1
# 三十九、day039-MySQL backup备份恢复_2【MySQL 复制技术与高可用】
# 四十、day040-MySQL 备份恢复backup_3_replication_1
# 四十一、day041-backup_4-replication_2
# 四十二、day042-replication_3
# 四十三、day043-replication_4-GTID 1
# 四十四、day044-replication_5-GTID 2
