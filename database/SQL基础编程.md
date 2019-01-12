#### 变量
- 定义

declare声明变量，只能在存储过程中使用
```
declare 变量名 类型 (default 值)

declare b int default 1;
```
set或select直接赋值
```
set a=1;//存储过程内设值
set @a=1;//存储过程外设值获取值都要加@
```
系统变量有两个@@，如
```
select @@global.sort_buffer_size
```
将select查询结果赋值给变量，但是select只能有一条数据返回，否则会报错
```
SELECT expression1 [, expression2 ....] INTO variable1 [, variable2 ...] 
```
将游标的结果赋值给变量，但是每次只能给一数据，多数据用循环处理
```
OPEN cursor1; 
FETCH cursor1 INTO l_customer_name,....;
close cursor1

DECLARE CONTINUE HANDLER FOR NOT FOUND SET l_last_row_fetched=1;//处理游标到最后
```
- 注释
```
/*
| ...
| ...
| ...
*/
```
- 操作符
+/-/*///,加减乘除
div/% ，整除/取余
- 
#### 逻辑
##### 块
```
[label:]begin
    variable and condition declarations
    cursor declarations
    handler declarations
    
    program code

end [label]
```
块相当于一个作用域，内部的变量不能被外部使用，但是嵌套的块，里面的可以访外面的，但是外面不能访问里面的，里面和外面的变量相互独立，可以取相同名字
##### 条件控制
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

##### 循环
1. loop
```
[label:] loop
    ...
end loop
[label];
```
loop之间的代码会一直循环，可以使用leave来终止，类似于break
```
level label;//终止label循环
```
```
SET i=1; 
myloop: LOOP 
    SET i=i+1; 
    IF i=10 then 
        LEAVE myloop;
    END IF; 
END LOOP myloop; 
SELECT 'I can count to 10';
```
2. iterate
```
iterate label;//终止label当前循环，跳到开始再执行下一个循环，类似于continue
```
3. repeat...until
```
[label:] repeat
    ...
until expression
end repeat [label];
```
4. while
```
[label:] while expression do
    ...
end while [label];
```


#### 存储过程
##### 基本结构
```
delimiter $$//定义$$作为存储过程结束的标志，mysql默认是;
drop procedure if exist 存储过程名$$ //如果存储过程存在就删除
create procedure 存储过程名(
    输出类型 数据类型 变量//in/out/inout int a
)
begin
    ....
end$$
```
##### 获取结果
1. 将单个记录数据存入本地变量，使用select...into
```
delimiter $$//定义$$作为存储过程结束的标志，mysql默认是;
drop procedure if exist total$$ //如果存储过程存在就删除
create procedure total(
    in_id int
)
begin
    declare total_sales numeric(8,2)
    select sum(sale) into total_sales//将记录存入本地变量
    from sales
    where id=in_id;
    slecet concat('total sales for ',in_id,'is',total_sales)//拼接字符串
end$$
```
2. 使用游标获取多条记录然后使用循环来取值
3. select/show返回存储过程查询结果



#### 存储函数
##### 基本结构
```
delimiter $$//定义$$作为存储过程结束的标志，mysql默认是;
drop function if exist 函数名$$ //如果存储过程存在就删除
create function 函数名(
    输出类型 数据类型 变量//输出类型只有in
)
begin
    ....
end$$
```

#### 预处理
##### 创建
```
PREPARE statement_name FROM sql_text
```
##### 调用
```
EXECUTE statement_name [USING variable [,variable...]]
```
##### 删除
```
DEALLOCATE PREPARE statement_name
```

#### 错误处理
```
DECLARE {CONTINUE | EXIT} HANDLER FOR {SQLSTATE sqlstate_code| MySQL error code| condition_name} stored_program_statement
```
