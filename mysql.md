net start mysql

net stop mysql

mysql -uroot -p -P3306 -h127.0.0.1

退出    exit;    quit;    \q

## 修改MySQL提示符

* mysql -uroot -p --prompt \h

  * \D    完整的日期

  * \d    当前数据库

  * \h    服务器名称

  * \u    当前用户

* 登陆后;    prompt mysql&gt;

  * prompt \u@\h \d&gt;

![](/assets/360截图20180517233215901.jpg)

## 常用命令以及语法规范

* 关键字与函数名称全部大写

* 数据库名称、表名称、字段名称全部小写

* SQL语句必须以分号结尾

* SELECT VERSION\(\); 显示当前服务器版本

* SELECT NOW\(\); 显示当前日期时间

* SELECT USER\(\); 显示当前用户

* create database \[if not exists\] xxx character set utf8;    创建数据库

* show databases;    查看服务器列表

* show warnings;    查看警告

* show create database t1;    查看数据库编码格式

* alter database t1 character set utf8;    修改数据库的编码方式

* drop database \[if exists\] t1;

## 数据类型

* 整型

  * TINYINT    -128到127\(7位\)    0到256\(8位\)

  * SMALLINT    -32768到32767\(15位\)    0到65535\(16位\)

  * MEDIUMINT    -8388608到8388607\(23位\)    0到16777215\(24位\)

  * INT    -2147483648到2147483647\(31位\)    0到4294967295\(32位\)

  * BIGINT    \(63位\)    \(64位\)

* 浮点型

  * FLOAT    精确到小数位7位

  * DOUBLE

* 日期时间型

  * YEAR    1

  * TIME    3

  * DATE    3

  * DATETIME    8

  * TIMESTAMP    4

* 字符型

![](/assets/360截图20180518004226274.jpg)

## 数据表操作

* use test；    打开数据库

* select database\(\);    查看当前数据库名

* create table \[if not exists\] \(\)    创建数据表

* `show tables [from db_name] [like 'patte' | where expr];`    查看数据表列表

* show columns from tbl\_name;    查看数据表结构

* desc tbl_name;    查看数据表结构

* `insert tbl_name[(col_name,...)] values(val,...) ;`   插入记录

  * 省略字段，则所有字段都需要赋值

* select expr,... from tbl\_name;

```mysql
create table tb1(
username varchar(20),
age tinyint unsigned,
salary float(8,2) unsigned
);
```

## 非空约束

* null；字段值可以为空

* not null；字段值禁止为空

## auto_increment

* 自动编号，且必须与主键组合使用

* 默认情况下，起始值为1，每次的增量为1

* 可以是整型和浮点型；但浮点型小数位数必须是0

## primary key

* 主键约束

* 每张表只能存在一个主键

* 主键保证记录的唯一性

* 主键自动为not null

* auto\_increament 只用于主键，不是必须使用的；且不需要赋值

## unique key

* 唯一约束可以保证记录的唯一性

* 唯一约束的字段可以为空值\(null\)；空值是唯一的

* 每张数据表可以存在多个唯一约束

## default

* 当插入记录时，如果没有明确为字段赋值，则自动赋予默认值。

```
create table tb5(
id smallint unsigned auto_increment primary key,
username varchar(20) not null unique key,
sex enum('1', '2', '3') default '3'
);
```

## 约束

* 保证数据的完整性和一致性

* 约束分为表级约束和列级约束

## foreign key

* 保持数据一致性，完整性

* 实现一对一或一对多关系。

## 外键约束的要求

* 数据表的引擎只能为InnoDB

  * my.ini    default-storage-engine=INNODB

* 外键列和参照列必须具有相似的数据类型

  * 数字长度和符号位必须相同

  * 字符的长度则可以不同

* 有外键的表为子表，子表所按照的表为父表

* 外键列和参照列必须创建索引。如果外键列不存在索引的话，mysql将自动创建索引。

show create table provinces;    查看数据表的存储引擎

show indexes from users\G;    查看数据表的索引

delete from provinces where id = 3;

```
create table users(
id smallint unsigned primary key auto_increment,
username varchar(20) not null,
pid smallint unsigned,
foreign key (pid) references provinces (id)
);
```

## 外键约束的参照操作

* cascade: 从父表删除或更新且自动删除或更新子表中匹配的行

* set null: 从父表删除或更新行，并设置子表中的外键为null。如果使用该选项，必须保证子表列没有指定not null。

* restrict： 拒绝对父表的删除或更新操作

* no action: 标准sql的关键字，在mysql中与restrict相同

