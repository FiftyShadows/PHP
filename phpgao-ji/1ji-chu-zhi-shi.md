## 什么是引用变量

- 在php中引用意味着用不用的名字访问同一个变量内容

- 定义方式：&符号

- COW机制，修改操作才有copy

- unset只会取消引用，不会销毁空间

- `var_dump(memory_get_usage())`

```php
$a = range(0, 1000);

$b = &$a;

$a = range(0, 1000);
```

![](/assets/360截图18430707394954.png)