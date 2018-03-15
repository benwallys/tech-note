# 1. 配置 .env 文件
```
/* 这部分是默认的数据库设置 */
DB_CONNECTION=mysql
DB_HOST=123.xxx.ooo.110
DB_PORT=3306
DB_DATABASE=default_db_name
DB_USERNAME=default_username
DB_PASSWORD=default_password

/* 这部分是新增的数据库设置 */
DB_HOST_NEW=119.xxx.ooo.74
DB_PORT_NEW=4412
DB_DATABASE_NEW=new_db_name
DB_USERNAME_NEW=new_db_username
DB_PASSWORD_NEW=new_db_password
```

# 2. 配置 \config\database.php 文件
```
/* 这是默认的设置 */
'mysql' => [
    'driver' => 'mysql',
    'host' => env('DB_HOST', '127.0.0.1'),
    'port' => env('DB_PORT', '3306'),
    'database' => env('DB_DATABASE', 'forge'),
    'username' => env('DB_USERNAME', 'forge'),
    'password' => env('DB_PASSWORD', ''),
    'unix_socket' => env('DB_SOCKET', ''),
    'charset' => 'utf8mb4',
    'collation' => 'utf8mb4_unicode_ci',
    'prefix' => '',
    'strict' => true,
    'engine' => null,
],

/* 这是需要新增加的设置内容，使用 .env 新增的内容 */
'mysql_new' => [
    'driver' => 'mysql',
    'host' => env('DB_HOST_NEW', 'localhost'),
    'port' => env('DB_PORT_NEW', '3306'),
    'database' => env('DB_DATABASE_NEW', 'forge'),
    'username' => env('DB_USERNAME_NEW', 'forge'),
    'password' => env('DB_PASSWORD_NEW', ''),
    'charset' => 'utf8',
    'collation' => 'utf8_unicode_ci',
    'prefix' => '',
    'strict' => false,
    'engine' => null,
],
……
```

# 3. 应用
## 3.1 Schema
在各种 Schema Builder 中使用，可以使用 Schema facade 类来连接任意想要连接的数据库，只需要使用 connection() 方法即可，如 migration 文件中应用：
```
Schema::connection('mysql_new')->create('some_table', function($table)
{
    $table->increments('id'):
});
```

## 3.2 Query
在 Query Builder 类中也是使用 connection() 方法选择数据库即可：
```
$users = DB::connection('mysql_new')->select(...);
```

## 3.1 Eloquent
这是连接默认数据库的 model 文件，直接使用表即可：
```
// \app\Models\Users.php
class Users extends Model
{
    // 数据库 'default_db_name' 中的 users 表
    protected $table = "users";
}
```

这是需要连接新增的数据库的 model 文件，使用 $connection 属性进行数据库关联：
```
// \app\Models\NewUsers.php
class NewUsers extends Model
{
    // 先连接新的数据库设置（config\database.php 新增的字段名）
    protected $connection = 'mysql_new';

    // 数据库 'new_db_name' 中的 users 表
    protected $table = "users";
}
```
然后在其他类文件中引用该 model 文件即可。

另外还可以在控制器中，运行程序的时候，通过 setConnection 方法定义连接：
```
<?php

class SomeController extends BaseController {

    public function someMethod()
    {
        $someModel = new SomeModel;

        $someModel->setConnection('mysql_new');

        $something = $someModel->find(1);

        return $something;
    }

}
```

# 4. 注意事项
## 4.1 关于配置
如果贪图方便，可以不配置 .env 的内容，直接在 \config\database.php 文件中新增数据库的地址端口账号密码等：
```
'mysql_new' => [
    'driver' => 'mysql',
    'host' => 'new_db_nost',
    'port' => 'new_db_port',
    'database' => 'new_db_name',
    'username' => 'new_db_username',
    'password' => 'new_db_password',
    'charset' => 'utf8',
    'collation' => 'utf8_unicode_ci',
    'prefix' => '',
    'strict' => false,
    'engine' => null,
],
```

## 4.2 关于告警报错
在 \config\database.php 文件中，如果默认的数据库无法连接，在使用非默认数据库的时候，程序运行过程中会报错，所以要先保证默认数据库能正常连接访问，再使用非默认的数据库。
