003.MySQL显示连接删除数据库,查看数据库中表
=========================================

###1.显示数据库
```
show databases;
show databases like '%keyword%';
select database();   #显示使用的数据库

root@hostname 07:51:22->select database();
+------------+
| database() |
+------------+
| NULL       |
+------------+
1 row in set (0.00 sec)

root@hostname 07:51:27->use mysql;
Database changed
root@hostname 07:51:30->select database();
+------------+
| database() |
+------------+
| mysql      |
+------------+
1 row in set (0.00 sec)
```

###2.连接数据库
```
use dbname;
root@hostname 07:54:28->use oldboy_gbk;
Database changed
```
###3.删除数据库
```
drop database demo;
```
###4.查看数据库中表
```
show tables;
show tables from dbname;
show tables in dbname;
```
###5.学习的潜意识，就是查看帮助文档
```
help show databases;
help use;
hep drop;
help grant;
```
###6.其他命令

查看当前用户
```
root@hostname 07:55:49->select user();
+----------------+
| user()         |
+----------------+
| root@localhost |
+----------------+
1 row in set (0.00 sec)
```
查看当前版本

```
root@hostname 07:55:47->select version();
+------------+
| version()  |
+------------+
| 5.5.32-log |
+------------+
1 row in set (0.00 sec)
```
查看当前时间
```
root@hostname 07:55:52->select now();
+---------------------+
| now()               |
+---------------------+
| 2016-05-21 19:55:56 |
+---------------------+
1 row in set (0.00 sec)
```

