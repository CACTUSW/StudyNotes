# 初识MySQL

**MySQL不区分大小写**

## 连接数据库

```sql
mysql -u root -p --连接数据库
flush privileges;	--刷新权限

------------------------------
--所有的语句用;结尾
show databases;	--查看所有数据库
use school;	--切换到school数据库

show tables;	--查看数据库中的所有表
describe（desc） student;	--删除数据库中所有的表的信息

create database xxx;	--创建数据库

exit;	--退出
--	单行注释
/*
这里是多行注释
*/

```



# 操作数据库

## 操作数据库

==MySQL数据库不区分大小写==

> 创建数据库

```sql
create databases [if not exists] xxx;
```

> 删除数据库

```sql
drop databases [if not exists] xxx;
```

> 使用数据库

```sql
use `xxx`;
--如果表名或者字段名是一个特殊字符，就需要带``
```

> 查看数据库

```sql
show databases;
```

## 数据库的数据类型

> 数值

+ tinyint	十分小的数据	1个字节
+ smallint  较小的数据      2个字节
+ mediumint  中等大小的数据  3个字节
+ int  标准的整数  4个字节
+ bigint  较大的数据  8个字节
+ float 浮点数  4个字节
+ double 浮点数  8个字节
+ decimal  字符串形式的浮点数  金融计算一般用

> 字符串

+ char  字符串固定大小的  0~255
+ varchar  可变字符串  0~65535  常用的  String
+ tinytext  微型文本  2\^8-1
+ text  文本串  2\^16-1  保存大文本

> 时间和日期

java.util.Date



+ date  yyyy-MM-DD,日期
+ time  HH:MM:SS
+ datetime  YYYY-MM-DD HH:MM:SS
+ timestamp  时间戳，1970.1.1到现在的毫秒数
+ year 年份表示

> null

+ 没有值，未知
+ ==不要使用null进行运算，结果为NULL==

## 数据库的字段属性

==Unsigned：==

+ 无符号的整数
+ 声明了该列不能声明为负数

==zerofill：==

+ 0填充
+ 不足的位数，使用0来填充

==自增：==

+ 通常理解为自增，自动在上一条记录的基础上+1（默认）
+ 通常用来设计唯一的主键~index，必须是整数类型
+ 可以自定义设计主键自增的起始值和步长

==非空：== NULL not null

+ 假设设置为 not null，如果不给他赋值，就会报错
+ NULL，如果不填写值，默认就是null！

==默认：==

+ 设置默认的值
+ sex，默认值为男，如果不指定该列的值，就会有默认的值

拓展：

```sql
/* 每一个表，都必须存在以下5个字段
id	主键
`version`	乐观锁
is_delete	伪删除
gmt_create	创建时间
gmt_update	修改时间
*/
```

## 创建数据库表

```sql
--auto_increment 自增
--字符串使用单引号''括起来
--所有的语句后面加,最后一个不用加
create table `student`(
	`id` int(4) not null auto_increment comment '学号',
    `name` varchar(30) not null default '匿名’,
    `pwd` varchar(20) not null default '123456' comment'密码',
    `sex` varchar(20) not null default '女' comment'性别',
    primary key(`id`)
)engine=innodb default charset=utf8
```

## 数据表的类型

```sql
--关于数据库引擎
/*
innodb	默认使用
myisam 早年使用
*/
```

|              | MYISAM | INNODB        |
| ------------ | ------ | ------------- |
| 事务支持     | 不支持 | 支持          |
| 数据行锁定   | 不支持 | 支持          |
| 外键约束     | 不支持 | 支持          |
| 全文索引     | 支持   | 不支持        |
| 表空间的大小 | 较小   | 较大，约为2倍 |

常规使用操作：

+ MYISAM  节约空间，速度较快
+ INNODB  安全性高，事务的处理，多表多用户操作

> 在物理空间存在的位置

所有的数据库文件都存在data目录下

本质还是文件存储



MYSQL引擎在物理文件上的区别：

+ InnoDB在数据库表中只有一个\*.frm文件，以及上级目录下的ibdata1文件
+ MYISAM对应文件
  + \*.frm  -表结构的定义文件
  + \*.MYD  数据文件（data）
  + \*.MYI  索引文件（index）

> 设置数据库表的字符集编码

 ```sql
charset=utf8
 ```

MYSQL默认编码是Latin1，不支持中文

## 修改删除表

> 修改

```sql
--修改表名
alter table 旧表名 rename as 新表名
--增加表的字段 
alter table 表名 add 字段名 列属性
--修改表的字段（重命名，修改约束）
alter table 表名 modify 字段名 列属性 --修改约束
alter table 表名 change 旧字段名 新字段名 列属性 --字段重命名
alter table 表名 drop 字段名 --删除表的字段
```

> 删除

```sql
--删除表
drop table [if exists] xxx;
```

==所有的删除和创建尽量加上判断，以免报错==

注意点：

+ ``，字段名，用这个包裹
+ 注释 --，/**/
+ sql关键字建议用小写
+ 所有的符号用英文

# MySQL数据管理

## 外键

> 方式一、在创建表的时候，增加约束

```sql
CREATE TABLE `grade`(
`gradeid` INT(10) NOT NULL AUTO_INCREMENT COMMENT '年级id',
`gradename` VARCHAR(50) NOT NULL COMMENT '年级名称',
PRIMARY KEY (`gradeid`)
)ENGINE=INNODB DEFAULT CHARSET=utf8

--学生表的gradeid字段，要去引用年级表的gradeid
--定义外键key
--给这个外键添加约束（执行引用） reference引用

CREATE TABLE IF NOT EXISTS `student` (
	`id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
	`name` VARCHAR(30) NOT NULL DEFAULT '' COMMENT'姓名',
	`password` INT(20) NOT NULL DEFAULT '12456' COMMENT '密码',
	`sex` VARCHAR(2) NOT NULL DEFAULT '未知'COMMENT '性别',
	`birthday` DATETIME DEFAULT NULL COMMENT '生日',
	`gradeid` INT(10) NOT NULL DEFAULT '学生的年级',
	`address` VARCHAR(100) DEFAULT NULL COMMENT '家庭住址',
	`email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
	PRIMARY KEY(`id`),
	KEY `FK_gradeid` (`gradeid`),
	CONSTRAINT `FK_gradeid` FOREIGN KEY (`gradeid`) REFERENCES `grade`(`gradeid`)
)ENGINE = INNODB DEFAULT CHARSET = utf8
```

