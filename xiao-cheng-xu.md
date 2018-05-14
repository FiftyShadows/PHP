##CMS两大类

- 基础数据的增删改查

- 特殊操作，比如实现发送微信消息




##课程特点

- AOP

- ORM的方式与数据库交互 Object relational mapping

- MySQL数据表设计与数据冗余的合理利用




##ThinkPHP

- 三大核心知识（路由、控制器、模型）

- 验证器、读取器、缓存与全局异常处理

- ORM：模型与关联模型





##微信

- 微信小程序

- 微信登录

- 微信支付（预订单、库存量检测与回调通知处理）

- 微信模板消息




##MySQL

- 数据库表设计

- 数据冗余的合理利用

- 事务与锁在订单（库存量）检测中的应用




netstat -ano

mysql -uroot -p

show databases;

exit

环境变量设置： 在Path下添加






##TP5层次结构

- 一个应用里包含了多个模块

    - controller不是用来写业务的，model写业务

- 用来管理系统架构以及生命周期的对象

![](/assets/360截图20180511003550405.jpg)





##启动tp自带的web服务器

- php -S localhost:8080 router.php





##URL路径格式

- http://serverName/index.php/module/controller/action/[param/value...]

- http://localhost/zerg/public /index.php /index/index/index

- APP_PATH常量路径定义

- URL不区分大小写    application/config.php    'url_convert' => true,

- 官方称为：PATH_INFO

- 某些情况下有可能是不支持的    

- 兼容模式    http://serverName/index.php?s=module/controller/action/p/v...



####缺点

- 太长

- URL路径暴露出了服务器文件结构

- 不够灵活

- 不能很好的支持URL语义化（最大的缺陷）




application/config.php    app_namespace

自动命名空间    设置 - directories - application -sources - app




##配置虚拟域名

D:\Server\xampp\apache\conf\extra

httpd-vhosts.conf

```
<VirtualHost *:80>

    DocumentRoot "D:/Server/xampp/htdocs/zerg/public"

    ServerName z.cn

</VirtualHost>

```

c:\Windows\system32\drivers\etc

hosts

127.0.0.1    z.cn



##apache服务器重写规则




##localhost跳转的问题

- 给localhost配置虚拟域名


