## 表级约束与列级约束

* 对一个数据列建立的约束，成为列级约束

* 对多个数据列建立的约束，成为表级约束

* 列级约束既可以在列定义时声明，也可以在列定义后声明

* 表级约束只能在列定义后声明

* not null default不存在表级约束

## 修改数据表

* `alter table tbl_name add col_name col_definition [first | after col_name];`    添加单列

* alter table tbl\_name add \(\);    添加多列，不能指定位置关系

* `alter table tbl_name drop col_name;`    删除列

* `alter table tbl_name drop a, drop b;`    删除多列，也可以在后边继续添加

* `alter table tbl_name add primary key [index_type] col_name;`    添加主键约束

* `alter table tbl_name add unique [index | key] [index_type] (col_name, ...);`    添加唯一约束

* `alter table tbl_name add foreign key (col_name, ...) references tabl_name (col_name);`    添加外键约束

* alter table tbl\_name alter col\_name {set default value \| drop default};    添加/删除默认约束

- alter table tbl_name drop primary key;  删除主键约束

- `alter table tbl_name drop {index | key} index_name;`  删除唯一约束

- `alter table tbl_bame drop foreign key fk_symble;`  删除外键约束

- `alter table tbl_name modify col_name col_definition [first | after col_name];`  修改列定义；大类型改到小类型有可能改造成数据的而丢失

- `alter table tbl_name change old_col_name new_col_name column_definition [first | after col_name];`  修改列名和列定义

- 数据表更名

  - `alter table tbl_name rename new_tbl_name;`
  
  - rename table tbl_name to new_tbl_name [, tbl_name to new_tbl_name, ...];  

![](/assets/360截图20180521004739614.jpg)





##insert value

- `insert tbl_name[(col_name,...)] {values | value} (), ();`

- 给自动编号的字段赋值时，可以设置为null或default

- default字段，可以设置值为default

- 字段值可以是：表达式、默认值、函数、空值    1*2*3  default  md5('123')  null




##insert set

- `insert tbl_name set col_name = {expr | default},...;`  可以使用子查询

- `insert tbl_name [(col_name,...)] select ...;`    将查询结果插入到指定数据表

  - insert test (username) select username from users where id = 5;
  
  - `insert tdb_goods_cate (cate_name) (select goods_cate from tdb_goods GROUP BY goods_cate);`





##update

- 单表更新： `update tab_reference set col_name1 = {expr | default} [, col_name2 = {expr | default}, ……] [where where_condition];`

  - update users set age = age * 5;
  
  - update users set age = age - id, sex = 0;
  
  - update users set age = age + 10 where id % 2 = 0;
  
  
  
  
  
  
  
##delete删除记录

- 单表删除： `delete from tbl_name [where where_condition];`

  - DELETE 别名 FROM 表名称 别名 WHERE 列名称 = 值






##select  表操作的80%以上使用率

```
select select_expr [,select_expr,……]

[
  from table_references
  [where where_condition]
  [group by {col_name | position} [asc | desc] , ……]
  [having where_condition]
  [order by {col_name | expr | position} [asc | desc] , ……]
  [limit {[offset, ] row_count | row_count offset offset}]
]
```

- 查找函数或表达式可以省略数据表

- 查找的列的顺序可以互换；查询表达式的顺序影响结果的顺序；别名也会影响顺序

- tbl_name.*可以表示命名表的所有列

- 查询表达式可以使用[as] alias_name为其赋予别名

- 别名可用于group by, order by 或 having 子句

  - select id [as] userid, username [as] uname from users;





##where条件表达式

- 在where表达式中，可以使用mysql支持的函数或表达式




##group by

- 查询结果分组

  - select * from users group by sex;  指定列名称
  
  - select * from users gourp by 1;  指定位置
  
  
  
  
##having

- 分组条件：分组条件为聚合函数(max, min, avg, sum)；或者字段必须出现在当前select语句当中

  - select sex,age from users group by sex having age > 0;
  
  - select sex,age from users group by sex having count(id) >= 2;





##order by 

- 对查询结果进行排序,默认升序；可对多个字段排序

  - select * from users order by id desc;
  
  - select * from users order by age, id desc;




##limit

- 限制查询结果返回的数量; 分页效果的实现

  - limit [索引,] 数量;  索引从0开始

  - select * from users limit 2;
  
  - select * from users limit 3,2;
  
  


set names gbk;

select * from tdb_goods\G;




##子查询

- 子查询指嵌套在查询内部，且必须始终出现在圆括号内。

