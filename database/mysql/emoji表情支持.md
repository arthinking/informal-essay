###1、数据库需要5.5.3+的支持

###2、修改database、table和column字符集
```sql
ALTER DATABASE guitargg CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci; 
ALTER TABLE gt_comment_content CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
ALTER TABLE gt_comment_content CHANGE content VARCHAR(2000) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

SHOW VARIABLES LIKE '%char%';
```
###3、修改配置文件
etc/mysql/my.cnf
```
[client]
default-character-set = utf8mb4

[mysql]
default-character-set = utf8mb4

[mysqld]
character-set-client-handshake = FALSE character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci init_connect='SET NAMES utf8mb4'
```
###4、重启MySQL Server
```
/etc/init.d/mysql restart
```
###5、升级JDBC驱动
确保mysql connector版本高于5.1.13
###6、检查应用的数据库连接配置：
```
jdbc.driverClassName=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/database?useUnicode=true&characterEncoding=utf8&autoReconnect=true&rewriteBatchedStatements=TRUE jdbc.username=root
jdbc.password=password
```

> [mysql/Java服务端对emoji的支持](http://segmentfault.com/blog/ilikewhite/1190000000616820 "mysql/Java服务端对emoji的支持")