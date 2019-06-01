# 数据库介绍

## 常见的数据库

### Oracle

运行稳定，可移植性高，性能超群；适用于大中型企业领域。

### DB2

速度快，可靠性好，适用于海量数据，恢复性强。适用于大中型企业领域。

### MySQL

开源，体积小，速度快，适用于中小型企业领域。

### SQL Server

全面，效率高，界面友好，操作容易，但是不跨平台。适用于中小型企业领域。

# 专业术语

### 表

具有固定列数和任意行数

### 列

Field字段

### 行

row

### 数据库

关联表的集合

### 主键

主键是唯一的，一个数据表中只能包含一个主键，你可以用主键来查询数据。

### 外键

用于关联多个表

### 索引

后续介绍。

# MySql数据库

## 简介

关系型数据库：所有数据都存在表里面以行列形式存储。（非关系型数据库：是以 key:value 键值对的形式进行存储。）

支持上千万条记录的大型数据库；

使用标准的SQL数据语言形式；

MySQL允许于多个系统，并且支持多用语言：C C++ Python Java Perl PHP Eiffel Quby Tcl

## 安装

请参考[CDSN](https://blog.csdn.net/skykingf/article/details/87090739).

# 常用数据库命令

# NaviCat可视化工具安装

https://www.waitsun.com/navicat-premium.html

# 笔记

## DQL

### 条件查询

```mysql
# 条件查询

# 性别为男，年龄为20
select * from student where stu_gender = '男' and stu_age = 20;

# id=1001 或 姓名为'ls'的
select * from student where id = 1001 or stu_name = 'ls';

# 查询学号为1001，1002，1003的记录
select * from student where id in(1001, 1002, 1003, 1004);

# 查询年龄为null的记录
select * from student where stu_age is null;

# 查询年龄在18到20之间的学生记录
select * from student where stu_age >= 18 and stu_age <= 20;
select * from student where stu_age between 18 and 20;

# 查询性别非男的学生记录
select * from student where stu_gender != '男';

# 查询姓名不为null的学生记录
select * from student where stu_age is not null;
```

### 模糊查询

```mysql
# 模糊查询

# 查询姓名由5个字母构成的学生记录
select * from student where stu_name like '__';

# 查询姓名由5个字母构成，并且第5个字母为“s”的学生记录
select * from student where stu_name like '____s';

# 查询姓名以“m”开头的学生记录
select * from student where stu_name like 'm%';

# 查询姓名中第2个字母为“u”的学生记录
select * from student where stu_name like '_u%';

# 查询姓名中包含“s”字母的学生记录
select * from student where stu_name like '%s%';
```



### 字段控制查询

```mysql
# 字段控制

# 去除重复记录
select distinct stu_name from student;

# 把查询字段的结果进行运算，必须都要是数据型
# 有null则返回null
select *, stu_age + stu_score from student;
# 去除null用ifnull(,)
select *, ifnull(stu_age, 0) + ifnull(stu_score, 0) from student;

# 对查询结果起别名as，as也可以省略
select *, ifnull(stu_age, 0) + ifnull(stu_score, 0) as total from student;
```



### 排序

```mysql
# 排序

# 对所有员工的薪水进行排序
SELECT * FROM employee ORDER BY salary DESC, id DESC;
```



### 聚合函数

```mysql
# COUNT()：统计指定列不为NULL的记录行数

# 查询employee表中记录数：
SELECT COUNT(*) FROM employee;
# 查询员工表中有绩效的人数：
SELECT COUNT(performance) FROM employee;
# 查询员工表中月薪大于2500的人数：
SELECT COUNT(*) FROM employee WHERE salary > 2500;
# 统计月薪与绩效之和大于5000元的人数：
SELECT COUNT(*) FROM employee
WHERE IFNULL(salary, 0) + IFNULL(performance, 0) > 5000;
# 查询有绩效的人数，和有管理费的人数：
SELECT COUNT(performance) as p_num, COUNT(manage) as m_num
FROM employee;

# sum()

# 查询所有雇员月薪和：
SELECT SUM(salary) FROM employee;
# 查询所有雇员月薪和，以及所有雇员绩效和
SELECT SUM(salary), SUM(performance) FROM employee;
# 查询所有雇员月薪+绩效和：
SELECT SUM(salary + IFNULL(performance, 0)) `total` FROM employee;
# 统计所有员工平均工资：
SELECT AVG(salary) as avg, MAX(salary) as mac, MIN(salary) as min
FROM employee;
```

### GROUP BY

```mysql
# GROUP BY, GOUP_CONCAT()

# 当group by单独使用时，只显示出每组的第一条记录；
# 这样不如使用dinstinct去重
SELECT * FROM employee GROUP BY gender;

# 查看每一组之内的内容，根据部门分组
SELECT department, GROUP_CONCAT(`name`)
FROM employee GROUP BY department;
# 根据性别分组
SELECT gender, GROUP_CONCAT(`name`)
FROM employee
GROUP BY gender;

# GROUP BY + aggregate function

SELECT gender, SUM(salary + IFNULL(performance,0)) as total
from employee
GROUP BY gender;
# 查询每个部门的部门名称和每个部门的工资和
SELECT department, SUM(salary)
FROM employee
GROUP BY department;
# 查询每个部门的部门名称以及每个部门的人数
SELECT department, COUNT(*)
FROM employee
GROUP BY department;
# 查询每个部门的部门名称以及每个部门工资大于1500的人数
SELECT department, COUNT(salary)
FROM employee
WHERE salary > 1500
GROUP BY department;

# GROUP BY + having
# 查询工资总和大于9000的部门名称以及工资和
# having后面可以使用分组函数(统计函数), where后面不可以使用分组函数
# WHERE是对分组前记录的条件，如果某行记录没有满足WHERE子句的条件，那么这行记录不会参加分组；而HAVING是对分组后数据的约束。
SELECT department, GROUP_CONCAT(salary), SUM(salary)
FROM employee
GROUP BY department
HAVING SUM(salary) >= 9000;
# 查询工资大于2000的，工资总和大于6000的部门名称以及工资和
SELECT department, SUM(salary)
FROM employee
WHERE salary > 2000
GROUP BY department
having SUM(salary) > 6000
ORDER BY SUM(salary) DESC;
```

### 书写顺序

```mysql
# 书写顺序
# SELECT FROM WHERE GROUP BY HAVING ORDER BY LIMIT
```

### 执行顺序

```mysql
# 执行顺序
# FROM WHERE GROUP BY HAVING SELECT ORDER BY LIMIT
```

### LIMIT

```mysql
# LIMIT
SELECT * FROM employee WHERE salary > 1000 LIMIT 3, 2;

# 分页思路
int curPage = 1;
int papeSize = 3;

SELECT * FROM employee LIMIT (curPage - 1) * pageSize, pageSize;

```



## 数据完整性

```mysql
# 实体完整性

# 主键

# 创建表时设置主键
CREATE TABLE `person`(
`id` BIGINT PRIMARY KEY,
`name` VARCHAR(50)
);

# 创建表时创建主键
CREATE TABLE `person2`(
`id` BIGINT,
`name` VARCHAR(50),
`age` int,
PRIMARY key(id));

# 创建表时创建联合主键
CREATE TABLE `person3`(
`id` bigint,
`snum` bigint,
`name` varchar(50)，
PRIMARY KEY(id, snum)
);

# 通过修改表来给表添加主键
ALTER TABLE person3 ADD CONSTRAINT PRIMARY KEY(id);

# 唯一约束
# 不能重复但是可以为null
CREATE TABLE `person`(
`id` BIGINT PRIMARY KEY,
`name` VARCHAR(50) UNIQUE
);

# 自动增长
CREATE TABLE `person`(
`id` BIGINT PRIMARY KEY auto_increment,
`name` VARCHAR(50) UNIQUE
);

# 域完整性
# 非空约束 NOT NULL
# 默认值 DEFAULT
CREATE TABLE person(
`id` BIGINT PRIMARY KEY auto_increment,
`name` VARCHAR(50) UNIQUE NOT NULL,
`gender` CHAR(1) DEFAULT '男'
);

# 参照完整性
# 两张表之间参照的关系
# 注意数据类型要一样
CREATE TABLE `student`(
`id` BIGINT PRIMARY KEY auto_increment,
`name` VARCHAR(50),
`age` INT
);

CREATE TABLE `score`(
`id` INT PRIMARY KEY auto_increment,
`score` INT,
`subject` VARCHAR(30),
`stu_id` BIGINT,
CONSTRAINT `sc_st_fk` FOREIGN KEY(`stu_id`) REFERENCES `student`(`id`)
);

ALTER TABLE `score` ADD CONSTRAINT `sc_st_fk`
FOREIGN KEY(`stu_id`) REFERENCES `student`(`id`);
```



## 表之间的关系

```mysql
# 一对多的关系
CREATE TABLE `person`(
`id` INT PRIMARY KEY,
`name` VARCHAR(30));

CREATE TABLE `car`(
`name` VARCHAR(20),
`color` VARCHAR(20),
`person_id` int,
CONSTRAINT `c_p_fk` FOREIGN KEY(`person_id`) REFERENCES `person` (`id`)
);

# 多对多的关系
# 创建老师表
CREATE TABLE `teacher` (
`tid` INT PRIMARY KEY auto_increment,
`name` VARCHAR(20),
`age` INT,
`gender` VARCHAR(1) DEFAULT '男'
);

# 创建学生表
CREATE TABLE `student` (
`sid` INT PRIMARY KEY auto_increment,	
`name` VARCHAR(20),
`gender` VARCHAR(1) DEFAULT '男');

# 添加学生老师关系表
CREATE TABLE `stu_tea_rel`(
`sid` INT,
`tid` INT);

# 添加外键
ALTER TABLE `stu_tea_rel`
ADD CONSTRAINT `fk_sid` FOREIGN KEY(`sid`) REFERENCES `student` (`sid`);

ALTER TABLE `stu_tea_rel`
ADD CONSTRAINT `fk_tid` FOREIGN KEY(`tid`) REFERENCES `teacher` (`tid`);
# 避免大量冗余数据的出现
```



## 多表查询

```mysql
# 多表查询

# 合并结果集
CREATE TABLE `A`(`name` VARCHAR(20), `score` int);
CREATE TABLE `B`(`name` VARCHAR(20), `score` int);

INSERT INTO `A` VALUES ('zs', 10), ('ls', 20), ('ww', 30);
INSERT INTO `B` VALUES ('zs', 10), ('ls', 40), ('ll', 90);
# 被合并的两个结果：列数、列类型必须相同;列名可以不同。
# UNION 去除重复项,重复指的是 所选择的字段 的值都一样
SELECT `name` FROM `A` UNION SELECT `name` FROM `B`;
SELECT * FROM `A` UNION SELECT * FROM `B`;
# UNION ALL 不去除重复项
SELECT `name` FROM `A` UNION ALL SELECT `name` FROM `B`;

# 多表联查

CREATE TABLE `student` (
`stu_id` INT PRIMARY KEY auto_increment,
`name` VARCHAR(30),
`gender` VARCHAR(1) DEFAULT '男');

CREATE TABLE `score` (
`scr_id` INT PRIMARY KEY auto_increment,
`subject` VARCHAR(30) NOT NULL,
`sco_val` INT,
`stu_id` INT,
CONSTRAINT `sco_stu_fk` FOREIGN KEY(`stu_id`) REFERENCES `student`(`stu_id`)
);

INSERT INTO `student` VALUES (1, 'zs', '男');
INSERT INTO `student` VALUES (2, 'ls', '女');
INSERT INTO `student` VALUES (3, 'ww', '女');
INSERT INTO `student` VALUES (4, 'zl', '男');
INSERT INTO `score` VALUES (1, '数学', 60, 1);
INSERT INTO `score` VALUES (2, '数学', 70, 2);
INSERT INTO `score` VALUES (3, '数学', 80, 3);
INSERT INTO `score` VALUES (4, '数学', 90, 4);
INSERT INTO `score` VALUES (5, '语文', 70, 4);
INSERT INTO `score` VALUES (6, '语文', 60, 3);
INSERT INTO `score` VALUES (7, '语文', 89, 2);
INSERT INTO `score` VALUES (8, '语文', 10, 1);

# 得到笛卡尔集
SELECT * FROM `student`, `score`;
# 多表联查
SELECT * FROM `student` st, `score` sc WHERE st.stu_id = sc.stu_id;
# 表美化，去除不需要的列
SELECT st.`name`, st.`gender`, sc.`subject`, sc.`sco_val` `value` 
FROM `student` `st`, `score` `sc` WHERE st.`stu_id` = sc.`stu_id`;

# 连接查询

# 内连接
SELECT st.`name`, st.`gender`, sc.`subject`, sc.`sco_val` FROM `student` st 
INNER JOIN `score` sc 
on st.stu_id = sc.stu_id
WHERE sc.sco_val > 60
AND st.gender = '男';
# 左连接
SELECT st.name, st.gender, sc.subject, sc.sco_val
FROM `student` st LEFT JOIN `score` sc
ON st.stu_id = sc.stu_id
WHERE st.gender = '男';
# 右连接
SELECT st.name, st.gender, sc.subject, sc.sco_val
FROM `student` st RIGHT JOIN `score` sc
ON st.stu_id = sc.stu_id;

# 内连接-多表连接
CREATE TABLE `course` (
`cou_id` INT PRIMARY KEY auto_increment,
`name` varchar(20));

# 通过99查询法
SELECT st.name, st.gender, co.name, sc.sco_val
FROM `student` st, `score` sc, `course` co
WHERE st.stu_id = sc.stu_id
AND sc.subject = co.cou_id;
# 通过内连接完成
SELECT st.name, st.gender, co.name, sc.sco_val FROM `student` st
INNER JOIN `score` sc ON st.stu_id = sc.stu_id
INNER JOIN `course` co ON sc.subject = co.cou_id;

# 内连接-非等值链接
# 以dept,emp,salgrade为例表
# 1.查询所有员工的姓名，工资
SELECT `ename`, `salary` FROM `emp`;
# 2.查询所有员工的姓名，工资和所有部门
SELECT em.ename, em.salary, de.dname
FROM `emp` em
INNER JOIN `dept` de
ON em.deptno = de.deptno
# 3.查询所有员工的姓名，工资和所在部门及工资等级
# 99法
SELECT em.ename, em.salary, de.dname, sa.grade
FROM `emp` em, `dept` de, `salgrade` sa
WHERE em.deptno = de.deptno
AND em.salary BETWEEN sa.lowSalary AND sa.highSalary;
# 链接法
SELECT em.ename, em.salary, de.dname, sa.grade
FROM `emp` em
INNER JOIN `dept` de ON em.deptno = de.deptno
INNER JOIN `salgrade` sa ON em.salary BETWEEN sa.lowSalary AND sa.highSalary;

# 自然连接
CREATE TABLE `stu` (`id` INT, `name` VARCHAR(20));
CREATE TABLE `score` (`sid` INT, `score` int);

SELECT * FROM `stu`, `score` WHERE stu.id = score.sid;

SELECT * FROM `stu` INNER JOIN `score` ON stu.id = score.sid;

# 以下是自然连接(列名和数据类型一致，多个字段名称相同则匹配时对应字段值也要相同）
ALTER TABLE `stu` CHANGE `id` `sid` INT;
SELECT * FROM `stu` NATURAL JOIN `score`;

# 多表查询并不一定是要有主外键，查询数据可以通过隐式查询（99查询）或多表查询的INNER/LEFET/RIGHT/NATURAL JOIN
# 来查询多表数据。

# 子查询

# 查询与项羽同一个部门人员工
SELECT ename, deptno
FROM emp
WHERE deptno = ( SELECT deptno FROM emp WHERE ename = '项羽');

# 查询工资高于程咬金的员工
SELECT ename, salary
FROM emp
WHERE salary >= (SELECT salary FROM emp WHERE ename = '程咬金');

# 工资高于30号部门所有人的员工信息
SELECT * FROM emp
WHERE salary > (SELECT max(salary) FROM emp WHERE deptno = 30);

# 查询工作和工资与妲己完全相同的员工信息
SELECT * FROM emp
WHERE (job, salary) IN (SELECT job, salary FROM emp WHERE ename = '妲己');

# 有2个以上直接下属的员工信息
SELECT ename FROM emp
WHERE empno IN (SELECT mgr FROM emp 
GROUP BY mgr HAVING COUNT(*) >= 2);

# 查询员工编号为7788的员工名称、员工工资、部门名称、部门地址
# 99写法
SELECT emp.ename, emp.salary, dept.dname, dept.local
FROM emp, dept
WHERE emp.deptno = dept.deptno
AND emp.empno = 7788;
# 多表查询
SELECT emp.ename, emp.salary, dept.dname, dept.local
FROM emp INNER JOIN dept ON emp.deptno = dept.deptno
WHERE emp.empno = 7788;
# select子查询写法
SELECT nt.ename, nt.salary, nt.dname, nt.local FROM 
(SELECT emp.empno, emp.ename, emp.salary, dept.dname, dept.local 
FROM emp INNER JOIN dept ON emp.deptno = dept.deptno) nt
WHERE nt.empno = 7788;

# 自连接
# 查询7369的姓名和经理姓名
SELECT e1.empno, e1.ename, e2.empno, e2.ename FROM emp e1, emp e2
WHERE e1.mgr = e2.empno
AND e1.empno = 7369;
```



## 存储过程

```mysql
# 创建存储过程
delimiter $$
CREATE PROCEDURE show_emp()
BEGIN
SELECT * FROM emp;
END$$
delimiter ;

# 删除存储过程；
DROP PROCEDURE show_emp();

# 调用存储过程
CALL show_emp();

# 显示所有的存储过程；
SHOW PROCEDURE STATUS;

# 显示当前数据库的存储过程；
SHOW PROCEDURE STATUS WHERE db = 'mydb';

# 查看所选存储过程的信息；
SHOW CREATE PROCEDURE show_emp;

# 在存储过程中声明一个变量
delimiter $$
CREATE PROCEDURE test();
BEGIN

-- 声明变量
DECLARE res VARCHAR(255) default '';
DECLARE x, y INT DEFAULT 0;

# 分配变量
-- 利用set赋值变量
SET x = 3;
SET y = 4;
-- 将查询的结果赋值给特定变量
DECLARE avgRes DOUBLE DEFAULT 0;
SELECT AVG(salary) INTO avgRes FROM emp;

END $$
delimiter ;

# 存储过程参数

# IN
delimiter $$
CREATE PROCEDURE get_name(IN name VARCHAR(255))
BEGIN
SELECT * FROM emp WHERE ename = name;
END$$
delimiter ;

CALL get_name('李白');

# OUT
delimiter $$
CREATE PROCEDURE get_salary(IN name VARCHAR(30), OUT res DOUBLE)
BEGIN

SELECT salary INTO res FROM emp WHERE ename = name;

END$$
delimiter ;

CALL get_salary('李白', @salary);
SELECT @salary;

# INOUT
delimiter $$
CREATE PROCEDURE get_sum(INOUT a INT, IN b INT)
BEGIN
SET a = a + b;
END$$
delimiter ;

SET @a = 20;
CALL get_sum(@a, 10);

SELECT @a;
```

### 创建一个插入随机字符的存储过程

```mysql
delimiter $$
CREATE PROCEDURE insert_employee(IN startNum INT, IN max_num INT)
BEGIN
-- 声明一个变量，记录当前是第几条数据
DECLARE i INT DEFAULT 0;
-- 默认情况是自动提交sql
SET autocommit = 0; -- 不让它自动提交sql
REPEAT
	SET i = i + 1;
	-- 插入数据
	INSERT INTO employee VALUES(startNum + i, rand_str(5), FLOOR(10 + rand() * 30));
UNTIL i = max_num END REPEAT;
COMMIT; -- 整体提交所有sql，提高效率
END$$
delimiter ;
```



## 自定义函数

```mysql
# 自定义函数
# 随机生成字符串

set global log_bin_trust_function_creators=1;

delimiter $$
CREATE FUNCTION rand_str(n INT) RETURNS VARCHAR(255)
BEGIN
DECLARE full_str VARCHAR(100) DEFAULT 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
DECLARE i INT DEFAULT 0;
DECLARE res_str VARCHAR(255) DEFAULT '';
WHILE i < n DO
SET res_str = CONCAT(res_str, SUBSTR(full_str, FLOOR(1 + RAND() * 52), 1));
SET i = i + 1;
END WHILE;
RETURN res_str;
END$$
delimiter ;

SELECT rand_str(10);
```

#### Mysql自定义函数报错（1418）解决方法原因：

出错信息：

`ERROR 1418 (HY000): This function has none of DETERMINISTIC, NO SQL, or READS SQL DATA in its declaration and binary logging is enabled (you *might* want to use the less safe log_bin_trust_function_creators variable)`

这是我们开启了bin-log, 我们就必须指定我们的函数是否是

1. `DETERMINISTIC` 不确定的
2. `NO SQL` 没有SQl语句，当然也不会修改数据
3. `READS SQL DATA` 只是读取数据，当然也不会修改数据
4. `MODIFIES SQL DATA` 要修改数据
5. `CONTAINS SQL` 包含了SQL语句

其中在function里面，只有 `DETERMINISTIC`, `NO SQL` 和 `READS SQL DATA` 被支持。如果我们开启了 `bin-log`, 我们就必须为我们的`function`指定一个参数。

在MySQL中创建函数时出现这种错误的解决方法：

```mysql
set global log_bin_trust_function_creators = TRUE;
set global log_bin_trust_function_creators = 1;
```
