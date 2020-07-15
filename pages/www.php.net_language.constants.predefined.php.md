# Magic constants



the difference between <br>__FUNCTION__ and __METHOD__ as in PHP 5.0.4 is that<br><br>__FUNCTION__ returns only the name of the function<br><br>while as __METHOD__ returns the name of the class alongwith the name of the function<br><br>class trick<br>{<br>      function doit()<br>      {<br>                echo __FUNCTION__;<br>      }<br>      function doitagain()<br>      {<br>                echo __METHOD__;<br>      }<br>}<br>$obj=new trick();<br>$obj-&gt;doit();<br>output will be ----  doit<br>$obj-&gt;doitagain();<br>output will be ----- trick::doitagain  

#

The __CLASS__ magic constant nicely complements the get_class() function.<br><br>Sometimes you need to know both:<br>- name of the inherited class<br>- name of the class actually executed<br><br>Here&apos;s an example that shows the possible solution:<br><br>

```
<?php

class base_class
{
    function say_a()
    {
        echo "'a' - said the " . __CLASS__ . "<br/>";
    }

    function say_b()
    {
        echo "'b' - said the " . get_class($this) . "<br/>";
    }

}

class derived_class extends base_class
{
    function say_a()
    {
        parent::say_a();
        echo "'a' - said the " . __CLASS__ . "<br/>";
    }

    function say_b()
    {
        parent::say_b();
        echo "'b' - said the " . get_class($this) . "<br/>";
    }
}

$obj_b = new derived_class();

$obj_b->say_a();
echo "<br/>";
$obj_b->say_b();

?>
```
<br><br>The output should look roughly like this:<br><br>&apos;a&apos; - said the base_class<br>&apos;a&apos; - said the derived_class<br><br>&apos;b&apos; - said the derived_class<br>&apos;b&apos; - said the derived_class  

#

There is no way to implement a backwards compatible __DIR__ in versions prior to 5.3.0.<br><br>The only thing that you can do is to perform a recursive search and replace to dirname(__FILE__):<br>find . -type f -print0 | xargs -0 sed -i &apos;s/__DIR__/dirname(__FILE__)/&apos;  

#

Note a small inconsistency when using __CLASS__ and __METHOD__ in traits (stand php 7.0.4): While __CLASS__ is working as advertized and returns dynamically the name of the class the trait is being used in, __METHOD__ will actually prepend the trait name instead of the class name!  

#

[Official documentation page](https://www.php.net/manual/en/language.constants.predefined.php)

**[To root](/README.md)**