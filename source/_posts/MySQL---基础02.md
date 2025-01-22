---
title: MySQL---基础02
date: 2020-06-08 09:01:33
author: ReadPond
tags: 
- MySQL
categories:
- 数据库
top: false
index_img: /img/202301181446480.png
banner_img: /img/202301181446480.png
---

### MySQL数据库

#### 1、简单修改

>   语法：
>
>   update 表名
>
>   set 列 = 新值,列 = 值,…
>
>   where 筛选条件;

```mysql
#将姓名为周的学生电话换成“166666666”
UPDATE beauty
SET phone = '1666666666'
WHERE NAME LIKE '周%';
```

>   修改多表记录
>
>   语法：
>
>   SQL99语法
>
>   update 表1 别名
>
>   inner/left/right join 表2 别名
>
>   on 连接条件
>
>   set 列 = 新值,列 = 值,…
>
>   where 筛选条件;
>
>   SQL92语法
>
>   update 表1 别名,表2 别名
>
>   set 列 = 新值,列 = 值,…
>
>   where 连接条件
>
>   and 筛选条件;

```mysql
#修改张无忌的女朋友的手机号为114
UPDATE boys b
INNER JOIN beauty be
ON b.`id` = be.`boyfriend_id`
SET phone = '114'
WHERE b.`boyName` = '张无忌';
```

#### 2、删除

>   语法
>
>   方式一：
>
>    1.单表的删除
>
>    	delete from 表名 where 筛选条件
>
>    2.多表的删除
>
>    	SQL92
>
>   ​		delete 别名(删除表1写表1别名，删除表2写表2别名，删除两个表就写两个表的别名)
>
>   ​		from 表1 别名,表2 别名
>
>   ​		where 连接条件
>
>   ​		and 筛选条件;
>
>   ​	SQL99语法
>
>   ​		delete 别名(删除表1写表1别名，删除表2写表2别名，删除两个表就写两个表的别名)
>
>   ​		from 表1 别名
>
>   ​		inner/left/right join 表2 别名 
>
>   ​		on 连接条件
>
>   ​		where 筛选条件;
>
>    方式二：
>
>     	truncate table 表名; 

-   注意：

    -   delete与truncate 的区别：

        -   1.delete可以加where条件，truncate不能加

        -   2.truncate删除效率较高

        -   3.假如要删除表中有自增长列，如果用delete删除后，再插入数据，自增长列的值从断点开始，而truncate删除后,再插入数据，自增长列的值从1开始 

-   4.truncate删除没有返回值，delete删除有返回值
-   5.truncate删除不能回滚，delete删除可以回滚

#### 3、模糊查询

>   like：
>
>   ​	与通配符进行使用
>
>   ​	"% ":任意多个字符
>
>   ​	"_"(下划线):任意单个字符
>
>    
>
>   between…..and….：
>
>   ​	可以提高语句简洁度
>
>   ​	包含临界值等价于大于等于/小于等于
>
>   ​	两个临界值不能颠倒
>
>   in ：
>
>   ​	含义：判断某字段的值是否属于in列表中的某一项
>
>   ​	特点：
>
>   ​	使用in提高语句简洁度
>
>   ​	in列表的值类型必须统一或兼容
>
>   ​	is null / is not null
>
>   ​	=/<>不能用于判断null值

```mysql
#查询员工名中带有字符a的员工信息
SELECT *
FROM
	employees
WHERE
	last_name LIKE '%a%';

#查询员工名中带有字符_的员工信息
SELECT *
FROM 
	employees
WHERE 
	last_name LIKE '_\_%';

#查询员工编号在100到120之间的员工信息
SELECT *
FROM employees
WHERE department_id BETWEEN 100 AND 200;

#查询员工工种编号AC_ACCOUNT AC_MGR 'AD_VP','FI_ACCOUNT'中的一个员工名和工种编号
SELECT 
	last_name,job_id
FROM 
	employees
WHERE
	job_id IN ('AC_ACCOUNT','AC_MGR','AD_VP','FI_ACCOUNT');
	
#查询没有奖金/有奖金的员工名和奖金率
SELECT 
	last_name,commission_pct
FROM
	employees
WHERE 
	#commission_pct is not null; 没有奖金
	#commission_pct <=> null; ----<=>安全等于
	`commission_pct`IS NULL;  #有奖金

```

#### 4、排序查询

