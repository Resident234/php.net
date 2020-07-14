# implode



it should be noted that an array with one or no elements works fine. for example:<br><br>

```
<?php
    $a1 = array("1","2","3");
    $a2 = array("a");
    $a3 = array();
    
    echo "a1 is: &apos;".implode("&apos;,&apos;",$a1)."&apos;&lt;br&gt;";
    echo "a2 is: &apos;".implode("&apos;,&apos;",$a2)."&apos;&lt;br&gt;";
    echo "a3 is: &apos;".implode("&apos;,&apos;",$a3)."&apos;&lt;br&gt;";
?>
```
<br><br>will produce:<br>===========<br>a1 is: &apos;1&apos;,&apos;2&apos;,&apos;3&apos;<br>a2 is: &apos;a&apos;<br>a3 is: &apos;&apos;  

#

It&apos;s not obvious from the samples, if/how associative arrays are handled. The "implode" function acts on the array "values", disregarding any keys:<br><br>

```
<?php
declare(strict_types=1);

$a = array( &apos;one&apos;,&apos;two&apos;,&apos;three&apos; );
$b = array( &apos;1st&apos; =&gt; &apos;four&apos;, &apos;five&apos;, &apos;3rd&apos; =&gt; &apos;six&apos; );

echo implode( &apos;,&apos;, $a ),&apos;/&apos;, implode( &apos;,&apos;, $b );
?>
```
<br><br>outputs:<br>one,two,three/four,five,six  

#

Can also be used for building tags or complex lists, like the following:<br><br>

```
<?php

$elements = array(&apos;a&apos;, &apos;b&apos;, &apos;c&apos;);

echo "&lt;ul&gt;&lt;li&gt;" . implode("&lt;/li&gt;&lt;li&gt;", $elements) . "&lt;/li&gt;&lt;/ul&gt;";

?>
```
<br><br>This is just an example, you can create a lot more just finding the right glue! ;)  

#

It might be worthwhile noting that the array supplied to implode() can contain objects, provided the objects implement the __toString() method.<br><br>Example:<br>

```
<?php

class Foo
{
    protected $title;

    public function __construct($title)
    {
        $this-&gt;title = $title;
    }

    public function __toString()
    {
        return $this-&gt;title;
    }
}

$array = [
    new Foo(&apos;foo&apos;),
    new Foo(&apos;bar&apos;),
    new Foo(&apos;qux&apos;)
];

echo implode(&apos;; &apos;, $array);
?>
```
<br><br>will output:<br><br>foo; bar; qux  

#

If you want to implode an array of booleans, you will get a strange result:<br>

```
<?php
var_dump(implode(&apos;&apos;,array(true, true, false, false, true)));
?>
```
<br><br>Output:<br>string(3) "111"<br><br>TRUE became "1", FALSE became nothing.  

#

Also quite handy in INSERT statements:<br><br>

```
<?php

   // array containing data
   $array = array(
      "name" =&gt; "John",
      "surname" =&gt; "Doe",
      "email" =&gt; "j.doe@intelligence.gov"
   );

   // build query...
   $sql  = "INSERT INTO table";

   // implode keys of $array...
   $sql .= " (`".implode("`, `", array_keys($array))."`)";

   // implode values of $array...
   $sql .= " VALUES (&apos;".implode("&apos;, &apos;", $array)."&apos;) ";

   // execute query...
   $result = mysql_query($sql) or die(mysql_error());

?>
```
  

#

It may be worth noting that if you accidentally call implode on a string rather than an array, you do NOT get your string back, you get NULL:<br>

```
<?php
var_dump(implode(&apos;:&apos;, &apos;xxxxx&apos;));
?>
```
<br>returns<br>NULL<br><br>This threw me for a little while.  

#

Even handier if you use the following:<br><br>

```
<?php
$id_nums = array(1,6,12,18,24);

$id_nums = implode(", ", $id_nums);
                
$sqlquery = "Select name,email,phone from usertable where user_id IN ($id_nums)";

// $sqlquery becomes "Select name,email,phone from usertable where user_id IN (1,6,12,18,24)"
?>
```
<br><br>Be sure to escape/sanitize/use prepared statements if you get the ids from users.  

#

[Official documentation page](https://www.php.net/manual/en/function.implode.php)

**[To root](/README.md)**