---
title: MySQL---基础01
date: 2020-06-04 16:28:33
author: ReadPond
tags: 
- MySQL
categories:
- 数据库
top: false
index_img: /img/202301181547411.png
banner_img: /img/202301181547411.png
---



### 一、MySQL数据库

#### 1、常见数据命令：

```mysql
select version(); #----查看数据库版本(或者：mysql -V)
show databases;   # -------查看当前所有数据库
use 库名  # ------打开指定库名
show tables from  库名; #-----------查看其他库的所有表
show          #----------查看当前数据库的所有表
# 创建表
Create table 表名(
		列名 列类型,
		列名 列类型,
		列名 列类型,
		……..
		列名 列类型
)
注意：结尾语句没有标点符号
#查看表结构
desc 表名;
# 创建数据库
create database 创建数据库名称; 

```

#### 2、简单查询语句

```mysql
/*
select 查询列表 from 表名;

 特点：
    1.查询列表可以是表中字段，常量值，表达式，函数
    2.查询的结果可以是一个虚拟的表格
*/

#1.查询表中单个字段
SELECT last_name FROM employees;

#2.查询表中多个字段
SELECT last_name,first_name,email,salary FROM employees;

#3.查询表中所有字段
SELECT * FROM employees;

#4.查询表中常量值
SELECT 600;

#5.查询表中函数
SELECT VERSION();

#6.查询表中表达式
SELECT 100 / 5;

#7.起别名
SELECT 100 % 98 AS 结果;
SELECT first_name AS 姓,last_name AS 名 FROM employees;

#8.去重。查询员工号表将多余的编号去掉
# 注意：disinct出现在两个字段之前表示两个字段联合起来去重
SELECT DISTINCT department_id,salary
FROM employees;

```

#### 3、常用函数

##### 3.1、**数学函数**

