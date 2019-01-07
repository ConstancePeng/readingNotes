#### 数据库情况
1. show databases；显示数据库列表
2. use 数据库名;选择数据库
3. show tables;显示数据库表列表
4. show columns from table;显示表的各列(字段)详情

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