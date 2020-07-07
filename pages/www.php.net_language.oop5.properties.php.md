# Properties





In case this saves anyone any time, I spent ages working out why the following didn&apos;t work:

class MyClass
{
&#xA0; &#xA0; private $foo = FALSE;

&#xA0; &#xA0; public function __construct()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;$foo = TRUE;

&#xA0; &#xA0; &#xA0; &#xA0; echo($this-&gt;$foo);
&#xA0; &#xA0; }
}

$bar = new MyClass();

giving &quot;Fatal error: Cannot access empty property in ...test_class.php on line 8&quot;

The subtle change of removing the $ before accesses of $foo fixes this:

class MyClass
{
&#xA0; &#xA0; private $foo = FALSE;

&#xA0; &#xA0; public function __construct()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;foo = TRUE;

&#xA0; &#xA0; &#xA0; &#xA0; echo($this-&gt;foo);
&#xA0; &#xA0; }
}

$bar = new MyClass();

I guess because it&apos;s treating $foo like a variable in the first example, so trying to call $this-&gt;FALSE (or something along those lines) which makes no sense. It&apos;s obvious once you&apos;ve realised, but there aren&apos;t any examples of accessing on this page that show that.

  

#



You can access property names with dashes in them (for example, because you converted an XML file to an object) in the following way:



```
<?php
$ref = new StdClass();
$ref-&gt;{&apos;ref-type&apos;} = &apos;Journal Article&apos;;
var_dump($ref);
?>
```



  

#



$this can be cast to array.&#xA0; But when doing so, it prefixes the property names/new array keys with certain data depending on the property classification.&#xA0; Public property names are not changed.&#xA0; Protected properties are prefixed with a space-padded &apos;*&apos;.&#xA0; Private properties are prefixed with the space-padded class name...





```
<?php



class test

{

&#xA0; &#xA0; public $var1 = 1;

&#xA0; &#xA0; protected $var2 = 2;

&#xA0; &#xA0; private $var3 = 3;

&#xA0; &#xA0; static $var4 = 4;

&#xA0; &#xA0; 

&#xA0; &#xA0; public function toArray()

&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; return (array) $this;

&#xA0; &#xA0; }

}



$t = new test;

print_r($t-&gt;toArray());



/* outputs:



Array

(

&#xA0; &#xA0; [var1] =&gt; 1

&#xA0; &#xA0; [ * var2] =&gt; 2

&#xA0; &#xA0; [ test var3] =&gt; 3

)



*/

?>
```




This is documented behavior when converting any object to an array (see &lt;/language.types.array.php#language.types.array.casting&gt; PHP manual page).&#xA0; All properties regardless of visibility will be shown when casting an object to array (with exceptions of a few built-in objects).



To get an array with all property names unaltered, use the &apos;get_object_vars($this)&apos; function in any method within class scope to retrieve an array of all properties regardless of external visibility, or &apos;get_object_vars($object)&apos; outside class scope to retrieve an array of only public properties (see: &lt;/function.get-object-vars.php&gt; PHP manual page).

  

#



Heredoc IS valid as of PHP 5.3 and this is documented in the manual at http://php.net/manual/en/language.types.string.php#language.types.string.syntax.heredoc

Only heredocs containing variables are invalid because then it becomes dynamic.

  

#



Do not confuse php&apos;s version of properties with properties in other languages (C++ for example).&#xA0; In php, properties are the same as attributes, simple variables without functionality.&#xA0; They should be called attributes, not properties.

Properties have implicit accessor and mutator functionality.&#xA0; I&apos;ve created an abstract class that allows implicit property functionality.



```
<?php

abstract class PropertyObject
{
&#xA0; public function __get($name)
&#xA0; {
&#xA0; &#xA0; if (method_exists($this, ($method = &apos;get_&apos;.$name)))
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; return $this-&gt;$method();
&#xA0; &#xA0; }
&#xA0; &#xA0; else return;
&#xA0; }
&#xA0; 
&#xA0; public function __isset($name)
&#xA0; {
&#xA0; &#xA0; if (method_exists($this, ($method = &apos;isset_&apos;.$name)))
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; return $this-&gt;$method();
&#xA0; &#xA0; }
&#xA0; &#xA0; else return;
&#xA0; }
&#xA0; 
&#xA0; public function __set($name, $value)
&#xA0; {
&#xA0; &#xA0; if (method_exists($this, ($method = &apos;set_&apos;.$name)))
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; $this-&gt;$method($value);
&#xA0; &#xA0; }
&#xA0; }
&#xA0; 
&#xA0; public function __unset($name)
&#xA0; {
&#xA0; &#xA0; if (method_exists($this, ($method = &apos;unset_&apos;.$name)))
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; $this-&gt;$method();
&#xA0; &#xA0; }
&#xA0; }
}

?>
```


after extending this class, you can create accessors and mutators that will be called automagically, using php&apos;s magic methods, when the corresponding property is accessed.

  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.properties.php)

**[To root](/README.md)**