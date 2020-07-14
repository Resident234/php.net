# Properties



In case this saves anyone any time, I spent ages working out why the following didn&apos;t work:<br><br>class MyClass<br>{<br>    private $foo = FALSE;<br><br>    public function __construct()<br>    {<br>        $this-&gt;$foo = TRUE;<br><br>        echo($this-&gt;$foo);<br>    }<br>}<br><br>$bar = new MyClass();<br><br>giving "Fatal error: Cannot access empty property in ...test_class.php on line 8"<br><br>The subtle change of removing the $ before accesses of $foo fixes this:<br><br>class MyClass<br>{<br>    private $foo = FALSE;<br><br>    public function __construct()<br>    {<br>        $this-&gt;foo = TRUE;<br><br>        echo($this-&gt;foo);<br>    }<br>}<br><br>$bar = new MyClass();<br><br>I guess because it&apos;s treating $foo like a variable in the first example, so trying to call $this-&gt;FALSE (or something along those lines) which makes no sense. It&apos;s obvious once you&apos;ve realised, but there aren&apos;t any examples of accessing on this page that show that.  

#

You can access property names with dashes in them (for example, because you converted an XML file to an object) in the following way:<br><br>

```
<?php
$ref = new StdClass();
$ref-&gt;{&apos;ref-type&apos;} = &apos;Journal Article&apos;;
var_dump($ref);
?>
```
  

#

$this can be cast to array.  But when doing so, it prefixes the property names/new array keys with certain data depending on the property classification.  Public property names are not changed.  Protected properties are prefixed with a space-padded &apos;*&apos;.  Private properties are prefixed with the space-padded class name...<br><br>

```
<?php

class test
{
    public $var1 = 1;
    protected $var2 = 2;
    private $var3 = 3;
    static $var4 = 4;
    
    public function toArray()
    {
        return (array) $this;
    }
}

$t = new test;
print_r($t-&gt;toArray());

/* outputs:

Array
(
    [var1] =&gt; 1
    [ * var2] =&gt; 2
    [ test var3] =&gt; 3
)

*/
?>
```
<br><br>This is documented behavior when converting any object to an array (see &lt;/language.types.array.php#language.types.array.casting&gt; PHP manual page).  All properties regardless of visibility will be shown when casting an object to array (with exceptions of a few built-in objects).<br><br>To get an array with all property names unaltered, use the &apos;get_object_vars($this)&apos; function in any method within class scope to retrieve an array of all properties regardless of external visibility, or &apos;get_object_vars($object)&apos; outside class scope to retrieve an array of only public properties (see: &lt;/function.get-object-vars.php&gt; PHP manual page).  

#

Heredoc IS valid as of PHP 5.3 and this is documented in the manual at http://php.net/manual/en/language.types.string.php#language.types.string.syntax.heredoc<br><br>Only heredocs containing variables are invalid because then it becomes dynamic.  

#

Do not confuse php&apos;s version of properties with properties in other languages (C++ for example).  In php, properties are the same as attributes, simple variables without functionality.  They should be called attributes, not properties.<br><br>Properties have implicit accessor and mutator functionality.  I&apos;ve created an abstract class that allows implicit property functionality.<br><br>

```
<?php

abstract class PropertyObject
{
  public function __get($name)
  {
    if (method_exists($this, ($method = &apos;get_&apos;.$name)))
    {
      return $this-&gt;$method();
    }
    else return;
  }
  
  public function __isset($name)
  {
    if (method_exists($this, ($method = &apos;isset_&apos;.$name)))
    {
      return $this-&gt;$method();
    }
    else return;
  }
  
  public function __set($name, $value)
  {
    if (method_exists($this, ($method = &apos;set_&apos;.$name)))
    {
      $this-&gt;$method($value);
    }
  }
  
  public function __unset($name)
  {
    if (method_exists($this, ($method = &apos;unset_&apos;.$name)))
    {
      $this-&gt;$method();
    }
  }
}

?>
```
<br><br>after extending this class, you can create accessors and mutators that will be called automagically, using php&apos;s magic methods, when the corresponding property is accessed.  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.properties.php)

**[To root](/README.md)**