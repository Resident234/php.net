# Static Keyword



Note that you should read "Variables/Variable scope" if you are looking for static keyword use for declaring static variables inside functions (or methods). I myself had this gap in my PHP knowledge until recently and had to google to find this out. I think this page should have a "See also" link to static function variables.<br>http://www.php.net/manual/en/language.variables.scope.php  

#

Here statically accessed property prefer property of the class for which it is called. Where as self keyword enforces use of current class only. Refer the below example:<br><br>

```
<?php
class a{

static protected $test="class a";

public function static_test(){

echo static::$test; // Results class b
echo self::$test; // Results class a

}

}

class b extends a{

static protected $test="class b";

}

$obj = new b();
$obj-&gt;static_test();
?>
```
  

#

It is worth mentioning that there is only one value for each static variable that is the same for all instances  

#

It is important to understand the behavior of static properties in the context of class inheritance:<br><br>- Static properties defined in both parent and child classes will hold DISTINCT values for each class. Proper use of self:: vs. static:: are crucial inside of child methods to reference the intended static property.<br><br>- Static properties defined ONLY in the parent class will share a COMMON value.<br><br>

```
<?php
declare(strict_types=1);

class staticparent {
    static    $parent_only;
    static    $both_distinct;
    
    function __construct() {
        static::$parent_only = &apos;fromparent&apos;;
        static::$both_distinct = &apos;fromparent&apos;;
    }
}

class staticchild extends staticparent {
    static    $child_only;
    static    $both_distinct;
    
    function __construct() {
        static::$parent_only = &apos;fromchild&apos;;
        static::$both_distinct = &apos;fromchild&apos;;
        static::$child_only = &apos;fromchild&apos;;
    }
}

$a = new staticparent;
$a = new staticchild;

echo &apos;Parent: parent_only=&apos;, staticparent::$parent_only, &apos;, both_distinct=&apos;, staticparent::$both_distinct, "&lt;br/&gt;\r\n";
echo &apos;Child:  parent_only=&apos;, staticchild::$parent_only, &apos;, both_distinct=&apos;, staticchild::$both_distinct, &apos;, child_only=&apos;, staticchild::$child_only, "&lt;br/&gt;\r\n";
?>
```
<br><br>will output:<br>Parent: parent_only=fromchild, both_distinct=fromparent<br>Child: parent_only=fromchild, both_distinct=fromchild, child_only=fromchild  

#

Static variables are shared between sub classes<br><br>

```
<?php
class MyParent {
    
    protected static $variable;
}

class Child1 extends MyParent {
    
    function set() {
        
        self::$variable = 2;
    }
}

class Child2 extends MyParent {
    
    function show() {
        
        echo(self::$variable);
    }
}

$c1 = new Child1();
$c1-&gt;set();
$c2 = new Child2();
$c2-&gt;show(); // prints 2
?>
```
  

#

To check if a function was called statically or not, you&apos;ll need to do:<br><br>

```
<?php
function foo () {
    $isStatic = !(isset($this) &amp;&amp; get_class($this) == __CLASS__);
}
?>
```
<br><br>More at (http://blog.phpdoc.info/archives/4-Schizophrenic-Methods.html). <br><br>(I&apos;ll add this to the manual soon).  

#

On PHP 5.2.x or previous you might run into problems initializing static variables in subclasses due to the lack of late static binding:<br><br>

```
<?php
class A {
    protected static $a;
    
    public static function init($value) { self::$a = $value; }
    public static function getA() { return self::$a; }
}

class B extends A {
    protected static $a; // redefine $a for own use
    
    // inherit the init() method
    public static function getA() { return self::$a; }
}

B::init(&apos;lala&apos;);
echo &apos;A::$a = &apos;.A::getA().&apos;; B::$a = &apos;.B::getA();
?>
```


This will output:
A::$a = lala; B::$a = 

If the init() method looks the same for (almost) all subclasses there should be no need to implement init() in every subclass and by that producing redundant code.

Solution 1:
Turn everything into non-static. BUT: This would produce redundant data on every object of the class.

Solution 2:
Turn static $a on class A into an array, use classnames of subclasses as indeces. By doing so you also don&apos;t have to redefine $a for the subclasses and the superclass&apos; $a can be private.

Short example on a DataRecord class without error checking:



```
<?php
abstract class DataRecord {
    private static $db; // MySQLi-Connection, same for all subclasses
    private static $table = array(); // Array of tables for subclasses
    
    public static function init($classname, $table, $db = false) {
        if (!($db === false)) self::$db = $db;
        self::$table[$classname] = $table;
    }
    
    public static function getDB() { return self::$db; }
    public static function getTable($classname) { return self::$table[$classname]; }
}

class UserDataRecord extends DataRecord {
    public static function fetchFromDB() {
        $result = parent::getDB()-&gt;query(&apos;select * from &apos;.parent::getTable(&apos;UserDataRecord&apos;).&apos;;&apos;);
        
        // and so on ...
        return $result; // An array of UserDataRecord objects
    }
}

$db = new MySQLi(...);
UserDataRecord::init(&apos;UserDataRecord&apos;, &apos;users&apos;, $db);
$users = UserDataRecord::fetchFromDB();
?>
```
<br><br>I hope this helps some people who need to operate on PHP 5.2.x servers for some reason. Late static binding, of course, makes this workaround obsolete.  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.static.php)

**[To root](/README.md)**