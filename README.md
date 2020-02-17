# mysql
## 1.select用法
```
select * from table;
select 'name' from table;（name为关键字，需括起来）
select 100;
select 100*100;
select hello;
select version();(接方法)
```
### 执行 
```
选中`select hello;`
F9执行,F12格式化代码
```
### 起别名 
```
select 'name' as "姓名" from table;
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
### 
```
```
### 
```
```
### 
```
```
### 
```
```
### 
```
```
### 
```
```
### 
```
```
### 
```
```


