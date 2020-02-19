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
-------
# ---------------------------------DML-----------------------------
```
往表中插入数据
  语法
insert into <表名> (<字段名1> ,<字段名n>) values (值1),(值2)
mysql> CREATE TABLE 'test' (
    -> id int(4) NOT NULL AUTO_INCREMENT,
    -> name char(20) NOT NULL,
    -> PRIMARY KEY(`id`)
    -> );

常规的插入方法
1. 指定所有列名，并且每个列都插入值
'''
mysql> insert into test(id,name) values(1,'scott');
Query OK, 1 row affected (0.00 sec)
mysql> select * from test;
+----+-------+
| id | name  |
+----+-------+
|  1 | scott |
+----+-------+
1 row in set (0.00 sec)
'''

2. 由于id列为自增的，所以，可以只在name列插入值
'''
mysql> insert into test(name) values('scott');
Query OK, 1 row affected (0.00 sec)

mysql> select * from test;
+----+-------+
| id | name  |
+----+-------+
|  1 | scott |
|  2 | scott |
+----+-------+
2 rows in set (0.00 sec)
'''

3. 如果不指定列，就要按规矩为每个列都插入适当的值
'''
mysql> insert into test values(4,'scott_boy');
Query OK, 1 row affected (0.00 sec)

mysql> select * from test;
+----+-----------+
| id | name      |
+----+-----------+
|  1 | scott     |
|  2 | scott     |
|  4 | scott_boy |
+----+-----------+
3 rows in set (0.00 sec)

'''

4. 批量插入数据的方法，提升效率，
'''
插入两个值
mysql> insert into test values(3,'scott_boy'),(5,'kaka');
Query OK, 2 rows affected (0.00 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql> select * from test;
+----+-----------+
| id | name      |
+----+-----------+
|  1 | scott     |
|  2 | scott     |
|  3 | scott_boy |
|  4 | scott_boy |
|  5 | kaka      |
+----+-----------+
5 rows in set (0.00 sec)
'''

删除数据
'''
mysql> delete from test;
Query OK, 5 rows affected (0.00 sec)

mysql> select * from test;
Empty set (0.00 sec)

mysql> desc test;
+-------+----------+------+-----+---------+----------------+
| Field | Type     | Null | Key | Default | Extra          |
+-------+----------+------+-----+---------+----------------+
| id    | int(4)   | NO   | PRI | NULL    | auto_increment |
| name  | char(20) | NO   |     | NULL    |                |
+-------+----------+------+-----+---------+----------------+

'''

插入多条数据
'''
mysql> insert into test values(1,'scott'),(2,'scot'),(4,'sco'),(3,'scott_boy'),(5,'kaka')
    -> ;
Query OK, 5 rows affected (0.00 sec)
Records: 5  Duplicates: 0  Warnings: 0

mysql> select * from test;
+----+-----------+
| id | name      |
+----+-----------+
|  1 | scott     |
|  2 | scot      |
|  3 | scott_boy |
|  4 | sco       |
|  5 | kaka      |
+----+-----------+
5 rows in set (0.00 sec)
'''
mysqldump -uroot -p -B scott > ./scott.sql

-A 指定所有的库
[root@mangodb ~]# grep -E -v "#|\/|'$|--" scott.sql
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
### 时间函数
`----------------------------------------------------------------时间函数`

<meta charset="utf-8">

#### NOW()、SYSDATE()、CURRENT_TIMESTAMP()

返回当前的日期和时间(以'yyyy-mm-dd hh:mm:ss'或yyyymmddhhmmss格式)

```
-- 2019-02-03 17:36:53
SELECT NOW()
-- 20190203173653.000000
SELECT CURRENT_TIMESTAMP() + 0

```

#### CURDATE()、CURRENT_DATE()

返回当前的日期(以'yyyy-mm-dd'或yyyymmdd格式)

```
-- 2019-02-03
SELECT CURDATE()
-- 2019-02-03
SELECT CURRENT_DATE()
-- 20190203
SELECT CURDATE() + 0

```

#### CURTIME()、CURRENT_TIME()

返回当前的时间(以'hh:mm:ss'或hhmmss格式)

```
-- 09:59:37
SELECT CURTIME()
-- 095937.000000
SELECT CURRENT_TIME() + 0

```

#### DATE()

返回日期或日期/时间表达式的日期部分

```
-- 2018-01-09
SELECT DATE('2018-01-09 09:45:45')
-- 2018-01-09
SELECT DATE('20180109')
-- NULL
SELECT DATE('123')

```

#### EXTRACT()

返回日期/时间的单独部分，比如年、月、日、小时、分钟等
语法：`EXTRACT(unit FROM date)`
其中unit值如下：
MICROSECOND、SECOND、MINUTE
HOUR、DAY、WEEK、MONTH、
QUARTER、YEAR、HOUR_MINUTE
DAY_MICROSECOND、DAY_SECOND
DAY_MINUTE、DAY_HOUR、YEAR_MONTH
SECOND_MICROSECOND、MINUTE_MICROSECOND MINUTE_SECOND、HOUR_MICROSECOND、HOUR_SECOND

```
SELECT EXTRACT(YEAR FROM NOW()) year,
       EXTRACT(MONTH FROM NOW()) month,
       EXTRACT(DAY FROM NOW()) day

