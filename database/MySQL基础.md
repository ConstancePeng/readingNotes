#### 数据库情况
1. show databases；显示数据库列表
2. use 数据库名;选择数据库
3. show tables;显示数据库表列表
4. show columns from table;显示表的各列(字段)详情

#### 数据表
1. 创建
```
create table 表名(表头 数据类型 特征，...，主键) engine
//特征指是否可以为null，是否自增，默认值defalut
//必须设置主键
```
2. 更改表结构
```
alter table 表名 add/drop/ 表头/foreign key(表头)/constraint/reference(表头)

//添加/删除 表头/外键/约束/外键(reference)
```
3. 删除表
```
drop table 表名 
```
4. 重命名
```
rename table 表名2 to 表名1
```
#### 插入数据
```
insert into table values(colums)
```
插入完整数据时
- 数据顺序必须和定义表格时表头的顺序对应一致
- 每列必须给一个值，没有值则用null代替
以上方式容易造成数据位置错位 ，安全的方法是在表格后用括号对应写上表头
```
insert into table(name,salary) values('ppp',100)
```
插入多行是可以写多个values，用逗号隔开

插入的数据可以来源于select查询的结果
#### 更新数据
```
update table set salary=100 where num>10;

update 表名 set 表头=值，表头=值 where 条件
```
注意使用where限定
#### 删除数据
```
delete from table where num>10;

delete from 表名 where 条件
//删除一整行数据
```
注意使用where限定


#### 数据查询 
##### 基本结构
select colum from table;(多个,隔开，*代表所有表头)
##### 条件筛选
1. 条件

基本语句：select num,salary from work

where 条件

条件有如下分类
- 大小比较:> <= = != <>
```
where num>50;
where salary <> 100;
where salary is NULL;//判断是否为空值
```
- 在列表内:in 
```
where num in [3,5,7,8]
```
- 在区间内between and
```
where num between 3 and 8
```
- 多个条件与/或 and or 
```
where num>10 and salary<>100;
//and的优先级高于or，如where num>10 or salary<>100 and num<10;
//计算顺序是 salary<>100 and num<10 或者 num>10
```
- like模糊查找，配合%
```
where num like "1%"//1开头的
//%表示任何字符出现任意次数
//_表示任何字符出现1次
```
- regexp正则表达式查找
```
where num regexp ".1"//
```
- 否not
```
where num like ""
```
2. 分组

- group by 列
```
select num,salary from work group by num
```
按照指定的列分组显示，即相同列的数据连续排列，多个列用逗号隔开，优先第一个列连续显示，第二个列再连续显示
- having

where 只能对原始数据进行条件筛选，对聚集数据(使用如sum，count，max处理过i的函数)进行筛选使用having
```JavaScript
select num,count(salary) as allsalary from work group by num having count(salary)>100;
```

3. 排序

order by 按照指定列的结果排序，默认升序，加desc反序

子查询，多级select嵌套
4. 限制个数
limit 开始位置,数量

表示只显示结果的开始位置后指定数量数据
##### 子查询
即嵌套查询，selec查询的结果可以:
1. 作为再次查询的过滤条件
```
select num,salary  from work where salary in (select salary from anotherTable);
```
2. 作为另外一个select的一个显示列数据
```
select num, (select salary where anotherTable) as allsalary from work ;
```
3.作为其他查询的数据源
##### 联结
联结是指查询数据来自不同的表，且不同的表之间有相同列关联

当两个表联结时，实际是左边表的每一行数据和右边的表的每一行数据比对是否相等，所有复杂度是O(n2)
1. inner join
只选择两表相等的
```
select n.bookname,p.price from name as n inner join price as p on n.id=p.id;
//使用on关联相同数据
select n.bookname,p.price from name as n,price as p where n.id=p.id;
//也可以使用where来做条件判断关联
```
2. left join
左边表全部和右边表相等的
3. right join
右边表全部和左边表相等的

##### 组合查询
合并多个查询结果，必须显示相同的列(顺序可以不一样)，使用union，union默认去重，不去重使用all
```
select num,salary from work where num>10 union all select num,salary from work where salary<100;
```
使用union时只能使用一个order by排序
```
select num,salary from work where num>10 union all select num,salary from work where salary<100 order by num;
```
#### 数据处理
1. 内置函数
- count 计数，有数据就会记，除非是null
- max 最大
- min 最小
- sum 求和
- distinct 去重
- exp rand sqrt 
- avg 平均值
2. 数据清洗

left right middle

left(列，分割位置)列数据分割位置左边的部分
middle(列，开始位置，结束位置)
substr和middle一样

locate(字符串，列)，字符串在列的数据中出现的位置

if(判断条件，true的显示,false的显示)

3. 拼接数据
concat(列，字符，列...)
```
select concat(num+'*'+salary) from work//最后返回的是12*15这样的表示方式
```
#### 时间
时间比较要使用date(时间) 然后字符串'年-月-日'比较

- now 返回当前时间
- week(time)返回时间的
- month(time)返回时间的月份
###### 时间运算：
- 加:date_add
- 求差：
    - datediff(date1,date2)
    - timediff(time1,time2)