>   语法：
>
>   select 查询列表
>
>   from 表
>
>   [where 筛选条件]
>
>   order by 排序列表 [asc | desc]
>
>   特点：
>
>   ​	1.asc：默认按升序排列可以不写
>
>   ​	2.desc：按降序排列
>
>   ​	3.order by 子句中可以支持单个字段，多个字段，表达式，函数，别名
>
>   ​	4.order by 子句一般是放在查询语句的最后面，但是limit子句除外

```mysql
#查询员工信息，按工资从高到底顺序/从低到高顺序查询
SELECT * FROM employees ORDER BY salary DESC;
SELECT * FROM employees ORDER BY salary ;

#查询部门编号>=90的员工信息，按入职时间先后进行排序
SELECT *
FROM employees
WHERE 
	`department_id` >= 90 
ORDER  BY hiredate;

#按年薪的高低显示员工的信息和年薪（按表达式排序）
SELECT * , salary * 12 * (1 + IFNULL(commission_pct,0)) AS 年薪
FROM employees
ORDER BY salary * 12 * (1 + IFNULL(commission_pct,0));

#按年薪的高低显示员工的信息和年薪（按别名排序）
SELECT * , salary * 12 * (1 + IFNULL(commission_pct,0)) AS 年薪
FROM employees
ORDER BY 年薪;

#按last_name中的字符长度排序（按函数排序）
SELECT  last_name, LENGTH(last_name) AS 名字长度
FROM employees
ORDER BY LENGTH(last_name)  DESC;

#查询员工信息，工资升序，员工编号降序(按多个字段排序)
SELECT * 
FROM employees
ORDER BY salary ASC,manager_id DESC;

#查询员工的姓名，部门和年薪，按年薪降序，姓名升序排序
SELECT last_name,job_id,12 * salary *(1 + IFNULL (commission_pct,0)) AS 年薪
FROM employees
ORDER BY 年薪 DESC,last_name ASC;

#查询工资不在8000-17000的员工的姓名和工资，按工资降序排序
SELECT	last_name,salary
FROM employees
WHERE 
	salary < 8000 OR salary > 17000
ORDER BY salary DESC;

#查询邮箱中包含e的员工信息，并按邮箱字节数降序再按部门号升序
SELECT * , LENGTH(email) AS 邮箱字节数
FROM employees
WHERE 
	email LIKE('%e%')
ORDER BY 邮箱字节数 DESC , manager_id;

```

#### 5、练习

