## PHP Database

Very simple database toolkit for PHP, providing simple pdo wrapper connection and query builder. 
It currently supports MySQL.
Сreated for the study. But you can use it in real projects.
Сode taken from Laravel Database Layer and simplified.

### Usage

First, create a new manager instance.

```PHP
use Database\Manager;

$manager = new Manager;

$manager->addConnection([
    'dsn' => 'mysql:dbname=testdb;host=127.0.0.1',
    'username' => 'root',
    'password' => null,
    'options' => [] // pdo driver options 
]);
```

Once the Manager instance has been registered. You may use it like so:

**Using Connection**

```PHP
$conn = $manager->getConnection();
$data = $conn->queryAll('select * from users where id in (?,?,?)', 1, 2, 3);
// $data -> [['id' => 1, 'name' => 'joe'], ...]

$conn->exec('insert into users (id, name) values (?, ?)', 1, 'joe'));

$conn->execBatch(file_get_contents('file_with_queries.txt'));
```

**Using The Query Builder**

```PHP
$data = $manager
            ->query('users')
            ->select('*')
            ->whereIn('id', [1,2,3])
            ->execute();

// $data -> [['id' => 1, 'name' => 'joe'], ...]


$data = $manager
            ->query('users')
            ->update(['votes' => 2])
            ->where('id', 1)
            ->toSql();
// $data -> ['update `user` set `votes` = ? where `id` = ?',[2,1]]
            
$data = $manager
          ->query('users')
          ->insert([
              ['email' => 'john@example.com', 'votes' => 0],
              ['email' => 'john1@example.com', 'votes' => 1]
          ])->execute();
```

More about query builder see in [QueryBuilderTest](https://github.com/itlessons/php-database/blob/master/tests/Database/Tests/QueryBuilderTest.php)

### Installation

The recommended way to install php-database is through [Composer][_Composer]. Just create a
``composer.json`` file and run the ``php composer.phar install`` command to
install it:

    {
        "require": {
            "itlessons/php-database": "*"
        }
    }

Alternatively, you can download the [php-database.zip][_php-database.zip] file and extract it.

### Resources

You can run the unit tests with the following command:

    $ cd path/to/php-database/
    $ composer.phar install
    $ phpunit
    
### Links

* [MySQL библиотека на сайте с помощью PHP](http://www.itlessons.info/php/database-mysql/)
    


[_Composer]: http://getcomposer.org
[_php-database.zip]:  https://github.com/itlessons/php-database/archive/master.zip