# ---------------------------------DDL-----------------------------
# *******************database定义*******************
### 创建数据库
```
create database if not exist db;
```
### 删除数据库
```
drop database if exist db;
```
### 修改数据库
```
alter database  db character set gbk;
```
# *************************table定义*************************
### 创建表
```
create table t(
name varchar(10),
age int
)
10为字符长度
```
### 修改表
```
***修改列名
alter table t change column name uname  varchar(11);
***修改列类型
alter table t modify column name int;
***增加列
alter table t add column city varchar(11);
***删除列
alter table t drop column city 
***修改表名
alter table t rename to ta;
```
### 删除表
```
drop table if exists t
```
### 复制表
```
***不含数据
create table tb like t;
***含数据
create table tb select * from t;
***部分字段及数据
create table tb select name from t where id=1;
```
#*************************数据类型*************************
### int
| name       | 字节          |有符号范围 |无符号范围|
| :-------------: |:-------------:| :-----:|:------------:|
| tinyint | 1     |        -128-127     |       0-255          |
|  smallint|  2    |        -32768-32767         | 0-65535                     |
|  mediumint|3      |                 |                   |
|  int|     4 |                         |                         |
|  bigint              |  8                |                         |                 |
|                                |                           |                                  |                                |
|                                |                           |                                  |                                |
```
create table t(
age int(10) unsigned zerofill
)
10代表不足十位前面补0，unsigned代表无符号int,zerofill代表开启补0
```
### 小数
| name       | 字节          |默认值 |用法|
| :-------------: |:-------------:| :-----:|:------------:|
|             float                   |    4                       |   插入任意float                               | float(5,2)最大插入5位数，小数点是两位，如insert into t values(5555,555)；插入临界值999.99                               |
|           double                     |             8              |                                  |                                |
|             decimal                   |                           |                     float(10,0)    |默认为10位整数         |
### 字符
| name       | -         |默认值 |用法|
| :-------------: |:-------------:| :-----:|:------------:|
|                  char              |                           |                     char（1）             |     长度固定，效率高                           |
|    varchar                           |                           |                      无默认值，需要指定            |         长度可变，效率低                       |
|                    enum            |                           |                                  |name enum('zhangsan','lisi');insert into t values('lisi')                                |
|                          set      |                           |                                  |        name set('li','hu','wang');insert into t values('li,hu')                        |
###日期
| name       | 字节          |默认值 |特点|
| :-------------: |:-------------:| :-----:|:------------:|
|    date          |             4              |                                  |                                |
|    time     |                    3       |                                  |                                |
|     year    |                      1     |                                  |                                |
|      datetime  |                  8         |                   范围较大               |                       不受时区影响         |
|         timestamp     |          4|                 小|受影响 now();当前时间              |
|              |          |                 |              |
# *************************约束*************************
### 六大约束
```
not null
default
unique
primary key
check
foreign key
***列级约束
foreign key无效果
***表级约束
not  null
default
无效果
```
###建表添加约束
```
***列级
create table t(
id int primary key;
age int not null;
name varchar(10) unique;
money int default 250；
)
***《show index from t查看表索引》
***表级
create table t(
CONSTRAINT [name] FOREIGN KEY(id) REFERENCES t2(id);
constraint [name] unique(age);可以为空，且多个
constraint [name] primary key(age,name);
一个且非空，联合主键
默认约束名为字段名
)
```
### 外键
从表为主要信息表
>1.从表添加外键
>2.主表被references字段必须为key，如unique，primary key
>3.相关联的外键数据类型一致，名称可以不一致
>4.先删除从表数据再删主表数据
>5.插入数据先主表再从表


###修改表添加约束
```
***列级
alter table t modify column name int primary key;
***表级
alter table t add [constraint name]
 foreign key(id) references t2(uid)
```
###修改表删除约束
```
***表级
alter table t drop primary key;
alter table t drop index name;
alter table t drop foreign key name;
show index from t
```
*****show variables like '%auto_increment%'
查看变量
*****set auto_increment_increment=3;
设置变量
###添加删除列自增长
```
alter table t modify column name int primary key auto_increment;
alter table t modify column name int primary key ;
列数据类型为数值型float int等
且为一个key且唯一
```
# ---------------------------------DQL----------------------------
## 1.select用法
```
select * from table;
select name from table;
select 100;
select 100*100;
select hello;
select 'hello';hello代表常量，加了字符串
select version();(接方法)
```
### 执行 
```
选中`select hello;`
F9执行,F12格式化代码
```
### 起别名 
```
select name as "姓名" from table;
```
### 去重
```
select distinct name from table;
```
### +号
```
select 11+11;结果22
select 11+'11';结果22
select 'join'+11;结果11
select null+11;结果null，null+任意类型=null
```
### concat字段拼接
```
select concat (name,age) from table;结果nameage
```
### and or not
```
select * from table where name=hello and age>100;
select * from table where name=hello or age>100;
select * from table where not(name=hello and age>100);
```
### like
```
select * from table where name like '%e%';name中有e的行
select * from table where name like '_e%';name第二个字符为e的行
select * from table where name like '_\_e';name第二三个字符为_e的行
select * from table where name like '_$(转义)_e' escape $;
```
### between and，in，is null，isnull
```
select * from table where age between 20 and 40;
select * from table where age in (20,40,60);
select * from table where name is not null and age=10;
select isnull(name,0) from table;
```
### 排序查询
```
select * from table where age>12 order by age desc;(降序，asc升序)
select age 年龄 from table where 
age>12 order by 年龄 desc;

```
### 函数
```
----------------------------------------------------------------字符函数
select length('join');
4
select length('长度');
6
select upper('join');
JOIN
select lower('JOin');
join
select substr('hello',2)
ello
select substr('hello',2,2)
el;2为长度，索引从1开始
select instr('hello','llo')
3
select length(trim("  hello  "));
hello
select trim('a' from "aaaheaalloaa");
heaallo
***左填充
select lpad('hello',10,'*')
hello
select lpad('hello',4,'*')
hell
***右填充
select rpad('hello',10,'*')
hello*****
***替换
select replace('hello','l','o')
heooo

----------------------------------------------------------------字符函数
----------------------------------------------------------------数字函数
***四舍五入
select round(1.5)
2
select round(-1.56)
-2
select round(1.56,1)
1.6
***向上取整
select ceil(-1.56)
-1
***向下取整
select floor(-1.56)
-2
***截断
select truncate(-1.56，1)
--1.5




----------------------------------------------------------------数字函数
```

