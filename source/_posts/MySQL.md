---
title: MySQL-(命令)
date: 2019-06-19 16:58:48
tags: github
---
## 数据库
1. 查看数据库
- 格式：show databases;
2. 创建数据库
- 格式：create database 数据库名;
3. 查看数据库详情
- 格式：show create database 数据库名;
4. 创建数据库指定字符集
- 格式：create database 数据库名 character set utf8/gbk;
5. 删除数据库
- 格式：drop database 数据库名;
6. 使用数据库
- 格式：use 数据库名;

## 表相关
1. 创建表
- 格式：create table 表名(字段1名 字段1类型，字段2名 字段2类型)
2. 查看所有表
- 格式：show tables;
3. 查看表详情
- 格式：show create table 表名;
4. 创建表指定引擎和字符集
- 格式：create table 表名(字段1名 字段1类型，字段2名 字段2类型)
engine = myisam/innodb charset = utf8/gbk;
5. 查看表字段
- 格式：desc 表名;
6. 删除表
- 格式：drop table 表名;
7. 修改表名
- 格式：rename table 原名 to 新名;
8. 修改引擎和字符集
- 格式：alter table 表名 engine = myisam/innodb charset = utf8/gbk;
9. 添加表字段
- 格式：alter table 表名 add 字段名 字段类型; //最后
- 格式：alter table 表名 add 字段名 字段类型 first; //最前面
- 格式：alter table 表名 add 字段名 字段类型 after XXX; //在XXX的后面
10. 删除表字段
- 格式：alter table 表名 drop 字段名;
11. 修改表字段名和类型
- 格式：alter table 表名 change 原字段名 新名 新类型;
12. 修改字段类型和位置
- 格式：alter table 表名 modify 字段名 字段类型 first/after XXX;

## 数据相关
1. 插入数据(增)
- 全表插入格式：insert into 表名 values(值1，值2，值3)
- 指定字段插入格式：insert into 表名(字段1名，字段2名) values(值1，值2)
- 中文问题：
	- insert into emp values (3,'刘备',30);
	- set names gbk;
- 批量插入：insert into 表名 values(值1，值2，值3)，(值1，值2，值3);
	- insert into 表名 (命名) values(值),(值),(值);
2. 查询数据
- 格式：select 字段信息 from 表名 where 条件;
	- select * from 表名;  查询所有数据的所有字段信息
3. 修改数据
- 格式：update 表名 set 字段名 = XXX where 条件;
```bash
		update emp set age=500 where name='悟空';
```
4. 删除数据
- 格式：delete from 表名 where 条件;
```bash
		delete from emp where age is null; #null不可以直接‘=’要用 "is"
```

### 主键约束 
- 格式：create table 表名(字段名 字段类型 primary key, 字段名 字段类型);

### 主键约束+自增
- 自增数值只增不减，从历史最大值上+1
- 格式：create table 表名(字段名 字段类型 primary key auto_increment, 字段名 字段类型);

### 注释
- 格式：create table 表名(字段名 字段类型 primary key auto_increment comment ' 注释 ', 字段名 字段类型 comment ' 注释 ');

### "`"的作用
- 用于修饰表名和字段名，可以省略
```bash
	create table `t4`(`id` int,`name` varchar(10));
```

### 事务
- 事务相关指令
	1. 开启指令：begin
	2. 提交事务：commit
	3. 回滚：rollback
	4. 保存回滚点：savepoint XXX
	5. 回到回滚点：rollback to XXX
```
	使用
	begin;
	update user set money=2 where id=2;
	savepoint s1;
	update user set money=1 where id=2;
	rollback to s1;
```

### and 和 or 
- 如果查询数据时需要同时满足多个条件则使用and   &&
- 如果查询数据时只需要满足条件中的某一个则使用or ||

### is null 和 is not null
```mysql
	从员工表中查询 没有上级领导的员工姓名,工资
	select ename,sal from emp where mgr is null;
```

### 别名 
```mysql
	select ename as '姓名' from emp;
	select ename '姓名' from emp;
	select ename 姓名 from emp;
```

### 比较运算符 
```mysql
	>  <  <=  >=  !=和<>
```

### 去重 distinct
```mysql
	查询员工表中有哪些职位
	select distinct job from emp;
```

### 模糊查询 like
- % 代表0或多个字符
- " _ " 代表单个未知字符
	- 包含：%X%

### 查询两组数据之间
- between x and y

### 查询多个字段值
- in(值 逗号隔开)

### 排序 order by
- 格式：order by 字段名 asc/desc  asc:升序(默认)  desc:降序
- order by 写在条件的后面
- 多字段排序：order by 字段1 asc/desc,字段2 asc/desc 

### 分页查询
- 格式：limit 跳过的条数，请求的条数
- 公式：limit (页数-1) * 数量,数量

### 数值计算
- “ + - * / % ”
```
	select ename,sal,sal+5 加薪后 from emp;
```

