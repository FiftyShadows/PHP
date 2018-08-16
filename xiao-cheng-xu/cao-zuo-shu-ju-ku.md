最重要的是可读性，不是性能。


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



## 封装性

- 隐藏细节和差异，调用更加方便



## 查询构造器封装了对不同数据库的操作




## find()只返回一条数据库记录，一位数组。select()返回所有满足查询条件的记录。

- `$result = Db::table('banneritem')->where('banner_id', '=', $id)->select();`



## Db辅助方法(链式方法)

- 辅助方法并不会真正的执行sql语句    table where

- 返回query对象

- 相同的链式方法调用顺序对结果不会有影响，相同的可能会有影响




## 数据库执行方法(select, update, delete, insert)

- 执行之后状态会被清除



## where(字段名，表达式，查询条件)

- 等号可省略



## where表达式、数组法（安全性问题，不够灵活）、闭包

```php
//闭包
$result = Db::table('banner_item')
    ->where(function ($query) use ($id){
        $query>where('banner_id', '=', $id);
    })
    ->select();
```



## 查看构造器原生sql

- fetchSql()



## sql记录日志

- database.php    'debug' => true

- config.php    'app_debug' => true    'level' => ['sql']    'type' => 'File'



## Object Relation Mapping对象关系映射



## 默认输出类型

- `'default_return_type' => 'json'`



## php think make:model api/BannerItem



## 数据库表名和model类名不一定一一对应

- `protected $table = 'banner_item';`



## 调用方式

```
//第一种方式
$banner = BannerModel::get($id);


//第二种方式
$banner = new BannerModel();
$banner = $banner->get();
```




## 几种查询动词的总结与ORM性能问题的探讨

- get, find, all, select



#### 四个原则

- 模型和数据库访问层是两个不同的概念

- 不能因为模型的性能稍差，而放弃使用模型

- 用面向对象的思维使用和设计模型

- 模型底层仍然是数据库访问抽象层




## 关联模型

```php
class Banner extends Model
{

    public function items(){
        return $this->hasMany('BannerItem', 'banner_id', 'id');
    }

}
```



## 嵌套关联关系

```php
$banner = BannerModel::with(['items', 'items.img'])->find($id);
```



## 隐藏模型字段

```php
//通过操作数组隐藏
$banner = BannerModel::getBannerByID($id);
$data = $banner->toArray();
unset($data['delete_time']);


//通过模型方法隐藏
$banner = BannerModel::getBannerByID($id);
$banner->hidden(['delete_time']);


//通过模型方法显示
$banner = BannerModel::getBannerByID($id);
$banner->visible(['id']);
```



## 在模型内部隐藏字段

```
protected $hidden = ['update__time', 'delete_time'];


protected $visible = [];
```



## 图片资源URL配置

- from1存储在本地    from2存储在云端

- application/extra/目录下可以定义自己的配置文件；会自动加载

- 通过config('setting.img_prefix')读取配置

- public是公开目录，不需要权限控制。放在其他目录下，访问不了。





## 读取器

- 通过modele读取器方法getUrlArr，返回拼接的字符串    格式为：get[字段名]Arr
































