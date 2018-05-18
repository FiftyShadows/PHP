net start mysql

net stop mysql

mysql -uroot -p -P3306 -h127.0.0.1

退出    exit;    quit;    \q




##修改MySQL提示符

- mysql -uroot -p --prompt \h

    - \D    完整的日期
    
    - \d    当前数据库
    
    - \h    服务器名称
    
    - \u    当前用户

- 登陆后;    prompt mysql>

    - prompt \u@\h \d>
    
![](/assets/360截图20180517233215901.jpg)
    
    


##常用命令以及语法规范

- 关键字与函数名称全部大写

- 数据库名称、表名称、字段名称全部小写

- SQL语句必须以分号结尾

- SELECT VERSION(); 显示当前服务器版本

- SELECT NOW(); 显示当前日期时间

- SELECT USER(); 显示当前用户

- create database [if not exists] xxx character set utf8;    创建数据库

- show databases;    查看服务器列表

- show warnings;    查看警告

- show create database t1;    查看数据库编码格式

- alter database t1 character set utf8;    修改数据库的编码方式

- drop database [if exists] t1;




##数据类型

- 整型

    - TINYINT    -128到127(7位)    0到256(8位)
    
    - SMALLINT    -32768到32767(15位)    0到65535(16位)
    
    - MEDIUMINT    -8388608到8388607(23位)    0到16777215(24位)
    
    - INT    -2147483648到2147483647(31位)    0到4294967295(32位)
    
    - BIGINT    (63位)    (64位)
    
- 浮点型

    - FLOAT    精确到小数位7位
    
    - DOUBLE
    
- 日期时间型
    
    - YEAR    1
    
    - TIME    3
    
    - DATE    3
    
    - DATETIME    8
    
    - TIMESTAMP    4
    
- 字符型

![](/assets/360截图20180518004226274.jpg)





##数据表操作

- use test；    打开数据库

- select database();    查看当前数据库名

- create table [if not exists] ()    创建数据表

- `show tables [from db_name] [like 'patte' | where expr];`    查看数据表列表

- show columns from tbl_name;    查看数据表结构

- `insert tbl_name[(col_name,...)] values(val,...) ;`   插入记录

    - 省略字段，则所有字段都需要赋值
    
- select expr,... from tbl_name;

```mysql
create table tb1(
username varchar(20),
age tinyint unsigned,
salary float(8,2) unsigned
);
```




##非空约束

- null；字段值可以为空

- not null；字段值禁止为空




##auto_increment

- 自动编号，且必须与主键组合使用

- 默认情况下，起始值为1，每次的增量为1

- 可以是整型和浮点型；但浮点型小数位数必须是0




##primary key

- 主键约束

- 每张表只能存在一个主键

- 主键保证记录的唯一性

- 主键自动为not null

- auto_increament 只用于主键，不是必须使用的；且不需要赋值




##unique key

- 唯一约束可以保证记录的唯一性

- 唯一约束的字段可以为空值(null)；空值是唯一的

- 每张数据表可以存在多个唯一约束




##default

- 当插入记录时，如果没有明确为字段赋值，则自动赋予默认值。

```
create table tb5(
id smallint unsigned auto_increment primary key,
username varchar(20) not null unique key,
sex enum('1', '2', '3') default '3'
);
```




##约束

- 保证数据的完整性和一致性

- 约束分为表级约束和列级约束




##foreign key

- 保持数据一致性，完整性

- 实现一对一或一对多关系。




##外键约束的要求

- 数据表的引擎只能为InnoDB

    - my.ini    default-storage-engine=INNODB
    
- 外键列和参照列必须具有相似的数据类型    

    - 数字长度和符号位必须相同
    
    - 字符的长度则可以不同
    
- 有外键的表为子表，子表所按照的表为父表

- 外键列和参照列必须创建索引。如果外键列不存在索引的话，mysql将自动创建索引。
    
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
    

    

##外键约束的参照操作

- cascade: 从父表删除或更新且自动删除或更新子表中匹配的行

- set null: 从父表删除或更新行，并设置子表中的外键为null。如果使用该选项，必须保证子表列没有指定not null。

- restrict： 拒绝对父表的删除或更新操作

- no action: 标准sql的关键字，在mysql中与restrict相同




##表级约束与列级约束

- 对一个数据列建立的约束，成为列级约束

- 对多个数据列建立的约束，成为表级约束

- 列级约束既可以在列定义时声明，也可以在列定义后声明

- 表级约束只能在列定义后声明

- not null default不存在表级约束




##修改数据表

- `alter table tbl_name add col_name col_definition [first | after col_name];`    添加单列

- alter table tbl_name add ();    添加多列，不能指定位置关系

- `alter table tbl_name drop col_name;`    删除列

- `alter table tbl_name drop a, drop b;`    删除多列，也可以在后边继续添加

- `alter table tbl_name add primary key [index_type] col_name;`    添加主键约束

- `alter table tbl_name add unique [index | key] [index_type] (col_name, ...);`    添加唯一约束

- `alter table tbl_name add foreign key (col_name, ...) references tabl_name (col_name);`    添加外键约束

- alter table tbl_name alter col_name {set default value | drop default};    添加/删除默认约束

































