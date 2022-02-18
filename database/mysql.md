不区分大小写

# 通用

```java
user DATABASE_NAME
-- 注释内容
/*
  注释内容
*/
show global variables // 查看全局变量
show session variables // 查看会话变量

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

# 插入

```java
create database [if not exists] DATABASE_NAME // 创建数据库
[default character set CHARACTER_SET_NAME |
default collate COLLATE_NAME]
```

```java
create table TABLE_NAME (ROW[, ROW]) // 创建表
```

```java
insert into TABLE_NAME () values ()
insert into TABLE_NAME set FIELD_NAME = VALUE [, FIELD_NAME = VALUE]
```

# 查询

```java
show databases // 查看所有数据库
show tables // 查看当前库所有表
show databases like '%test%' // 查看数据库名包含test的数据库
```

```java
desc[ribe] TABLE_NAME // 以表格形式展现表信息
show create table TABLE_NAME // 以sql语句形式展示表信息
```

```java
select distinct FIELD_NAME from TABLE_NAME // 去重
```

# 更新

```java
alter table TABLE_NAME engine = ENGINE // 修改表引擎
```

```java
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
```

```java
update TABLE_NAME set FIELD_NAME = VALUE [, FIELD_NAME = VALUE] [where] [order by] [limit]
```

# 删除

```java
drop database [if exists] DATABASE_NAME
```

```
drop table [if exists] TABLE_NAME[, TABLE_NAME]
```

```java
delete from TABLE_NAME [where] [order by] [limit] // 逐行删除
truncate table TABLE_NAME // 删除表，在重建
```

# 约束

**主键约束**

不能为空，不能重复，只能有一个主键约束

```java
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

```java
create table TABLE_NAME (
	ROW,
	constaint CONSTAINT_NAME foreign key FIELD_NAME references TABLE_NAME(FIELD_NAME)
)
```

```java
alter table TABLE_NAME add constraint CONSTAINT_NAME foreign key FIELD_NAME references TABLE_NAME(FIELD_NAME)
alter table TABLE_NAME drop foreign key CONSTAINT_NAME
```

**唯一约束**

可以为空，但不能重复，可以有多个唯一约束

```java
create table TABLE_NAME (
	FIELD_NAME TYPE unique
)
```

```java
alter table TABLE_NAME add constraint CONSTRAINT_NAME unique(FIELD_NAME)
alter tale TABLE_NAME drop index CONSTAINT_NAME
```

**检查约束**

```java
create table TABLE_NAME (
	ROW,
	check()
)
```

```java
alter table TABLE_NAME add constraint CONSTRAINT_NAME check()
alter table TABLE_NAME drop constraint CONSTRAINT_NAME
```

**非空约束**

```java
create table TALBE_NAME (
	FIELD_NAME TYPE not null
)
```

**默认值约束**

# 连接

**内连接**

设置连接条件的方式，来移除查询结果中某些数据行的交叉连接

```java
select * from TABLE_NAME inner join TABLE_NAME on (FIELD_NAME) 
```

**左外连接**

右表补充进左表，不存在则填充null

```java
select * from TABLE_NAME left join TABLE_NAME on (FIELD_NAME)
```

**右外连接**

# 索引

```java
create INDEX_NAME on TABLE_NAME (FIELD_NAME [length] [asc | desc])
```

