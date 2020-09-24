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

### 5.数据库完整性

实体完整性，域完整性，参照完整性，自定义完整性

设计数据库保证：

字段完整，保证实体的完整性（一张表里应当有主键约束）

有些字段可以为空，有些不能为空；default

可能需要对外部进行引用

### 6.外键

```mysql
create table stu(
stuID int(4) primary key,
name varchar(20)
); 
```

```mysql
create table eatery(
id int primary key,
money decimal(10,4),
stuID int(4),
foreign key (stuID) references stu(stuID)
);
```

实际开发中（尤其并发处理）禁止使用外键

后期添加：

```mysql
alter table eatery_2 add foreign key (stuID) references stu(stuID);
```

### 7.置空和级联

一般情况下，再删除的时候使用置空操作，不使用级联

级联操作会把所有的信息全部改变

> 置空操作一般情况下是留给外界进行删除数据的
>
> 级联操作一般是留给外界更新数据的

演示：


```mysql
create table stu(
stuID int(4) primary key,
name varchar(20)
);
```

```mysql
create table eatery(
id int(20) primary key,
money decimal(10, 4),
stuId int(4),
foreign key(stuId) references stu(stuId) on delete set null on update cascade
    #删除设置置空，更新设置级联
);
```

 ```mysql
insert into stu values(1, 'frank');
insert into stu values(2, 'jerry');
 ```

```mysql
insert into eatery values(1,20.5,2);
insert into eatery values(2,453.4,1);
insert into eatery values(3,56.3,1);
insert into eatery values(4,11.5,2);
insert into eatery values(5,6.5,1);
```

级联操作：

```mysql
update stu set stuId='4' where name='frank';

mysql> select * from stu;
+-------+-------+
| stuID | name  |
+-------+-------+
|     2 | jerry |
|     4 | frank |
+-------+-------+
2 rows in set (0.00 sec)

mysql> select * from eatery;
+----+----------+-------+
| id | money    | stuId |
+----+----------+-------+
|  1 |  20.5000 |     2 |
|  2 | 453.4000 |     4 |
|  3 |  56.3000 |     4 |
|  4 |  11.5000 |     2 |
|  5 |   6.5000 |     4 |
+----+----------+-------+
5 rows in set (0.00 sec)
```

置空操作：

```mysql
delete from stu where stuId='2';

mysql> select * from stu;
+-------+-------+
| stuID | name  |
+-------+-------+
|     4 | frank |
+-------+-------+
1 row in set (0.00 sec)

mysql> select * from eatery;
+----+----------+-------+
| id | money    | stuId |
+----+----------+-------+
|  1 |  20.5000 |  NULL |
|  2 | 453.4000 |     4 |
|  3 |  56.3000 |     4 |
|  4 |  11.5000 |  NULL |
|  5 |   6.5000 |     4 |
+----+----------+-------+
5 rows in set (0.00 sec)
```



## 六、数据库设计思维

### 1.数据库设计基本概念

关系？关系型数据库 两张表的共有字段去确定数据的完整性

行？ 一条数据、记录、实体

列？ 一个字段、属性

OOP 

数据库冗余（优点：提高性能；缺点：数据太多，不好维护）只能减少，不能避免

数据的完整行

### 2.实体和实体之间的关系

一对一

一对多

多对一

多对多

### 3.Code第一范式 确保每列原子性

例：

时间段2018-2019，拆分为2018和2019两个

确保字段的原子性（原子不能再分）

### 4.Code第二范式 非键字段必须依赖键字段

就是别他妈没事找事

例：student表

学生表里填学生的基本信息，不能说学生有多少钱，花多少钱都放进去

不该有的东西不该有，和student基础信息没关系的全部干掉

### 5.Code第三范式 消除传递依赖

**根据项目的需求来，没有标准**

例：大学成绩

有些情况，查询的时候直接返回语数外总分给字段，没必要设置总分字段，是多余的

（大学没有总分）

高考成绩：肯定要给

## 七、单表查询

### 1.select

```mysql
mysql> select '去你妈的';
+----------+
| 去你妈的 |
+----------+
| 去你妈的 |
+----------+
1 row in set (0.00 sec)

mysql> select 2*7;
+-----+
| 2*7 |
+-----+
|  14 |
+-----+
1 row in set (0.00 sec)
```

#as 取别名,as可以省略

```mysql
mysql> select 'Go fuck youself' as qnmd;
+-----------------+
| qnmd            |
+-----------------+
| Go fuck youself |
+-----------------+
1 row in set (0.00 sec)
```

### 2.from

来自哪张表 ，主要使用笛卡尔积

例：

```mysql
create table t1(
id int(11),
name varchar(20)
);
insert into t1 values(1,'frank');
insert into t1 values(2,'jerry');

create t2(
score1 int(20),
score2 int(20)
);
insert into t2 values(98,98);
insert into t2 values(933,91);
```

