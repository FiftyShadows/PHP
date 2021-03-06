方法名和类名保持一致，这个方法也是构造方法。

全局变量不能在函数内部直接访问

可变函数：函数名称可以用变量进行处理

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

## 

---

## 常量及数据类型

#### 字符串的定义方式及各自区别

* 单引号

  * 单引号不能解析变量

  * 单引号不能解析转义字符，只能解析单引号和反斜线本身

  * 变量和变量、变量和字符串、字符串和字符串之间可以用.连接

  * 单引号效率高于双引号

* 双引号

  * 双引号可以解析变量，变量可以使用特殊字符和{}包含

  * 双引号可以解析所有转义字符

  * 也可以使用.来连接

* heredoc和newdoc

  * heredoc类似于双引号

  * newdoc类似于单引号

  * 用来处理大文本

```php
$a = 666;

$str1 = 'abcde{$a}fg';


$str2 = "abcde&$a&fg";


$str3 = "abcde'$a'fg";


$str4 = "abcde{$a}fg";
```

#### 浮点类型不能运用到比较运算中

* 交给cpu进行计算的，cpu把它转成二进制，会有损耗

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

* `0, 0.0, '', '0', false, array(), NULL`

#### 数组类型 -- 超全局数组

* `$GLOABALS, $_GET, $_POST, $_REQUEST, $_SESSION, $_COOKIE, $_SERVER, $_FILES, $_ENV`

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

* 直接赋值为NULL

* 未定义的变量

* unset销毁的变量

#### 常量

* const、define

* const更快，是语言结构，define是函数

* define不能用于类常量的定义，const可以

* 常量一经定义，不能被修改，不能被删除

##### 预定义常量

* `__FILE__, __LINE__, __DIR__, __FUNCTION__, __CLASS__, __TRAIT__, __METHOD__, __NAMESPACE__`

## 

---

## 运算符

* 错误控制符：@。当将其放置在一个表达式之前，该表达式可能产生的任何错误信息都被忽略掉。

![](/assets/360截图17290507387441.png)

![](/assets/360截图168103029798122.png)

#### 比较运算符

* ==和===的区别

* 等值判断\(false的七种情况\)

* 递增/递减运算符不影响布尔值

* 递减NULL值没有效果

* 递增NULL值为1

* 递增和递减在前就先运算后返回，反之就先返回，后运算

#### 短路作用

* \|\|和&&与or和and的优先级不同

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

## 

---

## 流程控制

* if......elseif

  * 在elseif语句中只能有一个表达式为true，即在elseif语句中只能有一个语句块被执行，多个elseif从句是排斥关系。

  * 使用elseif语句有一个基本原则，总把优先范围小的条件放在前面处理。

* switch...case...

  * 和if不同的是，switch后面的控制表达式的数据类型只能是整形、浮点类型或者字符串。

  * continue语句作用到switch的作用类似于break

  * 跳出switch外的循环，可以使用continue2

  * switch...case会生成跳转表，直接跳转到对应case

  * 效率：如果条件比一个简单的比较要复杂得多或者在一个很多次的循环中，那么用switch语句可能会快一些

#### 遍历数组的三种方式及各自区别

* for循环、 while、 do... while

* foreach循环

* while、 list\(\)、 each\(\)组合循环

* for魂环只能遍历索引数组，foreach可以遍历索引和关联数组，联合使用list\(\)，each\(\)和while循环同样可以遍历索引和关联数组。

* while、 list\(\)、 each\(\)组合不会reset\(\)

* foreach遍历会对数组进行reset\(\)操作

## 

---

## 变量的作用域

* 变量的作用域也称变量的范围，变量的范围即它定义的上下文背景\(也是它的生效范围\)。大部分的php变量只有一个单独的范围。这个单独的范围跨度同样包含了include和require引入的文件。

* global关键字

* $GLOBALS及其他超全局数组

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

* 静态变量仅在局部函数域中存在，但当程序执行离开此作用域时，其值并不会消失。

* static关键字

  * 仅初始化一次

  * 初始化时需要赋值

  * 每次执行函数该值会保留

  * static修饰的变量是局部的，仅在函数内部有效

  * 可以记录函数的调用次数，从而可以在某些条件下终止递归

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

* 默认情况下，函数参数通过值传递

* 如果希望允许函数修改它的值，必须通过引用传递参数

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

* 值通过使用可选的返回语句\(return\)返回

* 可以返回包括数组和对象的任意类型

* 返回语句会中止函数执行，将控制权交回函数调用处

* 省略return,返回值为NULL，不可有多个返回值

## 函数的引用返回

* 从函数返回一个引用，必须在函数声明和指派返回值给一个变量时都是用引用运算符& 

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

* include/require语句包含并运行指定文件

* 如果给出路径名，按照路径来查找，否则从include\_path中查找

* 如果include\_path中也没有，则从调用脚本文件所在的目录和当前工作目录下寻找

* 当一个文件被包含时，其中所包含的代码继承了include所在行的变量范围

* 加载过程中未找到文件则include结构会发出一条警告；这一点和require不同，后者会发出一个致命错误。

* require在出错时产生`E_COMPILE_ERROR`级别的错误。将导致脚本终止而include只产生警告\(E\_WARNING\)，脚本会继续运行。

* `require(include)/require_one(include_once)`唯一区别是PHP会检查该文件是否已经被包含过，如果是则不会再次包含。

## 时间日期函数

* `date(), strtotime(), mktime(), time(), microtime(), date_default_timezone_set()`

## IP处理函数

* `if2long(), long2ip()`

## 打印处理

* `print(), printf(), print_r(), echo, sprintf(), var_dump(), var _export()`

* print只能打印一个，echo能打印多个，效率更高

* printf会根据格式进行输出，输出到缓存区，sprintf会返回不会输出