| 函数                | 作用                                                         |
| ------------------- | ------------------------------------------------------------ |
| ABS(x)              | 返回x的绝对值  SELECT ABS(-1) -- 返回1                       |
| CEIL(x),CEILING(x)  | 返回大于或等于x的最小整数  SELECT CEIL(1.5) -- 返回2         |
| FLOOR(x)            | 返回小于或等于x的最大整数  SELECT FLOOR(1.5) -- 返回1        |
| RAND()              | 返回0->1的随机数  SELECT  RAND() --0.93099315644334          |
| RAND(x)             | 返回0->1的随机数，x值相同时返回的随机数相同  SELECT  RAND(2) --1.5865798029924 |
| SIGN(x)             | 返回x的符号，x是负数、0、正数分别返回-1、0和1  SELECT SIGN(-10) -- (-1) |
| PI()                | 返回圆周率(3.141593）  SELECT PI() --3.141593                |
| TRUNCATE(x,y)       | 返回数值x保留到小数点后y位的值（与ROUND最大的区别是不会进行四舍五入）  SELECT TRUNCATE(1.23456,3) -- 1.234 |
| ROUND(x)            | 返回离x最近的整数SELECT ROUND(1.23456) --1                   |
| ROUND(x,y)          | 保留x小数点后y位的值，但截断时要进行四舍五入  SELECT  ROUND(1.23456,3) -- 1.235 |
| POW(x,y).POWER(x,y) | 返回x的y次方  SELECT POW(2,3) -- 8                           |
| SQRT(x)             | 返回x的平方根  SELECT SQRT(25) -- 5                          |
| EXP(x)              | 返回e的x次方  SELECT EXP(3) --  20.085536923188              |
| MOD(x,y)            | 返回x除以y以后的余数  SELECT MOD(5,2) -- 1                   |
| LOG(x)              | 返回自然对数(以e为底的对数)  SELECT LOG(20.085536923188) -- 3 |
| LOG10(x)            | 返回以10为底的对数  SELECT LOG10(100) -- 2                   |
| RADIANS(x)          | 将角度转换为弧度  SELECT RADIANS(180) --  3.1415926535898    |
| DEGREES(x)          | 将弧度转换为角度  SELECT  DEGREES(3.1415926535898) -- 180    |
| SIN(x)              | 求正弦值(参数是弧度)  SELECT SIN(RADIANS(30)) -- 0.5         |
| ASIN(x)             | 求反正弦值(参数是弧度)                                       |
| COS(x)              | 求余弦值(参数是弧度)                                         |
| ACOS(x)             | 求反余弦值(参数是弧度)                                       |
| TAN(x)              | 求正切值(参数是弧度)                                         |
| ATAN(x) ATAN2(x)    | 求反正切值(参数是弧度)                                       |
| COT(x)              | 求余切值(参数是弧度)                                         |

##### 3.2、**字符串函数**

| 函数                           | 说明                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| CHAR_LENGTH(s)                 | 返回字符串s的字符数  SELECT CHAR_LENGTH('你好123') -- 5      |
| LENGTH(s)                      | 返回字符串s的长度  SELECT LENGTH('你好123') -- 9             |
| CONCAT(s1,s2,...)              | 将字符串s1,s2等多个字符串合并为一个字符串  SELECT CONCAT('12','34') --  1234 |
| CONCAT_WS(x,s1,s2,...)         | 同CONCAT(s1,s2,...)函数，但是每个字符串直接要加上x  SELECT  CONCAT_WS('@','12','34') -- 12@34 |
| INSERT(s1,x,len,s2)            | 将字符串s2替换s1的x位置开始长度为len的字符串  SELECT  INSERT('12345',1,3,'abc') -- abc45 |
| UPPER(s),UCAASE(S)             | 将字符串s的所有字母变成大写字母  SELECT UPPER('abc') -- ABC  |
| LOWER(s),LCASE(s)              | 将字符串s的所有字母变成小写字母  SELECT LOWER('ABC') -- abc  |
| LEFT(s,n)                      | 返回字符串s的前n个字符  SELECT LEFT('abcde',2) -- ab         |
| RIGHT(s,n)                     | 返回字符串s的后n个字符  SELECT RIGHT('abcde',2) -- de        |
| LPAD(s1,len,s2)                | 字符串s2来填充s1的开始处，使字符串长度达到len  SELECT LPAD('abc',5,'xx') --  xxabc |
| RPAD(s1,len,s2)                | 字符串s2来填充s1的结尾处，使字符串的长度达到len  SELECT RPAD('abc',5,'xx') --  abcxx |
| LTRIM(s)                       | 去掉字符串s开始处的空格                                      |
| RTRIM(s)                       | 去掉字符串s结尾处的空格                                      |
| TRIM(s)                        | 去掉字符串s开始和结尾处的空格                                |
| TRIM(s1 FROM s)                | 去掉字符串s中开始处和结尾处的字符串s1  SELECT TRIM('@' FROM  '@@abc@@') -- abc |
| REPEAT(s,n)                    | 将字符串s重复n次  SELECT REPEAT('ab',3) --  ababab           |
| SPACE(n)                       | 返回n个空格                                                  |
| REPLACE(s,s1,s2)               | 将字符串s2替代字符串s中的字符串s1  SELECT  REPLACE('abc','a','x') --xbc |
| STRCMP(s1,s2)                  | 比较字符串s1和s2                                             |
| SUBSTRING(s,n,len)             | 获取从字符串s中的第n个位置开始长度为len的字符串              |
| MID(s,n,len)                   | 同SUBSTRING(s,n,len)                                         |
| LOCATE(s1,s),POSITION(s1 IN s) | 从字符串s中获取s1的开始位置  SELECT LOCATE('b', 'abc') -- 2  |
| INSTR(s,s1)                    | 从字符串s中获取s1的开始位置  SELECT INSTR('abc','b') -- 2    |
| REVERSE(s)                     | 将字符串s的顺序反过来  SELECT REVERSE('abc') -- cba          |
| ELT(n,s1,s2,...)               | 返回第n个字符串  SELECT ELT(2,'a','b','c') -- b              |
| EXPORT_SET(x,s1,s2)            | 返回一个字符串，在这里对于在“bits”中设定每一位，你得到一个“on”字符串，并且对于每个复位(reset)的位，你得到一个 “off”字符串。每个字符串用“separator”分隔(缺省“,”)，并且只有“bits”的“number_of_bits”  (缺省64)位被使用。  SELECT  EXPORT_SET(5,'Y','N',',',4) -- Y,N,Y,N |
| FIELD(s,s1,s2...)              | 返回第一个与字符串s匹配的字符串位置  SELECT FIELD('c','a','b','c') -- 3 |
| FIND_IN_SET(s1,s2)             | 返回在字符串s2中与s1匹配的字符串的位置                       |
| MAKE_SET(x,s1,s2)              | 返回一个集合 (包含由“,”  字符分隔的子串组成的一个 字符串)，由相应的位在bits集合中的的字符串组成。str1对应于位0，str2对 应位1，等等。  SELECT  MAKE_SET(1\|4,'a','b','c'); -- a,c |
| SUBSTRING_INDEX                | 返回从字符串str的第count个出现的分隔符delim之后的子串。  如果count是正数，返回第count个字符左边的字符串。  如果count是负数，返回第(count的绝对值(从右边数))个字符右边的字符串。  SELECT SUBSTRING_INDEX('a*b','*',1)  -- a  SELECT  SUBSTRING_INDEX('a*b','*',-1) -- b  SELECT  SUBSTRING_INDEX(SUBSTRING_INDEX('a*b*c*d*e','*',3),'*',-1) -- c |
| LOAD_FILE(file_name)           | 读入文件并且作为一个字符串返回文件内容。文件必须在服务器上，你必须指定到文件的完整路径名，而且你必须有file权 限。文件必须所有内容都是可读的并且小于max_allowed_packet。  如果文件不存在或由于上面原因之一不能被读出，函数返回NULL。 |

##### 3.3、**日期和时间函数**

| 函数                                                         |                             说明                             |
| ------------------------------------------------------------ | :----------------------------------------------------------: |
| CURDATE(),CURRENT_DATE()                                     |         返回当前日期  SELECT CURDATE()  ->2014-12-17         |
| CURTIME(),CURRENT_TIME                                       |          返回当前时间  SELECT CURTIME()  ->15:59:02          |
| NOW(),CURRENT_TIMESTAMP(),LOCALTIME(),  SYSDATE(),LOCALTIMESTAMP() |   返回当前日期和时间  SELECT NOW()  ->2014-12-17 15:59:02    |
| UNIX_TIMESTAMP()                                             | 以UNIX时间戳的形式返回当前时间  SELECT UNIX_TIMESTAMP()  ->1418803177 |
| UNIX_TIMESTAMP(d)                                            | 将时间d以UNIX时间戳的形式返回  SELECT UNIX_TIMESTAMP('2011-11-11  11:11:11')  ->1320981071 |
| FROM_UNIXTIME(d)                                             | 将UNIX时间戳的时间转换为普通格式的时间  SELECT FROM_UNIXTIME(1320981071)  ->2011-11-11 11:11:11 |
| UTC_DATE()                                                   |         返回UTC日期  SELECT UTC_DATE()  ->2014-12-17         |
| UTC_TIME()                                                   |    返回UTC时间  SELECT UTC_TIME()  ->08:01:45 (慢了8小时)    |
| MONTH(d)                                                     | 返回日期d中的月份值，1->12  SELECT MONTH('2011-11-11 11:11:11')  ->11 |
| MONTHNAME(d)                                                 | 返回日期当中的月份名称，如Janyary  SELECT MONTHNAME('2011-11-11  11:11:11')  ->November |
| DAYNAME(d)                                                   | 返回日期d是星期几，如Monday,Tuesday  SELECT DAYNAME('2011-11-11  11:11:11')  ->Friday |
| DAYOFWEEK(d)                                                 | 日期d今天是星期几，1星期日，2星期一  SELECT DAYOFWEEK('2011-11-11  11:11:11')  ->6 |
| WEEKDAY(d)                                                   |        日期d今天是星期几，   0表示星期一，1表示星期二        |
| WEEK(d)，WEEKOFYEAR(d)                                       | 计算日期d是本年的第几个星期，范围是0->53  SELECT WEEK('2011-11-11 11:11:11')  ->45 |
| DAYOFYEAR(d)                                                 | 计算日期d是本年的第几天  SELECT DAYOFYEAR('2011-11-11  11:11:11')  ->315 |
| DAYOFMONTH(d)                                                | 计算日期d是本月的第几天  SELECT DAYOFMONTH('2011-11-11  11:11:11')  ->11 |
| QUARTER(d)                                                   | 返回日期d是第几季节，返回1->4  SELECT QUARTER('2011-11-11  11:11:11')  ->4 |
| HOUR(t)                                                      |          返回t中的小时值  SELECT HOUR('1:2:3')  ->1          |
| MINUTE(t)                                                    |         返回t中的分钟值  SELECT MINUTE('1:2:3')  ->2         |
| SECOND(t)                                                    |         返回t中的秒钟值  SELECT SECOND('1:2:3')  ->3         |
| EXTRACT(type FROM d)                                         | 从日期d中获取指定的值，type指定返回的值  SELECT EXTRACT(MINUTE FROM  '2011-11-11 11:11:11')   ->11  type可取值为：  MICROSECOND  SECOND  MINUTE  HOUR  DAY  WEEK  MONTH  QUARTER  YEAR  SECOND_MICROSECOND  MINUTE_MICROSECOND  MINUTE_SECOND  HOUR_MICROSECOND  HOUR_SECOND  HOUR_MINUTE  DAY_MICROSECOND  DAY_SECOND  DAY_MINUTE  DAY_HOUR  YEAR_MONTH |
| TIME_TO_SEC(t)                                               |    将时间t转换为秒  SELECT TIME_TO_SEC('1:12:00')  ->4320    |
| SEC_TO_TIME(s)                                               | 将以秒为单位的时间s转换为时分秒的格式  SELECT SEC_TO_TIME(4320)  ->01:12:00 |
| TO_DAYS(d)                                                   | 计算日期d距离0000年1月1日的天数  SELECT TO_DAYS('0001-01-01  01:01:01')  ->366 |
| FROM_DAYS(n)                                                 | 计算从0000年1月1日开始n天后的日期  SELECT FROM_DAYS(1111)  ->0003-01-16 |
| DATEDIFF(d1,d2)  Timestampdiff()根据指定值返回               | 计算日期d1->d2之间相隔的天数  SELECT  DATEDIFF('2001-01-01','2001-02-02')  ->-32 |
| ADDDATE(d,n)                                                 |                  计算其实日期d加上n天的日期                  |
| ADDDATE(d，INTERVAL expr type)                               | 计算起始日期d加上一个时间段后的日期  SELECT ADDDATE('2011-11-11  11:11:11',1)  ->2011-11-12 11:11:11 (默认是天)  SELECT ADDDATE('2011-11-11  11:11:11', INTERVAL 5 MINUTE)  ->2011-11-11 11:16:11  (TYPE的取值与上面那个列出来的函数类似) |
| DATE_ADD(d,INTERVAL expr type)                               |                             同上                             |
| SUBDATE(d,n)                                                 | 日期d减去n天后的日期  SELECT SUBDATE('2011-11-11  11:11:11', 1)  ->2011-11-10 11:11:11 (默认是天) |
| SUBDATE(d,INTERVAL expr type)                                | 日期d减去一个时间段后的日期  SELECT SUBDATE('2011-11-11  11:11:11', INTERVAL 5 MINUTE)  ->2011-11-11 11:06:11  (TYPE的取值与上面那个列出来的函数类似) |
| ADDTIME(t,n)                                                 | 时间t加上n秒的时间  SELECT ADDTIME('2011-11-11  11:11:11', 5)  ->2011-11-11 11:11:16 (秒) |
| SUBTIME(t,n)                                                 | 时间t减去n秒的时间  SELECT SUBTIME('2011-11-11  11:11:11', 5)  ->2011-11-11 11:11:06 (秒) |
| DATE_FORMAT(d,f)                                             | 按表达式f的要求显示日期d  SELECT DATE_FORMAT('2011-11-11  11:11:11','%Y-%m-%d %r')  ->2011-11-11 11:11:11 AM |
| TIME_FORMAT(t,f)                                             | 按表达式f的要求显示时间t  SELECT TIME_FORMAT('11:11:11','%r')  11:11:11 AM |
| GET_FORMAT(type,s)                                           | 获得国家地区时间格式函数  select get_format(date,'usa')  ->%m.%d.%Y (注意返回的就是这个奇怪  的字符串(format字符串)) |
| LAST_DAY(date)                                               |                    返回当前日期的最后一天                    |
