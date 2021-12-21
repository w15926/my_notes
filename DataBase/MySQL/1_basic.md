# 配置

安装完mysql，使用网上各种配置教程尝试后，输入mysql -u root -p，仍会出现：zsh: command not found: mysql的提示。

解决方案：

1、在终端进入目录：

cd /usr/local/bin/

2、在终端设置mysql命令路径

sudo ln -fs /usr/local/mysql/bin/mysql mysql

3、输入mysql -u root -p



# 基础

**连接本地数据库**

```shell
mysql -u root -p
```



**连接远程数据库**

mysql -h ip地址 数据库名 -u用户名 -p



mysql -h 192.168.12.12 db_name -uusername -p

端口默认是3306，-h 后面只能接ip ，不能加端口号。



mysql -h 192.168.5.116 -P 3306 -u root -p123456



**退出**

```shell
exit;
```



**快捷键**

desc === describe



**数据类型 （command + click）**

https://www.runoob.com/mysql/mysql-data-types.html




# 增

## 创建数据库

```mysql
create database test; # 新建一个test数据库
```



## 进入数据库

```mysql
use sys # 进入sys数据库 sys为已有数据库名称
```




## 创建表

```mysql
create table person2(name VARCHAR(20),age int(3));
```



## 表中添加记录（数据）

要匹配表结构

```mysql
insert into person values('古力娜扎',14,'woman',3);
# 添加多条数据
insert into person
values('古力娜扎',14,'woman',3),('胖迪',14,'woman',1);
```



## 表中添加指定记录（数据）

单独给person表中的name结构添加一条或多条记录

```mysql
insert into person (name) values('小迪');
```





# 删

## 删除表

```mysql
drop table person2;
```



## 删除表中指定记录（数据）

```mysql
delete from person where name='范冰冰';
```



# 改

## 修改表中指定记录（数据）

person 表中匹配 id=0 的所有记录中 age 修改成 18

```mysql
update person set age=18 where id=0;
```




# 查

## 显示所有数据库

```mysql
show databases;
```



## 显示当前数据库下的所有表

```mysql
show tables;
```



## 显示表结构

```mysql
describe person; // person表
```



## 显示表里的所有记录（数据）

```mysql
select * from person; // person表
```



## 查询表中指定内容的所有记录

### 单表查询


- 结构查询

```mysql
select name,age from person; # 显示person表里name和age的所有数据
select name as user_name from person; # 别名
```

- 过滤重复

```mysql
select distinct name from person; # 显示person表里所有name数据（过滤重复）
select name from person group by person; # 把name抽离为一组显示，同样过滤重复
```

- 固定值查询

```mysql
select * from person where age=14; # 显示person表里age=14的所有数据
```

- 范围查询

```mysql
select * from person where age > 14 and id < 18; # 查询age > 14和id < 18的记录 

select * from person where age in (14,16,18,20); # age等于14、16、18、20的记录

# 或  查询 age=69和id=1002的记录  哪个满足返回哪个
select * from oerson where age=59 or id=1002; 

select * from person where age between 14 and 18; # 查询age值为14-18的记录 

# 查询成绩  高于学号为“102”和课程号为“3-105”  的成绩所有记录
select * from score
where degree>(select degree from score where sno='102' and cno='3-105');
```

- 升序、降序

```mysql
select * from person order by age asc; # 升序显示age  ascend
select * from person order by age desc; # 降序显示age  descend

select * from person order by age desc limit 0,1; # 降序拿到0，1的值

select * from person order by age desc,id asc; # age降序，id升序
```

- 统计数量

```mysql
select count(*) from person where age=14; # 返回age=14的总个数
```

- 最大值、最小值查询

```mysql
select max(age) from person; # 返回age最大的记录
select min(age) from person; # 返回age最小的记录

# 返回person表中age最大值的那条记录中的name和id
select name,id from person where age=(select max(age) from person);
```

- 平均值

