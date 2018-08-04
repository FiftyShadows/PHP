$result = Db::query('select * from banner_item where banner_id = ?', [$id]);



## Db    Collection    Query    Builder    

- Db是数据库操作的入口对象，根据配置文件连接数据库。通过实例化Collection对象，连接数据库。

- Collection连接器：内部通过PDO实现数据库连接；待命状态，惰性。节约服务器数据库资源。封装变化，隐藏细节和差异。

- Query查询器：CURD的封装，优雅的编写sql语句。支持链式操作。

- Builder生成器：翻译成原生sql。传给Collection。

![](/assets/360截图18430705586285.png)




## DAL，使用数据库中间层访问数据库的好处

- 简化sql编写，简洁

- 不用关心具体实现，跨数据库一致性。适配多种数据库的连接和操作。