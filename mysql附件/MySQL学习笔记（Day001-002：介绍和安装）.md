MySQL学习笔记（Day001-002：介绍和安装）
=====================================
@(MySQL学习)

[TOC]

##一.MySQL版本选择

1. MySQL5.6以后的版本，推荐使用官方版本。
2. Percona：在5.6版本以后，MySQL将Percon之前优化集成到官方版本中;
3. MariaDB：无INNODB;且核心代码较老
4. MySQL在5.6以后不断重构源码，安装包越来越大，功能和性能在持续改进

-----

## 二. MySQL官方网站介绍

官方网站：http://www.mysql.com
1. **Developer Zone**： MySQL开发工程师板块
    * Articles： Oracle工程师自己的博客
    * Plant MySQL： 和MySQL相关从业人员的博客
    * Bugs：MySQL BugList
    * Worklog：开发记录
    * Labs：MySQL实验性项目

2. **Downloads**：MySQL下载
    * Enterprise：MySQL企业版本相关，略过
    * Community：社区版，我们下载和使用社区版
        - MySQL Community Server：MySQL Server
        - MySQL Fabric : 和管理相关的工具
        - MySQL Router：路由中间件
        - MySQL Utilities：MySQL应用程序包
        - MySQL Workbench：官方图型化管理界面
        - MySQL Proxy：MySQL代理。Alpha版本，不推荐

3. **Documentation**：MySQL文档
    * 官方文档 版面更改，下载离线文档在左侧Menu的下面
        - [PDF A4](http://downloads.mysql.com/docs/refman-5.7-en.a4.pdf)
        - [EPUB](http://downloads.mysql.com/docs/refman-5.7-en.epub)
        - [HTML](http://downloads.mysql.com/docs/refman-5.7-en.html-chapter.zip)
        
-----

## 三. MySQL下载
1. 推荐下载`Linux-Generic`版本
2. `Source Code`版本主要作用是为了让开发人员研究源码使用，自己编译对性能提升不明显
3. 不推荐`Version 5.5.X`，有部分bug
4. 推荐使用`Version 5.6.X`和`Version 5.7.X`


>*下载地址：*
[MySQL Community Server 5.7.9 Linux Generic x86-64bit](http://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.9-linux-glibc2.5-x86_64.tar.gz)
[MySQL Community Server 5.6.27 Linux Generic x86-64bit](http://dev.mysql.com/get/Downloads/MySQL-5.6/mysql-5.6.27-linux-glibc2.5-x86_64.tar.gz)

-----

## 四. MySQL安装
1. 安装通用步骤：
    * 解压缩`mysql-VERSION-linux-glibc2.5-x86_64.tar.gz`
    * 打开`INSTALL_BINARY` 文件，按照`shell>`开头的步骤进行操作
    * 将`export PATH=/安装路径/mysql/bin:$PATH`添加到`/etc/profile`
    * `chkconfig mysqld on`或者`chkconfig mysqld.server on`视你的环境而定，详细步骤如下

2. MySQL 5.6.X 安装：
    ```bash
    shell> yum install libaio # Debain系用户:apt-get install libaio1
    shell> groupadd mysql
    shell> useradd -r -g mysql mysql
    shell> cd /usr/local
    shell> tar zxvf /path/to/mysql-VERSION-OS.tar.gz
    shell> ln -s full-path-to-mysql-VERSION-OS mysql
    shell> cd mysql
    shell> chown -R mysql .
    shell> chgrp -R mysql .
    shell> scripts/mysql_install_db --user=mysql
    shell> chown -R root .
    shell> chown -R mysql data
    shell> bin/mysqld_safe --user=mysql &
    # Next command is optional
    shell> cp support-files/mysql.server /etc/init.d/mysql.server 
    ```

3. MySQL 5.7.X 安装     
    ```bash
    shell> groupadd mysql
    shell> useradd -r -g mysql mysql
    shell> cd /usr/local
    shell> tar zxvf /path/to/mysql-VERSION-OS.tar.gz
    shell> ln -s full-path-to-mysql-VERSION-OS mysql
    shell> cd mysql
    shell> mkdir mysql-files
    shell> chmod 770 mysql-files
    shell> chown -R mysql .
    shell> chgrp -R mysql .
    shell> bin/mysqld --initialize --user=mysql #该步骤中会产生零时
                                                #root@localhost密码
                                                #需要自己记录下来
    shell> bin/mysql_ssl_rsa_setup          
    shell> chown -R root .
    shell> chown -R mysql data mysql-files
    shell> bin/mysqld_safe --user=mysql &
    # Next command is optional
    shell> cp support-files/mysql.server /etc/init.d/mysql.server
    ```


4. 验证安装
    * `data`目录在安装之前是空目录，安装完成后应该有`ibXXX`等文件
    * 安装过程中输出的信息中，不应该含有`ERROR`信息，错误信息`默认`会写入到`$HOSTNAME.err`的文件中
    * 通过`bin/mysql`命令（*5.7.X含有零时密码*）可以正常登录

5. MySQL启动
    * `mysqld_safe --user=mysql &` 即可启动，`mysqld_safe`是一个守护`mysqld`进程的脚本程序，旨在`mysqld`意外停止时，可以重启`mysqld`进程
    * 也可以通过`INSTALL_BINARRY`中的的步骤，使用`/etc/init.d/mysql.server  start`进行启动（启动脚本以你复制的实际名字为准，通常改名为`mysqld`,即`/etc/init.d/mysqld start`）

-----

## 五. 附录
1. 姜老师的配置文件`my.cnf`   
    
    ```bash
    [client]
    user=david
    password=88888888
    
    [mysqld]
    ########basic settings########
    server-id = 11 
    port = 3306
    user = mysql
    bind_address = 10.166.224.32   #根据实际情况修改
    autocommit = 0   #5.6.X安装时，需要注释掉，安装完成后再打开
    character_set_server=utf8mb4
    skip_name_resolve = 1
    max_connections = 800
    max_connect_errors = 1000
    datadir = /data/mysql_data      #根据实际情况修改,建议和程序分离存放
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


2. 几个重要的参数配置和说明 
    * `innodb_log_file_size = 4G ` :做实验可以更改的小点，线上环境推荐用4G，以前5.5和5.1等版本之所以官方给的值很小，是因为太大后有bug，现在bug已经修复
    
    * `innodb_undo_logs = 128`和`innodb_undo_tablespaces = 3`建议在安装之前就确定好该值，后续修改比较麻烦
    * `[mysqld]`，`[mysqld-5.7]`这种tag表明了下面的配置在什么版本下才生效,`[mysqld]`下均生效
    * `autocommit`,这个参数在5.5.X以后才有，安装5.6.X的时候要注意先把该参数注释掉，等安装完成后，再行打开, 5.7.X无需预先注释
    * `datadir`, `innodb_log_group_home_dir`, `innodb_undo_directory`一定要注意他的权限是 `mysql:mysql`


3. `my.cnf`问题
    * 使用`mysqld --help -vv | grep my.cnf `查看mysql的配置文件读取顺序
    * 后读取的`my.cnf`中的配置，如果有相同项，会覆盖之前的配置
    * 使用`--defaults-files`可指定配置文件
    