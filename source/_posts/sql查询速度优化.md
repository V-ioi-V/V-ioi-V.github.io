---
title: sql查询速度优化
tags: 数据库
category: CS大学生必备
date: 2020/03/20 21:16
---

<font size=4>

### 1.在表中建立索引，优先考虑where、 group by使用到的字段

### 2.查询条件中，一定不要使用*

因为会返回过多无用的字段会降低查询效率。应该使用具体的字段代替\*，只返回使用到的字段。

### 3.不要在where条件中使用左右两边都是%的like模糊查询，如：

SELECT * FROM t_order WHERE customer LIKE '%zhang%'

这样会导致数据库引擎放弃索引进行全表扫描。

优化：尽量在字段后面使用模糊查询。如下：

SELECT * FROM t_order WHERE customer LIKE 'zhang%'

### 4.尽量不要使用in 和not in，会造成全表扫描

如下：

SELECT * FROM t_order WHERE id IN (2,3)

SELECT * FROM t_order1 WHERE customer IN (SELECT customer FROM t_order2)

优化：

**对于连续的数值，能用 between 就不要用 in** ，如下：SELECT * FROM t_order WHERE id BETWEEN 2 AND 3

**对于子查询，可以用exists代替**。如下：SELECT * FROM t_order1 WHERE EXISTS (SELECT * FROM t_order2 WHERE t1.customer = t2.customer)

### 5.应尽量避免在 where 子句中使用 or 来连接条件

如果一个字段有索引，一个字段没有索引，将导致引擎放弃使用索引而进行全表扫描，尽量用union或者union all代替(在确认没有重复数据或者不用剔除重复数据时，union all会更好)；如下：

SELECT * FROM t_order WHERE id = 1 OR num = 3

优化：可以用**union或者union all**代替or。如下：

**(union all 不会去重)**

SELECT * FROM t_order WHERE id = 1

UNION

SELECT * FROM t_order WHERE num = 3

### 6.尽量不要在 where 子句中对字段进行表达式操作

这样也会造成全表扫描。如：

select id FROM t_order where num/2=100

应改为:

select id FROM t_order where num=100*2

### 7.where条件里尽量不要进行null值的判断

null的判断也会造成全表扫描。如下：

SELECT * FROM t_order WHERE score IS NULL

优化：

给字段添加默认值，对默认值进行判断。如：

SELECT * FROM t_order WHERE score = 0

### 8.尽量不要在where条件中等号的左侧进行表达式或函数操作

否则系统将可能无法正确使用索引,会导致全表扫描。如下：

SELECT * FROM t_order2 WHERE score/10 = 10

SELECT * FROM t_order2 WHERE SUBSTR(customer,1,5) = 'zhang'

优化：

将表达式.函数操作移动到等号右侧。如下：

SELECT * FROM t_order2 WHERE score = 10*10

SELECT * FROM t_order2 WHERE customer LIKE 'zhang%'

### 9.尽量不要使用where 1=1的条件

有时候，在开发过程中，为了方便拼装查询条件，我们会加上该条件，这样，会造成进行全表扫描。如下：

SELECT * FROM t_order WHERE 1=1

优化：

如果用代码拼装sql，则由代码进行判断，没where加where，有where加and

如果用mybatis，请用mybatis的where语法。

###10.应尽量避免在 where 子句中使用 != 或 <> 操作符，否则将引擎放弃使用索引而进行全表扫描

### 11.选择最有效率的表名顺序

数据库的解析器按照**从右到左的顺序处理FROM子句中的表名，FROM子句中写在最后的表将被最先处理**

在FROM子句中包含多个表的情况下：

- **如果三个表是完全无关系的话，将记录和列名最少的表，写在最后，然后依次类推**
- **也就是说：选择记录条数最少的表放在最后**

如果有3个以上的表连接查询：

- **如果三个表是有关系的话，将引用最多的表，放在最后，然后依次类推**。
- **也就是说：被其他表所引用的表放在最后**

例如：查询员工的编号，姓名，工资，工资等级，部门名

**emp表被引用得最多，记录数也是最多，因此放在form字句的最后面**

```
select emp.empno,emp.ename,emp.sal,salgrade.grade,dept.dname
from salgrade,dept,emp
where (emp.deptno = dept.deptno) and (emp.sal between salgrade.losal and salgrade.hisal)        
```

### 12.WHERE子句中的连接顺序

数据库采用自**右而左的顺序解析WHERE子句**,根据这个原理,**表之间的连接必须写在其他WHERE条件之左**,那些**可以过滤掉最大数量记录的条件必须写在WHERE子句的之右**。

**emp.sal可以过滤多条记录，写在WHERE字句的最右边**

```sql
select emp.empno,emp.ename,emp.sal,dept.dname
from dept,emp
where (emp.deptno = dept.deptno) and (emp.sal > 1500) 
```

### 13.尽可能的使用 varchar/nvarchar 代替 char/nchar 

因为首先变长字段存储空间小，可以节省存储空间，其次对于查询来说，在一个相对较小的字段内搜索效率显然要高些；

### 14.程序要尽量避免大事务操作，提高系统并发能力。

### 15.索引并不是越多越好

索引固然可以提高相应的 select 的效率，但同时也降低了 insert 及 update 的效率，因为 insert 或 update 时有可能会重建索引，所以怎样建索引需要慎重考虑，视具体情况而定。一个表的索引数最好不要超过6个，若太多则应考虑一些不常使用到的列上建的索引是否有 必要；

### 16.尽量避免使用子查询，使用join

- 子查询效率太差，执行子查询时，MYSQL需要创建临时表，查询完毕后再删除这些临时表，所以，子查询的速度会受到一定的影响，这里多了一个创建和销毁临时表的过程。

- 如果是JOIN的话，它是走嵌套查询的。小表驱动大表，且通过索引字段进行关联。如果表记录比较少的话，还是OK的。大的话业务逻辑中可以控制处理。

### 17.使用索引字段作为条件时

在使用索引字段作为条件时，如果该索引是复合索引，那么必须使用到该索引中的第一个字段作为条件时才能保证系统使用该索引，否则该索引将不会被使用，并且应尽可能的让字段顺序与索引顺序相一致。

---



参考:https://baijiahao.baidu.com/s?id=1628437659593476871&wfr=spider&for=pc

