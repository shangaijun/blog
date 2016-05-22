MySQL杂项
===

###1.查看数据库中表

语法:drop user 'username'@'主机域';
如果drop不掉某个用户,可以使用delete语句删除
```
mysql>delete from mysql.user where user='username' and host='hostname';
```
然后刷新下权限
```
mysql>flush privileges;
```

2.用户授权
```
mysql>help grant;
mysql>CREATE USER 'jeffrey'@'localhost' IDENTIFIED BY 'mypass';
mysql>GRANT ALL ON db1.* TO 'jeffrey'@'localhost';
mysql>GRANT SELECT ON db2.invoice TO 'jeffrey'@'localhost';
mysql>GRANT USAGE ON *.* TO 'jeffrey'@'localhost' WITH MAX_QUERIES_PER_HOUR 90;#创建了用户,但是没赋予权限的情况
```    
```
mysql>grant all on test.* to 'oldboy'@'localhost' identified by '123';
mysql>flush privileges;
```

|grant |all privileges|on dbname.*|to 'username'@'localhost'|identified by 'passwd'|
|:-----:|:-------------:|:-----------:|:------------------------:|:--------------------:|
|   命令    |   权限       |      目标库.表      | 用户名@主机 |用户密码|

3.查看权限
```
mysql> show **grants** for root@'localhost';
```
4.使用create和grant配合创建用户
```
help create;
help create user;
```
```
mysql> CREATE USER 'jeffrey'@'localhost' IDENTIFIED WITH my_auth_plugin;
mysql> create user oldgirl@'localhost' identified by 'passwd';
mysql> show grants for oldgirl@'localhost';  #查看用户授权情况
+----------------------------------------------------------------------------------------------------------------+
| Grants for oldgirl@localhost                                                                                   |
+----------------------------------------------------------------------------------------------------------------+
| GRANT USAGE ON *.* TO 'oldgirl'@'localhost' IDENTIFIED BY PASSWORD '*531E182E2F72080AB0740FE2F2D689DBE0146E04' |
+----------------------------------------------------------------------------------------------------------------+

mysql> grant all on dbname.* to oldgirl@'localhost';
mysql> show grants for oldgirl@'localhost';  #再次查看用户授权情况
```
5.授权其他机器连接数据库

mysql> create user test@'192.168.1.0/255.255.255.0' identified by 'test';

或者
```
mysql> create user test@'192.168.1.%' identified by 'test1';
mysql> grant all on test.* to test@'192.168.1.0/255.255.255.0';
mysql>flush privileges;
mysql>show grants for test@'192.168.1.0/255.255.255.0';
```
登陆测试:
```
[ashang@desktop mysql]$ mysql -utest -ptest -S mysql.sock -h 192.168.1.15 -P 3306;
```
出现
mysql>
就说明创建成功!
注意:实际测试,下面这样创建的用户无法登陆到mysql
```
create user test@'192.168.1.0/24' identified by 'test';
```
###6.使用revoke命令收回某个用户的某个权限
```
mysql>help revoke
mysql>REVOKE INSERT ON *.* FROM 'jeffrey'@'localhost';
```
要保证收回的权限要对应上某个数据库,使用下面的命令查看
```
mysql> show grants for test@'192.168.1.0/255.255.255.0';
```
执行revoke
```
mysql> revoke insert on test.* from test@'192.168.1.0/255.255.255.0';
```
再次查看权限:
```
mysql> show grants for test@'192.168.1.0/255.255.255.0';
```
或者
```
mysql -uroot -pshine123 -S mysql.sock  -e "show grants for test@'192.168.1.0/255.255.255.0';"
```
###7.企业环境怎么对用户授权

#### 7.1对web连接用户尽量采用最小化原则,很多开源软件都是web界面安装,因此安装期间给予select,insert,
update,delete4个权限外,还需要create,drop比较危险的权限.例如:
```
mysql>grant select,insert,update,delete,create,drop on blog.* to 'blog'@'10.0.0.%' identified by 'blog';
```
####7.2.生成数据库表后,收回create和drop权限.
```
   mysql>revoke create,drop on blog.* from blog@'10.0.0.%';
```
###8.远程登陆多实例MySQL数据库

服务器端
```
mysql> create user root@'192.168.1.3' identified by 'ashine123';
mysql> grant all on *.* to root@'192.168.1.3';
```
客户端
```
/usr/local/mysql/bin//mysql -uroot -pashine123 -h 192.168.1.15 -P 3308
```
注意:服务器端防火墙和Selinux设置问题

###9.MySQL备份数据库命令
```
/usr/local/mysql/bin/mysqldump -uroot -pashine -h 192.168.1.15 -P 3308 -B oldboy > /tmp/oldbooy.dump
```
###10.防止MySQL更新(update)数据的时候不指定where语句
```
mysql -uroot -pashine123 -h 192.168.1.15 -P 3308 -U
```
可以在系统上设定一个mysql的别名
```
alias mysql='mysql -U'
```

###11.delete table test 与 truncate table test 区别?
truncate 物理上的删除,直接清除物理文件,所以很快
delete 逻辑删除,一行一行删,很慢
###12.查看MySQL变量
```
mysql> show variables \G;
```
###13.查看status
```
mysql> show status \G;
mysql> show global status \G;
```
###14.设置变量
```
set global key_buffer_size=16777216
```
```
show variables like 'key_buffer%';  #不重启mysql,就生效
```
重启之后也生效,需要修改my.cnf


###15.如何修改全局变量后一直生效(不论mysql重启与否)
```
mysql>set global key_buffer_size=16777216;
```
修改my.cnf
```
key_buffer_size=16777216
```
所以无论怎么的,都生效

对于开关的变量不能修改,例如log_bin=ON


###16.额外给其他用户授权为root权限
```
mysql> grant all privileges on *.* to 'system'@'%' identified by 'oldboy123' with grant option;
```

###17.Mysql内部历史记录
```
cat /root/.mysql_history
```

###18.隐藏历史记录（bash）强制忽略敏感历史记录
```
vi /etc/profile
 #HISTCONTROL=ignorespace
```

###19.修改myql运行环境提示符

 # 临时生效
```
prompt \u@hostname \r:\m:\s->
```
 #永久生效，修改本地的/etc/my.cnf，记住，是本地的，不是服务器端的
```
[mysql]
prompt=\\u@hostname \\r:\\m:\\s->    
```

###20.使用MySQL的帮助
```
root@mysql 05:39:10->help

root@mysql 05:39:40->help create

root@mysql 06:10:19->help grant
```
善用手册，善用手册，善用手册

[初学者学习linux运维的几个问题及老鸟建议](http://oldboy.blog.51cto.com/2561410/1566703)

###21.MySQL密码操作
```
[root@mysql-study 3306]# mysqladmin -uroot -p123 password 'passwd123' -S mysql.sock
root@mysql 06:43:32->update user set password=PASSWORD('123') where user='root';
root@mysql 06:45:43->set password=password('456');  #修改当前登陆用户
```
[忘记MySQL密码？](https://github.com/shangaijun/blog/blob/master/articles/mysql/mysql.misc.md)



