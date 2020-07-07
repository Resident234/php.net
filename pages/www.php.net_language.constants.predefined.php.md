# Magic constants





the difference between 
__FUNCTION__ and __METHOD__ as in PHP 5.0.4 is that

__FUNCTION__ returns only the name of the function

while as __METHOD__ returns the name of the class alongwith the name of the function

class trick
{
&#xA0; &#xA0; &#xA0; function doit()
&#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo __FUNCTION__;
&#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; function doitagain()
&#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo __METHOD__;
&#xA0; &#xA0; &#xA0; }
}
$obj=new trick();
$obj-&gt;doit();
output will be ----&#xA0; doit
$obj-&gt;doitagain();
output will be ----- trick::doitagain

  

#



The __CLASS__ magic constant nicely complements the get_class() function.

Sometimes you need to know both:
- name of the inherited class
- name of the class actually executed

Here&apos;s an example that shows the possible solution:



```
<?php

class base_class
{
&#xA0; &#xA0; function say_a()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;&apos;a&apos; - said the &quot; . __CLASS__ . &quot;&lt;br/&gt;&quot;;
&#xA0; &#xA0; }

&#xA0; &#xA0; function say_b()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;&apos;b&apos; - said the &quot; . get_class($this) . &quot;&lt;br/&gt;&quot;;
&#xA0; &#xA0; }

}

class derived_class extends base_class
{
&#xA0; &#xA0; function say_a()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; parent::say_a();
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;&apos;a&apos; - said the &quot; . __CLASS__ . &quot;&lt;br/&gt;&quot;;
&#xA0; &#xA0; }

&#xA0; &#xA0; function say_b()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; parent::say_b();
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;&apos;b&apos; - said the &quot; . get_class($this) . &quot;&lt;br/&gt;&quot;;
&#xA0; &#xA0; }
}

$obj_b = new derived_class();

$obj_b-&gt;say_a();
echo &quot;&lt;br/&gt;&quot;;
$obj_b-&gt;say_b();

?>
```


The output should look roughly like this:

&apos;a&apos; - said the base_class
&apos;a&apos; - said the derived_class

&apos;b&apos; - said the derived_class
&apos;b&apos; - said the derived_class

  

#



There is no way to implement a backwards compatible __DIR__ in versions prior to 5.3.0.

The only thing that you can do is to perform a recursive search and replace to dirname(__FILE__):
find . -type f -print0 | xargs -0 sed -i &apos;s/__DIR__/dirname(__FILE__)/&apos;

  

#



Note a small inconsistency when using __CLASS__ and __METHOD__ in traits (stand php 7.0.4): While __CLASS__ is working as advertized and returns dynamically the name of the class the trait is being used in, __METHOD__ will actually prepend the trait name instead of the class name!

  

#

[Official documentation page](https://www.php.net/manual/en/language.constants.predefined.php)

**[To root](/README.md)**