## explode

- `explode(',', $arr)`将字符串分割成数组



## ORM

- ORM的缺点是会牺牲程序的执行效率和会固定思维模式。

- 在关系型数据库和业务实体对象之间作一个映射。在具体的操作业务对象的时候，就不需要再去和复杂的SQL语句打交道，只需简单的操作对象的属性和方法。

- 用于实现面向对象编程语言里不同类型系统的数据之间的转换



## cookie和session

- cookie相对不是太安全，容易被盗用导致cookie欺骗
单个cookie的值最大只能存储4k
每次请求都要进行网络传输，占用带宽



## 任何从浏览器发回的Cookie，PHP都会自动的将他存储在$_COOKIE的全局变量之中，因此我们可以通过$_COOKIE['key']的形式来读取某个Cookie值。



## 对象序列化，可以通过serialize方法将对象序列化为字符串，用于存储或者传递数据，然后在需要的时候通过unserialize将字符串反序列化成对象进行使用。



## 在一些特殊情况下，可以通过关键字clone来复制一个对象，这时__clone方法会被调用，通过这个魔术方法来设置属性的值。



## private构造函数，则不允许直接实例化对象了。可通过静态方法在类的内部实例化对象并返回。常用于单例模式



在面向过程的程序设计中function叫做函数，在面向对象中function则被称之为方法。



`一般通过->对象操作符来访问对象的属性或者方法，对于静态属性则使用::双冒号进行访问。当在类成员方法内部调用的时候，可以使用$this伪变量调用当前对象的属性。`



## 索引数组赋值有三种方式:

- 第一种：用数组变量的名字后面跟一个中括号的方式赋值，当然，索引数组中，中括号内的键一定是整数。比如，$arr[0]='苹果';

- 第二种：用array()创建一个空数组，使用=>符号来分隔键和值，左侧表示键，右侧表示值。当然，索引数组中，键一定是整数。比如，array('0'=>'苹果');

- 第三种：用array()创建一个空数组，直接在数组里用英文的单引号'或者英文的双引号"赋值，数组会默认建立从0开始的整数的键。比如array('苹果');这个数组相当于array('0'=>'苹果');



## 关联数组赋值有两种方式:

- 第一种：用数组变量的名字后面跟一个中括号的方式赋值，当然，关联数组中，中括号内的键一定是字符串。比如，$arr['apple']='苹果';

- 第二种：用array()创建一个空数组，使用=>符号来分隔键和值，左侧表示键，右侧表示值。当然，关联数组中，键一定是字符串。比如，array('apple'=>'苹果');



## strlen('hello world')返回字符串长度


## strpos("Hello world!","world")函数会返回第一个匹配的位置。如果未找到匹配，则返回 FALSE


## $ages = array("Peter"=>32, "Quagmire"=>30, "Joe"=>34);


## $_GET["name"]


## date(format,timestamp)


## include()函数会生成一个警告（但是脚本会继续执行），而 require() 函数会生成一个致命错误（fatal error）


## fopen("welcome.txt","r");函数用于在 PHP 中打开文件

- feof($file)函数检测是否已达到文件的末端

- fclose($file)函数用于关闭打开的文件

- fgets() 函数用于从文件中逐行读取文件

- fgetc() 函数用于从文件逐字符地读取文件



## 文件上传

- $_FILES["file"]["name"] - 被上传文件的名称 

- $_FILES["file"]["type"] - 被上传文件的类型 

- $_FILES["file"]["size"] - 被上传文件的大小，以字节计 

- $_FILES["file"]["tmp_name"] - 存储在服务器的文件的临时副本的名称 

- $_FILES["file"]["error"] - 由文件上传导致的错误代码



## 类

- 一般通过->对象操作符来访问对象的属性或者方法，对于静态属性则使用::双冒号进行访问。当在类成员方法内部调用的时候，可以使用$this伪变量调用当前对象的属性。

- 受保护的属性与私有属性不允许外部调用，在类的成员方法内部是可以调用的。 

- 被定义为受保护的类成员则可以被其自身以及其子类和父类访问。被定义为私有的类成员则只能被其定义所在的类访问。

- 如果构造函数定义成了私有方法，则不允许直接实例化对象了，这时候一般通过静态方法进行实例化，在设计模式中会经常使用这样的方法来控制对象的创建，比如单例模式只允许有一个全局唯一的对象。

- PHP中的重载指的是动态的创建属性与方法，是通过魔术方法来实现的。属性的重载通过__set，__get，__isset，__unset来分别实现对不存在属性的赋值、读取、判断属性是否设置、销毁属性。

- `方法的重载通过__call来实现，当调用不存在的方法的时候，将会转为参数调用__call方法，当调用不存在的静态方法时会使用__callStatic重载。`



## 使用__construct()定义一个构造函数

- `子类中如果定义了__construct则不会调用父类的__construct，如果需要同时调用父类的构造函数，需要使用parent::__construct()显式的调用`



## PHP5支持析构函数，使用__destruct()进行定义，析构函数指的是当某个对象的所有引用被删除，或者对象被显式的销毁时会执行的函数。

- unset($car)销毁



## 静态属性与方法可以在不实例化类的情况下调用，直接使用类名::方法名的方式进行调用。静态属性不允许对象使用->操作符调用。

- 静态方法中，$this伪变量不允许使用。可以使用self，parent，static在内部调用静态方法与属性。

- 静态方法也可以通过变量来进行动态调用。$func = 'getSpeed';$className = 'Car';echo $className::$func();

```
static : 是指静态方法标识符

调用静态方法时：

parent：是当前父类指针

self：是当前类指针
```


## 对象比较，当同一个类的两个实例的所有属性都相等时，可以使用比较运算符==进行判断，当需要判断两个变量是否为同一个对象的引用时，可以使用全等运算符===进行判断。























