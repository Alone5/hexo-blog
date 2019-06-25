---
title: MySQL-define
date: 2019-06-20 16:17:10
tags: github
---
## 数据库
### 主键约束 primary key
- 什么是主键: 表示数据唯一性的字段称为主键
- 什么是约束: 是创建表时给表字段添加的限制条件
- 主键约束: 让该字段的数据唯一且非空(不能重复,不能null)
```mysql
格式: create table t1(id int primary key,name varchar(10));
	insert into t1 values(1,'AAA'); //成功!
	insert into t1 values(1,'BBB');//报错 不能重复
	insert into t1 values(null,'CCC'); //报错 不能为null
```

### 主键约束+自增
- 自增数值只增不减,从历史最大值基础上+1
```mysql
格式: create table t2(id int primary key auto_increment,name varchar(10));
	insert into t2 values(null,'aaa'); //1
	insert into t2 values(null,'bbb'); //2
	insert into t2 values(3,'ccc');    //3
	insert into t2 values(10,'ddd');   //10
	insert into t2 values(null,'eee');  //11
	delete from t2 where id>=10;
	insert into t2 values(null,'fff');  //12
```

### 冗余
- 由于表设计不够合理导致的大量重复数据称为数据冗余

### 事务
- 什么是事务:事务是数据库中执行同一业务多条SQL语句的工作单元,可以保证多条SQL全部执行成功或全部执行失败.
- 事务相关指令:
	1. 开启事务   begin;
	2. 提交事务   commit;
	3. 回滚      rollback;
- 验证转账流程:
	- create table user(id int primary key auto_increment,name varchar(10),money int,status varchar(5));
	- insert into user values(null,'超人',50,'冻结'),(null,'蝙蝠侠',5000,'正常'),(null,'灭霸',20,'正常');
- 转账的SQL:
	- update user set money=money-2000 where id=2 and status='正常';
	- update user set money=money+2000 where id=1 and status='正常';
```mysql
有事务保护的情况下 回滚流程:
	1. 开启事务
		begin;
	2. 蝙蝠侠-2000
		update user set money=money-2000 where id=2 and status='正常';
	3. 此时在当前终端查询数据时 数据已经改变(因为查询到的是内存中的改动), 开启另外一个终端查询数据发现数据是没有改变的(因为新的终端查询到的是磁盘的数据)
	4. 超人+2000
		update user set money=money+2000 where id=1 and status='正常';
	5. 此时从执行结果中发现一条成功一条失败,应该执行回滚操作
		rollback;
- 有事务保护的情况下 提交流程:
	1. 开启事务
		begin;
	2. 蝙蝠侠-2000
		update user set money=money-2000 where id=2 and status='正常';
	3. 此时仍然是在内存中改动 磁盘数据没有发生改变 
	4. 灭霸+2000
		update user set money=money+2000 where id=3 and status='正常';
	5. 此时两次改动都是在内存中改完,发现两次全部成功,所以执行提交
		commit;
- 保存回滚点:
	begin;
	update user set money=1 where id=2;
	savepoint s1;
	update user set money=2 where id=2;
	savepoint s2;
	update user set money=3 where id=2;
	rollback to s2;
```

- **事务的ACID特性**
	- 保证事务正确执行的四大基本要素
1. Atomicity原子性: 最小不可拆分 保证全部执行成功或全部执行失败
2. Consistency一致性: 从一个一致状态到另一个一致状态
3. Isolation隔离性: 多个事务之间互相隔离互不影响 
4. Durability持久性: 当事务提交后数据保存到磁盘中持久生效  

### SQL分类
#### DDL Data Definition Language数据定义语言
- truncate table 表名; 
	删除表并创建新表    让自增数值清零
- 包括: create drop alter truncate 
- 不支持事务 
#### DML Data Manipulation Language数据操作语言
- 包括: insert update delete select
- 支持事务
#### DQL Data Query Language 数据查询语言
- 只包括: select
#### TCL Transaction Control Language 事务控制语言
- 包括: begin,commit,rollback,savepoint xxx, rollback to xxx;
#### DCL Data Control Language 数据控制语言
- 负责分配用户权限相关的SQL 

### 数据类型
1. 整数:  int(m) 和 bigint(m)  m代表的是显示长度,需要结合zerofill使用
	- create table t_int(id int,age int(10) zerofill); 
	- insert into t_int values(1,18);
	- select * from t_int;	
2. 浮点数: double(m,d)  m代表的是总长度 d代表小数长度,超高精度的浮点数decimal(m,d) 
3. 字符串: 
- char(m) 固定长度 最大长度255 好处执行效率略高  
- varchar(m) 可变长度 好处节省空间 最大长度65535(但是超过255建议使用text)  
- text(m) 可变长度 最大长度65535
4. 日期:
- date: 保存年月日
- time: 保存时分秒
- datetime: 保存年月日时分秒,默认值为null,最大9999-12-31
- timestamp(时间戳):保存年月日时分秒,默认值为当前系统时间,最大值2038-1-19
		- create table t_date(t1 date,t2 time,t3 datetime,t4 timestamp); 
		- insert into t_date values('2019-6-20',null,null,null);
		- insert into t_date values(null,'16:46:30','2019-06-20 16:30:30',null);

### 导入 * .sql文件
- windows系统 把文件放在C或D盘的根目录
	- source d:/tables.sql;
- linux系统 把文件放在桌面
	- source /home/soft01/桌面/tables.sql
	- 查询数据 如果是乱码  执行     set names gbk