```mysql
mysql> select * from t1,t2;
+------+-------+--------+--------+
| id   | name  | score1 | score2 |
+------+-------+--------+--------+
|    1 | frank |     98 |     98 |
|    2 | jerry |     98 |     98 |
|    1 | frank |    933 |     91 |
|    2 | jerry |    933 |     91 |
+------+-------+--------+--------+
4 rows in set (0.00 sec)
```



### 3.dual

dual 默认伪表

```mysql
mysql> select 2*7 as res from dual;
+-----+
| res |
+-----+
|  14 |
+-----+
1 row in set (0.00 sec)
```

### 4.where

条件筛选

```mysql
select * from t3 where age <= 18;
```

### 5.in / not...

```mysql
select * from t4 where address in('beijing','shanghai');
```

### 6.between...and... / not...

```mysql
select * from t3 where age>=15 and age<=20;
select * from t3 where age between 15 and 20; #包括20和15
```

### 7.is null /not...

```mysql
select * from t3 where age is null;
```

### 8.聚合函数

主要是做统计用的

```mysql
select sum(chinese) from score; #求成绩总和
select avg(chinese) from score; #求成绩平均值
select max(chinese) from score; #求成绩最大值
select min(chinese) from score; #求成绩最小值
select count(chinese) from score; #统计成绩次数
```

```mysql
select count(*) ...; #不要用，知道就行
```

### 9.like模糊查询

```mysql
select * from student where name like '张%';
```

%代表一个或多个字符

```mysql
select * from student where name like '张_';
```

_代表一个字符

### 10.order by排序查询

```mysql
select * from score order by chinese asc;
```

按照语文成绩排序，升序

**desc 降序**

### 11.group by 分组查询

```mysql
select avg(age) as '年龄' , gender '性别' from info group by gender;
```

分别求男和女的平均年龄

```mysql
select avg(age) as '年龄' , address '地区' from info group by address;
```

分别求北京和上海地区的平均年龄

### 12.group_concat

```mysql
select group_concat(name), gander from student group by gender;
```

聚合显示出名字和

### 13.having

不是对数据库中的数据进行筛选，而是查询后的结果进行筛选

对查询过后的数据不能用where

```mysql
select avg(age) as 'age', address as 'address' from info group by address having age>24;
```

前半部分是查询出来结果，形成一张虚拟的表，having 后根据形成的虚拟的表在进行查询(where)

注意添加别名（as...）便于筛选

### 14.limit

```mysql
select * from info limit 1,3;
```

从第二条数据开始往后查三个

```mysql
select * from info order by age desc limit 3;
```

查出这张表中年龄最大的三个人的数据，降序

### 15.distinct / all

```mysql
select distinct address from info;
```

查找去重

默认情况是all，通常忽略



## 八、多表查询

### 1.union联合查询

```mysql
select age,gender from info union select 'name',phone from teachar;
```

当两个表两个字段一样的时候，去重

```mysql
select age,gender from info union distinct select 'name',phone from teachar;
```

### 2.Inner join

innor join内连接，有公共字段

```mysql
select name,score from student innor join score on student.id=score.stuid;
```

两张表联合起来表示

### 3.left join

左连接

以左表为基准（谁在左边就以谁为主）

### 4.right join

右查询

### 5.cross join

```mysql
select * from t1 cross join t3;
```

返回笛卡尔积

### 6.natural join

自然连接

```mysql
select * from t1 natural join t3;
```

不用写两边公共的字段名，但是必须是字段名是一样的

默认自然内连接，同理还有自然左连接，自然右连接

### 6.无公共同名字段的自然连接返回笛卡尔积

### 7.using

当两张表字段都是一样的时候

```mysql
select * from t1 inner join t3 using(id);
```

（跟自然连接的效果是一样的）

### 8.哪一个连接实用？

看需求

## 九、子查询

### 1.子查询基本语法

```mysql
select * from student where id in (select stuid from score where score>=85);
```

### 2.in和not in



### 3.exists 和not exosts



## 十、高级部分

### 1.视图

1-5

### 2.事务

6-10

### 3.索引

11

### 4.存储过程

12-13

### 5.有趣的函数

14-16





## 十一、企业规范约束

### 1.库表字段约束规范

是不是VIP 字段：is_vip  类型：unsigned tinyint  长度：1

表名，字段名必须使用小写字母开头，不能数字开头，不能出现大写字母，分割单词必须用 “ _ ” 隔开

在windows下，默认不分大小写，但是在Linux下默认分大小写

表名不能出现复数，“students、teachers...”

凡是有小数的，不允许使用float，double，全部用decimal

如果需要的字符串很小，用char（定长），不要用varchar（varchar是可变长度字符串，不预分配内存空间，不建议超过5000，超过直接定义长文本text类型）

> 强制要求，表里必须定义三个字段，缺一不可(id,create_time,update_time)：
>
> id 必须为主键primary key，类型为bigint，无符号类型，单表的时候必须设为自增
>
> create_time、update_time 强制要求为datatime类型

定义年龄一般为tinyint，无符号的

### 2.索引规范









### 3.SQL开发约束



### 4.其他约束





