```mysql
select avg(age) from person where name='小三'; # 查询所有name=小三的age平均值

# 查询所有name的平均age（自动过滤重复name）
select avg(age) from person group by name;
```

- 模糊查询 like

```mysql
# 查询person表中至少有两人age的平均数
select age from person group by age having count(age)>=2;
# 查询person表中至少有两人age的平均数，并且结果以 1 开头
select age from person group by age having count(age)>=2 and age
like '1%';

# 模糊查询 like 相反也有 not like（）
```



### 多表查询

- 子表sno对应（外键）父表sno，让他们进行替换显示

```mysql
# 父
select sno,sname from student;
+-----+--------------+
| sno | sname        |
+-----+--------------+
| 101 | 迪丽热巴     |
| 102 | 马尔扎哈     |
| 103 | 古力娜扎     |
| 104 | 范冰冰       |
| 105 | 李冰冰       |
| 106 | 王冰冰       |
| 107 | 朱冰冰       |
| 108 | 孙冰冰       |
| 109 | 都冰冰       |
| 110 | 赵冰冰       |
+-----+--------------+
10 rows in set (0.00 sec)

# 子  让101替换迪丽热巴，102替换马尔扎哈...
select sno,cno,degree from score;
+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 101 | 3-105 |     59 |
| 102 | 3-105 |     60 |
| 103 | 3-105 |     60 |
| 104 | 3-105 |     60 |
| 105 | 3-106 |     61 |
| 106 | 3-106 |     61 |
| 107 | 3-106 |     62 |
| 108 | 3-107 |     62 |
| 109 | 3-107 |     63 |
| 110 | 3-108 |     64 |
+-----+-------+--------+
10 rows in set (0.00 sec)

# 替换显示 （让外键重复的地方连接起来）
select sname,cno,degree from student,score 
where student.sno=score.sno;
+--------------+-------+--------+
| sname        | cno   | degree |
+--------------+-------+--------+
| 迪丽热巴     | 3-105 |     59 |
| 马尔扎哈     | 3-105 |     60 |
| 古力娜扎     | 3-105 |     60 |
| 范冰冰       | 3-105 |     60 |
| 李冰冰       | 3-106 |     61 |
| 王冰冰       | 3-106 |     61 |
| 朱冰冰       | 3-106 |     62 |
| 孙冰冰       | 3-107 |     62 |
| 都冰冰       | 3-107 |     63 |
| 赵冰冰       | 3-108 |     64 |
+--------------+-------+--------+
10 rows in set (0.00 sec)
```



- 查询所有学生的sname、cname、degree

三张表处于外键关系，让外键重复的地方连接起来，否则每个人出现三次（三张表）

sname --> student

cname --> course

degree --> score

```mysql
select sname,cname,degree from student,course,score
where student.sno=score.sno
and course.cno=score.cno;
```



- 查询所有教师和同学的name、birthday

```mysql
# union并  student表里查询的三个值会对应teacher表里查询的三条内容
select tname as name,tsex as sex,tbirthday as birthday from teacher
union
select sname,ssex,sbirthday from student;

# tname和sname都显示在tname里了（别名name）
+--------------+-----+---------------------+
| name         | sex | birthday            |
+--------------+-----+---------------------+
| 李老师       | 女  | 1998-01-01 00:00:00 |
| 王老师       | 女  | 1998-01-01 00:00:00 |
| 胖老师       | 女  | 1998-01-01 00:00:00 |
| 迪老师       | 女  | 1998-01-01 00:00:00 |
| 迪丽热巴     | 女  | 2003-01-01 00:00:00 |
| 马尔扎哈     | 女  | 2003-01-01 00:00:00 |
| 古力娜扎     | 女  | 2003-01-01 00:00:00 |
| 范冰冰       | 女  | 2003-01-01 00:00:00 |
| 李冰冰       | 女  | 2003-01-01 00:00:00 |
| 王冰冰       | 女  | 2003-01-01 00:00:00 |
| 朱冰冰       | 女  | 2003-01-01 00:00:00 |
| 孙冰冰       | 女  | 2003-01-01 00:00:00 |
| 都冰冰       | 女  | 2003-01-01 00:00:00 |
| 赵冰冰       | 女  | 2003-01-01 00:00:00 |
+--------------+-----+---------------------+
14 rows in set (0.01 sec)
```