- 子查询可以包含多个关键字或条件，如distinct、group by、order by、limit、函数等

- 子查询的外层查询可以是：select、insert、update、set或do。

- 子查询可以返回标量、一行、一列或子查询。

- Out Query / SubQuery

- `select avg(goods_price) from tdb_goods;`

- `select round(avg(goods_price),2) from tdb_goods;`

- `select * from tdb_goods where goods_price >= 5336.36;`

- `select * from tdb_goods where goods_price >= (select round(avg(goods_price),2) from tdb_goods);`

- `select * from tdb_goods where goods_price >= (select goods_price from tdb_goods where goods_cate = '超极本');`  子查询结果不能超过1行




####使用比较运算符的子查询

- = , >, <, >=, <=, <>, !=, <>

- operand comparison_operator any [some | all] (subquery)





####子查询返回多个结果需要使用any、some或all修饰比较运算符

![](/assets/360截图18470129507466.png)

- operand comparison_operator any [some | all] (subquery)

`select * from tdb_goods where goods_price >= all (select goods_price from tdb_goods where goods_cate = '超极本');`




####使用[not] in的子查询

- operand comparison_operator [not] in (subquery)

- = any运算符与in等效

- !=all或<>all运符与not in等效




####使用[not] exists的子查询

- 如果子查询返回任何行，exists将返回true；否则为false。






##数据表的参照关系

- `table_reference { [inner | cross] join | {left | right} [outer] join } table_reference on conditional_expr`

- `table_reference tbl_name [as alias] table_subquery [as] alias`  数据表可以使用别名。table_subquery可以作为子查询使用在from子句中，这样的子查询必须为其赋予别名。

- `update tdb_goods inner join tdb_goods_cates on goods_cate = cate_name set goods_cate = cate_id;`

1. 创建表  ```create table if not exists tdb_goods_cates(
	cate_id smallint unsigned primary key auto_increment,
	cate_name varchar(40) not null
);```

2. insert select写入记录  `insert tdb_goods_cates (cate_name) select goods_cate from tdb_goods GROUP BY goods_cate;`

3. 多表更新  `update tdb_goods inner join tdb_goods_cates on goods_cate = cate_name set goods_cate = cate_id;`




##create……select

- 创建数据表同时将查询结果写入到数据表  `create table [if not exists] tbl_name [(create_definition,...)] select_statement;`

1. ```drop table if exists tdb_goods_brands;
create table tdb_goods_brands(
	brand_id smallint unsigned primary key auto_increment,
	brand_name varchar(40) not null
)
select brand_name from tdb_goods group by brand_name;```

2. `update tdb_goods inner join tdb_goods_brands on brand_name = brand_name set brand_name = brand_id;`  字段含义不明确

  - `update tdb_goods as g inner join tdb_goods_brands as b on g.brand_name = b.brand_name set g.brand_name = b.brand_id;`

3. 修改表结构，数据库瘦身。

  - `alter table tdb_goods change goods_cate cate_id smallint unsigned not null, change brand_name brand_id smallint unsigned not null;`






##连接

- mysql在select语句、多表更新、多表删除语句中支持join操作。





##连接类型

- 使用on关键字来设定连接条件，也可以使用where来代替。通常使用on关键字来设定连接条件，使用where关键字进行结果集记录的过滤。


  
  
  
####内连接

- 显示左表及右表符合连接条件的记录

- 如果使用内连接查找的记录在数据表中不存在，并且在where子句中尝试以下操作：col_namd is null时，如果col_name被定义为not null，停止搜索更多的行。

- `select goods_id, goods_name, cate_name from tdb_goods inner join tdb_goods_cates on tdb_goods.cate_id = tdb_goods_cates.cate_id;`





####外连接

- 左外连接：显示左表的全部记录及右表符合连接条件的记录

- 右外连接：显示右表的全部记录及左表符合连接条件的记录





##多表连接

```
select goods_id,goods_name,cate_name,brand_name,goods_price from tdb_goods g 
inner join tdb_goods_cates c on g.cate_id = c.cate_id
inner join tdb_goods_brands b on g.brand_id = b.brand_id;
```




##无限极分类表设计

```
create table tdb_goods_types(
	type_id smallint unsigned primary key auto_increment,
	type_name varchar(20) not null,
	parent_id smallint unsigned not null default 0
);

```



##自身连接

- 同一个数据表对其自身进行连接。

