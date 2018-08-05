## 什么是引用变量

* 在php中引用意味着用不用的名字访问同一个变量内容

* 定义方式：&符号

* COW机制，修改操作才有copy

* unset只会取消引用，不会销毁空间

* 对象本身就是引用传递

* `var_dump(memory_get_usage())`

* xdebug\_debug\_zval\('a'\);

```php
$a = range(0, 1000);

$b = &$a;

$a = range(0, 1000);
```

![](/assets/360截图18430707394954.png)




## 常量及数据类型



#### 字符串的定义方式及各自区别

- 单引号

    - 单引号不能解析变量
    
    - 单引号不能解析转义字符，只能解析单引号和反斜线本身
    
    - 变量和变量、变量和字符串、字符串和字符串之间可以用.连接
    
    - 单引号效率高于双引号

- 双引号

    - 双引号可以解析变量，变量可以使用特殊字符和{}包含
    
    - 双引号可以解析所有转义字符
    
    - 也可以使用.来连接

- heredoc和newdoc

    - heredoc类似于双引号
    
    - newdoc类似于单引号
    
    - 用来处理大文本
    
```php
$a = 666;

$str1 = 'abcde{$a}fg';


$str2 = "abcde&$a&fg";


$str3 = "abcde'$a'fg";


$str4 = "abcde{$a}fg";
```


#### 浮点类型不能运用到比较运算中

- 交给cpu进行计算的，cpu把它转成二进制，会有损耗

```php
$num1 = 0.1;

$num2 = 0.7;


if(($num1 + $num2) == 0.8){
    echo 'true';
}
else{
    echo 'false';
}
```



#### 布尔类型 -- false的七种情况

- `0, 0.0, '', '0', false, array(), NULL`




#### 数组类型 -- 超全局数组

- `$GLOABALS, $_GET, $_POST, $_REQUEST, $_SESSION, $_COOKIE, $_SERVER, $_FILES, $_ENV`

```php
$_SERVER['SERVER_ADDR'];            //服务器端IP地址
$_SERVER['SERVER_NAME'];            //服务器名称
$_SERVER['REQUEST_TIME'];           //请求时间
$_SERVER['QUERY_STRING'];           //请求参数
$_SERVER['HTTP_REFERER'];           //上级请求页面
$_SERVER['HTTP_USER_AGENT'];        //请求头用户信息
$_SERVER['REMOTE_ADDR'];            //客户端IP地址
$_SERVER['REQUEST_URI'];            //URI
$_SERVER['PATH_INFO'];              //路由
```



#### NULL -- 三种情况

- 直接赋值为NULL

- 未定义的变量

- unset销毁的变量



#### 常量

- const、define

- const更快，是语言结构，define是函数

- define不能用于类常量的定义，const可以

- 常量一经定义，不能被修改，不能被删除


##### 预定义常量

- `__FILE__, __LINE__, __DIR__, __FUNCTION__, __CLASS__, __TRAIT__, __METHOD__, __NAMESPACE__`




## 运算符

- 错误控制符：@。当将其放置在一个表达式之前，该表达式可能产生的任何错误信息都被忽略掉。

![](/assets/360截图17290507387441.png)


























