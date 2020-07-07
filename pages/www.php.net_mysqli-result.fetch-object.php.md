# mysqli_result::fetch_object





Please mind the difference between objects and arrays in PHP&gt;=5: arrays are by value while objects are by reference.

&lt;?
$o = mysqli_fetch_object($res);
$o1 = $o;
$o1-&gt;value = 10;

$a = mysqli_fetch_array($res);
$a1 = $a;
$a1[&apos;value&apos;] = 10;

echo $o-&gt;value; // 10
echo $a[&apos;value&apos;]; // (original value from db)
?>
```


Should same behaviour be intended, the object needs to be cloned:

&lt;?
$o1 = clone $o;
?>
```


More about object cloning:
http://php.net/manual/en/language.oop5.cloning.php

  

#



Since 5.6.21 and PHP 7.0.6

mysqli_fetch_object() sets the properties of the object AFTER calling the object constructor. Not BEFORE as was in previous versions.

So behaviour has changed. Seems a bug but not sure if was done intentionally.

https://bugs.php.net/bug.php?id=72151

  

#



As indicated in the user comments of the mysql_fetch_object, it is important to realize that class fields get values assigned to them BEFORE the constructor is called.
For example;


```
<?php

class Employee
{
&#xA0; private $id;

&#xA0; public function __construct($id = 0)
&#xA0; {
&#xA0; &#xA0; $this-&gt;id = $id;
&#xA0; }
}

// some code for creating a database connection... i.e. mysqli object
....
$result = $con-&gt;query(&quot;select id, name from employees&quot;);
$anEmployee = $result-&gt;fetch_object(&quot;Employee&quot;);
?>
```

will result in the ID being 0 because it is overridden by the constructor. Therefore, it is useful to check if the class field is already set.
I.e.


```
<?php
class Employee
{
&#xA0; private $id;

&#xA0; public function __construct($id = 0)
&#xA0; {
&#xA0; &#xA0; if (!$this-&gt;id)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0;&#xA0; $this-&gt;id = $id 
&#xA0; &#xA0; }
&#xA0; }
}
?>
```

Also note that the fields which will be assigned by fetch_object are case sensitive. If your table has the field &quot;ID&quot;, it will result in the class field $ID being set. A simple work-around is to use aliases. I.e. &quot;SELECT *, ID as id FROM myTable&quot;
I hope this helps some people.

  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-result.fetch-object.php)

**[To root](/README.md)**