```

#### DATE_ADD()、ADDDATE()

向日期添加指定的时间间隔，date参数是合法的日期表达式，expr参数是您希望添加的时间间隔(多个值以任意分隔符拼接)
语法：`DATE_ADD(date,INTERVAL expr type)`
type取值如下：
MICROSECOND、SECOND
MINUTE、HOUR
DAY、WEEK
MONTH、QUARTER
YEAR、SECOND_MICROSECOND
MINUTE_MICROSECOND、MINUTE_SECOND
HOUR_MICROSECOND、HOUR_SECOND
HOUR_MINUTE、DAY_MICROSECOND
DAY_SECOND、DAY_MINUTE
DAY_HOUR、YEAR_MONTH

```
-- 向指定字段添加两天
SELECT id,DATE_ADD(OrderDate,INTERVAL 2 DAY) AS OrderPayDate FROM Orders
-- 向指定字段减30分钟
SELECT closing_time,DATE_ADD(closing_time,INTERVAL -30 MINUTE) FROM work_shift_detail
-- 向指定时间加1分1秒(1:1 1-1都行)
SELECT DATE_ADD('2017-12-31 23:59:59', INTERVAL '1:1' MINUTE_SECOND) result;
-- 综合示例：查询最晚打卡时间(下班打卡时间加上下班有效打卡时间)
SELECT DATE_ADD(closing_time, 
INTERVAL + IF(ISNULL(closing_valid_time),0,closing_valid_time) MINUTE) 
as lateClosingTime

```

#### DATE_SUB()、SUBDATE()

从日期减去指定的时间间隔，参数同date_add
语法：`DATE_SUB(date,INTERVAL expr type)`

```
-- 减去两天
SELECT DATE_SUB(OrderDate,INTERVAL 2 DAY)

```

#### DATEDIFF()

返回两个日期之间的天数，date1和date2参数是合法的日期或日期/时间表达式，只有值的日期部分参与计算
语法：`DATEDIFF(date1,date2)`

```
-- 1
SELECT DATEDIFF('2017-12-30','2017-12-29') AS DiffDate
-- 38
SELECT DATEDIFF('20171230','2017-11-22') AS DiffDate
-- 0
SELECT DATEDIFF('2008-12-30 19:20:54','2008-12-30 11:20:54') AS DiffDate

```

#### DATE_FORMAT()、STR_TO_DATE()

DATE_FORMAT(date,format)，按指定格式对日期进行格式化操作
STR_TO_DATE(str,format)，将指定格式字符串转换为日期

```
-- 2019-02-05
SELECT DATE_FORMAT(NOW(), "%Y-%m-%d")
-- 2019-02-05 16:19:03
SELECT DATE_FORMAT(NOW(),'%Y-%m-%d %H:%i:%s')

```

format取值如下：

![image](//upload-images.jianshu.io/upload_images/14795543-f9185151d9fa2fc9.png?imageMogr2/auto-orient/strip|imageView2/2/w/900/format/webp)

#### TIME_FORMAT()

和date_format()类似，但time_format只处理小时、分钟和秒(其余符号产生一个null值或0)
语法：`TIME_FORMAT(time,format)`

```
-- 08 01
SELECT TIME_FORMAT('08:01:23','%h %i')
-- 08:01
SELECT TIME_FORMAT('2018-01-10 08:01:23','%h:%i')

```

#### UNIX_TIMESTAMP()

返回一个unix时间戳(从'1970-01-01 00:00:00'gmt开始的秒数,date默认值为当前时间)
语法：`UNIX_TIMESTAMP()`、`UNIX_TIMESTAMP(date)`

```
-- 1516423434
SELECT UNIX_TIMESTAMP()
-- 1516423424
SELECT UNIX_TIMESTAMP('2018-01-20 12:43:44')

```

#### FROM_UNIXTIME()

默认以'yyyy-mm-dd hh:mm:ss'或yyyymmddhhmmss格式返回时间戳的值(根据返回值所处上下文是字符串或数字)
语法：`FROM_UNIXTIME(unix_timestamp)`、`FROM_UNIXTIME(unix_timestamp,format)`

```
-- 2018-01-20 12:43:44
SELECT FROM_UNIXTIME(1516423424)
-- 18 01 20 12:51:55 2018
SELECT FROM_UNIXTIME(UNIX_TIMESTAMP(),'%y %m %d %h:%i:%s %x')

```

#### TIME_TO_SEC()

返回time值有多少秒
语法：`TIME_TO_SEC(time)`

```
-- 80580
SELECT TIME_TO_SEC('22:23:00')

```

#### SEC_TO_TIME()

以'hh:mm:ss'或hhmmss格式返回秒数转成的time值(根据返回值所处上下文是字符串或数字)
语法：`SEC_TO_TIME(seconds)`

```
-- 22:23:00
SELECT SEC_TO_TIME(80580)
-- 222300.000000
SELECT SEC_TO_TIME(80580) + 0