```
select s.type_id,s.type_name,p.type_name from tdb_goods_types s left join tdb_goods_types p
on s.parent_id = p.type_id;


select p.type_id,p.type_name,s.type_name from tdb_goods_types p left join tdb_goods_types s
on p.type_id = s.parent_id;


select p.type_id,p.type_name,s.type_name from tdb_goods_types p left join tdb_goods_types s
on p.type_id = s.parent_id group by p.type_name order by p.type_id;


select p.type_id,p.type_name,count(s.type_name) child_count from tdb_goods_types p left join tdb_goods_types s
on p.type_id = s.parent_id group by p.type_name order by p.type_id;
```




##多表删除

- 语法：`delete tbl_name[.*] [, tbl_name[.*]] ... from table_references [where where_condition]`

  - `select * from tdb_goods group by goods_name having count(goods_name) >= 2 order by goods_id;`
  
  - `delete t1 from tdb_goods t1 left join (select * from tdb_goods group by goods_name having count(goods_name) >= 2 order by goods_id) t2 on t1.goods_name = t2.goods_name where t1.goods_id > t2.goods_id;`





##MySql数据库函数

- 字符函数

  - concat()字符连接      `select concat(type_id,type_name) as fullname from tdb_goods_types;`
  
  - concat_ws()使用指定的分隔符进行字符连接    `select concat_ws('|', 'A', 'B', 'C');`
  
  - format()数字格式化    `select format(9191827391.6178236, 2);`
  
  - lower()小写    `select lower('HELLO WORLD');`
  
  - upper()大写
  
  - left()获取左侧字符    `select lower(left('MySql', 2));`
  
  - right()获取右侧字符
  
  - length获取字符串长度
  
  - ltrim()删除前导空格
  
  - rtrim()删除后导空格
  
  - trim()    `select trim(leading '?' from '???MySql?????');`      `select trim(trailing '?' from '???MySql?????');`    `select trim(both '?' from '???MySql?????');`
  
  - replace()    `select replace('???My??Sql????', '?', '');`
  
  - substring()从1开始，截取位数    `select substring('MySql', 1, 2);`
  
  - [not]like()模式匹配    `select 'MySql' like 'M%';`    `select * from test where first_name like '%1%%' escape '1';`
  
    - `ESCAPE '1'相当于把1定义为了转义字符，也就是说'%1%%'里  中间的%被1转义了，最后一个%还是匹配符`
  
    - `%代表任意0个或多个字符`
    
    - `_代表任意一个字符`


- 数值运算符与函数

  - ceil()进一取整
  
  - floor()舍一取整
  
  - div整数除法    select 3 div 4;
  
  - mod取余数    select 5 % 3;    select 5 mod 3;    select 5.3 mod 3;
  
  - power()幂运算    select power(3, 3);
  
  - round()四舍五入    select round(3.1415926, 2);
  
  - truncate()数字截取    select truncate(125.89, 2);    select truncate(125.89, -1);

- 比较运算符与函数

  - [not] between...and...    [不]在范围之内    select 10 between 1 and 26;
  
  - [not] in()    [不]在列出值范围内    select 13 in (5,10,13,15,20);
  
  - is [not] null    [不]为空    select null is null;

- 日期时间函数

  - now()当前日期和时间
  
  - curdate()
  
  - curtime()
  
  - `date_add()`日期变化    `select date_add('2018-6-15', interval -365 day);`  1 year/3 week
  
  - datediff()日期差值    select datediff('2013-3-12', '2018-6-15');
  
  - date_format()日期格式化    `select date_format('2018-6-2', '%m/%d/%Y');`

- 信息函数

  - connection_id()连接ID
  
  - database()当前数据库
  
  - `lase_insert_id()`最后插入记录的id，与insert组合使用
  
  - user()当前用户
  
  - version()版本信息

- 聚合函数，只有一个返回值

  - avg()平均值
  
  - count()计数
  
  - max()最大值
  
  - min()最小值
  
  - sum()求和

- 加密函数

  - md5()信息摘要算法
  
  - password()密码算法    set password = password('admin');






##自定义函数

- 用户自定义函数(user-defined function, udf)是一种对MySql扩展的途径，其用法与内置函数相同。

  - 自定义函数的两个必要条件： (1)参数，不超过1024  (2)返回值
  
- 语法

  - 函数体由合法的sql语句构成;
  
  - 函数体可以是简单的select或insert语句；
  
  - 函数体如果为复合结构则使用begin...end语句；
  
  - 复合结构可以包含声明，循环，控制结构；

