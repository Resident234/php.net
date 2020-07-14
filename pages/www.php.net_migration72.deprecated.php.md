# Deprecated features in PHP 7.2.x



Instead of __autoload(), you can use spl_autoload_register() very easily, as per the documentation:<br><br>spl_autoload_register(function ($class) {<br>    include &apos;classes/&apos; . $class . &apos;.class.php&apos;;<br>});<br><br>And this lets you have multiple autoloaders instead of one global one.  

#

[Official documentation page](https://www.php.net/manual/en/migration72.deprecated.php)

**[To root](/README.md)**