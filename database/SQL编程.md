#### 变量
- 定义

set或select直接赋值
```
set @a=1;
```
declare声明变量，只能在存储过程中使用
```
declare b int default 1;
```
系统变量有两个@@，如
```
select @@global.sort_buffer_size
```
#### 逻辑
#### if
1. IF(expr1,expr2,expr3)

如果 expr1 是TRUE，则 IF()的返回值为expr2; 否则返回值则为 expr3
```
select *,if(sva=1,"男","女") as ssva from taname where sva != ""
```
2. IFNULL(expr1,expr2)

expr1 不为 NULL，则返回 expr1; 否则返回expr2

3. if-elseif-else
```
IF search_condition THEN 
    statement_list  
ELSEIF search_condition THEN
    statement_list ...  
ELSE 
    statement_list
END IF
```

4. case
```
CASE expression
WHEN value1 THEN returnvalue1
WHEN value2 THEN returnvalue2
WHEN value3 THEN returnvalue3
……
ELSE defaultreturnvalue
END
```

#### 

SQL_MODE设置对数据的检查

分为global.sql_mode和session.sql_mode

```
set global sql_mode='strict_trans_tables'
```
- strict_trans_tables 对支持事务的表启用严格模式
- 

时间类型数据
datetime，时间加日期，'xxxx-xx-xx xx:xx:xx'

date，日期，'xxxx-xx-xx'


timestamp，显示形式与datetime一致，不过存储的是相对时间的毫秒数，类似于python的time.time()
- timestamp类型可以设置一个默认值
- timestamp会在更新表时更新为当前时间

都不会保存毫秒数据

now和current_timestamp返回开始执行sql语句时的时间

sysdate返回执行到当前sql语句的时间



date_add(date,interval expr unit)，加，expr可正可负
```
date_add(now(),interval 1 day)//第二天的当前时刻
```

date_sub(date,interval expr unit)，减



cast将字符串转化成目标类型
cast(字符串 as 类型)