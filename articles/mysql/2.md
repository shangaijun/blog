002.MySQL创建数据库
==================
###1.root@hostname 07:03:41->create database oldboy;   
 #未指定字符集,采用MySQL默认配置
```
root@hostname 07:03:42->show create database oldboy \G;
```
###2.创建一个数据库,指定字符集
```
root@hostname 07:03:43->create database oldboy_gbk DEFAULT CHARACTER SET gbk COLLATE gbk_chinese_ci;

root@hostname 07:03:44->show create database ashang_gbk\G;

*************************** 1. row ***************************
       Database: oldboy_gbk
Create Database: CREATE DATABASE `oldboy_gbk` /*!40100 DEFAULT CHARACTER SET gbk */
1 row in set (0.00 sec)

create database oldboy_utf8 DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;

root@hostname 07:03:46->show create database oldboy_utf8\G
*************************** 1. row ***************************
       Database: oldboy_utf8
Create Database: CREATE DATABASE `oldboy_utf8` /*!40100 DEFAULT CHARACTER SET utf8 */
1 row in set (0.00 sec)

```

>其中:DEFAULT CHARACTER SET 是字符集
>        :COLLATE是校对规则

字符集不一致是数据库中文内容乱码的罪魁祸首

编译安装MySQL的时候,不指定字符集和校对规则的话,默认的就是latine.

企业里怎么创建数据库呢?

   1. 根据开发程序确定字符集(建议UTF8)

   2. 确定了字符集之后,编译的时候自定字符集,例如

    -DDEFAULT_CHARSET=utf8 \

    -DDEFAULT_COLLATION=utf8_general_ci \

   3. 创建数据库的时候,创建的数据库默认就是UTF8了.

   4. 编译MySQL的时候没有指定字符集或者创建了和程序不同的字符集,如何解决?

    指定字符集创建即可.

   5. 如果希望以后使用别的字符集，编译MySQL的时候，需要增加如下参数：

    -DEXTRA_CHARSETS=gbk,gb2312,utf8,ascii \

