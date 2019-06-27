---
title: MySQL-define
date: 2019-06-20 16:17:10
tags: github
---
## 数据库
### 约束
- 用于限制约定表中的数据完整性, 有效性.
- 数据库提供了"基础的"完整性,有效性检查.

#### 主键约束 primary key
- 什么是主键: 表示数据唯一性的字段称为主键
- 什么是约束: 是创建表时给表字段添加的限制条件
- 主键约束: 让该字段的数据唯一且非空(不能重复,不能null)
```mysql
格式: create table t1(id int primary key,name varchar(10));
	insert into t1 values(1,'AAA'); //成功!
	insert into t1 values(1,'BBB');//报错 不能重复
	insert into t1 values(null,'CCC'); //报错 不能为null
```

#### 主键约束+自增
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

#### 非空约束
- 验证表中的列值, 不能添加空值.
- 在设计表的时候为列添加非空约束, 则在插入更改数据时候, 一旦数据为null则报错, 进制添加和修改.
- 在字段类型后面添加 “ not null ”
```mysql
建表
	create table t3(
    id int primary key,
    name varchar(50) not null,
    nick varchar(50)
	); 

测试
-- 正常插入 
    insert into t3 (id, name, nick) 
    values (1, '刘国斌', null);
    insert into t3 (id, name, nick) 
    values (2, '范传奇', '范老湿');
    -- 错误插入
    insert into t3 (id, name, nick) 
    values (3, null, '范');
```

#### 唯一约束
- 限定一个列中的值, 不能重复, 保持唯一. 除了主键以外的其他列保持唯一
- 在字段类型后面添加 unique
```mysql
建表
-- 设定邮箱地址列是唯一约束, 不能重复
	create table t4(
    id int primary key,
    name varchar(50) not null,
    email varchar(100) unique
	);      
验证: 插入重复的邮箱地址时候出现错误!
```

#### 默认约束
- 为列添加默认值
- 在字段类型后面添加  default '男' 
```mysql
建表
-- 将性别的默认值设置为 男 
	create table t5(
    id int primary key,
    name varchar(50) not null,
    sex varchar(10) default '男' 
	); 
	
验证
    insert into t5 (id, name) values (1, '范传奇');
    insert into t5 (id, name, sex) values (2,'刘国斌','女');
```

#### 外键约束
- 检查表中列取值于另外一个表的主键列
- 语法:   FOREIGN KEY(外键列) REFERENCES 主键表(主键列)
```mysql
建表
-- 主键表
	create table user(
    id int primary key,
    name varchar(50) not null
	); 
-- 外键表
	create table trad(
    id int primary key,
    uid int,
    money double,
    FOREIGN KEY(uid) REFERENCES user(id)
	);
外键列: uid 列的值必须是 user id 的值!
```

- 外键特点:
1. 添加外键约束的字段值可以为null，但是不能是关联表中不存在的数据
2. 如果建立了关系被关联的数据不能先删除，被关联的表不能先删除
3. 有外键关联以后,删除数据时候,就必须先删除外键关联, 在删除主键表中的数据.

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

### 关系
- 一对一, 性能优化
- 一对多, 解决业务关系, 使用最多
	- 自关联 一对多 分类树 
- 多对多: 中间的关系表
	- 用户和角色

### View (视图)
- 利用一个查询语句构建一个虚拟表. 如果需要查询就可以通过查询视图获得结果.
- 利用视图可以重用复杂的查询功能, 简化二次查询工作
- 视图不是表, 视图中不存储数据, 数据是通过内部封装的SQL语句动态查询得到的. 视图的本质是 **复用复杂查询**
- **示例：**
```mysql
	1. 创建视图
	create view student_course as (
    select s.name as student, c.name as course
    from t_student s 
    join t_choose cc    on s.id = cc.sid  
    join t_course c on cc.cid=c.id );
	2. 利用视图进行查询:
	select student, course from student_course 
	where student = '莫小贝'; 

	select student, course from student_course 
	where student = '白展堂'; 
```
- 管理view
	- 创建 create view 视图名 as 查询
	- 查询全部视图SHOW FULL TABLES WHERE TABLE_TYPE LIKE 'VIEW';
	- 删除视图 drop view 视图名
	- 视图可以嵌套使用

### 索引
- 数据库提供的一种高效查询算法! 可以提高海量数据查询效率!
- 索引可以大大加快大数量的查询效率!
- 数据量常见索引算法: B+树(B plus Tree)
	- B+树原理: 1. 数据库数据是连续存储到 磁盘 上的数据块. 每次读写一个数据块, 每个数据块读写 需要 几个ms. 如果是海量数据查询时候, 就需要很多次读写每个数据块, 累计时间很长! 2. B+树 建立两层索引数据块, 索引中包含数据的范围. 只需要读取两层索引块两次, 就能确定目标数据的位置. 查询到结果. 大大减少磁盘IO次数, 提高查询性能! 3. 在最终数据块上还有一次IO, 一共3次磁盘块IO就可以读取到数据, 其性能非常好!!
- 语法:
```mysql
create index 索引名 on 表名(字段名(?长度));

创建索引
	create index idxusername on user(name);

使用索引(自动使用索引!)
	select id, name from user where name='Tom';
```