```mysql
/*
1.创建表dept1
Name Null? Type
id          int(7)
name        varchar(25)

2.将表departments中的数据插入新表dept2中

3.创建表emp5
Name        Null?   Type
id                  int(7)
First_name          varchar(25)
Last_name           varchar(25)

4.将列Last_name的长度增加到50

5.根据表employees创建表employees2

6.删除表emp5

7.将表employees2重命名为emp5

8.在表dept和emp5中添加新列test_column

9.直接删除表emp5中的列dept_id
*/

#1.创建表
USE myemployees;
CREATE TABLE detp1(
    id INT(7),
    NAME VARCHAR(25)
);

#2
USE myemployees;
CREATE TABLE dept2
SELECT *
FROM departments;

#3.创建表emp5
USE myemployees;
CREATE TABLE emp5(
    id INT(7),
    First_name VARCHAR(25),
    Last_name VARCHAR(25)
);

#4.
ALTER TABLE emp5 MODIFY COLUMN Last_name VARCHAR(50);

#5.
USE myemployees;
CREATE TABLE employees2 LIKE employees;

#6.
DROP TABLE emp5;
 
#7.
ALTER TABLE employees2
RENAME TO emp5;

#8.
ALTER TABLE emp5
ADD COLUMN text_column VARCHAR(20);

#9.
ALTER TABLE emp5
DROP COLUMN dept_id;

#查询部门编号>90或邮箱中包含a的员工信息
SELECT * 
FROM employees 
WHERE department_id > 90
UNION
SELECT * 
FROM employees 
WHERE email LIKE '%a%';

/*
一 插入语句
语法：
	INSERT INTO 表名(列名,列名…...)
	VALUES(值1，…..)
*/
SELECT *
FROM beauty;
#1.插入值的类型要与列的类型一致或兼容
INSERT INTO beauty(id,NAME,sex,borndate,phone,photo,boyfriend_id)
VALUES(14,'汤姆','男','1999-5-6','18649662222',NULL,6);

#第二种插入
INSERT INTO beauty
SET id = 15,NAME = '爱因斯坦',sex = '男',phone = '8794565';

/*
修改单表记录
语法：
	update 表名
	set 列 = 新值,列 = 值,…
where 筛选条件;
*/

#将姓名为周的学生电话换成“166666666”
UPDATE beauty
SET phone = '1666666666'
WHERE NAME LIKE '周%';

/*
修改多表记录
语法：
SQL99语法
	update 表1 别名
	inner/left/right join 表2 别名
	on 连接条件
	set 列 = 新值,列 = 值,…
	where 筛选条件;
	
SQL92语法
	update 表1 别名,表2 别名
	set 列 = 新值,列 = 值,…
	where 连接条件
and 筛选条件;
*/
#修改张无忌的女朋友的手机号为114
UPDATE boys b
INNER JOIN beauty be
ON b.`id` = be.`boyfriend_id`
SET phone = '114'
WHERE b.`boyName` = '张无忌';

/*
删除语句
语法
  方式一：
    1.单表的删除
    delete from 表名 where 筛选条件
    2.多表的删除
    SQL92
	delete  别名(删除表1写表1别名，删除表2写表2别名，删除两个表就写两个表的别名)
	from 表1 别名,表2 别名
	where 连接条件
	and 筛选条件;
	
    SQL99语法
	delete  别名(删除表1写表1别名，删除表2写表2别名，删除两个表就写两个表的别名)
	from 表1 别名
	inner/left/right join 表2 别名 
	on 连接条件
	where 筛选条件;
	
  方式二：
    truncate table 表名;    
*/

#单表删除
#删除手机号结尾为9的信息
DELETE FROM beauty 
WHERE phone LIKE '%9';

#多表删除
#删除有关张无忌的信息
DELETE b
FROM beauty b
INNER JOIN boys bo
ON b.`boyfriend_id` = bo.`id`
WHERE bo.`boyName` = '张无忌';

#案例一：查询谁的工资比Abel高？
/*
1.查询Abel的工资
2.查询员工的信息，筛选salary > 1的结果
*/
SELECT salary
FROM employees
WHERE last_name = 'Abel';

SELECT *
FROM employees
WHERE salary > (
	SELECT salary
	FROM employees
	WHERE last_name = 'Abel'
);

#案例2：返回job_id与141号员工相同并且salary比143号员工多的员工姓名，job_id,salary
/*
1.查询141号员工的job_id
2.查询工资143号的salary
3.查询员工的姓名，job_id和工资，要求job_id=1并且salary>2
*/
#1.查询141号员工的job_id
SELECT job_id
FROM employees
WHERE employee_id = 141;

#2.查询工资143号的salary
SELECT salary
FROM employees
WHERE employee_id = 143; 

#3.查询员工的姓名，job_id和工资，要求job_id=1并且salary>2
SELECT last_name,job_id,salary
FROM employees
WHERE job_id = (
	SELECT job_id
	FROM employees
	WHERE employee_id = 141
)
AND  salary >(
	SELECT salary
	FROM employees
	WHERE employee_id = 143
);

#案例3：返回公司工资最少的员工的last_name,job_id,salary
/*
1.查询公司的最低工资
2.查询last_name,job_id,salary 要求salary=1
*/
SELECT MIN(salary)
FROM employees;

SELECT last_name,job_id,salary
FROM employees
WHERE salary = (
	SELECT MIN(salary)
	FROM employees
);

#案例4：查询最低工资大于50号部门最低工资的部门id和其最低工资
/*
1.查询50号部门的最低工资
2.查询每个部门的最低工资
3.再2的基础上筛选2 满足最低工资 > 1的结果 
*/
SELECT MIN(salary)
FROM employees
WHERE department_id = 50;

SELECT MIN(salary),department_id
FROM employees
GROUP BY department_id;

SELECT MIN(salary),department_id
FROM employees
GROUP BY department_id
HAVING MIN(salary) > (
	SELECT MIN(salary)
	FROM employees
	WHERE department_id = 50
);

#列子查询
#返回location_id是1400或1700的部门中的所有员工姓名
/*
1.查询location_id是1400或1700的部门编号
2.查询员工姓名，满足部门号是1列表中的某一个
*/
SELECT DISTINCT department_id
FROM departments
WHERE location_id IN(1400,1700);

SELECT last_name
FROM employees
WHERE department_id IN(
	SELECT DISTINCT department_id
	FROM departments
	WHERE location_id IN(1400,1700)
);

#返回其它工种中比job_id为‘IT_PROG’工种任一工资低的员工的员工号、姓名、job_id 以及salary
/*
1.查询job_id为‘IT_PROG’部门的工资
2.查询员工号、姓名、job_id 以及salary， salary＜１
*/
SELECT DISTINCT salary
FROM employees
WHERE job_id = 'IT_PROG';

SELECT last_name,employee_id,job_id,salary
FROM employees
WHERE salary < ANY(
	SELECT DISTINCT salary
	FROM employees
	WHERE job_id = 'IT_PROG'
)AND job_id <> 'IT_PROG';

```

