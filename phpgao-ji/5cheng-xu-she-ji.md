## PDO

![](/assets/360截图16751027105108110.png)

```php
$title = $_POST['title'];

$content = $_POST['content'];

$user_name = $_POST['user_name'];

echo $title, $content, $user_name;


try{
    $dsn = 'mysql:dbname=test;host=localhost;';

    $username = "root";

    $password = '123456';

    $attr = [
        PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
        PDO::MYSQL_ATTR_INIT_COMMAND=>'SET NAMES utf8'
    ];

    $pdo = new PDO($dsn, $username, $password, $attr);

    $sql = 'insert message(title,content,created_at,user_name) values(:title, :content, :created_at, :user_name)';

    $stmt = $pdo->prepare($sql);

    $data = [
        ':title' => $title,
        ':content' => $content,
        ':created_at' => time(),
        ':user_name' => $user_name
    ];

    $stmt->execute($data);

    $rows = $stmt->rowCount();

    if($rows)
    {
        exit('添加成功');
    }
    else{
        exit('添加失败');
    }

}
catch(PDOException $e){
    echo $e->getMessage();
}
```



## 无限分类表

![](/assets/360截图17860608314144.png)

















