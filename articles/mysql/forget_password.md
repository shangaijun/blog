001.忘记MySQL密码,如何破?
====================
###1.单实例情况

现将数据库停掉
```
/etc/init.d/mysqld stop
```
使用如下命令启动数据库
```
mysqld_safe --skip-grant-tables --user=mysql &
```
现在可以使用mysql命令登陆了
```
mysql -uroot
```
然后就可以改密码了
```
update mysql.user set password=password("password") where user='root' and host='localhost';
```
改完密码后，运行下面的命令关闭MySQL
```
mysqladmin -umysql shutdown
```
###2.多实例情况

先将数据库停掉,然后使用如下命令启动数据库
```
/bin/sh /app/mysql/bin/mysqld_safe --defaults-file=/data/3306/my.cnf --skip-grant-tables --user=mysql &
```
登陆
```
mysql -uroot -S mysql.sock
```
然后就可以改密码了
```
mysql>update mysql.user set password=password("password") where user='root' and host='localhost';
mysql>flush privileges;
```

修改mysql文件中关于password的部分，改为刚刚修改的密码

然后关闭数据库
```
./mysql stop
```
如果成功关闭，说明root密码修改成功，就可以正常使用数据库了