```

#### DAYOFWEEK()

返回日期date是星期几(1=星期天,2=星期一,……7=星期六,odbc标准)
语法：`DAYOFWEEK(date)`

```
-- 结果：7
SELECT DAYOFWEEK('2018-01-20')

```

#### WEEKDAY()

返回日期date是星期几(0=星期一,1=星期二,……6=星期天)
语法：`WEEKDAY(date)`

```
-- 结果：5
SELECT WEEKDAY('2018-01-20 20:00:00')

```

#### DAYOFMONTH()

返回date是一月中的第几日(在1到31范围内)
语法：`DAYOFMONTH(date)`

```
-- 结果：20
SELECT DAYOFMONTH('2018-01-20')

```

#### DAYOFYEAR()

返回date是一年中的第几日(在1到366范围内)
语法：`DAYOFYEAR(date)`

```
-- 结果：51
SELECT DAYOFYEAR('2018-02-20')

```

#### MONTH()

返回date中的月份数值
语法：`MONTH(date)`

```
-- 结果：1
SELECT MONTH('2018-01-20')

```

#### DAYNAME()

返回date是星期几(按英文名返回)
语法：`DAYNAME(date)`

```
-- 结果：Saturday
SELECT DAYNAME('2018-01-20')

```

#### MONTHNAME()

返回date是几月(按英文名返回)
语法：`MONTHNAME(date)`

```
-- 结果：January
SELECT MONTHNAME('2018-01-20')

```

#### QUARTER()

返回date是一年的第几个季度
语法：`QUARTER(date)`

```
-- 结果：3
SELECT QUARTER('2018-08-20')

```

#### WEEK()

返回日期的星期数，mode默认值0，mode取值1表示周一是周的开始，0从周日开始
语法：`WEEK(date[,mode])`

```
-- 结果：2
SELECT WEEK('2018-01-20')
-- -- 结果：3
SELECT WEEK('2018-01-20',1)

```

#### YEAR()

返回date的年份(范围在1000到9999)
语法：`YEAR(date)`

```
-- 结果：2018
SELECT YEAR('2018-01-20')

```

#### HOUR()

返回time的小时数(范围是0到23)
语法：`HOUR(time)`

```
-- 结果：10
SELECT HOUR('10:05:03')

```

#### MINUTE()

返回time的分钟数(范围是0到59)
语法：`MINUTE(time)`

```
-- 结果：15
SELECT MINUTE('1998-02-03 10:15:03')

```

#### SECOND()

返回time的秒数(范围是0到59)
语法：`YEAR(date)`

```
-- 结果：30
SELECT SECOND('22:24:30')

```

#### PERIOD_ADD()

增加n个月到时期p并返回(p的格式yymm或yyyymm)
语法：`PERIOD_ADD(P,N)`

```
-- 结果：201809
SELECT PERIOD_ADD('201801',8)

```

#### SPERIOD_DIFF()

返回在时期p1和p2之间月数(p1和p2的格式yymm或yyyymm)
语法：`PERIOD_DIFF(P1,P2)`

```
-- 结果：11
SELECT PERIOD_DIFF(9802,199703)

```

#### TO_DAYS()

返回日期date是西元0年至今多少天(不计算1582年以前)
语法：`TO_DAYS(date)`

```
-- 结果：728779
SELECT TO_DAYS(950501)
-- 结果：737070
SELECT TO_DAYS('2018-01-11')

```

#### FROM_DAYS()

给出西元0年至今多少天返回date值(不计算1582年以前)
语法：`FROM_DAYS(N)`

```
-- 结果：2018-01-11
SELECT FROM_DAYS(737070)
```

作者：若汐缘
链接：https://www.jianshu.com/p/01296699e3e7
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
`----------------------------------------------------------------日期函数`
## 其他函数
`----------------------------------------------------------------其他函数`
```
select database();
select user
select version
```

`----------------------------------------------------------------其他函数`
# `------------------`分组函数
`----------------------------------------------------------------分组函数`
```
#分组函数
#用作统计使用，又称为聚合函数或组函数
/*
sum:
avg
max
min
count:

*/
SELECT SUM(salary) FROM employees;

SELECT AVG(salary) FROM employees;

SELECT MAX(salary) FROM employees;

SELECT MIN(salary) FROM employees;

SELECT COUNT(salary) FROM employees;


# 使用特点
#参数类型支持哪些类型: 
#sum avg : 数值型
#max min : 任何类型
#count :非空值个数


#是否忽略null值: 
# sum null值不参与运算
# avg 忽略null值
# max min 
# count 忽略


# 可以和distinct 搭配
SELECT SUM(DISTINCT salary), SUM(salary) FROM employees;
SELECT COUNT(DISTINCT salary), COUNT(salary) FROM employees;

#count 的详细介绍
SELECT COUNT(salary) FROM employees;

# 任何非空列都包括
# 常用来统计包含多少行
SELECT COUNT(*) FROM employees; //只要每列的值不全为空，都计算在内

SELECT COUNT(1) FROM employees; //相当于添加一列，每列值为1，统计1的个数