```
create function function_name
returns 
{string | integer | real | decimal}
routine_body

create function f1() returns varchar(30)
return date_format(now(), '%Y年%m月%d日 %H点:%i分:%s秒');

select f1();

drop function if exists f2;
create function f2(num1 smallint unsigned, num2 smallint unsigned)
returns float(10, 2) unsigned
return (num1 + num2) / 2;

select f2(48142, 75497287);

```




##存储过程

- 存储过程是sql语句和控制语句的预编译集合，以一个名称存储并作为一个单元处理。存储在数据库内，可以由应用程序通过调用来执行，而且允许用户声明变量，以及进行有条件的执行。

- 优点

  - 增强sql语句的功能和灵活性
  
  - 实现较快的执行速度
  
  - 减少网络流量
  
- 缺点

  - 存储过程不利于将来分库分表
  
  - 存储过程可能无法和许多中间件、ORM库一起使用。某些特殊的兼容MySQL的实现可能根本就不支持存储过程，那就更不用说了。
  
  - 阿里这种互联网高并发的场景，很多数据都是分库分表的，而且要求高度可扩展，原则是对db的保护做到最大化，能减少db压力的就减少db压力，尽量把运算逻辑拉到代码里面。存储过程的优点在于封装性好，直接让db进行运算，但是缺点在于难以维护，而且大大增大db压力。
  
  - 针对互联网企业。单次请求涉及数据少，数据关系简单，但是更新频率高；工程的迭代速率高，数据关系随时可能扩展修改。
  
- 创建存储过程

- 注意事项

  - 需要用过delimiter修改定界符
  
  - 如果有多个语句，需要包含在begin……end语句块中
  
  - 通过call来调用

```
create
[definer = {user | current_user}]
procedure sp_name ([proc_parameter[, ...]])
[characteristic ...] routine_body

pro_parameter:
[ in | out | inout] param_name type

//in输入类型参数，表示该参数的值必须在条存储过程时指定
//out输出类型参数，表示该参数的值可以被存储过程改变，并且可以返回
//inout输入&&输出类型参数,表示该参数的调用时指定，并且可以被改变和返回

//特性
//comment:注释
//contains sql:包含sql语句，但不包含读或写数据的语句
//no sql:不包含sql语句
//reads sql data:包含读数据的语句
//modifies sql data:包含写数据的语句
//sql security {definer | invoker} 指明谁有权限来执行
```

- 调用存储过程

  - call sp_name([parameter [,..]])

  - call sp_name[()]
  
```
drop procedure if exists removeUsersById；
delimiter //
create procedure removeUsersById (in p_id int unsigned)
begin
delete from users where id = p_id;
end
//
delimiter ;

call removeUsersById(16);


drop procedure if exists removeUserAndReturnUserNums;
delimiter //
create procedure removeUserAndReturnUserNums (in p_id int unsigned, out userNums int unsigned)
begin
delete from users where id = p_id;
select count(id) from users into userNums;
end
//
delimiter ;

call removeUserAndReturnUserNums(14, @nums);
select @nums;

局部变量和用户变量
set @i = 7;
select @i;


drop procedure if exists removeByAgeAndReturnInfos;
delimiter //
create procedure removeByAgeAndReturnInfos(in p_age int unsigned, out num1 int unsigned, out num2 int unsigned)
begin
delete from users where age = p_age;
select row_count() into num1;
select count(id) from users into num2;
end
//
delimiter ;

call removeByAgeAndReturnInfos(23, @num1, @num2);
select @num1,@num2;

```




##存储过程与自定义函数的区别

- 存储过程实现的功能复杂一些；而函数针对性更强

- 存储过程可以返回多个值；函数只能有一个返回值

- 存储过程一般独立执行；而函数可以作为其他sql语句的组成部分来出现

- 函数效率较慢

![](/assets/360截图185907269010682.png)





##存储引擎

- MySql可以将数据以不同的技术存储在文件中，这种技术就称为存储引擎。

- 每一种存储引擎使用不同的存储机制、索引技巧、锁定水平，最终提供广泛且不同的功能。




##并发控制

- 当多个连接对记录进行修改时保证数据的一致性和完整性。




##锁

- 共享锁（读锁）：在同一时间段内，多个用户可以读取同一个资源，读取过程中数据不会发生任何变化。

- 排它锁（写锁）：在任何时候只能有一个用户写入资源，当进行写锁时会阻塞其他的读锁或者写锁操作。




##锁颗粒

- 表锁，是一种开销最小的锁策略。

- 行锁，是一种开销最大的锁策略。




##事务

- 事务用于保证数据库的完整性

- 特性

  - 原子性
  
  - 一致性
  
  - 隔离性
  
  - 持久性






















































 
 