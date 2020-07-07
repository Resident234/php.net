# Name resolution rules





If you like to declare an __autoload function within a namespace or class, use the spl_autoload_register() function to register it and it will work fine.

  

#



The term &quot;autoload&quot; mentioned here shall not be confused with __autoload function to autoload objects. Regarding the __autoload and namespaces&apos; resolution I&apos;d like to share the following experience:

-&gt;Say you have the following directory structure:

- root
&#xA0; &#xA0; &#xA0; | - loader.php 
&#xA0; &#xA0; &#xA0; | - ns
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | - foo.php

-&gt;foo.php



```
<?php
namespace ns;
class foo
{
&#xA0; &#xA0; public $say;
&#xA0; &#xA0; 
&#xA0; &#xA0; public function __construct()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;say = &quot;bar&quot;;
&#xA0; &#xA0; }
&#xA0; &#xA0; 
}
?>
```


-&gt; loader.php



```
<?php
//GLOBAL SPACE &lt;--
function __autoload($c)
{
&#xA0; &#xA0; require_once $c . &quot;.php&quot;;
}

class foo extends ns\foo // ns\foo is loaded here
{
&#xA0; &#xA0; public function __construct()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; parent::__construct();
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;&lt;br /&gt;foo&quot; . $this-&gt;say;
&#xA0; &#xA0; }
}
$a = new ns\foo(); // ns\foo also loads ns/foo.php just fine here.
echo $a-&gt;say;&#xA0;&#xA0; // prints bar as expected.
$b = new foo;&#xA0; // prints foobar just fine.
?>
```


If you keep your directory/file matching namespace/class consistence the object __autoload works fine.
But... if you try to give loader.php a namespace you&apos;ll obviously get fatal errors. 
My sample is just 1 level dir, but I&apos;ve tested with a very complex and deeper structure. Hope anybody finds this useful.

Cheers!

  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.rules.php)

**[To root](/README.md)**