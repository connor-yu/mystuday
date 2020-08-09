# 数据库 MySQL



## C/S架构

client 客户端（terminal、cmd、powershell...）

通过命令去访问/操作 数据库

server 服务端

MySQL Server mysql的服务端



## 一、数据库的基本操作



### 1.mysql的启动和停止

1.在服务中启动和停止

2.管理员身份运行terminal

net stop mysql57 停止

net start mysql57 启动



### 2.mysql连接与关闭

连接

```mysql
mysql -u root -p
Enter password: 123456 #123456为密码
```

```mysql
mysql -u root -p123456
```

关闭

```mysql
exit
\q
quit
```



### 3.创建data文件夹

```mysql
cd C:\Program Files\MySQL\MySQL Server 5.7

mysqld --initialize-insecure --user=root
```



### 4.数据库的显示

显示出所有仓库

```mysql
show databases;
```



### 5.创建数据库

```mysql
create database student; #创建一个student仓库
```

注意：关键词不能作为数据库名称

```mysql
create database `database`; #强制创建
```

不能创建已经存在的仓库

```mysql
create database if not exists student;
```

```mysql
create database if not exists `student`;
```

### 6.删除数据库

删除已经存在的仓库

```mysql
drop database student;
```

```mysql
drop database if exists `database`;
```

不存在的数据库

```mysql
drop database if exists student;
```

### 7.查看创建数据库的SQL

查看当初库是怎么创建的

```mysql
show create database student;
```

### 8.创建数据库指定字符编码以及查看数据库的字符编码

**实际开发中是utf8，gbk学习使用**

```mysql
create database if not exists `students` charset=gbk; 
```

### 9.修改数据库字符编码

```mysql
alter database teacher charset=gbk; #更新数据库
```



## 二、表的基本操作

### 1.引用数据库和查看查看数据库中的表

```mysql
create database if not exists `frank_school` charset=gbk;
```

指定要操作的仓库

```mysql
use frank_school;
```

查看表

```mysql
show tables;
```

### 2.创建表



1.普通

```mysql
create table student(
id int,
name varchar(30),
age int
);
```

查看表

```mysql
show tables;
```



2.B格

```mysql
create table if not exists teacher(
id int auto_increment primary key comment '主键id',
name varchar(30) not null comment '老师的名字',
phone varchar(20) comment '电话号码',
address varchar(100) default '暂时未知' comment '住址'
)engine=innodb;
```



> id name age 字段
>
> auto_increment 自动增长
>
> primary key 主键 最主要的 靠它来区分学生这张表 唯一的（一般情况下添加在id后边，不在name加，考虑重名的情况）
>
> comment 注释
>
> not null 不能为空
>
> default 默认值



查看创建的表

```mysql
show create table teacher;
show create table student;
```

### 3.查看表的结构

```mysql
desc teacher;
```

显示：

```mysql
mysql> desc teacher;
+---------+--------------+------+-----+----------+----------------+
| Field   | Type         | Null | Key | Default  | Extra          |
+---------+--------------+------+-----+----------+----------------+
| id      | int(11)      | NO   | PRI | NULL     | auto_increment |
| name    | varchar(30)  | NO   |     | NULL     |                |
| phone   | varchar(20)  | YES  |     | NULL     |                |
| address | varchar(100) | YES  |     | 暂时未知 |                |
+---------+--------------+------+-----+----------+----------------+
4 rows in set (0.00 sec)

mysql> desc student;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(30) | YES  |     | NULL    |       |
| age   | int(11)     | YES  |     | NULL    |       |
| class | int(11)     | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
```

### 4.删除表

```mysql
drop table student;
```

```mysql
drop table if exists student;
```

多个一起删除，用逗号隔开

```mysql
drop table if exists student, stu, oooo, jjjj;
```

### 5.修改表

修改表的字符编码

```mysql
alter table student charset=gbk;
```


修改之前：

```mysql
mysql> desc student;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(30) | YES  |     | NULL    |       |
| age   | int(11)     | YES  |     | NULL    |       |
| class | int(11)     | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
```

添加表的字段：

```mysql
alter table student add phone varchar(20);
```

在某一字段后添加：

```mysql
alter table student add gender varchar(1) after name;
```

添加字段放在开头：

```mysql
alter table student add address varchar(1) first;
```

修改之后：