> 方式二、创建表成功后，添加外键约束

```sql
CREATE TABLE IF NOT EXISTS `student` (
	`id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
	`name` VARCHAR(30) NOT NULL DEFAULT '' COMMENT'姓名',
	`password` INT(20) NOT NULL DEFAULT '12456' COMMENT '密码',
	`sex` VARCHAR(2) NOT NULL DEFAULT '未知'COMMENT '性别',
	`birthday` DATETIME DEFAULT NULL COMMENT '生日',
	`gradeid` INT(10) NOT NULL COMMENT '学生的年级',
	`address` VARCHAR(100) DEFAULT NULL COMMENT '家庭住址',
	`email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
	PRIMARY KEY(`id`)
)ENGINE = INNODB DEFAULT CHARSET = utf8

CREATE TABLE `grade`(
`gradeid` INT(10) NOT NULL AUTO_INCREMENT COMMENT '年级id',
`gradename` VARCHAR(50) NOT NULL COMMENT '年级名称',
PRIMARY KEY (`gradeid`)
)ENGINE=INNODB DEFAULT CHARSET=utf8

ALTER TABLE `student`
ADD CONSTRAINT `FK_gradeid` FOREIGN KEY (`gradeid`) REFERENCES `grade`(`gradeid`)
-- ALTER TABLE 表 ADD CONSTRAINT 约束名 FOREIGN KEY (作为外键的列) REFERENCES 哪个表(哪个字段)
```

数据库级别的外键，不建议用

==最佳实践==

+ 数据库就是单纯的表，只用来存数据
+ 用程序去实现外键

## DML语言

DML语言：数据操作语言

+ insert
+ update
+ delete



## 添加

> insert

```sql
-- 插入语句（添加）
-- insert into 表名([字段名1，字段名2，字段名3])values('值1'),('值2'),('值3'),...
-- 由于主键自增我们可以省略（如果不写表的字段，他就会一一匹配）
INSERT INTO `grade`(`gradename`)VALUES('大四')
INSERT INTO `grade`(`gradename`)
VALUES ('大二'),('大一')

INSERT INTO `student`(`name`,`password`,`sex`,`gradeid`)
VALUES ('testname','123456','男','1'),('Volunteer','123456','男','2')
INSERT INTO `student` VALUES (4,'nameless','123456','男','2000-1-1','3','Earth','noemail@no.com')
```

注意事项：

1. 字段和字段之间是用英文逗号连接
2. 字段是可以省略的，但是后面的值必须要以一一对应
3. 可以同时插入多条数据，values后面的值，需要使用,隔开，e.g.values(),()

## 修改

> update

```sql
-- 修改学员的名字
UPDATE `student` SET `name` ='Volunteer' WHERE id = 1;
-- 不指明条件的情况下，会改动所有的表
UPDATE `student` SET `name`='testname'
-- 修改多个属性，用逗号隔开
UPDATE `student` SET `name`='testname',`email`='no@email.com' where id =1;

-- 语法
-- update 表名 set colnum_name = value,[colnum_name = value,...] where [条件]
```

条件：where 子句，运算符，id，等于某个值，大于某个值，在某个区间内修改...

|      操作符      | 含义         | 范围 | 结果 |
| :--------------: | ------------ | ---- | ---- |
|        =         | 等于         |      |      |
|        >         |              |      |      |
|        <         |              |      |      |
|        <=        |              |      |      |
|        >=        |              |      |      |
| between...and... | 在某个范围内 |      |      |
|       and        | 和           |      |      |
|        or        | 或           |      |      |
|      <>或!=      | 不等于       |      |      |

注意：

+ column_name是数据库的列，尽量带上``

+ 条件，筛选的条件，如果没有指定，则会修改所有的列 

+ value，是一个具体的值，也可以是一个变量

  ```sql
  UPDATE `student` SET `birthday`=CURRENT_TIME WHERE `name`='testname' AND id= 1
  ```

+ 多个设置的属性之间用英文逗号隔开

## 删除

> delete

语法：`delete from 表名`

```sql
-- 删除数据（这样写会导致全部删除）
DELETE FROM `student`

-- 删除指定数据
DELETE FROM `student` WHERE id =1
```

>  truncate命令

作用：完全清空一个数据表，表的结构和索引约束不会改变

```sql
-- 清空student表
TRUNCATE `student`
```

> delete和truncate的区别

+ 相同点：都能删除数据，都不会影响数据结构

+ 不同点：
  + truncate 重新设置 自增列 计数器会归零
  + truncate 不会影响事物 

```sql
-- 测试delete和truncate区别
CREATE TABLE `test`(
`id` INT(4) NOT NULL AUTO_INCREMENT,
`coll` VARCHAR(20) NOT NULL,
 PRIMARY KEY (`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT INTO `test`(`coll`) VALUES('1'),('2'),('3')

