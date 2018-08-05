变量不赋值，默认为NULL

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

![](/assets/360截图168103029798122.png)




#### 比较运算符

- ==和===的区别

- 等值判断(false的七种情况)

- 递增/递减运算符不影响布尔值

- 递减NULL值没有效果

- 递增NULL值为1

- 递增和递减在前就先运算后返回，反之就先返回，后运算



#### 短路作用

- ||和&&与or和and的优先级不同

```php
$a = 0;
$b = 0;

if($a = 3 > 0 || $b = 3 > 0){
    var_dump($a, $b);
    exit;
    $a++;
    $b++;
    echo $a. "\n";
    echo $b. "\n";
}
```


## 流程控制

- if......elseif

    - 在elseif语句中只能有一个表达式为true，即在elseif语句中只能有一个语句块被执行，多个elseif从句是排斥关系。

    - 使用elseif语句有一个基本原则，总把优先范围小的条件放在前面处理。

- switch...case...

    - 和if不同的是，switch后面的控制表达式的数据类型只能是整形、浮点类型或者字符串。
    
    - continue语句作用到switch的作用类似于break
    
    - 跳出switch外的循环，可以使用continue2
    
    - switch...case会生成跳转表，直接跳转到对应case
    
    - 效率：如果条件比一个简单的比较要复杂得多或者在一个很多次的循环中，那么用switch语句可能会快一些


#### 遍历数组的三种方式及各自区别

- for循环、 while、 do... while

- foreach循环

- while、 list()、 each()组合循环

- for魂环只能遍历索引数组，foreach可以遍历索引和关联数组，联合使用list()，each()和while循环同样可以遍历索引和关联数组。

- while、 list()、 each()组合不会reset()

- foreach遍历会对数组进行reset()操作



## 变量的作用域

- 变量的作用域也称变量的范围，变量的范围即它定义的上下文背景(也是它的生效范围)。大部分的php变量只有一个单独的范围。这个单独的范围跨度同样包含了include和require引入的文件。

- global关键字

- $GLOBALS及其他超全局数组

```php
$outer = 'str';

function mgfunc(){
    global $outer;
    echo $outer;
    echo $GLOBALS['outer'];

}

mgfunc();
```


## 静态变量

- 静态变量仅在局部函数域中存在，但当程序执行离开此作用域时，其值并不会消失。

- static关键字

    - 仅初始化一次
    
    - 初始化时需要赋值
    
    - 每次执行函数该值会保留
    
    - static修饰的变量是局部的，仅在函数内部有效
    
    - 可以记录函数的调用次数，从而可以在某些条件下终止递归
    
```php
function mgFunc()
{
    static $a = 1;
    echo $a++;
}

mgFunc();
```

![](/assets/360截图1700101210113093.png)



## 函数的参数

- 默认情况下，函数参数通过值传递

- 如果希望允许函数修改它的值，必须通过引用传递参数

```php
$a = 666;

function mgFunc(&$b){
    $b = 999;
    echo $b;
}

mgFunc($a);

echo $a;
```



## 函数的返回值

- 值通过使用可选的返回语句(return)返回

- 可以返回包括数组和对象的任意类型

- 返回语句会中止函数执行，将控制权交回函数调用处

- 省略return,返回值为NULL，不可有多个返回值



## 函数的引用返回

- 从函数返回一个引用，必须在函数声明和指派返回值给一个变量时都是用引用运算符& 

```php
function &mgFunc()
{
    static $b = 10;
    return $b;
}

$a = mgFunc();

$a = 100;

echo mgFunc();

$a = &mgFunc();

$a = 100;

echo mgFunc();
```



## 外部文件的导入

- include/require语句包含并运行指定文件

- 如果给出路径名，按照路径来查找，否则从include_path中查找

- 如果include_path中也没有，则从调用脚本文件所在的目录和当前工作目录下寻找

- 当一个文件被包含时，其中所包含的代码继承了include所在行的变量范围

- 加载过程中未找到文件则include结构会发出一条警告；这一点和require不同，后者会发出一个致命错误。

- require在出错时产生`E_COMPILE_ERROR`级别的错误。将导致脚本终止而include只产生警告(E_WARNING)，脚本会继续运行。










