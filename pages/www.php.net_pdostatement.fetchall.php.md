# PDOStatement::fetchAll



I still don&apos;t understand why FETCH_KEY_PAIR is not documented here (http://php.net/manual/fr/pdo.constants.php), because it could be very useful!<br><br>

```
<?php
  var_dump($pdo-&gt;query(&apos;select id, name from table&apos;)-&gt;fetchAll(PDO::FETCH_KEY_PAIR));
?>
```
<br><br>This will display:<br>array(2) {<br>  [2]=&gt;<br>  string(10) "name2"<br>  [5]=&gt;<br>  string(10) "name5"<br>}  

#

Getting foreach to play nicely with some data from PDO FetchAll()<br>I was not understanding to use the $value part of the foreach properly, I hope this helps someone else.<br>Example:<br>

```
<?php 
$stmt = $this-&gt;db-&gt;prepare(&apos;SELECT title, FMarticle_id FROM articles WHERE domain_name =:domain_name&apos;);
            $stmt-&gt;bindValue(&apos;:domain_name&apos;, $domain);
            $stmt-&gt;execute();
            $article_list = $stmt-&gt;fetchAll(PDO::FETCH_ASSOC);
?>
```

which gives:

array (size=2)
  0 =&gt; 
    array (size=2)
      &apos;title&apos; =&gt; string &apos;About Cats Really Long title for the article&apos; (length=44)
      &apos;FMarticle_id&apos; =&gt; string &apos;7CAEBB15-6784-3A41-909A-1B6D12667499&apos; (length=36)
  1 =&gt; 
    array (size=2)
      &apos;title&apos; =&gt; string &apos;another cat story&apos; (length=17)
      &apos;FMarticle_id&apos; =&gt; string &apos;0BB86A06-2A79-3145-8A02-ECF6EA5C405C&apos; (length=36)

Then use:


```
<?php
foreach ($article_list as $row =&gt; $link) {
  echo  &apos;&lt;a href="&apos;.  $link[&apos;FMarticle_id&apos;].&apos;"&gt;&apos; . $link[&apos;title&apos;]. &apos;&lt;/a&gt;&lt;/br&gt;&apos;;
  }
?>
```
  

#

You might find yourself wanting to use FETCH_GROUP and FETCH_ASSOC at the same time, to get your table&apos;s primary key as the array key:<br>

```
<?php
// $stmt is some query like "SELECT rowid, username, comment"
$results = $stmt-&gt;fetchAll(PDO::FETCH_GROUP|PDO::FETCH_ASSOC);

// It does work, but not as you might expect:
$results = array(
    1234 =&gt; array(0 =&gt; array(&apos;username&apos; =&gt; &apos;abc&apos;, &apos;comment&apos; =&gt; &apos;[...]&apos;)),
    1235 =&gt; array(0 =&gt; array(&apos;username&apos; =&gt; &apos;def&apos;, &apos;comment&apos; =&gt; &apos;[...]&apos;)),
);

// ...but you can at least strip the useless numbered array out easily:
$results = array_map(&apos;reset&apos;, $results);
?>
```
  

#

Interestingly enough, when you use fetchAll, the constructor for your object is called AFTER the properties are assigned. For example:<br><br>

```
<?php
class person {
    public $name;

    function __construct() {
       $this-&gt;name = $this-&gt;name . " is my name.";
    }
}

# set up select from a database here with PDO
$obj = $STH-&gt;fetchALL(PDO::FETCH_CLASS, &apos;person&apos;);
?>
```


Will result in &apos; is my name&apos; being appended to all the name columns. However if you call it slightly differently:



```
<?php
$obj = $obj = $STH-&gt;fetchAll(PDO::FETCH_CLASS | PDO::FETCH_PROPS_LATE, &apos;person&apos;);
?>
```
<br><br>Then the constructor will be called before properties are assigned. I can&apos;t find this documented anywhere, so I thought it would be nice to add a note here.  

#

PLEASE BE AWARE: If you do an OUTER LEFT JOIN and set PDO FetchALL to PDO::FETCH_ASSOC, any primary key you used in the OUTER LEFT JOIN will be set to a blank if there are no records returned in the JOIN.<br><br>For example:<br>

```
<?php
//query the product table and join to the image table and return any images, if we have any, for each product
$sql = "SELECT * FROM product, image
LEFT OUTER JOIN image ON (product.product_id = image.product_id)";

$array = $stmt-&gt;fetchAll(PDO::FETCH_ASSOC);

print_r($array);
?>
```


The resulting array will look something like this:

Array
(
    [0] =&gt; Array
        (
            [product_id] =&gt; 
            [notes] =&gt; "this product..."
            [brand] =&gt; "Best Yet"
            ...

The fix is to simply specify your field names in the SELECT clause instead of using the * as a wild card, or, you can also specify the field in addition to the *. The following example returns the product_id field correctly:



```
<?php
$sql = "SELECT *, product.product_id FROM product, image
LEFT OUTER JOIN image ON (product.product_id = image.product_id)";

$array = $stmt-&gt;fetchAll(PDO::FETCH_ASSOC);

print_r($array);
?>
```
<br><br>The resulting array will look something like this:<br><br>Array<br>(<br>    [0] =&gt; Array<br>        (<br>            [product_id] =&gt; 3<br>            [notes] =&gt; "this product..."<br>            [brand] =&gt; "Best Yet"<br>            ...  

#

to fetch rows grouped by primary id or any other field you may use FETCH_GROUP with FETCH_UNIQUE:<br><br>

```
<?php

//prepare and execute a statement returning multiple rows, on a single one
$stmt = $db-&gt;prepare(&apos;SELECT id,name,role FROM table&apos;);
$stmt-&gt;execute();
var_dump($stmt-&gt;fetchAll(PDO::FETCH_GROUP | PDO::FETCH_UNIQUE));

//returns an array with the first selected field as key containing associative arrays with the row. This mode takes care not to repeat the key in corresponding grouped array.

$result = array
 (1 =&gt; array
   (&apos;name&apos;=&gt;&apos;foo&apos;,
    &apos;role&apos;=&gt;&apos;sage&apos;,),
  2 =&gt; array
   (&apos;name&apos;=&gt;&apos;bar&apos;,
    &apos;role&apos;=&gt;&apos;rage&apos;,),);

// &apos;SELECT name,id,role FROM table&apos; would result in that:

$result = array
 (&apos;foo&apos; =&gt; array
   (&apos;id&apos;=&gt;1,
    &apos;role&apos;=&gt;&apos;sage&apos;,),
  &apos;bar&apos; =&gt; array
   (&apos;id&apos;=&gt;2,
    &apos;role&apos;=&gt;&apos;rage&apos;,),);

?>
```
  

#

If no rows have been returned, fetchAll returns an empty array.  

#

Note that fetchAll() can be extremely memory inefficient for large data sets. My memory limit was set to 160 MB this is what happened when I tried:<br><br>

```
<?php
$arr = $stmt-&gt;fetchAll();
// Fatal error: Allowed memory size of 16777216 bytes exhausted
?>
```


If you are going to loop through the output array of fetchAll(), instead use fetch() to minimize memory usage as follows:



```
<?php
while ($arr = $stmt-&gt;fetch()) {
    echo round(memory_get_usage() / (1024*1024),3) .&apos; MB&lt;br /&gt;&apos;;
    // do_other_stuff();
}
// Last line for the same query shows only 28.973 MB usage
?>
```
  

#

There is also another fetch mode supported on Oracle and MSSQL: <br>PDO::FETCH_ASSOC<br><br>&gt; fetches only column names and omits the numeric index.<br><br>If you would like to return all columns from an sql statement with column keys as table headers, it&apos;s as simple as this:<br><br>

```
<?php
$dbh = new PDO("DS", "USERNAME", "PASSWORD");
$stmt = $dbh-&gt;prepare("SELECT * FROM tablename");
$stmt-&gt;execute();
$arrValues = $stmt-&gt;fetchAll(PDO::FETCH_ASSOC);
// open the table
print "&lt;table wdith=\"100%\"&gt;\n";
print "&lt;tr&gt;\n";
// add the table headers
foreach ($arrValues[0] as $key =&gt; $useless){
    print "&lt;th&gt;$key&lt;/th&gt;";
}
print "&lt;/tr&gt;";
// display data
foreach ($arrValues as $row){
    print "&lt;tr&gt;";
    foreach ($row as $key =&gt; $val){
        print "&lt;td&gt;$val&lt;/td&gt;";
    }
    print "&lt;/tr&gt;\n";
}
// close the table
print "&lt;/table&gt;\n";
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.fetchall.php)

**[To root](/README.md)**