```mysql
mysql> desc student;
+---------+-------------+------+-----+---------+-------+
| Field   | Type        | Null | Key | Default | Extra |
+---------+-------------+------+-----+---------+-------+
| address | varchar(30) | YES  |     | NULL    |       |
| id      | int(11)     | YES  |     | NULL    |       |
| name    | varchar(30) | YES  |     | NULL    |       |
| gender  | varchar(1)  | YES  |     | NULL    |       |
| age     | int(11)     | YES  |     | NULL    |       |
| class   | int(11)     | YES  |     | NULL    |       |
| phone   | varchar(20) | YES  |     | NULL    |       |
+---------+-------------+------+-----+---------+-------+
7 rows in set (0.00 sec)
```

删除字段

```mysql
alter table student drop address;
```

修改字段

```mysql
alter table student change phone tel_phone int(11);
```

修改之后：

```mysql
mysql> desc student;
+-----------+-------------+------+-----+---------+-------+
| Field     | Type        | Null | Key | Default | Extra |
+-----------+-------------+------+-----+---------+-------+
| id        | int(11)     | YES  |     | NULL    |       |
| name      | varchar(30) | YES  |     | NULL    |       |
| gender    | varchar(1)  | YES  |     | NULL    |       |
| age       | int(11)     | YES  |     | NULL    |       |
| class     | int(11)     | YES  |     | NULL    |       |
| tel_phone | int(11)     | YES  |     | NULL    |       |
+-----------+-------------+------+-----+---------+-------+
6 rows in set (0.00 sec)
```

只修改字段类型

```mysql
alter table student modify tel_phone varchar(13);
```

修改之后：

```mysql
mysql> desc student;
+-----------+-------------+------+-----+---------+-------+
| Field     | Type        | Null | Key | Default | Extra |
+-----------+-------------+------+-----+---------+-------+
| id        | int(11)     | YES  |     | NULL    |       |
| name      | varchar(30) | YES  |     | NULL    |       |
| gender    | varchar(1)  | YES  |     | NULL    |       |
| age       | int(11)     | YES  |     | NULL    |       |
| class     | int(11)     | YES  |     | NULL    |       |
| tel_phone | varchar(13) | YES  |     | NULL    |       |
+-----------+-------------+------+-----+---------+-------+
6 rows in set (0.00 sec)
```

改表名

```mysql
alter table student rename to students;
```





## 三、数据操作

### 1.插入数据

1.

```mysql
insert into teacher (id ,name, phone, address) values(1, 'Frank', '188888888888','ShangHai');
```

2.“赋值”不一定要按照顺序，必须一一对应，如：

```mysql
insert into teacher (phone, address, id ,name) values('188888889999','BeiJing', 2, 'Jeff');
```

3.简写

```mysql
insert into teacher values(3, 'Tom', '1000000000', 'NanJing');
```



看数据：

```mysql
select * from teacher;
```

```mysql
mysql> select * from teacher;
+----+-------+-------------+----------+
| id | name  | phone       | address  |
+----+-------+-------------+----------+
|  1 | Frank | 18888888888 | ShangHai |
+----+-------+-------------+----------+
1 row in set (0.00 sec)
```

4.

```mysql
insert into teacher values(NULL, 'Tom', NULL, NULL);
```

结果：teacher创建的时候id自增的，name不能为空

```mysql
mysql> select * from teacher;
+----+-------+-------------+----------+
| id | name  | phone       | address  |
+----+-------+-------------+----------+
|  1 | Frank | 18888888888 | ShangHai |
|  2 | Jeff  | 18889999999 | BeiJing  |
|  3 | Tom   | 10000000000 | NanJing  |
|  4 | Tom   | NULL        | NULL     |
+----+-------+-------------+----------+
4 rows in set (0.00 sec)
```

5.

```mysql
insert into teacher values(NULL, 'Jerry', NULL, default);
```

结果：

```mysql
mysql> select * from teacher;
+----+-------+-------------+----------+
| id | name  | phone       | address  |
+----+-------+-------------+----------+
|  1 | Frank | 18888888888 | ShangHai |
|  2 | Jeff  | 18889999999 | BeiJing  |
|  3 | Tom   | 10000000000 | NanJing  |
|  4 | Tom   | NULL        | NULL     |
|  5 | Jerry | NULL        | 暂时未知 |
+----+-------+-------------+----------+
5 rows in set (0.00 sec)
```

6.

```mysql
insert into teacher (name, phone, address) values('Jerry', NULL, default);
```

**一次性插入多条数据**

```mysql
insert into teacher values(NULL, 'TOM_1', NULL, default),(NULL, 'Jerry_1', NULL, default);
```



