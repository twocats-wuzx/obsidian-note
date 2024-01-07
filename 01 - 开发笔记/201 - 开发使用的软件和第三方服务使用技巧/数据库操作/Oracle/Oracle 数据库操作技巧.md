
# DB系统相关  
<br/>  

## 误删数据处理
<br/>

### Delete 误删除操作恢复

#### 1. ==闪回==方式找回数据
>适用于`delete`删除的数据, <mark class="hltr-red">注意`truncate`方式删除的数据不能恢复</mark>
```sql
-- 首先查询历史记录找到执行SQL的时间节点
select * from 表名 as of timestamp to_timestamp('删除时间点','yyyy-mm-dd hh24:mi:ss')

select SQL_TEXT, SQL_FULLTEXT, SQL_ID, FIRST_LOAD_TIME, LAST_LOAD_TIME, LAST_ACTIVE_TIME from v$sql where sql_text like '%T_USER_M%';

-- 通过那个时间节点找到未被删除的数据的数据(可以使用筛选条件), 插入零食表中进行处理然后在恢复数据
insert into 表名 (select * from 表名 as of timestamp to_timestamp('删除时间点','yyyy-mm-dd hh24:mi:ss'));

insert into T_USER_M (select * from T_USER_M as of timestamp to_timestamp('2023-04-26 14:52:16','yyyy-mm-dd hh24:mi:ss') t1 where t1.USERNAME like '5301%';

```
<br/>

#### 2. ==Drop==方式删除的数据找回. 

查询这个“回收站”或者查询user_table视图来查找已被删除的表
```sql
-- 查询user_tables
select table_name,dropped from user_tables;

-- 查询回收站
select object_name,original_name,type,droptime from user_recyclebin；

-- 记得住表名可以用以下语句直接恢复
flashback table 原表名 to before drop；
-- 记不住表名可以用回收站的名字恢复
flashback table "回收站中的表名(如：Bin$DSbdfd4rdfdfdfegdfsf==$0)" to before drop rename to 新表名
```
<br/>

#### 3. 通过闪回方式是数据库回到过去某一个状态
```sql
alter database flashback on 
flashback database to scn SCNNO;
flashback database to timestamp to_timestamp('2007-2-12 12:00:00','yyyy-mm-dd hh24:mi:ss');
```


# 增删改查技巧
<br/>

## 1. 新增数据

###  1.1 批量将查询到的数据插入某一个表
```sql
insert into table_name_1 (select * from table_name_2 where xxx);
```

### 1.2 批量插入数据（可以是不同的表）

```sql
insert all
	into table_name_1 (name) values ('张三')
	into table_name_2 (male) values ('男')
select 1 from dual;
```

<br/>

## 2.删除数据

<br/>

## 3.更新数据
### 3.1 联表更新数据
```SQL
-- 第一种两张表的关联字段都必须是唯一健
update(  
    select t1.NICK_NAME A1,  
           t1.AGE A2,
           t2.NICK_NAME B1,  
           t2.AGE B2
    from T_ACCOUNT t1, T_USER t2  
    where t1.ACCOUNT_NAME = t2.USER_NAME   
and T1.DR = 0   
set A1=B1,A2=B2 where 1 = 1;

-- merge into 进行关联更新
merge into T_ACCOUNT t1
	using (select USER_NAME, NICK_NAME, AGE from T_USER) t2
	on (t1.ACCOUNT_NAME = t2.USER_NAME)
when matched
	then update 
		set t1.NICK_NAME = t2.NICK_NAME, t1.AGE = t2.AGE;
```

<br/>

## 4.查询数据