# -------------------------------变量----------------------------
#变量
##全局变量
```
show global variables
show global variables like 'autocommit'
select @@global.autocommit
set @@global.autocommit=0
```
##会话变量
```
show session variables
show session variables like 'autocommit'
select @@session.autocommit
set @@session.autocommit=0
```
##用户变量（作用范围会话）
```
set @name='li'
select name into @name from t
select @name查询 
name的结果只能是一个
```
##局部变量（作用于begin end之间）
```
begin
declare id int default 11；
set id=12
select id
end
```
# -------------------------------事务-------------------------
`show engines` ;default engine支持事务
#特性
一致性
原子性
隔离性
持久性
#开启事务
`show variable 'autocommit'`
   
```
 SET autocommit=0;单次有效，修改my.ini永久有效
start transaction;
语句一
语句二
commit
or
rollback
***提交或回滚只能发生一次
```
###隔离级别（mysql默认Repeatable read）
```
Read uncommitted	√	√	√
Read committed	×	√	√
Repeatable read	×	×	√
Serializable
设置回滚点
语句一
savepoint a；
语句二
rollback to a；语句一还是持久化了
```
# ---------------------------视图----------------------------------
#创建
```
create view other as select a.name, a.age from user as a;
```
#删除
```
DROP  VIEW  IF  EXISTS  other
```
#查看
```
SHOW  CREATE  VIEW  other
```

# --------------------------过程----------------------------------
#结束符
```
例1:mysql>select * from student;   #回车时就会执行这条语句
例2:mysql>delimiter $
mysql>select * from student;   #回车时不会执行
->$    #在此回车才会执行上述语句
mysql>delimiter ;#将命令结束符重新设定为(;)
```
#空参创建及调用,删除
```
create procedure testa()
begin
    select * from users;
    select * from orders;
end;
--------------------
call testa()
--------------------
drop procedure testa;
show create procedure testa
```
#有参
```
create procedure 名称([IN|OUT|INOUT] 参数名 参数数据类型 )
begin
.........
end
------------------------------------------------------
***out案例
create procedure test5(in userId int,out username varchar(32))
        begin
            select name into username from users where id=userId;
        end;
***调用
set @uname=' ';
call test5(1,@uname);
select @uname
>
 概括：
        1、传出参数：在调用存储过程中，可以改变其值，并可返回；
        2、out是传出参数，不能用于传入参数值；
        3、调用存储过程时，out参数也需要指定，但必须是变量，不能是常量；
        4、如果既需要传入，同时又需要传出，则可以使用INOUT类型参数
原文链接：https://blog.csdn.net/qq_33157666/article/details/87877246

---------------------------------------------------
*** inout案例
create procedure test6(inout userId int,inout username varchar(32))
begin
    set userId=2;
    set username='';
    select id,name into userId,username from users where id=userId;
end;
***调用
set @uname=' ';
set @uid=0；
call test6(@uid,@uname)
select @uid,@uname
>
  概括：
        1、可变变量INOUT:调用时可传入值，在调用过程中，可修改其值，同时也可返回值；
        2、INOUT参数集合了IN和OUT类型的参数功能；
        3、INOUT调用时传入的是变量，而不是常量；

```

# --------------------------函数----------------------------------
 #有参有返回
```
 #案例1：根据员工名，返回它的工资

CREATE FUNCTION myf2(empName VARCHAR(10)) RETURNS DOUBLE

BEGIN

DECLARE salary DOUBLE DEFAULT 0;#定义局部变量
SELECT e.`salary` INTO salary #为局部变量赋值
FROM employees e
WHERE e.`last_name`=empName;
RETURN salary;

END $
 #删除函数
DROP FUNCTION myf2;
 #调用函数
SELECT myf2('kochhar')$
```

