#### 全局搜索
在创建表时通过fulltext指定可被全局搜索的索引，然后在查询时通过match和against来搜索，match指定搜索的列(必须被fulltext指定)，against指定搜索表达式
```
create table book (
    id int not null auto_increment,
    name char(10) not null,
    describe text null,
    primary key(id),
    fulltext(describe)//指定decsribe为全局搜素，会创建索引
) engine=MyISAM

select describe from book where match(describe) against('zheshiyaidaoguang')
```
全局搜索拓展功能
- 查询拓展，会做两次查询
    - 第一次根据against中的文本查询到相关数据
    - 第二次查询会依据前一次的结果，放宽关键词，即会查询出现在前已查询结果中的文本
```
select describe from book where match(describe) against('zheshiyaidaoguang' with query expansion)

//在没有加with query expansion之前结果为

zheshiyaidaoguang eqw weqweqw

加了之后结果为
zheshiyaidaoguang eqw weqweqw

weqweqw dddddd

//会针对正常的全局搜素再进行一次查询，查询的内容是出现在正常里出现的其他字符
```

- 布尔搜素，即在搜索时会屏蔽指定值
```
select describe from book where match(describe) against('zheshi -rope*' in boolean mode)
//其中-repo* 即表示不包含repo开头的词，最终查询结果是包含zheshi，不包含含repo开头的词的行
```
上例中in boolean mode开启布尔搜素。
-，*都是布尔操作符，用于过滤全局搜索的结果，还有其他的：
操作符 | 作用
---|---
+| 包含
- | 不包含
() | 组成子表达式
* | 词尾通配符
~ | 取消排序
等

#### 视图
视图其实相当于将select查询语句作为一张表来操作，视图实际不保存数据，只是数据的查询语句

create view viewname as ....(查询语句) with check-option(限制对视图的更新要满足视图中where语句)

show create view viewname

drop view viewname

create or replace view

#### 存储过程
类似于函数，可以将一系列sql语句封装成一个存储过程
1. 创建存储过程

使用create procedure 创建，格式如下
```
create procedure 过程名称(
    out 参数名 类型//比如 out p  decimal(8,2)
    //in 传入，out 传出，into 传入传出
    //类型就是mysql支持的数据类型
)
begin
    sql语句 into 参数名；//比如select max(num) into p from work 
    //into 用来指定参数赋值
end；

获取返回
select @变量名；//如 select @p
```
2. 使用存储过程

借用call调用，一般格式为
```
call 存储过程名 (@参数名...)
```
3. 删除存储过程

使用drop，一般格式为
```
drop procedure 存储过程名
```
3. 检查存储过程

使用show，一般格式为
```
show create procedure 过程名称;
```

#### 游标
1. 创建
用declare语句创建，declare命名游标，并定义相应的select语句。游标只能用于存储过程中
```
create procedure ptj()
begin
    declare numorder cursor//declare 还可以声明变量，declare 变量名 数据类型
    for 
    select num from work;
end;
```
2. 打开游标
```
open 游标名

open numorder;
```
3. 关闭游标
```
close 游标名

close numorder;
```
4. 使用游标数据
```
fetch next/prior/first/last from 游标名 into 变量名
//用指定的游标读取下一行，并将一行中列数据类型与变量相同的放置到变量中
fetch next from numorder into nums;
```
#### 触发器
触发器是响应以下语句而自动执行的一条MySQL语句
- delete
- update
- insert
- 存储过程 begin与end之间的一组语句

1. 创建
```
create trigger 触发器名 before/after inser/update/delete on 表名 for each row 行为(如 select 'add',显示信息,多个语句的话使用begin 和end 包起来)

定义触发器时
    insert 可以使用名为new的虚拟表，可以访问插入的行
    delete/update  可以使用名为old的虚拟表，可以访问更新/删除的行

create trigger newnum after insert on work for each row 
begin
    select new.num;
    select new.salary;
end;
```
2. 删除
```
drop trigger 触发器名 
```
#### 事务处理
事务处理是用来维护数据库的完整性，保证成批的操作要么完全执行，要么完全不执行。
- 事务指一组sql语句
- 回退指撤销指定sql语句
- 提交指将为存储的sql语句结果写入数据库表
- 保留点指事务中设置的临时占位符
1. 事务开始
start transaction
2. 回退事务
rollback; 回退与start transaction之间的sql操作
```
start transaction
delete from work where num>10;
rollback;//回退与start transaction之间的sql操作
```
3. 提交事务
commit;在与start transaction之间的sql语句执行不出错时，将结果写入数据库表
```
start transaction
delete from work where num>10;
commit;//在与start transaction之间的sql语句
//执行不出错时，将结果写入数据库表
```
4. 保留点
保留点用来占位复杂多sql中的某个状态，便于在后续出现问题时回退或提交，关键词为savepoint 保留点名
```
start transaction
delete from work where num>10;
savepoint d1
delete from work where salary>10;
rollback d1;//回退到d1保留点，即保留d1之前的sql结果
```
#### 权限管理
1. 用户管理
- create user 名字 identified by '密码'
- rename user 名字 to 新名字
- drop user 名字
2. 权限
- show grant for 名字
- grant 行为(select/delete/update...) on 库名.表名(库名.* 表示库的所有表) to 用户名 
- revoke 与grant相反，表示撤销权限



