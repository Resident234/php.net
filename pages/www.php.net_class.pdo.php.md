# The PDO class





&quot;And storing username/password inside class is not a very good idea for production code.&quot;

Good idea is to store database connection settings in *.ini files but you have to restrict access to them. For example this way:

my_setting.ini:
[database]
driver = mysql
host = localhost
;port = 3306
schema = db_schema
username = user
password = secret

Database connection:


```
<?php
class MyPDO extends PDO
{
&#xA0; &#xA0; public function __construct($file = &apos;my_setting.ini&apos;)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if (!$settings = parse_ini_file($file, TRUE)) throw new exception(&apos;Unable to open &apos; . $file . &apos;.&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; $dns = $settings[&apos;database&apos;][&apos;driver&apos;] .
&#xA0; &#xA0; &#xA0; &#xA0; &apos;:host=&apos; . $settings[&apos;database&apos;][&apos;host&apos;] .
&#xA0; &#xA0; &#xA0; &#xA0; ((!empty($settings[&apos;database&apos;][&apos;port&apos;])) ? (&apos;;port=&apos; . $settings[&apos;database&apos;][&apos;port&apos;]) : &apos;&apos;) .
&#xA0; &#xA0; &#xA0; &#xA0; &apos;;dbname=&apos; . $settings[&apos;database&apos;][&apos;schema&apos;];
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; parent::__construct($dns, $settings[&apos;database&apos;][&apos;username&apos;], $settings[&apos;database&apos;][&apos;password&apos;]);
&#xA0; &#xA0; }
}
?>
```


Database connection parameters are accessible via human readable ini file for those who screams even if they see one PHP/HTML/any_other command.

  

#



PDO and Dependency Injection

Dependency injection is good for testing.&#xA0; But for anyone wanting various data mapper objects to have a database connection, dependency injection can make other model code very messy because database objects have to be instantiated all over the place and given to the data mapper objects.

The code below is a good way to maintain dependency injection while keeping clean and minimal model code.



```
<?php

class DataMapper
{
&#xA0; &#xA0; public static $db;
&#xA0; &#xA0; 
&#xA0; &#xA0; public static function init($db)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; self::$db = $db;
&#xA0; &#xA0; }
}

class VendorMapper extends DataMapper
{
&#xA0; &#xA0; public static function add($vendor)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $st = self::$db-&gt;prepare(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;insert into vendors set
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; first_name = :first_name,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; last_name = :last_name&quot;
&#xA0; &#xA0; &#xA0; &#xA0; );
&#xA0; &#xA0; &#xA0; &#xA0; $st-&gt;execute(array(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;:first_name&apos; =&gt; $vendor-&gt;first_name,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;:last_name&apos; =&gt; $vendor-&gt;last_name
&#xA0; &#xA0; &#xA0; &#xA0; ));
&#xA0; &#xA0; }
}

// In your bootstrap
$db = new PDO(...);
DataMapper::init($db);

// In your model logic
$vendor = new Vendor(&apos;John&apos;, &apos;Doe&apos;);
VendorMapper::add($vendor);

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/class.pdo.php)

**[To root](/README.md)**