### 日期相关SQL
- 获取系统时间 now( )
```
	create table t_date(name varchar(10),birthday datetime);
	insert into t_date values('AAA',now());
```
- 获取当前的年月日 和当前的时分秒  cur=current 当前
```
	select "helloworld";
	select curdate(),curtime();
```
- 从年月日时分秒中 提取年月日 和提取时分秒
```mysql
	select date(now())
	select time(now())
```
- 从年月日时分秒中提取时间分量
```mysql
	select extract(year from now()); #年
	select extract(month from now());#月
	select extract(day from now());  #日
	select extract(hour from now()); #时
	select extract(minute from now());#分
	select extract(second from now());#秒
```
- 日期格式化 date_format(时间，格式)
	- 格式规则：%Y 四位年 %y 两位年 (19)
	- %m 2位月 (06) %c 一位月
	- %d 日  %H 24小时  %h 12小时
	- %i 分钟  %s 秒
```mysql
	select date_format(now(),'%Y年%m月%d号 %H点%i分%s秒');
```
- 反向格式化 str_to_date(时间,格式)
```mysql
	select str_to_date('20.06.2019 15:36:20','%d.%m.%Y %H:%i:%s');
```

### ifnull(x,y)
- age=ifnull(x,y)  如果x值为null则age=y  否则age=x

### 聚合函数
- 对查询的多条数据进行统计查询：平均值 最大值 最小值 求和 计数
1. 平均值 avg(字段名)
2. 最大值 max(字段名)
3. 最小值 min(字段名)
4. 求和 sum(字段名)
5. 计数 count(字段名)
```mysql
	select count(字段名),sum(字段名) from 表名 where 条件;
```

### 字符串相关
1. 字符串拼接 concat(s1,s2)
```mysql
	select concat('aa','bb');
```
2. 获取字符串的长度 char_length(str)
```mysql
	select char_length('aa');
```
3. 获取字符串出现的位置 instr(str,substr)
```mysql
	select instr('abcdef','d');
```
4. 转大小写
```mysql
	select upper('abc'),lower('ABC');
```
5. 截取字符串
	- 左边截取：select left('abcdefg',2);
	- 右边截取：select right('abcdefg',2);
	- 自由截取：select substring('abcdefg',2,3); 3代表长度
6. 去两端空白 tirm()
7. 重复 repeat(字符串，重复的次数)
8. 替换 replace(字符串，需要替换的字符，替换的字符)
9. 反转 reverse(字符串)

#### 数学相关
- 向下取整 floor(num)
- 四舍五入 round(num)
- 四舍五入 round(num,m) m代表保留小数位数
- 非四舍五入 truncate(num,m) m代表保留小数位数
- 随机数 rand()  获得0-1的随机数

#### 分组查询
- 查询每个部门的平均工资
```mysql
	select deptno,avg(sal) from emp group by deptno;
```
- 多字段分组
	- group by 字段名1,字段名2
```mysql
	select deptno,job,avg(sal) from emp group by deptno,job;
```

#### having
- where 后面只能写普通字段的条件
- having 后面写聚合函数的条件，having和分组查询结合使用
- 各关键字的顺序：select......from 表名 where......group by ... having ... order by ...  limit......

### 子查询（嵌套查询）
- 查询员工表中工资最高的员工信息
```mysql
	select * from emp where sal=(select max(sal) from emp);
```
- 子查询可以写的位置
	1. 写在where和having后面，当做查询条件的值
	2. 写在创建表的时候，把查询结果保存到新的表中
	```mysql
	create table emp_10 as (select * from emp where deptno=10);
	```
	3. 写在from后面  **一定要有别名**
	```mysql
	select ename from (select * from emp where deptno=10) t;
	```

#### 关联查询
- 同时查询多张表的查询方式为关联查询
- 关联查询必须写关联关系，如果不写则会得到两张表的乘积，这个乘积称为笛卡尔积，这是错误的查询结果，切记工作中不要出现
	- 查询每一个员工的姓名对应的部门名
```mysql
	select e.ename,d.dname
	from emp e,dept d
	where e.deptno=d.deptno;
```

#### 关联查询的查询方式之等值连接和内连接
1. 等值连接：
```mysql
	select * from A,B where A.x=B.x and A.y>2000;
```
2. 内连接：
```mysql
	select * from A [?inner] join B on A.x=B.x where A.y>2000;
	查询每一个员工的姓名与对应的部门名
	select e.ename,d.dname
	from emp e join dept d
	on e.deptno=d.deptno;
```

#### 关联查询的查询方式之 外连接
- 等值连接和内连接查询的是两张表的交集数据
- 外连接查询的是一张表的全部数据和另外一张表的交集数据
```mysql
	格式：select * from A left/right join B on A.x=B.x where A.y>2000;
	查询所有的部门名和对应的员工姓名
	select d.dname,e.ename
	from emp e right join dept d
	on e.deptno=d.deptno;
```
- 总结: 如果查询的数据是两张表的交集数据使用等值连接或内连接(推荐),如果查询的是一张表的全部数据和另外一张表的交集数据则使用外连接。

