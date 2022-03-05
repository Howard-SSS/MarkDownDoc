[toc]



不区分大小写

# 三个范式

**第一范式**

每一列属性不可再分，且为基本数据类型

**第二范式**

非主键完全依赖主键，不能只依赖主键一部分

**第三范式**

非主键不能传递依赖主键

# 通用

```mysql
user DATABASE_NAME
-- 注释内容
/*
  注释内容
*/
show global variables // 查看全局变量
show session variables // 查看会话变量
show databases // 查看所有数据库
show tables // 查看当前库所有表
show databases like '%test%' // 查看数据库名包含test的数据库
```

# 类型

**数值类型**

tinyint(1)、smallint(2)、mediumint(3)、int(4)、bigint(8)、float(4)、double(8)、decimal

**日期/时间**

year(1)、time(3)、date(3)、datetime(8)、timestamp(4)

**字符串类型**

char、varchar、binary、varbinary、blob、text、enum、set

**二进制类型**

bit、binary、varbinary、tinyblob、blob、mediumeblob、longblob

# 引擎

**InnoDB**

支持事务安装、灾难恢复、使用行级锁、缓冲处理、支持外键；主要用于很多的更新、删除，并发要求数据一致性

**MyISAM**

不支持事务和外键，访问速度块；主要用于读取和写入，对事务的完整性、并发性不高的场景

**MEMORY**

存储于内存

# 数据库

```java
创建数据库
create database [if not exists] DATABASE_NAME 
[default character set CHARACTER_SET_NAME |
default collate COLLATE_NAME]
```

```java
删除数据库
drop database [if exists] DATABASE_NAME
```

# 表