#效率
MYISAM存储引擎下，COUNT(*)的效率高
INNODB存储引擎下，COUNT(*)和COUNT(1)的效率差不多，比COUNT(字段)要高一些
COUNT(字段)需要判断字段的值是否为null,为null不计算在内，涉及到筛选操作

#和分组函数一同查询的字段要求是group by 后的字段
SELECT AVG(salary),employee_id FROM employees;
此处employee_id没有意义


#  查询最大入职天数和最小入职天数的相差天数
SELECT DATEDIFF(MAX(hiredate),MIN(hiredate)) FROM employees;

# 查询部门为90的员工个数
 SELECT COUNT(*) FROM employees
WHERE department_id=90;
```
`----------------------------------------------------------------分组函数`
## 分组查询
>一、概要
Group By语句从英文的字面意义上理解就是“根据(by)一定的规则进行分组(Group)”。它的作用是通过一定的规则将一个数据集划分成若干个小的区域，然后针对若干个小区域进行数据处理。 如果在查询的过程中需要按某一列的值进行分组,以统计该组内数据的信息时,就要使用group by子句。不管select是否使用了where子句都可以使用group by子句group by子句一定要与分组函数结合使用,否则没有意义。

>二、语法格式
----语句------------------------------------------------------------执行顺序-----
SELECT [DISTINCT] * | 列名称 [别名] , 列名称 [别名] ,... | 统计函数   4、确定查询列
FROM 数据表 [别名] , 数据表 [别名] ,...                              1、数据来源
[WHERE 条件(s)]                                                    2、过滤数据行
[GROUP BY 分组字段, 分组字段, ...]                                   3、执行分组操作
[ORDER BY 字段 [ASC | DESC] , 字段 [ASC | DESC] ,...]               5、数据排序


`三、示例代码1`
## 查询每个部门的人数

```
SELECT deptno ,COUNT(*)
FROM emp
GROUP BY deptno;
显示每个部门员工的平均工资
SELECT deptno ,AVG(sal) 平均工资
FROM emp
GROUP BY deptno;
显示各个部门员工的工资+奖金
SELECT deptno,SUM(sal + IFNULL(comm,0))
FROM emp
GROUP BY deptno;
按照部门编号分组，求出每个部门的人数，平均工资(要求截取2位)(配合单行函数使用)
SELECT deptno, COUNT(empno), ROUND(AVG(sal),2)
FROM emp
GROUP BY deptno;
按照职位分组，求出每个职位的最高和最低工资(单字段分组)
SELECT job, MAX(sal), MIN(sal)
FROM emp
GROUP BY job;
查询每个部门的每种岗位的平均工资和最低工资
SELECT AVG(sal), MIN(sal)
FROM emp
GROUP BY job;
先统计出各个职位(job)的平均工资(AVG),再统计平均工资最高的工资(分组函数嵌套)
SELECT MAX(AVG(sal))
FROM emp
GROUP BY job
注意:分组函数允许嵌套，但是嵌套之后的分组函数的查询之中不能再出现任何的其他字段
查询每个岗位的总工资但不包括'SALESMAN'岗位(配合Where使用)
SELECT
FROM emp
WHERE name !='SALESMAN'
按部门、不同的职位，统计员工的工资总额 (多字段统计)
SELECT deptno, job, sum(sal)
FROM emp
GROUP BY deptno, job;
查询各个部门中相同职位的员工人数并且按部门编号排序(多字段统计排序)
SELECT DEPTNO, JOB,COUNT(*)
FROM  emp
GROUP BY deptno,job
ORDER BY deptno;
四、注意事项
GROUP BY后不可以接列的别名（根据执行顺序分析就知道了）
SELECT  deptno dn ,AVG(sal)
FROM emp
GROUP BY dn;  --错误
GROUP BY 后不能接数字
SELECT  deptno dn ,AVG(sal)
FROM emp
GROUP BY 1;   --错误
GROUP BY 后不可以接select后没有的列名
SELECT  deptno dn ,AVG(sal)
FROM emp
GROUP BY job;
如果一个SELECT中使用了分组函数，任何不在分组函数中的列(表达式)必须要在GROUP BY中
SELECT  job ,deptno dn ,AVG(sal) --deptno列group by 后面没有，使用会报错
FROM emp
GROUP BY job;
笔记：3和4总结为一句话

1、在select中出现的列名必须在group by 中出现，否则，其他列名只能在分组函数中使用;而在group by 中出现的字段不一定要在select中出现

group by之前可以使用where过滤数据,因为where是在分组之前起作用的，(执行顺序分析) ----废话
五、使用HAVING过滤分组
1、说明
首先对数据行进行分组。
把所得到的分组应用到分组函数中。
最后显示满足having条件的记录
作用：在分组之后再过滤掉不符合条件的分组
2、与where的区别
只有having里面可以使用分组函数，where中不允许出现分组函数
相同作用——都是根据条件过滤数据；不同的是where是在分组之前过滤数据，having是分组之后过滤分组数据。
原则：能在where里过滤的数据就不要在having里面去过滤
3、语法格式
----语句-----------------------------------------------------------执行顺序---------
SELECT [DISTINCT] * | 列名称 [别名] , 列名称 [别名] ,... | 统计函数   5、确定查询列
FROM 数据表 [别名] , 数据表 [别名] ,...                              1、数据来源
[WHERE 条件(s)]                                                     2、过滤数据行
[GROUP BY 分组字段, 分组字段, ...]    [HIAVING 过滤分组]              3、执行分组操作
[HAVING 条件(s)]                                                    4、过滤分组数据
[ORDER BY 字段 [ASC | DESC] , 字段 [ASC | DESC] ,...]               6、数据排序


