---
title: MySQL-(练习题)
date: 2019-06-24 19:05:42
tags: github
---
### 练习题
1. 查询没有上级领导的员工的编号，姓名，工资
```mysql
	select empno,ename,sal from emp where mgr is null;
```
2. 查询emp表中没有奖金的员工的姓名，职位，工资，以及奖金
```mysql
	select ename,job,sal,comm from emp where comm=0 or comm is null;
```
3. 查询emp表中含有奖金的员工的编号，姓名，职位，以及奖金
```mysql
	select empno,ename,job,comm from emp where comm>0;
```
4. 查询含有上级领导的员工的姓名，工资以及上级领导的编号
```mysql
	select ename,sal,mgr from emp where mgr is not null;
```
5. 查询emp表中名字以‘S’开头的所有员工的姓名
```mysql
	select ename from emp where ename like 's%';
```
6. 查询emp表中名字的最后一个字符是'S'的员工的姓名
```mysql
	select ename from emp where ename like '%s';
```
7. 查询倒数的第2个字符是‘E’的员工的姓名
```mysql
	select ename from emp where ename like '%e_';
```
8. 查询emp表中员工的倒数第3个字符是‘N’的员工姓名
```mysql
	select ename from emp where ename like '%n__';
```
9. 查询emp表中员工的名字中包含‘A’的员工的姓名
```mysql
	select ename from emp where ename like '%a%';
```
10. 查询emp表中名字不是以'K'开头的员工的所有信息
```mysql
	select * from emp where ename not like 'k%';
```
11. 查询emp表中名字中不包含‘A’的所有员工的信息
```mysql
	select * from emp where ename not like '%a%';
```
12. 做文员的员工人数（job 中 含有 CLERK 的）
```mysql
	select count(*) from emp where job='clerk';
```
13. 销售人员 job: SALESMAN 的最高薪水
```mysql
	select max(sal) from emp where job='SALESMAN';
```
14. 最早和最晚入职时间
```mysql
	select min(hiredate),max(hiredate) from emp;
```
15. 查询类别 163的商品总库存量
```mysql
	select sum(num) from t_item where category_id=163;
```
16. 查询 类别 163 的商品
```mysql
	select * from t_item where category_id=163;
```
17. 查询商品价格不大于100的商品名称列表
```mysql
	select title from t_item where price<=100;
```
18. 查询品牌是联想,且价格在40000以上的商品名称和价格
```mysql
	select title,price from t_item where title like '%联想%' and price>40000;
```
19. 查询品牌是三木,或价格在50以下的商品名称和价格
```mysql
	select title,price from t_item where title like '%三木%' or price<50;
```
20. 查询品牌是三木、广博、齐心的商品名称和价格
```mysql
	select title,price from t_item where title like '%三木%' or title like '%广博%' or title like '%齐心%';
```
21. 查询品牌不是联想、戴尔的商品名称和价格
```mysql
	select title,price from t_item where title not like '%联想%' and title not like '%戴尔%';
```
22. 查找品牌是联想且价格大于10000的名称
```mysql
	select title from t_item where title like '%联想%' and price>10000;
```
23. 查询联想或戴尔的电脑名称列表
```mysql
	select title from t_item where title like '%联想%' or title like '%戴尔%';
```
24. 查询emp表中员工的编号，姓名，职位，工资，并且工资在1000~2000之间。
```mysql
	select empno,ename,job,sal from emp where sal between 1000 and 2000;
```
25. 查询emp表中员工在10号部门，并且含有上级领导的员工的姓名，职位，上级领导编号以及所属部门的编号
```mysql
	select ename,job,mgr,deptno from emp where deptno=10 and mgr is not null;
```
26. 查询emp表中名字中包含'E'，并且职位不是MANAGER的员工的编号，姓名，职位，以及工资。
```mysql
	select empno,ename,job,sal from emp where ename like '%e%' and job!='manager';
```
27. 查询emp表中10号部门或者20号部门中员工的编号，姓名，所属部门的编号
```mysql
	select empno,ename,deptno from emp where deptno in(10,20);
```
28. 查询emp表中没有奖金或者名字的倒数第2个字母不是T的员工的编号，姓名，职位以及奖金
```mysql
	select empno,ename,job,comm from emp where comm=0 or ename not like '%t_';
```
29. 查询工资高于3000或者部门编号是30的员工的姓名，职位，工资，入职时间以及所属部门的编号
```mysql
	select ename,job,sal,hiredate,deptno from emp where sal>3000 or deptno=30;
```
30. 查询不是30号部门的员工的所有信息
```mysql
	select * from emp where deptno!=30;
```
31. 查询奖金不为空的员工的所有信息
```mysql
	select * from emp where comm is not null;
```
32. 查询emp表中所有员工的编号，姓名，职位，根据员工的编号进行降序排列
```mysql
	select empno,ename,job from emp order by empno desc;
```
33. 查询emp表中部门编号是10号或者30号中，所有员工姓名，职务，工资，根据工资进行升序排列
```mysql
	select ename,job,sal from emp where deptno in(10,30) order by sal;
```
34. 查询emp表中所有的数据，然后根据部门的编号进行升序排列，如果部门编号一致，根据员工的编号进行降序排列
```mysql
	select * from emp order by deptno,empno desc;
```
35. 查询emp表中工资高于1000或者没有上级领导的员工的编号，姓名，工资，所属部门的编号，以及上级领导的编号，根据部门编号进行降序排列，如果部门编号一致根据工资进行升序排列。
```mysql
	select empno,ename,sal,deptno,mgr from emp where sal>1000 or mgr isnull order by deptno desc,sal;
```
36. 查询emp表中名字中不包含S的员工的编号，姓名，工资，奖金，根据工资进行升序排列，如果工资一致，根据编号进行降序排列
```mysql
	select empno,ename,sal,comm from emp where ename not like '%s%' order by sal,empno desc;
```
37. 统计emp表中员工的总数量
```mysql
	select count(*) from emp;
```
38. 统计emp表中获得奖金的员工的数量
```mysql
	select count(*) from emp where comm>0;
```
39. 求出emp表中所有的工资累加之和
```mysql
	select sum(sal) from emp;
```
40. 求出emp表中所有的奖金累加之和
```mysql
	select sum(comm) from emp;
```
41. 求出emp表中员工的平均工资
```mysql
	select avg(sal) from emp;
```
42. 求出emp表中员工的平均奖金
```mysql
	select avg(comm) from emp;
```
43. 求出emp表中员工的最高工资
```mysql
	select max(sal) from emp;
```
44. 求出emp表中员工编号的最大值
```mysql
	select max(empno) from emp;
```
45. 查询emp表中员工的最低工资。
```mysql
	select min(sal) from emp;
```
46. 查询emp表中员工的人数，工资的总和，平均工资，奖金的最大值，奖金的最小值,并且对返回的列起别名。
```mysql
	select count(*) 人数,sum(sal) 工资总和,avg(sal) 平均工资, max(comm) 奖最大, min(comm) 奖金最小 from emp;
```