DELETE FROM `test`	-- 不会影响自增
TRUNCATE TABLE `test`	-- 自增会归零
```

了解：

`delete删除的问题，重启数据库，现象`

+ innodb 自增列会重1开始（存在内存中的，不会丢失）
+ MYISAM 继续从上一个自增量开始（存在文件中，不会丢失）

# DQL查询数据

## DQL

(Data Query Language:数据查询语言)

+ 所有的查询操作都用它 select
+ 简单复杂的查询都能做
+ ==数据库最核心的语言最重要的语句==
+ 使用频率最高的语句 

> SELECT语法

```sql
select [all | distinct]
{* | table.* | [table.field1[as alias1][,table.field2[as alias2]][,...]]}
from table_name [as table_alias]
	[left | right |inner join table_name2]	-- 联合查询
	[where ...]	-- 指定结果需满足的条件
	[group by ...]	-- 指定结果按照哪几个字段来分组
	[having]	-- 过滤分组的记录必须满足的次要条件
	[order by ...]	-- 指定查询记录按一个或多个条件排序
	[limit {[offset,]row_count | row_countoffset offset}];	-- 指定查询的记录从哪条至哪条
```

**注意：[]括号代表可选的，{}括号代表必选的**

## 指定查询字段

```sql
-- 查询所有的学生	SELECT 字段 FROM 表
SELECT * FROM `student`

-- 查询指定字段
SELECT `StudentNo`,`StudentName` FROM `student`

-- 别名，给结果起一个名字	AS可以给字段起别名，也可以给表起别名
SELECT `StudentNo` AS 学号,`StudentName` AS 学生姓名 FROM `student` AS STDT

-- 函数 Concat(a,b)
SELECT CONCAT('姓名',StudentName) AS 新名字 FROM `student` 
```

语法：`select 字段，... from 表`

> 去重 distinct 

作用：去除select查询出来的结果中重复的数据，重复的数据只显示一条

```sql
 -- 查询那些同学参加了考试，成绩
 SELECT * FROM result	-- 查询全部成绩 
 SELECT `StudentNo` FROM result	-- 查询哪些同学参加了考试
 -- 发现重复数据，去重
 SELECT DISTINCT `StudentNo` FROM result
```

> 数据库的列

```sql
 SELECT VERSION()	-- 查询版本（函数）
 SELECT 100*3-1 AS 计算结果	-- 用于计算（表达式）
 SELECT @@auto_increment_increment	-- 查询自增步长（变量）
 
 -- 学员考试成绩 +1分查看
 SELECT `StudentNo`,`StudentResult`+1 AS '提分后' FROM result
```

数据库中的表达式：

+ 文本值
+ 列
+ NULL
+ 函数
+ 计算表达式
+ 系统变量

select `表达式` from 表

## where条件字句

作用：检索数据中`符合条件`的值

搜索的条件有一个或多个表达式构成，结果布尔值

> 逻辑运算符

|  运算符  | 语法            | 描述                           |
| :------: | --------------- | ------------------------------ |
| and  &&  | a and b    a&&b | 逻辑与，两个都为真，结果为真   |
| or  \|\| | aor b    a\|\|b | 逻辑或，其中一个为真，结果为真 |
| not  ！  | not a    !a     | 逻辑非，真为假，假为真         |

```sql
 -- --------------where------------------------
 SELECT `StudentNo`,`StudentName` FROM result
 
 -- 查询考试成绩在95~100分之间
 SELECT StudentNo,`StudentResult` FROM result
 WHERE StudentResult >=95 AND StudentResult<=100
 
 -- 模糊查询（区间）
 SELECT StudentNo,`StudentResult` FROM result
 WHERE StudentResult BETWEEN 95 AND 100
 
 -- 除了1000号学生之外的学生的成绩
 SELECT StudentNo,`StudentResult` FROM result
 WHERE StudentNo!= 1000
 
 -- ------------------!-------------not
 SELECT StudentNo,`StudentResult` FROM result
 WHERE NOT StudentNo = 1000
```

> 模糊查询： 比较运算符

| 运算符      | 语法               | 描述                                            |
| ----------- | ------------------ | ----------------------------------------------- |
| IS NULL     | a is null          | 如果操作符为NULL，结果为真                      |
| IS NOT NULL | a is not null      | 如果操作符不为NULL，结果为真                    |
| BETWEEN     | a between b and c  | 若a在b和c之间，则结果为真                       |
| LIKE        | a like b           | SQL匹配，如果a匹配b，结果为真                   |
| IN          | a in (a1,a2,a3...) | 假设a在a1，或者a2....其中的某一个值中，结果为真 |

```sql
 -- --------------模糊查询---------------
 -- 查询姓刘的同学
 -- like结合 %（代表0到任意个字符）_（代表一个字符）
SELECT `StudentNo`,`StudentName` FROM `student`
WHERE StudentName LIKE '刘%'
 
 -- 查询姓刘的同学，名字后面只有一个字的
SELECT `StudentNo`,`StudentName` FROM `student`
WHERE StudentName LIKE '刘_'
 
 -- 查询名字中间有嘉字的同学
SELECT `StudentNo`,`StudentName` FROM `student`
WHERE StudentName LIKE '%刘%'
 
 -- ---------------- in (具体的一个或多个值)---------------------
 -- 查询10001，1002,1003号学员
SELECT `StudentNo`,`StudentName` FROM `student`
WHERE StudentNo IN (1001,1002,1003);
 
 -- 查询在安徽，河南洛阳的学生
SELECT `StudentNo`,`StudentName` FROM `student`
WHERE `Address` IN ('安徽'，'河南洛阳')

-- ----------------- null        not null-------------------------
-- 查询地址为空的学生 null ''
SELECT `StudentNo`,`StudentName` FROM `student`
WHERE address = '' OR adddress IS NULL

-- 查询有出生日期的学生		不为空
SELECT `StudentNo`,`StudentName` FROM `student`
WHERE `BronDate` IS NOT NULL
```

## 连表查询

```sql
-- =============联表查询==================

-- 查询参加了考试的同学（学号，姓名，科目编号，分数）
SELECT * FROM student
SELECT * FROM result