### 2.删除数据

对id删除

```mysql
delete from teacher where id=9;
```

对name删除，会删除所有的name相同的

```mysql
delete from teacher where name="Tom";
```

对int类型的字段删除

```mysql
delete frome teacher where id>2;
```

表中的数据全部删除（一个一个遍历删除）

```mysql
delete from teacher;
```

**清空表**

```mysql
truncate table student;
```

### 3.更新数据

```mysql
update teacher set name='frank' where id=1;
```

同理

```mysql
update teacher set name='tom' where phone=2222;
```

改多个值

```mysql
update teacher set name='tom', address='ShenZhen' where phone=2222;
```

如果没有where，所有数据都改了



```mysql
update teacher set address='shanghai' where phone=11111 or phone=2222;
```



### 4.查询表数据（基本）

```mysql
select phone, address from teacher;
```

用逗号隔开

### 5.SQL语句区分

DDL  data definition language  数据库定义语言 create alter drop show

DML  data manipulation language 数据库操纵语言  insert update delete select

DCL  data control language  数据库控制语言



### 6.字符编码问题

查看字符编码

```mysql
show variables like 'character_set_%';
```

改字符编码，实际开发中都是utf8

主要是client和results

```mysql
set character_set_client=gbk;
```



## 四、数据类型

### 1.int类型

```mysql
create table emp(
id smallint unsigned auto_increment primary key comment 'id',
age tinyint unsigned,
kkk int(6)
);
```



```mysql
mysql> desc emp;
+-------+----------------------+------+-----+---------+----------------+
| Field | Type                 | Null | Key | Default | Extra          |
+-------+----------------------+------+-----+---------+----------------+
| id    | smallint(5) unsigned | NO   | PRI | NULL    | auto_increment |
| age   | tinyint(3) unsigned  | YES  |     | NULL    |                |
| kkk   | int(6)               | YES  |     | NULL    |                |
+-------+----------------------+------+-----+---------+----------------+
3 rows in set (0.00 sec)
```

(5)：5宽度

### 2.浮点数

```mysql
number_1 float(3,1),
number_23 double(5,2)
```

（整数位数，小数位数）

小数位超过时，四舍五入

丢失精度

### 3.定点数

decimal 

不会丢失精度，支持无符号

钱 用这个

### 4.字符串与文本类型

char 不会回收多余的空间  效率高

varchar 回收多余的空间  效率不如char

### 5.布尔类型

### 6.枚举类型

```mysql
gender enum('man', 'woman', '?', 'nothing', 'it')
```

```mysql
insert into t_5 values('man');
insert into t_5 values('?');
insert into t_5 values('nothing');
insert into t_5 values(2); #可以是数字，比较方便
```

### 7.set集合类型

就是取多个

```mysql
hobby set('哲学', '经济学', '文学', 'IT', '数学', 'MBA')
```

```mysql
insert into t_6 values('IT,经济学'); #顺序无所谓，显示的时候按照顺序显示
```

### 8.时间日期类型

```mysql
createdTime datetime
```

```mysql
insert into t_7 values('2020-07-28 18:49:33');
```



## 五、列属性完整性

### 1.Primary Key主键的作用及企业用途

绝对唯一，能确定数据存在，不可重复

身份证号：查你身份证号，你的信息全出来了

学号（学校）：食堂、宿舍、电费

对其他的表提供帮助，因为其他的数据也可能会用到这个主键，主键的作用不仅仅局限在这一张表里（其他表里就不是这个主键了）

主键不允许是空值（NULL），除非设置为自增（auto_increment）

添加主键：

```mysql
alter table t_8 add primary key (id);
```

删除主键：

```mysql
alter table t_8 drop primary key;
```

一张表可以有两个主键，用途不广



### 2.组合键（复合主键）

某些网站：用户编号、昵称、ID...都是唯一的

### 3.unique唯一建的作用

保证数据的唯一

```mysql 
create table t_9(
id int primary key,
phone varchar(20) unipue
);
```

比如：

```mysql
insert into t_9 values(1, "123456");
insert into t_9 values(2, "123456");
```

这样会报错，因为phone重复

添加唯一建：

```mysql
alter table t_10 add unique(phone);
```

删除唯一键：

```mysql
alter table t_11 drop index phone;
```

### 4.sql内注释和代码注释

```mysql
create table t_12(
id int(20),	# 注释...
name varchar(20) comment '姓名'
);
```





