```
`4、示例代码`
##查询部门的员工人数大于五部门编号
```

SELECT deptno,COUNT(*)
FROM emp
GROUP BY deptno
HAVING COUNT(*)> 5;
查询部门工资总和大于10000的部门编号
SELECT deptno, SUM(sal)
FROM emp
GROUP BY deptno
HAVING SUM(sal)>10000;
查询平均工资低于2000的部门号和它的平均工资
SELECT deptno,AVG(sal) a
FROM emp
GROUP BY deptno
HAVING avg(sal)>2000;
查询每个岗位的总工资并且不包括职位是'SALESMAN'岗位而且工资和大于5000
SELECT SUM(sal)
FROM emp
WHERE job!='SALESMAN'
GROUP BY job  HAVING SUM(sal)>5000
六、综合示例
查询非销售人员工作名称以及从事同一工作雇员的月工资的总和，并且要满足从事同一工作的雇员的月工资合计大于$5000，输出结果按月工资的合计升序排列
1、查询出所有的非销售人员的信息
SELECT * FROM emp WHERE job!=SALESMAN';
2、按照职位进行分组，并且使用SUM函数统计
SELECT job,SUM(sal)
FROM emp
WHERE job<>'SALESMAN'
GROUP BY job;
3、月工资的合计是通过统计函数查询的，所以现在这个对分组后的过滤要使用HAVING子句完成
SELECT job,SUM(sal)
FROM emp
WHERE job!='SALESMAN'
GROUP BY job
HAVING SUM(sal)>5000;
4、按照升序排列
SELECT job,SUM(sal) sum
FROM emp
WHERE job!='SALESMAN'
GROUP BY job
HAVING SUM(sal)>5000
ORDER BY sum ASC;
显示部门编号不是30的，的部门详细信息（部门编号、部门名称、部门人数、部门月薪资总和），并要求 部门月工资总和大于8000，输出结果按部门月薪资的总和降序排列。
SELECT d.deptno,d.dname,COUNT(*) 人数,ifnull(SUM(e.sal),0) 月总收入
FROM dept d,emp e
WHERE d.deptno=e.deptno AND d.deptno!=30
GROUP BY d.deptno,d.dname
HAVING SUM(e.sal) >8000
ORDER BY SUM(e.sal) DESC;
或
select deptno,d.dname ,count(*) peonum,sum(e.sal) s
from dept d left join emp e using(deptno)  --注意:using()中的字段在使用时不能有前缀。
where deptno !=30
group by deptno ,d.dname
having sum(e.sal)>8000
order by s desc;
```
>七、性能问题
能在where能过滤数据不要在having里过滤，A和B都能达到同样的目的，
但是A性能相对好一些，因为A现将deptno=30的数据筛选出来，
然后在将筛选的数据放入到临时表空间内进行分组；
而B将全部的数据都读到临时表空间内，
然后在临时表空间进行筛选数据，
这样一来B就需要更大的临时表空间进行分组筛选，索引性能较差。

八、多字段分组
```
select count(*),id,name from t group by id ,name;
```

作者：唯老
链接：https://www.jianshu.com/p/fb1c7bc4da39
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
# 连接查询
```
连接查询
连接查询：将多张表（大于等于 2 张表）按照某个指定的条件进行数据的拼接，其最终结果记录数可能有变化，但字段数一定会增加。
意义：在用户查询数据的时候，需要显示的数据来自多张表。
连接查询的语法格式：左表 join 右表
连接查询的分类：
内连接
外连接
自然连接
交叉连接
交叉连接
交叉连接：cross join，从一张表中循环取出每一条记录，每条记录都去另外一张表进行匹配，匹配的结果都保留（没有条件匹配），而连接本身的字段会增加，最终形成的结果为笛卡尔积形式。实际上，笛卡尔积形式（交叉连接和多表查询）的结果并没有什么实际意义，应该尽量避免，其存在的价值就是保证连接这种结构的完整性。
基本语法：左表 cross join 右表，效果与多表查询相同！
mysql> SELECT * FROM student cross join class;
+----+--------+------+-------+------+------+----+-------+------+
| id | name   | age  | grade | sex  | c_id | id | grade | room |
+----+--------+------+-------+------+------+----+-------+------+
|  1 | curry  |   30 |   3.1 | 男   |    1 |  1 |   3.1 | A    |
|  1 | curry  |   30 |   3.1 | 男   |    1 |  2 |   3.2 | B    |
|  1 | curry  |   30 |   3.1 | 男   |    1 |  3 |   3.4 | C    |
|  2 | durant |   29 |   3.4 | 男   |    1 |  1 |   3.1 | A    |
|  2 | durant |   29 |   3.4 | 男   |    1 |  2 |   3.2 | B    |
|  2 | durant |   29 |   3.4 | 男   |    1 |  3 |   3.4 | C    |
|  3 | Riuo   |   27 |   3.6 | 女   |    1 |  1 |   3.1 | A    |
|  3 | Riuo   |   27 |   3.6 | 女   |    1 |  2 |   3.2 | B    |
|  3 | Riuo   |   27 |   3.6 | 女   |    1 |  3 |   3.4 | C    |
|  4 | harden |   29 |   3.2 | 男   |    1 |  1 |   3.1 | A    |
|  4 | harden |   29 |   3.2 | 男   |    1 |  2 |   3.2 | B    |
|  4 | harden |   29 |   3.2 | 男   |    1 |  3 |   3.4 | C    |
+----+--------+------+-------+------+------+----+-------+------+
12 rows in set (0.16 sec)