/*
1.分析需求，废墟查询的字段来自那些表
2.确定使用哪种连接查询？	七种
判断交叉点（这两个表中那个数据是相同的）
判断的条件： 学生表中的 StudentNo = 成绩表中的 StudentNo
*/

-- join on 连接查询（on 为条件判断）
-- where   等值查询

SELECT s.StudentNo，StudentName,SubjectNo,StudentResult
FROM Student AS s
INNER JOIN result AS r
ON s.StudentNo = r.StudentNo

-- Right Join
SELECT s.StudentNo,StudentNAme,SubjectNo,StudentResult
FROM Student s
RIGHT JOIN result r
ON s.StudentNo = r.studentNo

-- Left Join
SELECT s.StudentNo,StudentNAme,SubjectNo,StudentResult
FROM Student s
LEFT JOIN result r
ON s.StudentNo = r.studentNo
```

| 操作       | 描述                                     |
| ---------- | :--------------------------------------- |
| Inner Join | 如果表中至少有一个匹配，就返回行         |
| Left Join  | 会从左表中返回所有值，即使右表中没有匹配 |
| Right Join | 会从右表中返回所有值，即使左表中没有匹配 |

```sql
-- 思考题（查询参加了考试的同学的信息：学号，写生姓名，科目名，分数）
/*思路
1.分析需求，分析查询的字段来自哪些表，student、result、subject（连接查询）
2.确定使用哪种连接查询？	七种
判断交叉点（这两个表中那个数据是相同的）
判断的条件： 学生表中的 StudentNo = 成绩表中的 StudentNo
*/
SELECT studentNO,studentName,SubjectNo,`studentResult`
FROM student s
RIGHT JOIN result r
ON r.studentNo = s.student
INNER JOIN `subject` sub
ON r.SubjectNo = sub.SubjectNo

-- 我要查询哪些数据 select...
-- 从那几个表中查 from 表 xxx join 连接的表 on 交叉条件
-- 假设存在一种多张表查询，慢慢来，先查询两张表然后慢慢增加

-- from a left join b
-- from a right join b
```

> 自连接

自己的表和自己的表连接，核心：==一张表拆为两张一样的表==

父类

| categoryid | categoryName |
| ---------- | ------------ |
| 2          | 信息技术     |
| 3          | 软件开发     |
| 5          | 美术设计     |

子类

| pid  | categoryID | categoryName |
| ---- | ---------- | ------------ |
| 3    | 4          | 数据库       |
| 2    | 8          | 办公信息     |
| 3    | 6          | web开发      |
| 5    | 7          | ps技术       |

操作：查询父类对应的子类关系

| 父类     | 子类     |
| -------- | -------- |
| 信息技术 | 办公信息 |
| 软件开发 | 数据库   |
| 软件开发 | web开发  |
| 美术设计 | ps技术   |

## 分页和排序

> 排序

```sql
 -- 查询的结果根据 成绩降序 排序
 -- ORDER BY 通过那个字段排序，怎么排
 -- 查询的结果根据 成绩降序 排序
 SELECT s.`studentNo`,`StudentName`,`SubjectName`,`StudentResult`
 FROM student s
 INNER JOIN `result` r
 ON s.studentNo=r.studentNo
 INNER JOIN `subject` sub
 ON r.`SubjectNo`=sub.`SubjectNo`
 WHERE subjectName = '数据库结构-1'
 ORDER BY studentResult ASC/DESC
 -- ASC升序排列
 -- DESC降序排列
```

> 分页

```sql
 -- 为什么要分页
 -- 缓解数据库压力，给人的体验更好
 
 -- 分页，每页只显示五条数据
 -- 语法： limit 起始值，页面大小
 -- 网页应用：当前，总的页数，页面大小
 -- LIMIT 0,5 第一到五条数据
 -- IMIT 1,6 第二到六条数据
  SELECT s.`studentNo`,`StudentName`,`SubjectName`,`StudentResult`
 FROM student s
 INNER JOIN `result` r
 ON s.studentNo=r.studentNo
 INNER JOIN `subject` sub
 ON r.`SubjectNo`=sub.`SubjectNo`
 WHERE subjectName = '数据库结构-1'
 ORDER BY studentResult ASC
 LIMIT 0,5
 -- 第N页	limit(pageSize*(n-1),pageSize)
 -- 【pagesize：页面大小，pageSize*(n-1)：起始值，n：当前页】
 -- 【总页数：n%==5？n/5:n/5+1】
```

语法：`limit(查询起始下标，pagesize)`

## 子查询

where(这个值是计算出来的)

本质：`在where语句中嵌套一个子查询语句`

```sql
 -- ==================== where =====================
 -- 1、查询数据壳结构-1 的所有考试结果（学号，科目编号，成绩），降序排列
 -- 方式一：使用连接查询
 SELECT `Student `,r.`SubjectNo`,`StudentResult`
 FROM `result` r
 INNER JOIN `subject` sub
 ON r.SubjectNo=sub.SubjectNo
 WHERE SubjectName='数据库结构-1'
 ORDER BY StudentResult DESC
 
 -- 方式二：使用子查询
 SELECT `Student `,r.`SubjectNo`,`StudentResult`
 FROM `result`
 WHERE StudentNo=( 
	SELECT SubjectNo FROM `subject` 
	WHERE subjectName='数据库结构-1'
 )
 ORDER BY StudentResult DESC

-- 查询课程为 高等数学-2 且分数不小于80 的同学的学号和姓名（方法一）
SELECT s.Student,StudentName
FROM student s
INNER JOIN result r
ON s.StudentNo = r.StudentNo
INNER JOIN `subject` sub
ON r.`SubjectNo` = sub.`SubjectNo`
WHERE `SubjectName` = ''高等数学-2 AND StudentResult>=80