### 连接查询

- 内连接 

> inner join 或者 join

```mysql
# 打印person表里内容，让person表里的cardID关联于card表里的id（类似于外键）
select * from person 
inner join card on person.cardID=card.id;
+----+--------+--------+----+-----------+
| id | name   | cardId | id | name      |
+----+--------+--------+----+-----------+
|  1 | 张三   |      1 |  1 | 饭卡      |
|  2 | 李四   |      3 |  3 | 农行卡    |
+----+--------+--------+----+-----------+
2 rows in set (0.00 sec)

# person
mysql> select * from person;
+----+--------+--------+
| id | name   | cardId |
+----+--------+--------+
|  1 | 张三   |      1 |
|  2 | 李四   |      3 |  # cardId 3 对应 card表里的 id 3
|  3 | 王五   |      6 |  # 6没有对应，如果用外键的话还插入不了
+----+--------+--------+
3 rows in set (0.00 sec)

# card
mysql> select * from card;
+----+-----------+
| id | name      |
+----+-----------+
|  1 | 饭卡      |
|  2 | 建行卡    |
|  3 | 农行卡    |
|  4 | 工商卡    |
|  5 | 邮政卡    |
+----+-----------+
5 rows in set (0.00 sec)
```



- 外连接  

> 左连接 left join 或者 left outer join
>
> 右连接 right join 或者 right outer join
>
> 完全外连接 full join 或者 full outer join  （MySQL暂不支持）

```mysql
# 左连接  person表里的内容取出来，如果左边person表里cardID对应右边card表id，就取出来，没有返回NULL
select * from person left join card on person.cardId=card.id;
+----+--------+--------+------+-----------+
| id | name   | cardId | id   | name      |
+----+--------+--------+------+-----------+
|  1 | 张三   |      1 |    1 | 饭卡      |
|  2 | 李四   |      3 |    3 | 农行卡    |
|  3 | 王五   |      6 | NULL | NULL      |
+----+--------+--------+------+-----------+
3 rows in set (0.05 sec)

# 右连接  与左相反
select * from person right join card on person.cardId=card.id;
+------+--------+--------+----+-----------+
| id   | name   | cardId | id | name      |
+------+--------+--------+----+-----------+
|    1 | 张三   |      1 |  1 | 饭卡      |
| NULL | NULL   |   NULL |  2 | 建行卡    |
|    2 | 李四   |      3 |  3 | 农行卡    |
| NULL | NULL   |   NULL |  4 | 工商卡    |
| NULL | NULL   |   NULL |  5 | 邮政卡    |
+------+--------+--------+----+-----------+
5 rows in set (0.01 sec)

# 完全外连接  MySQL不支持，只能用union并上
select * from person left join card on person.cardId=card.id
union
select * from person right join card on person.cardId=card.id;
+------+--------+--------+------+-----------+
| id   | name   | cardId | id   | name      |
+------+--------+--------+------+-----------+
|    1 | 张三   |      1 |    1 | 饭卡      |
|    2 | 李四   |      3 |    3 | 农行卡    |
|    3 | 王五   |      6 | NULL | NULL      |
| NULL | NULL   |   NULL |    2 | 建行卡    |
| NULL | NULL   |   NULL |    4 | 工商卡    |
| NULL | NULL   |   NULL |    5 | 邮政卡    |
+------+--------+--------+------+-----------+
6 rows in set (0.02 sec)
```



# 约束

## 主键约束

主键约束使得该记录在表中是唯一的

设置id的值在此表中是     唯一     不可重复      不能为空