[SQLBolt - 学习 SQL - SQL 课程 X：无穷大及更远！](https://sqlbolt.com/lesson/end)

```mysql
#创建表
create table TABLE_NAME (
	FIELD_NAME, TYPE
)
#删除表
drop table [if exists] TABLE_NAME
```

## 展现表

```mysql
#以表格形式展现表信息
desc[ribe] TABLE_NAME
#以sql语句形式展示表信息
show create table TABLE_NAME
```

## 修改表

```mysql
#修改表引擎
alter table TABLE_NAME engine = ENGINE
#修改表结构
alter table TABLE_NAME {
    add column FIELD_NAME TYPE
	| change column OLD_FIELD_NAME new_FIELD_NAME TYPE
	| alter column FIELD_NAME {set default DEFAULT_VALUE | drop default}
	| modify column FIELD_NAME TYPE |
	| drop column FIELD_NAME |
	| rename to NEW_TABLE_NAME |
	| character set CHARACTER_SET_NAME |
	| collate COLLATE_NAME
}
#删除
drop table [if exists] TABLE_NAME
```

## 插入

```mysql
#不完整插入需要指定对应字段
insert into TABLE_NAME (FIELD_NAME, FIELD_NAME) values (VALUE, VALUE)
insert into TABLE_NAME set FIELD_NAME = VALUE
#完整插入可以忽略字段
insert into TABLE_NAME values (VALUE, VALUE)
```

## 查询

**去重**

```mysql
select distinct FIELD_NAME from TABLE_NAME
```

**范围**

```mysql
#数值在区间中的行
select * from TABLE_NAME where FIELD_NAME >= NUMBER1 and FIELD_NAME <= NUMBER2
select * from TABLE_NAME where FIELD_NAME between NUMBER1 and NUMBER2
#值在列表中的行
select * from TABLE_NAME where FIELD_NAME in (VALUE1, VALUE2)
```

**类似**

```mysql
#%代替多个字符
select * from TABLE_NAME where FIELD_NAME like '%ok%'
#_代替单个字符
select * from TABLE_NAME where FIELD_NAME like '_ok_'
```

**排序**

```mysql
#以FIELD_NAME排序,跳过NUMBER2行，取出NUMBER1行
select * from TABLE_NAME order by FIELD_NAME asc(升序)/desc(降序) limit NUMBER1 offset NUMBER2
```

**为空**

```mysql
select * from TABLE_NAME where FIELD_NAME is null
```

**分组**

```mysql
#计算每个分组有多少行
select count(*) from TABLE_NAME group by FIELD_NAME 
#计算以名字分组，其中组内包含‘enginner’的工作时间总和
select name, sum(time) from TABLE_NAME group by name having role = 'enginner'
```

**select执行顺序**

```mysql
from 将多个表通过笛卡尔积变成一个表
on 筛选
join 将剩余行添加到表
where 筛选
group by 分组
having 分组结果筛选
select 返回
distinct 去重
order by 排序
limit 限制
```

## 更新

```mysql
update TABLE_NAME set FIELD_NAME = VALUE [, FIELD_NAME = VALUE] [where] [order by] [limit]
```

## 删除

```mysql
#逐行删除
delete from TABLE_NAME [where] [order by] [limit]
#删除表，在重建
truncate table TABLE_NAME
```

# 约束

**主键约束**

不能为空，不能重复，只能有一个主键约束

```mysql
create table TABLE_NAME (
    FIELD_NAME TYPE primary key [, FIELD_NAME TYPE]
)
create table TABLE_NAME (
    FIELD_NAME TYPE [, FIELD_NAME TYPE]
    [constaint CONSTAINT_NAME] primary key (FIELD_NAME)
)
```

**外键约束**

可以为空，不为空则必须等于主表主键某个值，可以有多个外键约束

```mysql
create table TABLE_NAME (
	ROW,
	constaint CONSTAINT_NAME foreign key FIELD_NAME references TABLE_NAME(FIELD_NAME)
)
```

```mysql
alter table TABLE_NAME add constraint CONSTAINT_NAME foreign key FIELD_NAME references TABLE_NAME(FIELD_NAME)
alter table TABLE_NAME drop foreign key CONSTAINT_NAME
```

**唯一约束**

可以为空，但不能重复，可以有多个唯一约束

```mysql
create table TABLE_NAME (
	FIELD_NAME TYPE unique
)
```

```mysql
alter table TABLE_NAME add constraint CONSTRAINT_NAME unique(FIELD_NAME)
alter tale TABLE_NAME drop index CONSTAINT_NAME
```

**检查约束**

```mysql
create table TABLE_NAME (
	ROW,
	check()
)
```

```mysql
alter table TABLE_NAME add constraint CONSTRAINT_NAME check()
alter table TABLE_NAME drop constraint CONSTRAINT_NAME
```

**非空约束**

```mysql
create table TALBE_NAME (
	FIELD_NAME TYPE not null
)
```

**默认值约束**

# 连接

**内连接**

设置连接条件的方式，来移除查询结果中某些数据行的交叉连接

```mysql
select * from TABLE_NAME inner join TABLE_NAME on (FIELD_NAME) 
```

**左外连接**

右表补充进左表，不存在则填充null

```mysql
select * from TABLE_NAME left join TABLE_NAME on (FIELD_NAME)
```

**右外连接**

# 索引

```mysql
create INDEX_NAME on TABLE_NAME (FIELD_NAME [length] [asc | desc])
```

# SQL优化

**索引检索代替全表检索**

1. 避免字段**开头**模糊查询，会导致数据库引擎放弃索引进行全表扫描
2. 避免使用in和not in，between或exists代替
3. 避免使用or，union代替
4. 避免使用null判断，设置默认值
5. 避免where等号左侧使用表达式、函数
6. 123
7. 避免使用<>和!=
8. 条件包含前置索引
9. 避免类型隐式转换
10. 尽量order by条件与where条件一致

**获取尽可能少的列**

1. 避免select *
2. 避免出现不确定结果的函数
3. 多表关联，小表在前，大表在后
4. 使用表别名
5. where替换having
6. 过滤数据多的条件往前放

[SQL优化最干货总结 - MySQL（2020最新版） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/265852739)

# 事务

**事务的特性**

- 原子性

  将事务中所有操作捆绑成一个不可分割的单元

- 一致性

  事务完成时，所有数据都保持一致状态

- 隔离性

  事务的执行不能被其他事务干扰，事务内部操作及使用的数据对并发的其他事务是隔离的

- 持久性

  事务一旦提交，对数据库改变时永久的

**并发问题**

- 脏读

  一个事务读取到另一个事务未提交的数据

- 不可重复读

  事务对同一行数据重复读两次，得到的结果不同

- 幻读

  第二次查询结果包含了第一次查询中未出现的数据

- 丢失更新

  两个事务同时更新一行数据，后提交的将数据覆盖

**隔离级别**

- read uncommitted

  事务写数据，其他事务不允许同时写，但允许读。防止丢失更新

- read committed

  事务可以访问提交插入和修改的数据。事务读数据，允许其他事务继续访问该行数据。防止脏读

- repeatable read

  事务可以访问提交插入的数据。防止不可重复读和脏读

- serializable

  不能并发执行。防止脏读、不可重复读、幻读

```mysql
#开启一个事务
begin 
#事务回滚
rollback
#事务确认
commit
```