#### 6、分页查询

```mysql
/**
应用场景：
	当前显示数据一页显示不全，需要分页提交sql请求
语法：
	select 查询列表
	[连接类型join 表2]
	limit offset,size
	offset表示要显示条目的起始索引(从0开始)
	size要显示的条目个数
	
特点：
	limit语句放在查询语句的最后
	公式：要显示的页数page 每页的条目数size
	select 查询列表
	from 表
	limit   (page - 1) * size ,size
*/	
#查询前五条员工信息
SELECT *
FROM employees
LIMIT 0,5;

#查询第11条到第25条员工信息
SELECT *
FROM employees
LIMIT 10,15

```

#### 7、变量

##### 7.1、系统变量

-   变量由系统定义，不是用户定义，属于服务器层面
-   全局变量：针对整个服务器

-   语法：
    -   show **global** variables
    -   会话变量：针对客户端的一次连接

-   语法：
    -   show **session** variables
-   查看满足条件的部分系统变量
    -   语法：
    -   show global variables like '%char%';
-   查看指定的某个系统变量的值
    -   语法：
    -   select @@系统变量名
    -   select @@**global/session**.系统变量名--查看指定的全局/会话的某个系统变量的值
-   为某个系统变量赋值
    -   set **global/session** 系统变量名 = 值
    -   set @@ **global/session.**系统变量名 = 值

##### 7.2、自定义变量

-   用户变量
-   作用域：针对当前会话(连接)有效，同于会话变量的作用域

-   1）声明并初始化
    -   set @用户变量名 = 值;
    -   set @用户变量名：= 值;
    -   select @用户变量名: = 值;
-   2）赋值(更新用户变量的值)
    -   方式一：
        -   set @用户变量名 = 值;
        -   set @用户变量名：= 值;
        -   select @用户变量名: = 值;
    -   方式二：
        -   select 字段 into 变量名
        -   from 表;
-   3)使用
    -   select @用户变量名; 

-   **局部变量**
    -   作用域：仅仅在定义的begin end 中有效
    -   应用：在begin end 中的第一句话
-   1）声明
    -   declare 变量名 **类型**；
    -   declare 变量名 **类型** default 值;
-   2）赋值
    -   方式一：
        -   set 局部变量名 = 值；
        -   set 局部变量名：= 值;
        -   select @局部变量名: = 值;
-   方式二：
    -   select 字段 into 局部变量名
    -   from 表;
-   3）使用：
    -   select 局部变量名;

#### 8、存储过程

##### 8.1、存储过程

-   含义：一组预先编译好的SQL语句的集合，理解成皮处理语句

    -   1.提高代码重用性

    -   2.简化操作

    -   3.减少编译次数并且减少了和数据库服务器的连接次数，提高了效率

-   二，创建语法
    -   create procedure 存储过程名(参数列表)
    -   begin 
    -   存储过程体(一组合法的SQL语句)
    -   end 

-   注意：
    -   1.参数列表包含三部分：参数模式，参数名，参数类型
        -   eg: in stuname varchar（20）
    -   2.参数类型：
        -   in:该参数可以作为输入，也就是说需要调用方传入值
        -   out：该参数可以作为输出，可以作为返回值
        -   inout：该参数既可以输入也可以作为输出，既需要传入值又需要返回值
    -   3.如果存储过程体仅仅只有一句话，begin end 可以省略
    -   4.存储过程体中每条SQL语句结尾必须加分号，存储过程的结尾可以使用DELIMITER 重新设置
    -   语法：DELIMITER 结束标记
-   三，调用语法
-   call 存储过程名(实参列表); 

```mysql
DELIMITER $
CREATE PROCEDURE myp3()
BEGIN 
  INSERT INTO xs_kc(学号,课程号,成绩,学分)
  VALUES('18010221','103','95',4),
​       ('18010222','113','100',4),
​       ('18010223','134','55',1),
​       ('18010224','145','64',2),
​       ('18010225','161','73',3),
​       ('18010226','111','86',3),
​       ('18010227','161','91',4);
END $

#调用存储过程
CALL myp3()$
```

#### 9、视图

>   定义：
>
>   一种虚拟存在的表，行和列的数据来自定义视图的查询中使用的表，并且是在使用视图时动态生成的，只保存了SQL逻辑，不保存查询结果
>
>   应用场景：
>
>   1）多个地方用到相同的查询结果
>
>   2）该查询结果使用SQL语句较复杂
>
>   一，创建视图：
>
>   语法：
>
>     create view 视图名
>
>     as
>
>     查询语句;

