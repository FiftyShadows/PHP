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

## auto\_increment

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


























































 