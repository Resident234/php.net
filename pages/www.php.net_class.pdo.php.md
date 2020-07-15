# The PDO class



"And storing username/password inside class is not a very good idea for production code."<br><br>Good idea is to store database connection settings in *.ini files but you have to restrict access to them. For example this way:<br><br>my_setting.ini:<br>[database]<br>driver = mysql<br>host = localhost<br>;port = 3306<br>schema = db_schema<br>username = user<br>password = secret<br><br>Database connection:<br>

```
<?php
class MyPDO extends PDO
{
    public function __construct($file = 'my_setting.ini')
    {
        if (!$settings = parse_ini_file($file, TRUE)) throw new exception('Unable to open ' . $file . '.');
        
        $dns = $settings['database']['driver'] .
        ':host=' . $settings['database']['host'] .
        ((!empty($settings['database']['port'])) ? (';port=' . $settings['database']['port']) : '') .
        ';dbname=' . $settings['database']['schema'];
        
        parent::__construct($dns, $settings['database']['username'], $settings['database']['password']);
    }
}
?>
```
<br><br>Database connection parameters are accessible via human readable ini file for those who screams even if they see one PHP/HTML/any_other command.  

#

PDO and Dependency Injection<br><br>Dependency injection is good for testing.  But for anyone wanting various data mapper objects to have a database connection, dependency injection can make other model code very messy because database objects have to be instantiated all over the place and given to the data mapper objects.<br><br>The code below is a good way to maintain dependency injection while keeping clean and minimal model code.<br><br>

```
<?php

class DataMapper
{
    public static $db;
    
    public static function init($db)
    {
        self::$db = $db;
    }
}

class VendorMapper extends DataMapper
{
    public static function add($vendor)
    {
        $st = self::$db->prepare(
            "insert into vendors set
            first_name = :first_name,
            last_name = :last_name"
        );
        $st->execute(array(
            ':first_name' => $vendor->first_name,
            ':last_name' => $vendor->last_name
        ));
    }
}

// In your bootstrap
$db = new PDO(...);
DataMapper::init($db);

// In your model logic
$vendor = new Vendor('John', 'Doe');
VendorMapper::add($vendor);

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/class.pdo.php)

**[To root](/README.md)**