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




