### 关联
1. 每个部门的人数，根据人数降序排序
```mysql
	select deptno,count(*) c from emp group by deptno order by c desc;
```
2. 每个部门中，每个主管的手下人数
```mysql
	select deptno,mgr,count(*) from emp where mgr is not null group by deptno,mgr;
```
3. 每种工作的平均工资
```mysql
	select job,avg(sal) from emp group by job;
```
4. 每年的入职人数
```mysql
	select extract(year from hiredate) y,count(*) from emp group by y;
```
5. 拿最低工资的员工信息
```mysql
	select * from emp where sal=(select min(sal) from emp);
```
6. 少于等于3个人的部门信息
```mysql
	不考虑40号部门	
	select deptno from emp group by deptno having count(*)<=3;
	select * from dept where deptno in(上面一坨);
	考虑40号部门
	select d.*
	from emp e right join dept d
	on e.deptno=d.deptno
	group by d.deptno
	having count(e.ename)<=3;  
```
7. 只有一个下属的主管信息
```mysql
	查询只有一个下属的主管编号
	select mgr from emp where mgr is not null group by mgr having count(*)=1;
	通过主管编号查询主管信息
	select * from emp where empno in(select mgr from emp where mgr is not null group by mgr having count(*)=1);
```
8. 每月发工资最多的部门信息
```mysql
	得到最高的工资数
	select sum(sal) s from emp group by deptno order by s desc limit 0,1;
	通过最高工资获取部门编号
	select deptno from emp group by deptno having sum(sal)=(select sum(sal) s from emp group by deptno order by s desc limit 0,1);
	通过部门编号得到部门信息
	select * from dept where deptno in(上面一坨);
```
9. 下属最多的人，查询其个人信息
```mysql
	- 得到最多的人数
		select count(*) from emp group by mgr order by count(*) desc limit 0,1;
	- 通过人数获取主管编号
		select mgr from emp group by mgr having count(*)=(select count(*) from emp group by mgr order by count(*) desc limit 0,1);
	- 通过主管编号得到员工信息
		select * from emp where empno in(上面一坨);
```
10. 拿最高工资员工的同事信息
```mysql
	- 查询最高工资
		select max(sal) from emp;
	- 查询最高工资员工的部门编号
		select deptno from emp where sal=(select max(sal) from emp);
	- 通过部门编号查询员工信息
		select * from emp where deptno=(select deptno from emp where sal=(select max(sal) from emp)) and sal!=(select max(sal) from emp);
```
11. 和最后入职的员工在同一部门的员工信息 实现流程和第十题一样
```mysql
	- 得到时间最大值
	select max(hiredate) from emp;
	- 获取最后入职员工的部门编号
	select deptno from emp where hiredate=(select max(hiredate) from emp);
	- 通过部门编号获取员工信息
	select * from emp where deptno=(select deptno from emp where hiredate=(select max(hiredate) from emp)) and hiredate!=(select max(hiredate) from emp);
```
12. 查询平均工资高于20号平均工资的部门信息
```mysql
	-20号部门的平均工资
	select avg(sal) from emp where deptno=20;
	- 查询高于20号部门平均工资的部门编号
	select deptno from emp group by deptno having avg(sal)>(select avg(sal) from emp where deptno=20);
	- 通过部门编号查询部门信息
	select * from dept where deptno in(select deptno from emp group by deptno having avg(sal)>(select avg(sal) from emp where deptno=20));
```
13. 查询员工信息和员工对应的部门名称
```mysql
	select e.*,d.dname
	from emp e join dept d
	on e.deptno=d.deptno;
```
14. 查询员工信息，部门名称，所在城市
```mysql
	select e.*,d.dname,d.loc
	from emp e join dept d
	on e.deptno=d.deptno;
```
15. 查询Dallas市所有的员工信息
```mysql
	select e.*
	from emp e join dept d
	on e.deptno=d.deptno
	where d.loc='dallas';
```
16. 计算每个城市的员工数量
```mysql
	select d.loc,count(e.ename)
	from emp e right join dept d
	on e.deptno=d.deptno group by d.loc;
```
17. 查询员工信息和他的主管姓名
```mysql
	- 自关联查询时把一张表当成两张表 
	select e.ename,m.ename
	from emp e join emp m
	on e.mgr=m.empno;
```
18. 员工信息，员工主管名字，部门名
```mysql
	select e.*,m.ename,d.dname
	from emp e join emp m
	on e.mgr=m.empno
	join dept d
	on e.deptno=d.deptno;	
```
19. 员工名和他所在部门名
```mysql
	select e.ename,d.dname
	from emp e join dept d
	on e.deptno=d.deptno;
```
20. 查询emp表中所有员工的姓名以及该员工上级领导的编号，姓名，职位，工资
```mysql
	select e.ename,m.empno,m.ename,m.job,m.sal
	from emp e left join emp m
	on e.mgr=m.empno;
```
21. 查询emp表中名字中没有字母'K'的所有员工的编号，姓名，职位以及所在部门的编号，名称，地址
```mysql
	select e.empno,e.ename,e.job,d.*
	from emp e join dept d
	on e.deptno=d.deptno
	where e.ename not like '%k%';
```
22. 查询dept表中所有的部门的所有的信息，以及与之关联的emp表中员工的编号，姓名，职位，工资
```mysql
	select d.*,e.empno,e.ename,e.job,e.sal
	from emp e right join dept d
	on e.deptno=d.deptno;
```

