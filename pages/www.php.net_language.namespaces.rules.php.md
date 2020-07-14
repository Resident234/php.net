# Name resolution rules



If you like to declare an __autoload function within a namespace or class, use the spl_autoload_register() function to register it and it will work fine.  

#

The term "autoload" mentioned here shall not be confused with __autoload function to autoload objects. Regarding the __autoload and namespaces&apos; resolution I&apos;d like to share the following experience:<br><br>-&gt;Say you have the following directory structure:<br><br>- root<br>      | - loader.php <br>      | - ns<br>             | - foo.php<br><br>-&gt;foo.php<br><br>

```
<?php
namespace ns;
class foo
{
    public $say;
    
    public function __construct()
    {
        $this->say = "bar";
    }
    
}
?>
```


-> loader.php



```
<?php
//GLOBAL SPACE &lt;--
function __autoload($c)
{
    require_once $c . ".php";
}

class foo extends ns\foo // ns\foo is loaded here
{
    public function __construct()
    {
        parent::__construct();
        echo "&lt;br /&gt;foo" . $this->say;
    }
}
$a = new ns\foo(); // ns\foo also loads ns/foo.php just fine here.
echo $a->say;   // prints bar as expected.
$b = new foo;  // prints foobar just fine.
?>
```
<br><br>If you keep your directory/file matching namespace/class consistence the object __autoload works fine.<br>But... if you try to give loader.php a namespace you&apos;ll obviously get fatal errors. <br>My sample is just 1 level dir, but I&apos;ve tested with a very complex and deeper structure. Hope anybody finds this useful.<br><br>Cheers!  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.rules.php)

**[To root](/README.md)**