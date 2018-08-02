Directories 配置根命名空间


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

exit;

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




##TP5三个灵活之处

- 路由

- 获取http参数

- 数据库操作




##TP5编写路由的两种方式

- 配置式：router.php

- 动态注册

- 一旦给操作方法定义了路由之后，原有的pathinfo url就会失效




##URL路径格式

- PATH_INFO

- 混合模式

- 强制使用路由模式




##路由的配置    config.php

- url_route_on    => true,    是否开启路由

- url_route_must    => false,    是否强制使用路由(开启后PATH_INFO不可用)





##完整的路由定义格式

- Route::rule('路由表达式', '路由地址', '请求类型', '路由参数(数组)', '变量规则(数组)');

- Route::rule('hello', 'sample/Test/hello', 'GET|POST', ['https'=>false]);

- 请求类型，默认`* `   //GET, POST, DELETE, PUT, *

- 快捷注册方式：Route::get, Route::post, Route::any





##路由中传递参数

- get两种方式：1.路径里传参，2.?key=value

- post三种方式：1.路径里传参，2.?key=value,3.body里





##控制器操作方法获取参数三种方式

- 在方法形参自动获取

- Request类获取

    - Request::instance()->param('id');不区分请求类型
    
    - $all=Request::instance()->param();获取所有请求变量
    
    - route方法获取路径后边的参数
    
    - get获取?后边的参数
    
    - post获取body里的参数
    
- input助手函数: $all = input('param.');

- 依赖注入Request

```
use think\Request;

class Test
{
//    public function hello($id, $name, $age){
//        echo $id;
//        echo '|';
//        echo $name;
//        echo '|';
//        echo $age;
//    }

    public  function  hello(Request $request){
//        $id = Request::instance()->param('id');
//        $name = Request::instance()->param('name');
//        $age = Request::instance()->param('age');
//        echo $id.'|'.$name.'|'.$age;

//        $all = Request::instance()->get();

//        $all = input('param.');


        $all = $request->param();

        var_dump($all);


    }

}
```




##产品

- 想要对样式和代码有更多掌控力，就把它写死

- 重复的东西需要用循环来展现

- 分类：无限极分类不实用，会引起诸多问题

- 购物车信息与服务器通信的好处，websocket实时通信机制

    - 多设备登陆：在不同的设备上登陆时，显示购物车信息
    
    - 分析用户消费习惯：购物车信息也需要存储到数据库当中
    
- 商品数量+-

    - 按照惯例数量不可以减到0；删除按钮比较隐蔽






##MySQL

- 没有使用外键约束

    - 对于业务不复杂，业务变更频度低的可以用外键约束
    
- 数据假删除

    - 系统稳定性和容错率会更高
    
    - 所有的数据都有一定的价值，用于做数据分析
    
    - 外键约束不好用
    
- 数据库迭代







































































































