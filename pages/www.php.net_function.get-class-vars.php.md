# get_class_vars



If you want to retrieve the class vars from within the class itself, use $this.<br><br>

```
<?php
class Foo {

    var $a;
    var $b;
    var $c;
    var $d;
    var $e;

    function GetClassVars()
    {
        return array_keys(get_class_vars(get_class($this))); // $this
    }

}

$Foo = new Foo;

$class_vars = $Foo->GetClassVars();

foreach ($class_vars as $cvar)
{
    echo $cvar . "&lt;br /&gt;\n";
}
?>
```
<br><br>Produces, after PHP 4.2.0, the following:<br><br>a<br>b<br>c<br>d<br>e  

#

[Official documentation page](https://www.php.net/manual/en/function.get-class-vars.php)

**[To root](/README.md)**