mysql> SELECT * FROM student, class;
+----+--------+------+-------+------+------+----+-------+------+
| id | name   | age  | grade | sex  | c_id | id | grade | room |
+----+--------+------+-------+------+------+----+-------+------+
|  1 | curry  |   30 |   3.1 | 男   |    1 |  1 |   3.1 | A    |
|  1 | curry  |   30 |   3.1 | 男   |    1 |  2 |   3.2 | B    |
|  1 | curry  |   30 |   3.1 | 男   |    1 |  3 |   3.4 | C    |
|  2 | durant |   29 |   3.4 | 男   |    1 |  1 |   3.1 | A    |
|  2 | durant |   29 |   3.4 | 男   |    1 |  2 |   3.2 | B    |
|  2 | durant |   29 |   3.4 | 男   |    1 |  3 |   3.4 | C    |
|  3 | Riuo   |   27 |   3.6 | 女   |    1 |  1 |   3.1 | A    |
|  3 | Riuo   |   27 |   3.6 | 女   |    1 |  2 |   3.2 | B    |
|  3 | Riuo   |   27 |   3.6 | 女   |    1 |  3 |   3.4 | C    |
|  4 | harden |   29 |   3.2 | 男   |    1 |  1 |   3.1 | A    |
|  4 | harden |   29 |   3.2 | 男   |    1 |  2 |   3.2 | B    |
|  4 | harden |   29 |   3.2 | 男   |    1 |  3 |   3.4 | C    |
+----+--------+------+-------+------+------+----+-------+------+
12 rows in set (0.00 sec)
```
## 内连接
>内连接：inner join，从左表中取出每一条记录，和右表中的所有记录进行匹配，并且仅当某个条件在左表和右表中的值相同时，结果才会保留，否则不保留。
基本语法：左表 + [inner] join + 右表 + on 左表.字段 = 右表.字段;其中，关键字on表示连接条件，两表中的条件字段有着相同的业务含义。
实例：将表student与表class进行内连接
```
mysql> SELECT * FROM student join class
    -> on student.grade = class.grade;
+----+--------+------+-------+------+------+----+-------+------+
| id | name   | age  | grade | sex  | c_id | id | grade | room |
+----+--------+------+-------+------+------+----+-------+------+
|  1 | curry  |   30 |   3.1 | 男   |    1 |  1 |   3.1 | A    |
|  2 | durant |   29 |   3.4 | 男   |    1 |  3 |   3.4 | C    |
|  4 | harden |   29 |   3.2 | 男   |    1 |  2 |   3.2 | B    |
+----+--------+------+-------+------+------+----+-------+------+
3 rows in set (0.00 sec)
注意：如果两表中有某张表的条件字段名唯一，那么在书写连接条件的时候，可以省略表名，直接书写字段名。MySQL 会自动识别唯一字段名，但不建议这么做。此外，在上面的结果中有同名字段，这会给我们理解数据的意义造成一定的困扰，这时就需要使用字段别名和表别名做区别啦！
实例：将表student与表class进行内连接，起别名
mysql> SELECT s.*, c.id as class_id, c.grade as c_grade,room
    -> from student as s
    -> inner join class as c
    -> on s.grade = c.grade;
+----+--------+------+-------+------+------+----------+---------+------+
| id | name   | age  | grade | sex  | c_id | class_id | c_grade | room |
+----+--------+------+-------+------+------+----------+---------+------+
|  1 | curry  |   30 |   3.1 | 男   |    1 |        1 |     3.1 | A    |
|  2 | durant |   29 |   3.4 | 男   |    1 |        3 |     3.4 | C    |
|  4 | harden |   29 |   3.2 | 男   |    1 |        2 |     3.2 | B    |
+----+--------+------+-------+------+------+----------+---------+------+
3 rows in set (0.00 sec)
最后，内连接可以没有连接条件：可以没有on及之后的内容，这时内连接的结果全部保留，与交叉连接的结果完全相同。而且在内连接的时候可以使用where关键字代替on，但不建议这么做，因为where没有on的效率高。
```

## 外连接

>外连接：left\right join，以某张表为主表，取出里面的所有记录，然后让主表中的每条记录都与另外一张表进行连接。不管能否匹配成功，其最终结果都会保留，匹配成功，则正确保留；匹配失败，则将另外一张表的字段都置为NULL。
基本语法：左表 + left\right + join + 右表 + on + 左表.字段 = 右表.字段;其中，关键字on表示连接条件，两表中的条件字段有着相同的业务含义。在这里，以主表为依据，外连接分为两种，分别为：
left join：左外连接（左连接），以左表为主表；
right join： 右外连接（右连接），以右表为主表；
实例：将表 student 与 class 进行左连接

```
mysql> SELECT s.*, c_id as class_id, c.grade as c_grade, room
    -> FROM student as s
    -> left join class as c
    -> on s.grade = c.grade;