### 增

> 创建表时对id设置主键约束

```mysql
create table person2(id int primary key,name varchar(20));
```



> 对新建的表中的某一结构单独设置主键（必须此表为空）

```mysql
alter table person add primary key(id);
```



> 联合主键
>
> > 只要id和name两个值加在一起在表中不重复就行

```mysql
create table person2(id int,name varchar(20),primary key(id,name));
```



### 删


> 删除表中所有的主键

```mysql
alter table person2 drop primary key;
```



### 改

> 对表中某一结构单独设置主键

```mysql
alter table person2 modify id int primary key;
```



### 查

直接显示表结构就可以了



## 自增约束

> 如果不赋值，默认为1，每次自增+1（auto_increment）

此时id默认为1，表中每多一条记录id +1

```mysql
create table person2(id int primary key auto_increment,name varchar(20));
```



## 唯一约束

**唯一约束和主键约束的区别是唯一可以为null，主键不能为null。**

**主键适合id，唯一适合可以为空但是又不能重复的**

### 增

> 新建表时设置name的值为唯一的

```mysql
create table person3(id int,name varchar(20),unique(name)); # 方法一可以写多个
create table person3(id int,name varchar(20) unique); # 写法二单独设置
```



> 联合唯一
>
> > 只要id和name两个值加在一起在表中不重复就行

```mysql
create table person3(id int,name varchar(20),unique(id,name)); 
```



> 对没有约束的结构添加唯一约束

```mysql
alter table person3 add unique(name);
```




> 修改当前约束为唯一

```mysql
alter table person3 modify name varchar(20) unique;
```



### 删

> 删除指定的结构的唯一约束

```mysql
alter table person3 drop index name; # 删除person2表里name结构的唯一约束
```




## 非空约束

修饰的字段不能为空 NULL

```mysql
create table person2 (id int primary key,name varchar(20) not null);
```

还可以使用约束通用的 alter...add和alter...modify（详见唯一约束）



## 默认约束

当没有添加值时设置默认值

```mysql
create table person2 (id int,name varchar(20),age int default 14);
```

还可以使用约束通用的 alter...add和alter...modify（详见唯一约束）



## 外键约束

涉及到两个表：父表、子表（主表、副表）

如果主表中的记录被副表引用，那么主表该记录无法删除

- 父表

```mysql
create table father (
    id int primary key,
    name varchar (20)
);
```

- 子表

```mysql
create table son (
    id int primary key,
    name varchar (20),
    son_id int,
    # son_id这个值必须和father表里的id值一样，如果超出则报错
    foreign key (son_id) references father(id) 
);
```



# 事务

让两条sql语句同时进行，可以同时成功和失败，可用于转账等

**注意：事务开启不能回滚** rollback;

```mysql
#  默认开启
mysql> select @@autocommit;
+--------------+
| @@autocommit |
+--------------+
|            1 |
+--------------+
1 row in set (0.01 sec)

# 手动关闭
mysql> set autocommit=0;
# 关闭后每次操作数据库都是暂存在虚拟表里，并没有真正提交给数据库，需要手动commit
# commit之后无法回滚
mysql> commit;

mysql> select @@autocommit;
+--------------+
| @@autocommit |
+--------------+
|            0 |
+--------------+
1 row in set (0.00 sec)
```



手动临时开启事务

```mysql
begin;
update user set money=money-100 where name='a';
update user set money=money+100 where name='b';

# 方法二
start transaction;
update user set money=money-100 where name='a';
update user set money=money+100 where name='b';

# 最后如果代码执行成功，别忘了commit
```



隔离

```mysql
# 系统
# mysql 8.xx版本查看方法
select @@global.transaction_isolation;
+--------------------------------+
| @@global.transaction_isolation |
+--------------------------------+
| REPEATABLE-READ                |
+--------------------------------+
1 row in set (0.00 sec)
```



***------------------------------ 隔离待补 ------------------------------***

