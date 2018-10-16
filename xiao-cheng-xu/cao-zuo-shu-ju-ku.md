## 三种查询数据库的方式

- 原生sql语句

    - `Db::query('select * from banner_item where banner_id=?', [$id])`

- 查询构造器

    - `Db::table('banner_item')->where('banner_id', '=', $id)->select()`
    
    - table,where(辅助方法/链式方法)返回的是query对象，用于链式操作
    
        - 只能在真正的执行方法之前调用
        
        - 不同链式方法没有先后顺序
        
        - 相同的链式方法位置关系可能有影响
        
        - 非链式调用Db::table('banner_item'); Db::where(); Db::select();
                
    - 只有使用find,select才会生成sql语句执行调用，查出最终结果
    
        - 无状态
    
    - find只返回一条数据
    
    - select返回所有
    
    - update,delete,insert

- 模型 关联模型



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

- 忽略eq大小写



## where表达式、数组法（安全性问题，不够灵活）、闭包

```php
//闭包,use传递变量
$result = Db::table('banner_item')
    ->where(function ($query) use ($id){    
        $query->where('banner_id', '=', $id)->where(xxx);
    })
    ->select();
```



## 查看构造器原生sql

- fetchSql()

- sql记录日志



## sql记录日志

- database.php    'debug' => true    config.php    'app_debug' => true    'level' => ['sql']    'type' => 'File'

- 在入口文件出初始化Log记录sql

- 生产环境不要开启，写文件耗费服务器性能。处理bug可以临时开启 



## Object Relation Mapping对象关系映射



## 默认输出类型

- `'default_return_type' => 'json'`



## php think make:model api/BannerItem



## 数据库表名和model类名不一定一一对应

- `protected $table = 'banner_item';`



## 调用方式

- 静态方法调用更加便捷

```
//第一种方式
$banner = BannerModel::get($id);


//第二种方式
$banner = new BannerModel();
$banner = $banner->get();
```




## 几种查询动词的总结与ORM性能问题的探讨

- get, find    只能查询到一条数据库记录

- all, select



#### 四个原则

- 模型和数据库访问层是两个不同的概念

- 不能因为模型的性能稍差，而放弃使用模型

- 用面向对象的思维使用和设计模型

- 模型底层仍然是数据库访问抽象层




## 关联模型

- with可以是字符串也可以是数组

- hasMany是一对多关系

- belongsTo是一对一关系，可省略后两个参数(有外键)    hasOne(没有外键)    分别表示主从关系

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
//items是banner的关联；items.img是嵌套表的关联
```



## 隐藏模型字段

- 操作数组的方式 $data = $banner->toArray;    unset($data['delete_time'])

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

- 通过modele读取器方法getUrlArr，返回拼接的字符串    固定格式为：get[字段名]Arr

```
class Image extends Model
{
    //
    public function getUrlAttr($value, $data){
        $finalUrl = $value;
        if($data['from'] == 1){
            $finalUrl = config('setting.imgprefix').$finalUrl;
        }
        return $finalUrl;
    }
}
```



## 给每一个增加url读取器

- 公共函数

- Model基类




## 通过自定义模型基类的方式使每个img模型继承读取器方法

- 增加可维护性

- 并不是所有的url字段都需要修改；所以基类下的public function getUrlAttr不合适，可以定义为protected function getUrlAttr在子类的读取器中调用

```
class BaseModel extends Model
{
    //
    protected function prefixImgUrl($value, $data){
        $finalUrl = $value;
        if($data['from'] == 1){
            $finalUrl = config('setting.imgprefix').$finalUrl;
        }
        return $finalUrl;
    }
}



class Image extends BaseModel
{
    //
    protected function getUrlAttr($value, $data)
    {
        return $this->prefixImgUrl($value, $data);
    }


}

```


## 两个表之间的多对多关系

- 推荐用第三张表表示

- 不推荐在表里添加字段，用字符串表示




## 动态参数路由

`Route::rule('api/:version/banner/:id', 'api/:version.banner/getBanner');`



## 数据库一对一的主从关系

- belongsTo可省略后两个参数(有外键)    

- hasOne(没有外键)



## 通过在BaseException中定义通用的验证方法，达到复用

- protected $message定义验证不通过的错误提示


