+----+--------+------+-------+------+------+----------+---------+------+
| id | name   | age  | grade | sex  | c_id | class_id | c_grade | room |
+----+--------+------+-------+------+------+----------+---------+------+
|  1 | curry  |   30 |   3.1 | 男   |    1 |        1 |     3.1 | A    |
|  4 | harden |   29 |   3.2 | 男   |    1 |        1 |     3.2 | B    |
|  2 | durant |   29 |   3.4 | 男   |    1 |        1 |     3.4 | C    |
|  3 | Riuo   |   27 |   3.6 | 女   |    1 |        1 |    NULL | NULL |
+----+--------+------+-------+------+------+----------+---------+------+
4 rows in set (0.03 sec)
无论以那张表为主表，其外连接的结果（记录数量）都不会少于主表的记录总数。此外，虽然左连接与右连接有主表差异，但显示的结果都是：左表的数据在左边，右表的数据在右边。
```
## 自然连接
>自然连接：nature join，自然连接其实就是自动匹配连接条件，系统以两表中同名字段作为匹配条件，如果两表有多个同名字段，那就都作为匹配条件。在这里，自然连接可以分为自然内连接和自然外连接。
1.自然内连接语法：左表 + natural + join + 右表;
实例：将表 student 与 class 进行自然内连接
```
mysql> SELECT * from student natural join class;
+----+-------+-------+------+------+------+------+
| id | grade | name  | age  | sex  | c_id | room |
+----+-------+-------+------+------+------+------+
|  1 |   3.1 | curry |   30 | 男   |    1 | A    |
+----+-------+-------+------+------+------+------+
1 row in set (0.00 sec)
实例：将表 student 与 class 进行内连接，连接条件为 id 和 grade
mysql> SELECT * from
    -> student inner join class
    -> on
    -> student.id = class.id and student.grade = class.grade;
+----+-------+------+-------+------+------+----+-------+------+
| id | name  | age  | grade | sex  | c_id | id | grade | room |
+----+-------+------+-------+------+------+----+-------+------+
|  1 | curry |   30 |   3.1 | 男   |    1 |  1 |   3.1 | A    |
+----+-------+------+-------+------+------+----+-------+------+
1 row in set (0.00 sec)
自然连接自动使用同名字段作为连接条件，而且在连接完成之后合并同名字段。
2.自然外连接语法：左表 + natural + left/right + join + 右表;
实例：将表 student 与 class 进行自然左外连接
mysql> SELECT * FROM
    -> student natural left join class;
+----+-------+--------+------+------+------+------+
| id | grade | name   | age  | sex  | c_id | room |
+----+-------+--------+------+------+------+------+
|  1 |   3.1 | curry  |   30 | 男   |    1 | A    |
|  2 |   3.4 | durant |   29 | 男   |    1 | NULL |
|  3 |   3.6 | Riuo   |   27 | 女   |    1 | NULL |
|  4 |   3.2 | harden |   29 | 男   |    1 | NULL |
+----+-------+--------+------+------+------+------+
4 rows in set (0.00 sec)
实例：将表 student 与 class 进行自然右外连接
mysql> SELECT * FROM
    -> student natural right join class;
+----+-------+------+-------+------+------+------+
| id | grade | room | name  | age  | sex  | c_id |
+----+-------+------+-------+------+------+------+
|  1 |   3.1 | A    | curry |   30 | 男   |    1 |
|  2 |   3.2 | B    | NULL  | NULL | NULL | NULL |
|  3 |   3.4 | C    | NULL  | NULL | NULL | NULL |
+----+-------+------+-------+------+------+------+
3 rows in set (0.00 sec)
可以用内连接和外连接来模拟自然连接，模拟的关键就在于使用同名字段作为连接条件及合并同名字段。
基本语法：左表 + inner/left/right + join + 右表 + using(字段名);其中，using内部的字段名就是作为连接条件的字段，也是需要合并的同名字段。
将表 student 与 class 进行自然左外连接/
mysql> SELECT * FROM student natural left join class;
+----+-------+--------+------+------+------+------+
| id | grade | name   | age  | sex  | c_id | room |
+----+-------+--------+------+------+------+------+
|  1 |   3.1 | curry  |   30 | 男   |    1 | A    |
|  2 |   3.4 | durant |   29 | 男   |    1 | NULL |
|  3 |   3.6 | Riuo   |   27 | 女   |    1 | NULL |
|  4 |   3.2 | harden |   29 | 男   |    1 | NULL |
+----+-------+--------+------+------+------+------+
4 rows in set (0.00 sec)
-- 用左外连接模拟自然左外连接
mysql> SELECT * FROM student left join class using(id, grade);
+----+-------+--------+------+------+------+------+
| id | grade | name   | age  | sex  | c_id | room |
+----+-------+--------+------+------+------+------+
|  1 |   3.1 | curry  |   30 | 男   |    1 | A    |
|  2 |   3.4 | durant |   29 | 男   |    1 | NULL |
|  3 |   3.6 | Riuo   |   27 | 女   |    1 | NULL |
|  4 |   3.2 | harden |   29 | 男   |    1 | NULL |
+----+-------+--------+------+------+------+------+
4 rows in set (0.06 sec)