* `print_r`会将数组、对象格式化好并打印，false不会打印出来，true的打印是1

* `var_dump`会把每个值得类型显示出来，建议使用`var_dump`

* `var_export`\($xx, true\)不打印，返回\(符合php语法结构\)；false时打印，不返回

## 序列化及反序列化函数

* `serialize(), unserialize()`

## 字符串处理函数

* `implode(), explode(), join(), strrev(), trim(), ltrim(), rtrim(), strstr(), number_format()...`

## 数组处理函数

* `array_rand(), array_keys(), array_values(), array_diff(), array_intersect(), array_merge(), array_shift(), array_unshift(), array_pop(), array_push(), sort()...`

![](/assets/360截图177104097694126.png)

## 

---

## 正则表达式

![](/assets/360截图17290507759292.png)

* 后向引用

![](/assets/360截图17571113267773.png)

* 贪婪模式

![](/assets/360截图18010806665483.png)

![](/assets/360截图17571117497889.png)

![](/assets/360截图17980102468475.png)

* 练习常见正则表达式\(URL, Email, IP地址, 手机号码等\)

![](/assets/360截图17571115776398.png)

## 

---

## 文件读取/写入操作

* fopen\(\)函数：用来打开一个文件，打开时需要指定打开模式

  * 打开模式：r/r+, w/w+, a/a+, x/x+, b, t

* 写入函数： `fwrite(), fputs()`

* 读取函数： `fread(), fgets()获取一行, fgetc()获取一个字符`

* 关闭文件函数： `fclose()`

* 不需要fopen\(\)打开的函数： `file_get_contents(), file_put_contents()`

* 其他读取函数： `file()将整个文件读取到数组里面去, readfile()读取并输出到缓冲区`

* 访问远程文件： php.ini开启`allow_url_fopen`，HTTP协议连接只能使用只读，FTP协议可以使用只读或者只写

* 目录相关： `basename(), dirname(), pathinfo()`

* 目录读取： `opendir(), readdir(), closedir(), rewinddir()`

* 目录删除： `rmdir()`

* 目录创建： `mkdir()`

* 文件大小： `filesize()`

* 磁盘大小： `disk_free_space()剩余空间, disk_total_space()总共大小`

* 文件拷贝： copy\(\)

* 删除文件： unlink\(\)

* 文件类型\(file/dir\)： filetype\(\)

* 重命名文件或者目录\(移动位置\)： rename\(\)

* 文件截取： ftruncate\(\)

* 文件属性： `file_exists(), is_readable(), is_writable(), is_executable(), filectime(), fileatime(), filetime()`

* 文件锁： flock\(\)

* 文件指针： ftell\(\), fseek\(\), rewind\(\)

```php
//在文件开头加入Hello world

$file = './hello.txt';

$handle = fopen($file, 'r');

$content = fread($handle, filesize($file));

$content = 'Hello world.' . $content;

fclose($handle);

$handle = fopen($file, 'w');

fwrite($handle, $content);

fclose($handle);


//对目录进行遍历

$dir = '../demo';

function loopDir($dir)
{
    $handle = opendir($dir);

    while (false !== ($file = readdir($handle))) {
        echo $file . "<br>";
        if ($file != '.' && $file != '..'){
            if(filetype($dir . '/' . $file) == 'dir'){
                loopDir($dir . '/' . $file);
            }
        }
    }
}

loopDir($dir);
```

## 

---

## 会话控制技术

#### Cookie

* setcookie\($name, $value, $expire, $path, $domain, $secure\);

* $\_COOKIE

* setcookie

* setcookie\($name, '', time\(\) - 1000\);    删除cookie

#### Session

* session基于cookie

* 禁用了cookie，客户度发送session\_id

* session信息存储在服务器当中的文件里

#### session操作

* session\_start\(\)

* $\_SESSION

* $\_SESSION = \[\];    清空session

* session\_destroy\(\)    删除session文件

#### session配置

* `session.auto_start`    是否自动开启`session_start`

* `session.cookie_domain`    存储session的cookie有效域名

* `session.cookie_lifetime`

* `session.cookie_path`

* `session.name`    sessid    SID

* `session.save_path`

* `session.use_cookies`

* `session.use_trans_sid`

* `session.gc_probability = 1`

* `session.gc_divisor = 100`

* `session.gc_maxlifetime = 1440`

* `session.save_handler`

* session存储    `session_set_save_handler()`    MySQL、Memcache、Redis等

## PHP类权限控制修饰符

* public, protected, private

## 面向对象的继承

* 单一继承

* 方法重写

## 面向对象的多态

* 抽象类的定义

* 接口的定义

## 魔术方法

![](/assets/360截图162511108385115.png)

## 设计模式

![](/assets/360截图17290429272758.png)

## 网络协议

![](/assets/360截图188407158511581.png)

## get和post请求方法的区别

* get在后退或刷新的时候没有变化，post的数据会被重新提交

* get可以被收藏为书签，post不能被收藏为书签

* get请求能够被浏览器缓存，而post请求不能被浏览器缓存

* get的请求编码类型是urlencoded；post还有json

* get在历史记录中保存参数

* get有数据长度限制，最大2048字符; post没有限制

* get只允许ascⅡ

* post更安全

## HTTPS的工作原理

* https是一种基于ssl/tls的http协议，所有的http数据都是在ssl/tls协议封装之上传输的。

* https协议在http协议的基础上，添加了ssl/tls握手以及数据加密传输，也属于应用层协议。

## 常见网络协议含义及端口

* FTP端口21

* Telnet远程登录23

* SMTP简单邮件传输协议25

* POP3接收邮件110

* HTTP

* DNS域名解析协议53

## PHP常见配置项

![](/assets/360截图1847012310610788.png)