```mysql
	#查询姓名中包含a字符的员工名，部门名和工种信息
	#1.创建视图
	CREATE VIEW my_v1
	AS
	SELECT last_name,department_name,job_title
	FROM employees e
	JOIN departments d ON e.department_id = d.department_id
	JOIN jobs j ON j.job_id = e.job_id;
	
	#2.使用视图
	SELECT *
	FROM my_v1
	WHERE last_name LIKE '%a%';
	
	#查询各部门的平均工资级别
	CREATE VIEW my_v2
	AS
	SELECT AVG(salary) 平均工资,department_id
	FROM employees
	GROUP BY department_id;
	
	SELECT my_v2.`平均工资`,g.grade_level
	FROM my_v2
	JOIN job_grades g
	ON my_v2.`平均工资` BETWEEN g.lowest_sal AND g.highest_sal;
	
	#查询平均工资最低的部门信息
	SELECT *
	FROM my_v2
	ORDER BY 平均工资 LIMIT 1;
	
	#查询平均工资最低的部门名和工资
	CREATE VIEW my_v3
	AS
	SELECT *
	FROM my_v2
	ORDER BY 平均工资 LIMIT 1;
	
	SELECT d.*,m.平均工资
	FROM my_v3 m
	JOIN departments d
	ON m.`department_id` = d.`department_id`;

/**
视图的优点：
	重用SQL语句
	简化复杂的SQL操作，不必知道它的查询细节
	保护数据，提高安全性
*/
#二，修改视图
/*
方式一：
    create or replace view 视图名
    as 
    查询语句;
    
方式二：
    alter view 视图名
    as
    查询语句;
*/

#三，删除视图
/*
drop view 视图名,视图名,....;
*/

#四，查看视图(建议在命令行当中使用)
/*
方式一：
    desc 视图名;
方式二：
    show create view 视图名;
*/

```

#### 10、TCL:事务控制语言

-   事务：

    -   一个或一组sql语句组成一个执行单元，这个执行单元要么全部执行要么全部不执行。如果单元中的某条SQL语句执行失败或产生错误，整个单元将会回滚，所有受到影响的数据将返回到事务开始以前的状态；如果单元中的所有SQL语句均执行成功，则事务被顺利执行。

-   存储引擎：

    -   1.在MySQL中的数据用各种不同的技术存储在文件或内存中
    -   2.通过show engines 来查看mysql支持的存储引擎
    -   3.在MySQL中用到最多的存储引擎有：innodb,myisam,memory等，其中innodb支持事务，而myisam,memory等不支持事务

-   事务的ACID属性：

    -   1.原子性(Atomicity)
        -   是指事务是一个不可分割的工作单位，事务中的操作要么都发生要么都不发生
    -   2.一致性(Consistency)
        -   事务必须是数据库从一个一致性的状态变换到另一个一致性的状态
    -   3.隔离性(Isolation)
        -   是指一个事务的执行不能被其他事务干扰。即一个事务内部的操作及使用的数据对并发的其他事务是隔离的，并发执行的各个事务之间不能相互干扰。
    -   4.持久性(Durability)
        -   指一个事务一旦被提交，它对数据库中数据的改变就是永久性的，接下来的其他操作和数据库故障不应该对其有任何影响
    -   事务的创建：
        -   隐式事务：事务没有明显的开启和结束的标记。如：insert，update语句。
        -   显示事务：事务具有明显的开启和结束标记，前提必须先设置自动提交功能为禁用(set autocommit = 0)

-   步骤：

-   1.开启事务

-   ```mysql
    set autocommit = 0;
    start transaction;（可选的）
    ```

-   2.编写事务中的SQL语句(select insert update delete)

    -   语句1;
    -   语句2;
    -   语句3;
    -   ...

-   3.结束事务

    -   commit;提交事务
    -   rollback;回滚事务
    -   delete和truncate在事务使用时的区别
    -   delete支持回滚，truncate不支持回滚

#### 11、错误解决方案

MySQL错误：Can't create table‘..’ （errno:150）解决方案

-   (1)、检查sc表的外键字段的类型以及大小是否和s表c表完全一致
-   (2)、试图引用的其中一个外键没有建立起索引，或者不是一个primary key , 如果其中一个不是primary key 的放，你必须为它创建一个索引。
-   (3)、一个或两个表是MyISAM引擎的表，若想要使用外键约束，必须是InnoDB引擎