作者：curry_coder
链接：https://www.jianshu.com/p/395fef684dca
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
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

## if函数
>功能：实现分支流
语法：
if(表达式1,表达式2,表达式3)
执行顺序：
执行表达式1，成立返回表达式2的值，不成立则返回表达式3的值
应用：任何地方
```
案例1：
if(num>10,'true','false')

案例2：
create FUNCTION test_if(age int) returns CHAR(16)
begin
IF age>60 AND age<=90 THEN RETURN '老奶奶';
ELSEIF age>30 AND age<=60 THEN RETURN '大姐姐';
ELSEIF age>18 AND age<=30 THEN RETURN '小姐姐';
ELSEIF age>12 AND age<=18 THEN RETURN '女孩子';
ELSEIF age>0 AND age<=12 THEN RETURN '小女生';
END IF;

END

drop FUNCTION test_if;
select test_if(6)
```

# --------------------------流程----------------------------------
## case
>情况1：一般用于等值判断
语法：
CASE 变量|表达式|字段
WHEN 要判断的值 THEN 返回的值1或语句1;
WHEN 要判断的值 THEN 返回的值2或语句2;
……
ELSE 要返回的值n或语句n;
END CASE

>情况2：多重if语句，区间判断
语法：
CASE 变量|表达式|字段
WHEN 要判断的条件1 THEN 返回的值1或语句1;
WHEN 要判断的条件2 THEN 返回的值2或语句2;
……
ELSE 要返回的值n或语句n;
END CASE

>特点1：
可以作为表达式，嵌套其他语句使用，放在begin end内外都可以
可以作为单独语句使用，只能放在begin end中
特点2：
如果when的值满足，则执行then后面的语句，并结束case
都不满足，则执行else中的值或语句
特点3：
else可以省略，若else省略，且所有when都不满足，则返回null

```
例2：

/*输入年龄提示年龄阶段*/
delimiter //
create PROCEDURE test_case(in age int)
BEGIN

CASE
WHEN age>60 AND age<=90 THEN select '老奶奶';
WHEN age>30 AND age<=60 THEN select '大姐姐';
WHEN age>18 AND age<=30 THEN select '小姐姐';
WHEN age>12 AND age<=18 THEN select '女孩子';
WHEN age>0 AND age<=12 THEN select '小女生';
END CASE;
END
//

call test_case(18)

 ```
# 循环
### while
```
  -- MySQL中的三中循环 while 、 loop 、repeat  求  1-n 的和


   -- 第一种 while 循环 
   -- 求 1-n 的和
   /*  while循环语法：
   while 条件 DO
               循环体;
   end while;
   */
    create procedure sum1(a int) 
    begin
        declare sum int default 0;  -- default 是指定该变量的默认值
        declare i int default 1;
    while i<=a DO -- 循环开始
        set sum=sum+i;
        set i=i+1;
    end while; -- 循环结束
    select sum;  -- 输出结果
    end;
    -- 执行存储过程
    call sum1(100);
    -- 删除存储过程
    drop procedure if exists sum1；
```
### loop
```
-- 第二种 loop 循环
  /*loop 循环语法：
  loop_name:loop
          if 条件 THEN -- 满足条件时离开循环
               leave loop_name;  -- 和 break 差不多都是结束训话
       end if；
  end loop;
  */
create procedure sums(a int)
begin
        declare sum int default 0;
        declare i int default 1;
        loop_name:loop -- 循环开始
            if i>a then 
                leave loop_name;  -- 判断条件成立则结束循环  好比java中的 boeak
            end if;
            set sum=sum+i;
            set i=i+1;
        end loop;  -- 循环结束
        select sum; -- 输出结果
end;
 -- 执行存储过程
call sums(100);
-- 删除存储过程
drop procedure if exists  sums;
```
### repeat
```
-- 第三种 repeat 循环
  /*repeat 循环语法
  repeat
      循环体
  until 条件 end repeat;
  */


  -- 实例；
  create procedure sum55(a int)
  begin
       declare sum int default 0;
      declare i int default 1;
       repeat -- 循环开始
           set sum=sum+i;
           set i=i+1;
       until i>a end repeat; -- 循环结束
       select sum; -- 输出结果
  end;

  -- 执行存储过程
     call sum55(100);
  -- 删除存储过程
  drop procedure if exists sum55;
```


 




