-- 分数不小于80分的学生的学号和姓名（方法二）
SELECT DISTINCT s.`StudentNo`,`StudentName`
FROM student s
INNER JOIN result r
ON r.StudentNo = s.StudentNo
WHERE `StudentResult`>=80
-- 在这个基础上增加一个学科，高等数学-2
-- 查询 高等数学-2 的编号
SELECT DISTINCT s.`StudentNo`,`StudentName`
FROM student s
INNER JOIN result r
ON r.StudentNo = s.StudentNo
WHERE `StudentResult`>=80 AND `SubjectNo` = (
  SELECT SubjectNo FROM `subject`
  WHERE `SubjectName` = '高等数学-2'
)

-- 再改造（方法三）
-- 执行顺序：由里及外
SELECT StudentNo,StudentName FROM student WHERE StudentNo IN (
	SELECT StudentNo FROM result WHERE StudentResult>80 AND SubjectNo = (
		SELECT SubjectNo FROM `subject` WHERE `SubjectName` = '高等数学-2'
	)
```

## 分组、过滤

```sql
 -- 查询不同课程的平均分，最高分，最低分，平均分大于80
 -- 核心：（根据不同的课程分组）
 SELECT SubjectName,AVG(StudentResult) AS 平均分,MAX(StudnetResult) AS 最高分,MIN(StudnetResult) AS 最低分
 FROM result r
 INNER JOIN `subject` sub
 ON r.`SubjectNo` = sub.`SubjectNo`
 GROUP BY r.SubjectNo -- 通过什么字段来分组
 HAVING 平均分>80 
```

# MySQL函数

官网：https://dev.mysql.com/doc/refman/8.0/en/functions.html

## 常用函数

```sql
-- ============常用函数===============

-- 数学运算
SELECT ABS(-8)	-- 绝对值
SELECT CEILING(9.4)	-- 向上取整
SELECT FLOOR(9.4)	-- 向下取整
SELECT RAND()	-- 返回一个0~1之间的随机数
SELECT SIGN(10)	-- 判断一个数的符号 0返回0，负数返回-1，正数返回1

-- 字符串函数
SELECT CHAR_LENGTH('这里是一个用于测试的字符串')	-- 字符串长度
SELECT CONCAT('用于','连接','测试')	-- 拼接字符串
SELECT INSERT('我爱编程',1,2,'超级热爱')		-- 查询，从某个位置开始替换某个长度
SELECT LOWER('TestVolunteer')	-- 转小写
SELECT UPPER('TestVolunteer') 	-- 转大写
SELECT INSTR('testname','m')	-- 返回第一次出现的字串的索引
 
 -- 时间和日期函数
 SELECT CURRENT_DATE()	-- 获取当前日期
 SELECT CURDATE()	-- 获取当天日期
 SELECT NOW()	 -- 获取当前时间
 SELECT LOCALTIME()	-- 本地日期
 SELECT SYSDATE()	-- 系统时间
 
 SELECT YEAR(NOW())
 SELECT MONTH(NOW())
 SELECT DAY(NOW())
 SELECT HOUR(NOW())
 SELECT MINUTE(NOW())
 SELECT SECOND(NOW())
 
 -- 系统
 SELECT SYSTEM_USER()	-- 获取当前用户
 SELECT USER()
```



## 聚合函数

| 函数    | 描述   |
| ------- | ------ |
| count() | 计数   |
| sum()   | 求和   |
| avg()   | 平均值 |
| max()   | 最大值 |
| min()   | 最小值 |
| ......  | ...... |

```sql
 -- =============聚合函数=============
 -- 查询表中记录
 SELECT COUNT(student) FROM student;	-- Count(字段)，会忽略所有的null值
 SELECT COUNT(*) FROM student;	-- count(*)，不会忽略null值
 SELECT COUNT(1) FROM student;	-- count(1)，不会忽略null值
 
 SELECT SUM(`StudentResult`) AS 总分 FROM result
 SELECT AVG(`StudentResult`) AS 平均分 FROM result
 SELECT MAX(`StudentResult`) AS 最高分 FROM result
 SELECT MIN(`StudentResult`) AS 最低分 FROM result
```

# 事务

## 什么是事务

将一组SQL放在一个批次中去执行

> 事务原则：ACID原则 原子性，一致性，隔离性，持久性	(脏读，幻读......)

**原子性（Atomicity）**

都成功，或都失败

**一致性（Consistency）**

事务前后的数据完整性要保持一致

**持久性（Durability）——事务提交**

事务一旦提交则不可逆，被持久化到数据库中

**隔离性（Isolation）**

事务的隔离性是多个用户并发访问数据库时，数据库为每个用户开启的事务，不能被其他事务的操作数据所干扰，事物之间要相互隔离

```sql
 -- =============事务==================
 
 -- mysql 是默认开启事务自动提交的
 SET autocommit = 0	-- 关闭
 SET autocommit = 1	-- 开启（默认）
 
 -- 手动处理事务
 SET autocommit = 0
 -- 事务开启
 START TRANSACTION	-- 标记一个事物的开始，从这个之后的SQL都在同一个事务内
 
 
-- 提交：持久化（成功！）
COMMIT
-- 回滚：回到原来的样子（失败！） 
 ROLLBACK
 -- 事务结束
 SET autocommit = 1	-- 开启自动提交
 
 -- 了解
 SAVEPOINT 保存点名	-- 设置一个事务的保存点
 ROLLBACK TO SAVEPOINT 保存点名	-- 回滚到保存点
 RELEASE SAVEPOINT 保存点名	-- 撤销保存点
```

> 模拟场景

```sql
 -- 转账
 CREATE DATABASE shop CHARACTER SET utf8 COLLATE utf8_general_ci
 USE shop
 
 CREATE TABLE `account`(
`id` INT(3) NOT NULL AUTO_INCREMENT,
`name` VARCHAR(30) NOT NULL ,
`money` DECIMAL(9,2) NOT NULL,
PRIMARY KEY (`id`) 
 )ENGINE=INNODB DEFAULT CHARSET = utf8
 
 INSERT INTO ACCOUNT(`name`,`money`)
 VALUES ('A',2000.00),('B',10000.00)
 
 -- 模拟转账
 SET autocommit = 0;	-- 关闭自动提交
 START TRANSACTION	-- 开启一个事务
 
 UPDATE `account` SET money =money-500 WHERE `name` = 'A'	-- A减500
 UPDATE `account` SET money =money+500 WHERE `name` = 'B'	-- B加500
 
 COMMIT;	-- 提交事务
 ROLLBACK;	-- 回滚
 
 SET autocommit = 1;	-- 恢复默认值
```

# 索引

> MySQL官方对索引的定义为：**索引（index）是帮助MYSQL高效获取数据的数据结构。**
>
> 提取句子主干就可以得到索引的本质：**索引是数据结构**

## 索引的分类

+ 主键索引（PRIMARY KEY）
  + 唯一的标识，不可重复，只能有一个列作为主键
+ 唯一索引（UNIQUE KEY）
  + 避免的重复的列出现，唯一索引可以重复，多个列都可以标识为唯一索引
+ 常规索引（KEY/INDEX）
  + 默认的，index、key关键字设置
+ 全文索引（FULLTEXT）
  + 在特定的数据库引擎下才有
  + 快速定位

基础语法

```sql
 -- 索引的使用
 -- 1、在创建表的时候给字段增加索引
 -- 2.创建完毕后，增加索引
 
 -- 显示所有的索引
 SHOW INDEX FROM student
 
 -- 增加一个全文索引
 ALTER TABLE school.student ADD FULLTEXT INDEX `StudnetName`(`StudentName`);
 
 -- explain 分析SQL执行的情况
 EXPLAIN SELECT * FROM student;	-- 非全文索引
 SELECT * FROM student WHERE MATCH(StudentName) AGAINST('刘'); 
```

## 测试索引

```sql
 -- 插入百万数据
 CREATE TABLE `app_user` (
`id` BIGINT(20) UNSIGNED NOT NULL AUTO_INCREMENT,
`name` VARCHAR(50) DEFAULT'' COMMENT'用户昵称',
`eamil` VARCHAR(50) NOT NULL COMMENT'用户邮箱',
`phone` VARCHAR(20) DEFAULT'' COMMENT'手机号',
`gender` TINYINT(4) UNSIGNED DEFAULT '0'COMMENT '性别（0：男;1:女）',
`age` TINYINT(4) DEFAULT'0'  COMMENT '年龄',
`create_time` DATETIME DEFAULT CURRENT_TIMESTAMP,
`update_time` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8 COMMENT = 'app用户表'

DROP TABLE `app_user`
-- 插入100万数据.
DELIMITER $$
-- 写函数之前必须要写，标志
CREATE FUNCTION mock_data ()
RETURNS INT DETERMINISTIC
BEGIN
	DECLARE num INT DEFAULT 1000000;
	DECLARE i INT DEFAULT 0;
	WHILE i<num DO
		INSERT INTO `app_user`(`name`,`eamil`,`phone`,`gender`,)VALUES(CONCAT('用户',i),'19224305@qq.com','123456789',FLOOR(RAND()*2));
		SET i=i+1;
	END WHILE;
	RETURN i;
END;

SELECT mock_data() -- 执行此函数 生成一百万条数据

SELECT * FROM app_user WHERE `name` = '用户99999';	-- 总耗时      : 0.292 sec

EXPLAIN SELECT * FROM app_user WHERE `name` = '用户99999';

-- id_表名_字段名
-- CREATE INDEX 索引名 on 表(字段)
CREATE INDEX id_app_user_name ON app_user(`name`);

SELECT * FROM app_user WHERE `name` = '用户99999';	-- 总耗时      : 0.002 sec

EXPLAIN SELECT * FROM app_user WHERE `name` = '用户99999';
```

索引在小数据量的时候，用处不大，但是在大数据的时候，区别十分明显

## 索引原则

+ 索引不是越多越好
+ 不要对经常变动的数据加索引
+ 小数据量的表不需要加索引
+ 索引一般加在常用查询的字段上

> 索引的数据结构

Hash类型的索引

Btree：InnoDB的默认数据结构



# 数据库备份

why?

+ 保证数据不丢失
+ 数据转移

MySQL数据库备份方式

+ 直接拷贝物理文件

+ 在SQLyog这种可视化工具中导出

  + 在想要导出的表或者库中，右键选择导出或备份
  + 

+ 使用命令行导出mysqldump 命令行使用

  ```bash
  # mysqldump -h 主机 -u 用户名 -p 密码 数据库 表名 > 物理磁盘位置/文件名
  mysqldump -hlocalhost -uroot -p123456 school student >D:/a.sql
  
  # mysqldump -h 主机 -u 用户名 -p 密码 数据库 表1 表2 表3 > 物理磁盘位置/文件名
  mysqldump -hlocalhost -uroot -p123456 school student >D:/a.sql
  
  # mysqldump -h 主机 -u 用户名 -p 密码 数据库 > 物理磁盘位置/文件名
  mysqldump -hlocalhost -uroot -p123456 school >D:/a.sql
  
  #导入
  #登录的情况下，切换到指定的数据库
  #source 备份文件
  source d:/a.sql
  
  mysql -u用户名 -p密码 库名< 备份文件
  ```

  

# 权限管理

## 用户管理

> SQLyog可视化管理

<img src="http://lsky.volerde.space/i/2022/05/16/628242d0b155c.png" alt="1652703950192.png" style="zoom:80%;" />

> SQL命令操作

用户表：mysql.user

本质：对这张表进行增删改查

```sql
-- 创建用户	CREATE USER 用户名 IDENTIFIED BY '密码'
CREATE USER VOlunteer IDENTIFIED BY '123456'

-- 修改密码（修改当前用户的密码）
SET PASSWORD = PASSWORD('123456')

-- 修改密码（修改指定用户的密码）
SET PASSWORD FOR VOlunteer = PASSWORD('123456')

-- 重命名 remname user 原来的名字 to 新的名字
RENAME USER Volunteer TO VOlunteer2

-- 用户授权 ALL PRIVILEGES 
-- 不可给别人授权
GRANT ALL PRIVILEGES ON *.* TO Volunteer2

-- 查询权限
SHOW GRANTS FOR VOlunteer2	-- 查看指定用户的权限
SHOW GRANTS FOR root@localhost

-- 撤销权限 revoke
REVOKE ALL PRIVILEGES ON *.* FROM Volunteer2

-- 删除用户
DROP USER Volunteer2
```

# 数据库的规约，三大范式

## 规范数据库设计

> 为什么需要设计

==当数据库比较复杂的时候，我们就需要设计了==

**糟糕的数据库设计：**

+ 数据冗余，浪费空间
+ 数据库插入和删除都会麻烦、异常【屏蔽使用物理外键】
+ 程序的性能差

**良好的数据库设计：**

+ 节省内存空间
+ 保证数据库的完整性
+ 方便开发系统

**软件开发中，关于数据库的设计**

+ 分析需求：分析业务和需要处理的数据库的需求
+ 概要设计：设计关系图E-R图

**设计数据库的步骤**（个人博客）

+ 收集信息，分析需求

  + 用户表（用户登录注销，用户的个人信息，写博客，创建个人分类）
  + 分类表（文章分类）
  + 文章表（文章的信息）
  + 评论表
  + 友链表（友链信息）

+ 标识实体（把需求落地到每个字段）

+ 标识实体之间的关系

## 三大范式

> 为什么需要数据规范化？

+ 信息重复
+ 更新异常
+ 插入异常
  + 无法正常显示信息
+ 删除异常
  + 丢失有效信息

> 三大范式（了解）

**第一范式（1NF）**

原子性：保证每一列不可再分

**第二范式（2NF）**

前提：满足第一范式

每张表只描述一件事情

**第三范式（3NF）**

前提：满足第一范式和第二范式

第三范式需要确保数据表中的每一列数据都和主键直接相关，而不能间接相关

（规范数据库的设计）

**规范性和性能问题**

关联查询的表不得超过三张表

+ 考虑商业化的需求和目标，（成本，用户体验）数据库的性能更加重要
+ 在规范性能的问题的时候，需要适当的考虑一下规范性
+ 故意给某些表增加一些冗余的字段（从多表查询变为单表查询）
+ 故意增加一些计算列（从大数据降为小数据量的查询：索引）

# JDBC

java.sql

javax.sql 

还需要导入一个数据库驱动包

## 第一个JDBC

> 测试数据库

```sql
CREATE DATABASE jdbcStudy CHARACTER SET utf8 COLLATE utf8_general_ci;

USE jdbcStudy;

CREATE TABLE `users`(
	id INT PRIMARY KEY,
	NAME VARCHAR(40),
	PASSWORD VARCHAR(40),
	email VARCHAR(60),
	birthday DATE
);

INSERT INTO `users`(id,NAME,PASSWORD,email,birthday)
VALUES(1,'zhansan','123456','zs@sina.com','1980-12-04'),
(2,'lisi','123456','lisi@sina.com','1981-12-04'),
(3,'wangwu','123456','wangwu@sina.com','1979-12-04')
```



1. 创建一个普通项目

2. 导入数据库驱动

3. 编写测试代码

   ```java
   public class JdbcFirstDemo {
       public static void main(String[] args) throws ClassNotFoundException, SQLException {
           //1.加载驱动
           Class.forName("com.mysql.jdbc.Driver");//固定写法，加载驱动
           //2.用户信息和URL
           String url = "jdbc:mysql://localhost:3306/jdbcstudy?useUnicode=true&characterEncoding=utf8&useSSL=true";
           String usename = "root";
           String password = "123456";
           //3.连接成功,连接成功   Connection 代表数据库
           Connection connection = DriverManager.getConnection(url,usename,password);
           //4.执行SQL的对象  Statement 执行SQL对象
           Statement statement = connection.createStatement();
           //5.执行SQL的对象去执行SQL，可能存在结果，查看返回结果
           String sql = "select * from users";
           ResultSet resultSet = statement.executeQuery(sql);//返回的结果集,结果集中封装了我们全部的查询出来的结果
           while (resultSet.next()){
               System.out.println("id="+resultSet.getObject("id"));
               System.out.println("name="+resultSet.getObject("NAME"));
               System.out.println("pwd="+resultSet.getObject("PASSWORD"));
               System.out.println("email="+resultSet.getObject("email"));
               System.out.println("birthday="+resultSet.getObject("birthday"));
               System.out.println("=============================");
           }
           //6.释放连接
           resultSet.close();
           statement.close();
           connection.close();
       }
   }
   ```

   步骤总结：

   1. 加载驱动
   2. 连接数据库 DriverManager
   3. 获取执行SQL的对象 Statement
   4. 获得返回的结果集
   5. 释放连接

> DriverManager

```java
 Class.forName("com.mysql.jdbc.Driver");//固定写法，加载驱动
 Connection connection = DriverManager.getConnection(url,usename,password);
//connectio--代表数据库
//数据库设置自动提交
connection.setAutoCommit();
//事务提交
connection.commit();
//事务回滚
connection.rollback();
```



> URL

```java
String url = "jdbc:mysql://localhost:3306/jdbcstudy?useUnicode=true&characterEncoding=utf8&useSSL=true";
//mysql默认端口	--3306
//jdbc:mysql://主机地址:端口号/数据库名？参数1&参数2&参数3

//oracle --1521
//jdbc:oracle:thin:@localhost:1521:sid
```

> Statement	执行SQL的对象	PrepareStatement执行SQL的对象

```java
String sql = "SELECT * FROM USER";//编写SQL

statement.executeQuery();//查询操作返回ResultSet
statement.execute();//执行任何SQL
statement.executeUpdate();//更新、插入、删除都是用这个，返回一个受影响的行数
```

> ResultSet查询的结果集：封装了所有的查询结果

获得指定的数据类型

```java
resultSet.getObject();//在不知道列类型的情况下使用
//如果知道具体的类就使用指定类型
resultSet.getString();
resultSet.getInt();
resultSet.getFloat();
resultSet.getDouble();
resultSet.getDate();
```

遍历、指针

```java
resultSet.beforeFirst();//移动到最前面
resultSet.afterLast();//移动到最后面
resultSet.next();//移动到下一个数据
resultSet.previous();//移动到前一行
resultSet.absolute(row);//移动到指定行
```

> 释放资源

```java
//6.释放连接
resultSet.close();
statement.close();
connection.close();
//耗资源
```

## statement对象

==jdbc中的statement对象用于向数据库发送SQL语句，想完成对数据库的增删改查，只需要通过这个对象向数据库发送增删改查语句即可==

Statement对象的executeUpdate方法，用于向数据库发送增删改查的SQL语句，恶心cuteUpdate执行完后，将会返回一个整数（即增删改查语句导致了数据库几行数据发生了变化）。

Statement.executeQuery方法用于向数据库发送查询语句，executeQuery方法返回代表查询结果的ResultSet对象。

> CRUD操作-create

使用executeUpdate(String sql)方法完成数据删除操作，示例操作:

```java
Statement st = connection.createStatement();
String sql = "insert into user(...) values(...)";
int num = st.executeUpdate(sql);
if (num>0){
    System.out.println("插入成功！");
}
```

> CRUD操作-delete

使用executeUpdate(String sql)方法完成数据修改操作，示例操作:

```java
Statement st = connection.createStatement();
String sql = "update user set name='' where name=''";
int num = st.executeUpdate(sql);
if (num>0){
    System.out.println("修改成功！");
}
```

> CRUD操作-read

使用executeQuery(String sql)方法完成数据查询操作，示例操作：

```java
Statement st = connection.createStatement();
String sql = "select * from user where id = 1";
ResultSet rs = st.executeQuery(sql);
where(rs.next()){
    //根据获取列的数据类型，分别调用rs的相应方法映射到java对象中
}
```

> 代码实现

```java
package com.test.lesson02;

import com.test.lesson02.utils.JdbcUtils;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class TestInsert {
    public static void main(String[] args) {

        Connection connection = null;
        Statement statement = null;
        ResultSet resultSet = null;
        try {
           connection= JdbcUtils.getConnection();//获取数据库连接
            statement = connection.createStatement();//获得SQL的执行对象
            String sql = "INSERT INTO users(id,`NAME`,`PASSWORD`,`email`,`birthday`)"+
                    "NALUES(4,`Volunteer`,`123456`,`test@VOlunteer.com`,`2020-01-01`)";
            int i = statement.executeUpdate(sql);
            if(i>0){
                System.out.println("插入成功");
            }
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }finally {
            JdbcUtils.release(connection,statement,resultSet);
        }
    }
}
```

> SQL注入的问题

SQL存在漏洞，会受到攻击，导致数据泄露

## PreparedStatement对象

PreparedStatement可以防止SQL注入。效果更好

新增



删除



更新



查询



## 事务

> ACID原则

原子性：要么全部完成，要么都不完成

一致性：总数不变

隔离性：**多个进程互不干扰**

持久性：一旦提交不可逆，持久化到数据库中了



隔离性问题：

+ 脏独：一个事务读取了另外一个没有提交的事务
+ 不可重复读：在一个事务内，重复读取表中的数据，表数据发生了变化
+ 虚度（幻读）：在一个事务内，读取到了别人插入的数据，导致前后读出来的结果不一致

```java
public class TestTransaction {
    public static void main(String[] args) {
        Connection connection = null;
        PreparedStatement statement = null;
        ResultSet resultSet = null;

        try{
            connection = JdbcUtils.getConnection();
            //关闭数据库的自动提交
            connection.setAutoCommit(false);//开启事务

            String sql1 = "update account set money = money-100 where mame ='A'";
            statement = connection.prepareStatement(sql1);
            statement.executeUpdate();
            String sql2 = "update account set money = money+100 where mame ='B'";
            statement = connection.prepareStatement(sql2);
            statement.executeUpdate();

            //业务完毕，提交事务
            connection.commit();
            System.out.println("success");
        } catch (SQLException throwables) {
            try {
                connection.rollback();//如果失败则回滚事务
            } catch (SQLException e) {
                e.printStackTrace();
            }
            throwables.printStackTrace();
        }finally {
            JdbcUtils.release(connection,statement,resultSet);
        }
    }
}
```

## 数据库连接池

数据库连接--执行完毕--释放

连接--释放	十分浪费资源

**池化技术：准备一些预先的资源，过来就会连接，预先准备好的**

+ 常用连接数

+ 最小连接数

+ 最大连接数

编写连接池，实现一个接口DataSource

> 开源数据源实现

DBCP

C3P0

Druid

使用了这些数据库连接池之后，我们在项目开发中就不需要编写连接数据库的代码了

> DBCP

需要用到的jar包

commons-dbcp、commons-pool

> C3P0

c3p0、mchange-commons-java

> 结论

无论是用什么数据源，，本质还是一样的，DataSource接口不会变，方法就不会变

