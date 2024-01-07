# 数据库系统相关



# 增删改查技巧
<br/>

## 1. 新增数据

### 1.1 批量将查询到的数据插入某一个表
```sql

```

### 1.2 批量插入数据

```sql
INSERT INTO t_user (id, username, password, nickname, age, gender, backup) VALUES
(1, '121982731', 'mfne#wk>jtbfyldkj[doqw', '两只猫', 18, 1, '-' ),
(2, '121982732', 'mfne#wk>jtbiamrfbylenz', '般般', 1, 1, '-' );
```

<br/>

## 2.删除数据
### 2.1 删除重复数据
```sql
SELECT *  
FROM t_user  
WHERE id NOT IN (SELECT MIN(id) FROM t_user GROUP BY user_name);  
  
DELETE  
FROM t_user t1  
WHERE t1.id <>  
      (SELECT t2.max_id FROM (SELECT MAX(t3.id) AS max_id FROM t_user t3 WHERE t1.user_name = t3.user_name) t2 );

-- 要删除的数据量大的时候推荐使用EXISTS
DELETE
FROM t_user t1
WHERE EXISTS 
      (SELECT t2.max_id FROM (SELECT MAX(t3.id) AS max_id FROM t_user t3 WHERE t1.user_name = t3.user_name) t2);
```


<br/>

## 3.更新数据
### 3.1 联表更新数据
```SQL

```

<br/>

## 4.查询数据