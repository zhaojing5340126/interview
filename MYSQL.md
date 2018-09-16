

<!-- TOC -->

- [一、day1-3：MySQL连接、登录、安装、升级](#一day1-3mysql连接登录安装升级)
    - [1、连接和登录和安装位置介绍 **](#1连接和登录和安装位置介绍-)
        - [1.1 连接和登录](#11-连接和登录)
        - [1.2 一些安装位置介绍](#12-一些安装位置介绍)
    - [2、安装和升级](#2安装和升级)
        - [2.1 MySQL下载](#21-mysql下载)
        - [2.2 MySQL安装](#22-mysql安装)
        - [2.3 MySQL升级](#23-mysql升级)
    - [3.附录](#3附录)
        - [3.1 配置文件my.cnf讲解](#31-配置文件mycnf讲解)
        - [3.2 几种登录方式（本地socket连接【方法二、四】或TCP/IP远程连接【方法三】）](#32-几种登录方式本地socket连接方法二四或tcpip远程连接方法三)
        - [3.3. 免密码登录](#33-免密码登录)
        - [3.4. MySQL 参数介绍和设置](#34-mysql-参数介绍和设置)
- [二、day4：权限、Role模拟、workbench、MySQL体系结构](#二day4权限role模拟workbenchmysql体系结构)
    - [1、权限管理](#1权限管理)
        - [1.1 “’用户‘ + @ + ’IP‘”=完整的用户标识](#11-用户----ip完整的用户标识)
        - [1.2 系统表权限信息与常用权限](#12-系统表权限信息与常用权限)
        - [1.3. 创建用户（create user ...）及授予权限（grant...to...）](#13-创建用户create-user-及授予权限grantto)
        - [1.4 撤销权限(revoke...from...只删除权限，drop user ...删除用户)](#14-撤销权限revokefrom只删除权限drop-user-删除用户)
        - [1.5 查看某一个用户的权限（show grants for 'perf'@'127.0.0.1';）](#15-查看某一个用户的权限show-grants-for-perf127001)
        - [1.6 mysql库主要负责存储数据库的用户、权限设置、关键字等mysql自己需要使用的控制和管理信息](#16-mysql库主要负责存储数据库的用户权限设置关键字等mysql自己需要使用的控制和管理信息)
        - [1.7 information_schema](#17-information_schema)
    - [2、Role模拟](#2role模拟)
        - [2.1 角色的定义及角色模拟操作：](#21-角色的定义及角色模拟操作)
    - [3. Workbench与Utilities介绍](#3-workbench与utilities介绍)
    - [4. MySQL体系结构 **](#4-mysql体系结构-)
        - [4.1 数据库与数据库实例](#41-数据库与数据库实例)
        - [4.2 MySQL体系结构：单进程多线程、插件式存储引擎架构、存储引擎的对象是表](#42-mysql体系结构单进程多线程插件式存储引擎架构存储引擎的对象是表)
        - [4.3 MySQL物理存储结构【主要文件：数据库配置文件、表结构定义文件、错误文件、慢查询日志、通用日志】](#43-mysql物理存储结构主要文件数据库配置文件表结构定义文件错误文件慢查询日志通用日志)
            - [**1. MySQL配置文件my.cnf**](#1-mysql配置文件mycnf)
            - [**2. 表结构的组成**](#2-表结构的组成)
            - [**3. 错误日志文件（用于定位错误）**](#3-错误日志文件用于定位错误)
            - [**4. 慢查询日志文件（用于优化查询）**](#4-慢查询日志文件用于优化查询)
            - [**5、通用日志(generic_log)与审计**](#5通用日志generic_log与审计)
- [三、day5： 存储引擎](#三day5-存储引擎)
    - [1、存储引擎的概念](#1存储引擎的概念)
    - [2、各种存储引擎介绍](#2各种存储引擎介绍)
        - [2.1、MyISAM](#21myisam)
        - [2.2 Memory存储引擎](#22-memory存储引擎)
        - [2.3 CSV存储引擎](#23-csv存储引擎)
        - [2.4 Federated存储引擎](#24-federated存储引擎)
        - [2. Federated 语法](#2-federated-语法)
    - [五. 多实例安装【未】](#五-多实例安装未)
        - [1.  多实例介绍](#1--多实例介绍)
        - [2. 安装要求](#2-安装要求)
        - [3. 安装操作](#3-安装操作)
    - [六. Federated测试【未】](#六-federated测试未)

<!-- /TOC -->

# 一、day1-3：MySQL连接、登录、安装、升级
## 1、连接和登录和安装位置介绍 **
### 1.1 连接和登录
**1.1 启动MySQL：** /etc/init.d/mysql.server start   【etc里面一般是配置文件】<br>
**1.2 登录MySQL：** mysql -uroot -p(密码)<br>
**1.3 退出MySQL：** exit<br>
**1.3 关闭MySQL：** /etc/init.d/mysql.server stop<br>
### 1.2 一些安装位置介绍
* 安装位置：/usr/local/
* 可执行文件：/usr/local/mysql/bin，比如mysql_safa 
```mysql
./mysqld_safe &     //&后台启动mysql 
ps -ef | grep mysqld    //查看当前所有的进程，grep是文本搜索工具
```
* 配置文件my.cnf：启动实例时，MySQL数据库会根据配置文件的参数来启动数据库实例，如果有几个配置文件都有同一个参数，MySQL会以读取到的最后一个配置文件中的参数为准。【若没有配置文件，会按照编译时默认的参数设置启动实例】
    * datadir参数：指定了数据库数据所在的路径，默认/usr/local/mysql/data，但一般我们会将数据与数据库分离，用户可以直接修改datadir参数，也可以使用该路径，只是该路径是一个链接而已。
        * 并且必须保证其权限，只有mysql用户和组可以访问，通常MySQL数据库访问权限为mysql：mysql
```mysql
mysql --help | grep my.cnf 

//查看MySQL数据库启动实例时会在哪些位置查找配置文件，/etc/my.cnf  /etc/mysql/my.cnf  /usr/local/mysql/etc/my.cnf  ~/.my.cnf   【我自己的后面三个位置都不存在，配置文件在/etc/my.cnf】
```


## 2、安装和升级
### 2.1 MySQL下载
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
        
-----



# 二、day4：权限、Role模拟、workbench、MySQL体系结构
## 1、权限管理
### 1.1 “’用户‘ + @ + ’IP‘”=完整的用户标识
>`bob@127.0.0.1` 和 `bob@loalhost` 以及 `bob@192.168.1.100` 这三个其实是`不同`的 **用户标识** 

### 1.2 系统表权限信息与常用权限

* **系统表权限信息:**
    - **a) 用户名和IP是否允许**
    - **b) 查看mysql.user表**  `// 查看全局所有库的权限`
    - **c) 查看mysql.db表**  `// 查看指定库的权限`
    - **d) 查看mysql.table_priv表** `// 查看指定表的权限`
    - **e) 查看mysql.column_priv表** `// 查看指定列的权限`

    ***tips**: mysql> desc [tablename]; 可以查看表的结构信息；*
    
* **常用权限：**
    - SQL语句：SELECT、INSERT、UPDATE、DELETE、INDEX
    - 存储过程：CREATE ROUTINE、ALTER ROUTINE、EXECUTE、TRIGGER
    - 管理权限：SUPER、RELOAD、SHOW DATABASE、SHUTDOWN、
    
    [所有权限猛戳这里](https://dev.mysql.com/doc/refman/5.x/en/privileges-provided.html)


* **可选资源:**
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

-----
## 2、Role模拟
### 2.1 角色的定义及角色模拟操作：
>`角色`(Role)可以用来批量管理用户，同一个角色下的用户，拥有相同的权限。
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

-----

## 3. Workbench与Utilities介绍
 1. 下载
* [Workbench-win32下载](http://dev.mysql.com/get/Downloads/MySQLGUITools/mysql-workbench-community-6.3.5-win32.msi)
* [Workbench-win64下载](http://dev.mysql.com/get/Downloads/MySQLGUITools/mysql-workbench-community-6.3.5-winx64.msi)
* [Utilities下载](http://dev.mysql.com/get/Downloads/MySQLGUITools/mysql-utilities-1.5.6.tar.gz)


 2. Workbench功能概述
* **SQL语句格式化**
* **SQL关键字upcase**
* **MySQL Dashboard**
* **SQL语法提示**
* **ER图**
* **Forward Engine**  `//ER图 --> DB表结构`
* **Reverse**  `//DB表结构 --> ER图`

 3. Utilities安装
```bash
shell> python setup.py install  # 如果安装不成功，查看一下python的版本。推荐2.7.X
```
-----

## 4. MySQL体系结构 **
### 4.1 数据库与数据库实例
>* **数据库：** 物理操作系统文件或其他形式文件类型集合，（数据库文件）是一个或者一组二进制文件。在MySQL中，数据库文件可以是frm、MYD、MYI、ibd结尾的文件。当使用NDB引擎时，数据库文件可能不是操作系统上的文件，而是存放在内存中的文件，但是定义仍然不变。【物理文件是指外存中的存放的文件】

>* **数据库实例：**用来操作数据库文件的，由数据库后台进程/线程以及一个共享区域组成(程序的概念)，共享内存可以被运行的后台进程/线程所共享。【用户对数据库的任何操作都是在数据库实例下进行的】

**`注意：MySQL中，数据库实例和数据库是一一对应的。【多实例：一个数据库对应多个实例，mysql通常不支持，oracle支持】`**


### 4.2 MySQL体系结构：单进程多线程、插件式存储引擎架构、存储引擎的对象是表


1. 体系结构图

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

2. 单进程多线程结构
    - （多进程程序，进程间通信开销大于多线程）。MySQL数据库实例在系统上的表现就是一个进程。


3. 存储引擎的概念
    - 可以理解成文件系统，例如FAT32, NTFS, EXT4。 一个表是一个分区，引擎就是分区的文件系统
    - `存储引擎的对象就是表`
    - show tables; 可以看到每个表对应的是上面引擎(Engine)
    - 除了特殊情况，我们现在就只考虑`INNODB`




4. 逻辑存储结构
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
#### **1. MySQL配置文件my.cnf**
* `datadir`
        - 存储数据二进制文件的路径

#### **2. 表结构的组成**
- .frm：表结构定义文件（二进制）
- .MYI：索引文件
- .MYD：数据文件
    - 可以用`hexdump -c XXX.frm`查看二进制文件(意义不大)
    - `show create table tablename;`
    - **用mysqlfrm命令查看.frm文件 (utilities工具包)**

        ```bash
        shell> mysqlfrm --diagnostic /data/mysql_data/aaa/.a.frm  #可将frm文件转成create table的语句
        ```
    


#### **3. 错误日志文件（用于定位错误）**
* `log_err`
        - 建议配置成统一的名字`err.log`，方便定位错误

#### **4. 慢查询日志文件（用于优化查询）**
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

#### **5、通用日志(generic_log)与审计**
**5.1、 通用日志作用**
* 当需要查找某条特定SQL语句，且该SQL语句执行较快，无法记录到slow_log中时，可以开启通用日志`generic_log`,进行全面记录， 可用于审计`Audit`

* 通用日志会记录所有操作，性能下降明显。所以如果需要审计，需要`Audit Plugin`（插件）


**5.2、审计插件**
* MariaDB Audit 插件
    - MySQL社区版本目前没有提供Audit的功能，企业版本提供了该功能。MariaDB 提供了开源的Audit插件，且MySQL也能使用。


# 三、day5： 存储引擎

## 1、存储引擎的概念
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

## 五. 多实例安装【未】
### 1.  多实例介绍
* 一台服务器上安装多个MySQL数据库实例
* 可以充分利用服务器的硬件资源
* 通过mysqld_multi进行管理

### 2. 安装要求
* MySQL实例1 - `mysql1`
    - port = 3306
    - datadir = /data1
    - socket = /tmp/mysql.sock1

* MySQL实例2 - `mysql2`
    - port = 3307
    - datadir = /data2
    - socket = /tmp/mysql.sock2

* MySQL实例3 - `mysql3`
    - port = 3308
    - datadir = /data3
    - socket = /tmp/mysql.sock3

* MySQL实例4 - `mysql4`
    - port = 3309
    - datadir = /data4
    - socket = /tmp/mysql.sock4

>**`该三个参数必须定制，且必须不同 (port / datadir / socket)`**
`server-id`和多数据库实例没有关系，和数据库复制有关系。

### 3. 安装操作
```bash
#
# 多实例配置文件，可以mysqld_multi --example 查看例子
#
[root@MyServer /]> cat /etc/my.cnf 
#[client]           # 这个标签如果配置了用户和密码，
                    # 并且[mysqld_multi]下没有配置用户名密码，
                    # 则mysqld_multi stop时, 会使用这个密码
                    # 如果没有精确的匹配，则匹配[client]标签
#user = root        
#password = 123
#-------------
[mysqld_multi]
mysqld = /usr/local/mysql/bin/mysqld_safe
mysqladmin = /usr/local/mysql/bin/mysqladmin
user = multi_admin
pass = 123  # 官方文档中写的password，但是存在bug，需要改成pass(v5.7.9)
            # 写成password，start时正常，stop时，报如下错误
            # Access denied for user 'multi_admin'@'localhost' (using password: YES)
log = /var/log/mysqld_multi.log


[mysqld1]  # mysqld后面的数字为GNR, 是该实例的标识
           # mysqld_multi  start 1,  mysqld_multi start 2-4
server-id = 11
socket = /tmp/mysql.sock1
port = 3306
bind_address = 0.0.0.0
datadir = /data1
user = mysql
performance_schema = off
innodb_buffer_pool_size = 32M
skip_name_resolve = 1
log_error = error.log
pid-file = /data1/mysql.pid1


[mysqld2]
server-id = 12
socket = /tmp/mysql.sock2
port = 3307
bind_address = 0.0.0.0
datadir = /data2
user = mysql
performance_schema = off
innodb_buffer_pool_size = 32M
skip_name_resolve = 1
log_error = error.log
pid-file = /data2/mysql.pid2


[mysqld3]
server-id = 13
socket = /tmp/mysql.sock3
port = 3308
bind_address = 0.0.0.0
datadir = /data3
user = mysql
performance_schema = off
innodb_buffer_pool_size = 32M
skip_name_resolve = 1
log_error = error.log
pid-file = /data3/mysql.pid3


[mysqld4]
server-id = 14
socket = /tmp/mysql.sock4
port = 3309
bind_address = 0.0.0.0
datadir = /data4
user = mysql
performance_schema = off
innodb_buffer_pool_size = 32M
skip_name_resolve = 1
log_error = error.log
pid-file = /data4/mysql.pid4
```

```bash
#
# 准备好数据目录，并初始化安装
#
[root@MyServer ~]> mkdir /data1
[root@MyServer ~]> mkdir /data2
[root@MyServer ~]> mkdir /data3
[root@MyServer ~]> mkdir /data4
[root@MyServer ~]> chown mysql.mysql /data{1..4}
[root@MyServer ~]> mysqld --initialize --user=mysql --datadir=/data1
#
# 一些日志输出，并提示临时密码，下同
#
[root@MyServer ~]> mysqld --initialize --user=mysql --datadir=/data2
[root@MyServer ~]> mysqld --initialize --user=mysql --datadir=/data3
[root@MyServer ~]> mysqld --initialize --user=mysql --datadir=/data4
# 安装后，需要检查error.log 确保没有错误出现
[root@MyServer ~]> cp /usr/local/mysql/support-files/mysqld_multi.server  /etc/init.d/mysqld_multid 
# 拷贝启动脚本，方便自启
[root@MyServer ~]> chkconfig mysqld_multid on
```

```bash
[root@MyServer ~]> mysqld_multi  start
[root@MyServer ~]> mysqld_multi  report
Reporting MySQL servers
MySQL server from group: mysqld1 is running
MySQL server from group: mysqld2 is running
MySQL server from group: mysqld3 is running
MySQL server from group: mysqld4 is running
[root@MyServer ~]> netstat -tunlp | grep mysql
[root@MyServer ~]> netstat -tunlp | grep mysql
tcp        0      0 :::3307                     :::*                        LISTEN      6221/mysqld         
tcp        0      0 :::3308                     :::*                        LISTEN      6232/mysqld         
tcp        0      0 :::3309                     :::*                        LISTEN      6238/mysqld         
tcp        0      0 :::3306                     :::*                        LISTEN      6201/mysqld         

[root@MyServer ~]> mysql -u root -S /tmp/mysql.sock1 -p -P3306
#
# 使用-S /tmp/mysql.sock1 进行登录，并输入临时密码后，修改密码，下同
#
[root@MyServer ~]> mysql -u root -S /tmp/mysql.sock2 -p -P3307
[root@MyServer ~]> mysql -u root -S /tmp/mysql.sock3 -p -P3308
[root@MyServer ~]> mysql -u root -S /tmp/mysql.sock4 -p -P3309
```

```sql
--
-- mysql1
--
mysql> show variables like "port"; 
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| port          | 3306  |
+---------------+-------+
1 row in set (0.00 sec)

mysql> show variables like "socket";
+---------------+------------------+
| Variable_name | Value            |
+---------------+------------------+
| socket        | /tmp/mysql.sock1 |
+---------------+------------------+
1 row in set (0.01 sec)

mysql> show variables like "datadir";
+---------------+---------+
| Variable_name | Value   |
+---------------+---------+
| datadir       | /data1/ |
+---------------+---------+
1 row in set (0.00 sec)

--
-- 这样才能进行关闭数据库的操作
-- 和[mysqld_multi]中的user，pass(注意在5.7.9中不是password)对应起来 （类比[client]标签）
-- 一会测试federated链接，需要增加federated参数，并重启mysql2
--
mysql> create user 'multi_admin'@'localhost' identified by '123';
Query OK, 0 rows affected (0.00 sec)
mysql> grant shutdown on *.* to 'multi_admin'@'localhost';

--
-- mysql2, mysql3, mysql4 类似。可以看到与my.cnf中对应的port和socket
--
```

## 六. Federated测试【未】
```sql
--
-- mysql1 准备数据
--
mysql> show variables like "port"; 
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| port          | 3306  |   -- mysql1 实例端口
+---------------+-------+
1 row in set (0.00 sec)

mysql> create database burn;
Query OK, 1 row affected (0.00 sec)

mysql> use burn;
Database changed

mysql> create table book (
    -> id int not null auto_increment,
    -> name varchar(128) not null,
    -> primary key(id)
    -> );
Query OK, 0 rows affected (0.20 sec)

mysql> insert into book values(1, "book1");
Query OK, 1 row affected (0.02 sec)

mysql> select * from book;
+----+-------+
| id | name  |
+----+-------+
|  1 | book1 |
+----+-------+
1 row in set (0.00 sec)

mysql> create user 'burn'@'127.0.0.1' identified by '123';
Query OK, 0 rows affected (0.00 sec)

mysql> grant select on burn.* to 'burn'@'127.0.0.1';
Query OK, 0 rows affected (0.00 sec)

mysql> show grants for 'burn'@'127.0.0.1';
+------------------------------------------------+
| Grants for burn@127.0.0.1                      |
+------------------------------------------------+
| GRANT USAGE ON *.* TO 'burn'@'127.0.0.1'       |
| GRANT SELECT ON `burn`.* TO 'burn'@'127.0.0.1' |
+------------------------------------------------+
2 rows in set (0.00 sec)
```

```sql
--
-- mysql2 测试Federated
--
mysql> show variables like "port";
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| port          | 3307  |  -- msyql2 实例端口
+---------------+-------+
1 row in set (0.01 sec)

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
--
-- federated 引擎没有打开
--
9 rows in set (0.00 sec)


```

```bash
#
# 在[mysqld2]标签下面增加federated
#

[root@MyServer ~]> cat /etc/my.cnf
# ... 省略 ...
[mysqld2]
federated # 新增的配置项，表示打开federated引擎
# ... 省略 ...
[root@MyServer ~]> mysqld_multi stop 2
[root@MyServer ~]> mysqld_multi start 2   # 重启配置
```

```sql
mysql> show variables like "port";
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| port          | 3307  |  -- msyql2 实例端口
+---------------+-------+
1 row in set (0.01 sec)

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
| FEDERATED          | YES     | Federated MySQL storage engine                                 | NO           | NO   | NO         |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
9 rows in set (0.00 sec)
--
-- 显示 federated 已经启用
--
mysql> create database federated_test;
Query OK, 1 row affected (0.00 sec)

mysql> use federated_test;
Database changed

mysql> create table federated_table_1 (
    -> id int not null auto_increment,
    -> name varchar(128) not null,
    -> primary key(id)
    -> ) engine=federated
    -> connection='mysql://burn:123@127.0.0.1:3306/burn/book';
Query OK, 0 rows affected (0.04 sec)

mysql> select * from federated_table_1;
+----+-------+
| id | name  |
+----+-------+
|  1 | book1 |  -- 和 mysqld1 上的内容一致。
+----+-------+
1 row in set (0.00 sec)
--
-- 由于只有select权限，无法对该表进行insert操作
--
mysql> insert into  federated_table_1 values(2, "book2");
ERROR 1296 (HY000): Got error 10000 'Error on remote system: 1142: INSERT command denied to user 'burn'@'127.0.0.1' for table 'book'' from FEDERATED
```


