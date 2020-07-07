# Returning References





a little addition to the example of pixel at minikomp dot com here below


```
<?php

&#xA0; &#xA0; function &amp;func(){
&#xA0; &#xA0; &#xA0; &#xA0; static $static = 0;
&#xA0; &#xA0; &#xA0; &#xA0; $static++;
&#xA0; &#xA0; &#xA0; &#xA0; return $static;
&#xA0; &#xA0; }

&#xA0; &#xA0; $var1 =&amp; func();
&#xA0; &#xA0; echo &quot;var1:&quot;, $var1; // 1
&#xA0; &#xA0; func();
&#xA0; &#xA0; func();
&#xA0; &#xA0; echo &quot;var1:&quot;, $var1; // 3
&#xA0; &#xA0; $var2 = func(); // assignment without the &amp;
&#xA0; &#xA0; echo &quot;var2:&quot;, $var2; // 4
&#xA0; &#xA0; func();
&#xA0; &#xA0; func();
&#xA0; &#xA0; echo &quot;var1:&quot;, $var1; // 6
&#xA0; &#xA0; echo &quot;var2:&quot;, $var2; // still 4

?>
```



  

#



Sometimes, you would like to return NULL with a function returning reference, to indicate the end of chain of elements. However this generates E_NOTICE. Here is little tip, how to prevent that:



```
<?php
class Foo {
&#xA0;&#xA0; const $nullGuard = NULL;
&#xA0;&#xA0; // ... some declarations and definitions
&#xA0;&#xA0; public function &amp;next() {
&#xA0; &#xA0; &#xA0; // ...
&#xA0; &#xA0; &#xA0; if (!$end) return $bar;
&#xA0; &#xA0; &#xA0; else return $this-&gt;nullGuard;
&#xA0;&#xA0; }
}
?>
```


by doing this you can do smth like this without notices:



```
<?php
$f = new Foo();
// ...
while (($item = $f-&gt;next()) != NULL) {
// ...
}
?>
```


you may also use global variable:
global $nullGuard;
return $nullGuard;

  

#



I haven&apos;t seen anyone note method chaining in PHP5.&#xA0; When an object is returned by a method in PHP5 it is returned by default as a reference, and the new Zend Engine 2 allows you to chain method calls from those returned objects.&#xA0; For example consider this code:



```
<?php

class Foo {

&#xA0; &#xA0; protected $bar;

&#xA0; &#xA0; public function __construct() {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;bar = new Bar();

&#xA0; &#xA0; &#xA0; &#xA0; print &quot;Foo\n&quot;;
&#xA0; &#xA0; }&#xA0; &#xA0; 
&#xA0; &#xA0; 
&#xA0; &#xA0; public function getBar() {
&#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;bar;
&#xA0; &#xA0; }
}

class Bar {

&#xA0; &#xA0; public function __construct() {
&#xA0; &#xA0; &#xA0; &#xA0; print &quot;Bar\n&quot;;
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; public function helloWorld() {
&#xA0; &#xA0; &#xA0; &#xA0; print &quot;Hello World\n&quot;;
&#xA0; &#xA0; }
}

function test() {
&#xA0; &#xA0; return new Foo();
}

test()-&gt;getBar()-&gt;helloWorld();

?>
```


Notice how we called test() which was not on an object, but returned an instance of Foo, followed by a method on Foo, getBar() which returned an instance of Bar and finally called one of its methods helloWorld().&#xA0; Those familiar with other interpretive languages (Java to name one) will recognize this functionality.&#xA0; For whatever reason this change doesn&apos;t seem to be documented very well, so hopefully someone will find this helpful.

  

#



An example of returning references:

&lt;?

$var = 1;
$num = NULL;

function &amp;blah()
{
&#xA0; &#xA0; $var =&amp; $GLOBALS[&quot;var&quot;]; # the same as global $var;
&#xA0; &#xA0; $var++;
&#xA0; &#xA0; return $var;
}

$num = &amp;blah();

echo $num; # 2

blah();

echo $num; # 3

?>
```


Note: if you take the &amp; off from the function, the second echo will be 2, because without &amp; the var $num contains its returning value and not its returning reference.

  

#

[Official documentation page](https://www.php.net/manual/en/language.references.return.php)

**[To root](/README.md)**