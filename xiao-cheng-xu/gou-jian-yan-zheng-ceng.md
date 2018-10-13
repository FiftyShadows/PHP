## 控制器文件层级解决方法

* Route::get\('banner/:id', 'api/v1.Banner/getBanner'\);

  * 不要传入控制器命名空间

  * api/v1.Banner/getBanner    B可以小写
  
  - 三段式：模块名/控制器名/操作方法名

## validate两种用法

* 独立验证

```php
public function getBanner($id){
    $data = [
        'name' => 'vendor11111',
        'email' => 'vendorqq.com'
    ];
    $validate = new Validate([
        'name' => 'require|max: 10',
        'email' => 'email'
    ]);
    $result = $validate ->batch() -> check($data);
    var_dump($validate->getError());
}
```

* 验证器

  * 大幅度简化代码的写法    官方推荐

* 区别：验证器对于Validate的规则做了更好的封装



## Vlidate类有batch方法，用于批量验证

- 将所有参数都验证一遍，返回的错误是一个数组

- 'num' => 'in:1,2,3'


