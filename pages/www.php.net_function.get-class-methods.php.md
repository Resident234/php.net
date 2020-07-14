# get_class_methods



It should be noted that the returned methods are dependant on the current scope. See this example:<br><br>

```
<?php
class C
{
    private function privateMethod()
    {
        
    }
    public function publicMethod()
    {
        
    }
    public function __construct()
    {
        echo &apos;$this:&apos;;
        var_dump(get_class_methods($this));
        echo &apos;C (inside class):&apos;;
        var_dump(get_class_methods(&apos;C&apos;));
    }
}
$c = new C;
echo &apos;$c:&apos;;
var_dump(get_class_methods($c));
echo &apos;C (outside class):&apos;;
var_dump(get_class_methods(&apos;C&apos;));
?>
```
<br><br>Output:<br><br>$this:<br>array<br>  0 =&gt; string &apos;privateMethod&apos; (length=13)<br>  1 =&gt; string &apos;publicMethod&apos; (length=12)<br>  2 =&gt; string &apos;__construct&apos; (length=11)<br><br>C (inside class):<br>array<br>  0 =&gt; string &apos;privateMethod&apos; (length=13)<br>  1 =&gt; string &apos;publicMethod&apos; (length=12)<br>  2 =&gt; string &apos;__construct&apos; (length=11)<br><br>$c:<br>array<br>  0 =&gt; string &apos;publicMethod&apos; (length=12)<br>  1 =&gt; string &apos;__construct&apos; (length=11)<br><br>C (outside class):<br>array<br>  0 =&gt; string &apos;publicMethod&apos; (length=12)<br>  1 =&gt; string &apos;__construct&apos; (length=11)  

#

It is important to note that get_class_methods($class) returns not only methods defined by $class but also the inherited methods.<br><br>There does not appear to be any PHP function to determine which methods are inherited and which are defined explicitly by a class.  

#

[Official documentation page](https://www.php.net/manual/en/function.get-class-methods.php)

**[To root](/README.md)**