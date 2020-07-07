# PDOStatement::fetchAll





I still don&apos;t understand why FETCH_KEY_PAIR is not documented here (http://php.net/manual/fr/pdo.constants.php), because it could be very useful!



```
<?php
&#xA0; var_dump($pdo-&gt;query(&apos;select id, name from table&apos;)-&gt;fetchAll(PDO::FETCH_KEY_PAIR));
?>
```


This will display:
array(2) {
&#xA0; [2]=&gt;
&#xA0; string(10) &quot;name2&quot;
&#xA0; [5]=&gt;
&#xA0; string(10) &quot;name5&quot;
}

  

#



Getting foreach to play nicely with some data from PDO FetchAll()
I was not understanding to use the $value part of the foreach properly, I hope this helps someone else.
Example:


```
<?php 
$stmt = $this-&gt;db-&gt;prepare(&apos;SELECT title, FMarticle_id FROM articles WHERE domain_name =:domain_name&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $stmt-&gt;bindValue(&apos;:domain_name&apos;, $domain);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $stmt-&gt;execute();
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $article_list = $stmt-&gt;fetchAll(PDO::FETCH_ASSOC);
?>
```

which gives:

array (size=2)
&#xA0; 0 =&gt; 
&#xA0; &#xA0; array (size=2)
&#xA0; &#xA0; &#xA0; &apos;title&apos; =&gt; string &apos;About Cats Really Long title for the article&apos; (length=44)
&#xA0; &#xA0; &#xA0; &apos;FMarticle_id&apos; =&gt; string &apos;7CAEBB15-6784-3A41-909A-1B6D12667499&apos; (length=36)
&#xA0; 1 =&gt; 
&#xA0; &#xA0; array (size=2)
&#xA0; &#xA0; &#xA0; &apos;title&apos; =&gt; string &apos;another cat story&apos; (length=17)
&#xA0; &#xA0; &#xA0; &apos;FMarticle_id&apos; =&gt; string &apos;0BB86A06-2A79-3145-8A02-ECF6EA5C405C&apos; (length=36)

Then use:


```
<?php
foreach ($article_list as $row =&gt; $link) {
&#xA0; echo&#xA0; &apos;&lt;a href=&quot;&apos;.&#xA0; $link[&apos;FMarticle_id&apos;].&apos;&quot;&gt;&apos; . $link[&apos;title&apos;]. &apos;&lt;/a&gt;&lt;/br&gt;&apos;;
&#xA0; }
?>
```



  

#



You might find yourself wanting to use FETCH_GROUP and FETCH_ASSOC at the same time, to get your table&apos;s primary key as the array key:


```
<?php
// $stmt is some query like &quot;SELECT rowid, username, comment&quot;
$results = $stmt-&gt;fetchAll(PDO::FETCH_GROUP|PDO::FETCH_ASSOC);

// It does work, but not as you might expect:
$results = array(
&#xA0; &#xA0; 1234 =&gt; array(0 =&gt; array(&apos;username&apos; =&gt; &apos;abc&apos;, &apos;comment&apos; =&gt; &apos;[...]&apos;)),
&#xA0; &#xA0; 1235 =&gt; array(0 =&gt; array(&apos;username&apos; =&gt; &apos;def&apos;, &apos;comment&apos; =&gt; &apos;[...]&apos;)),
);

// ...but you can at least strip the useless numbered array out easily:
$results = array_map(&apos;reset&apos;, $results);
?>
```



  

#



Interestingly enough, when you use fetchAll, the constructor for your object is called AFTER the properties are assigned. For example:



```
<?php
class person {
&#xA0; &#xA0; public $name;

&#xA0; &#xA0; function __construct() {
&#xA0; &#xA0; &#xA0;&#xA0; $this-&gt;name = $this-&gt;name . &quot; is my name.&quot;;
&#xA0; &#xA0; }
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


Then the constructor will be called before properties are assigned. I can&apos;t find this documented anywhere, so I thought it would be nice to add a note here.

  

#



PLEASE BE AWARE: If you do an OUTER LEFT JOIN and set PDO FetchALL to PDO::FETCH_ASSOC, any primary key you used in the OUTER LEFT JOIN will be set to a blank if there are no records returned in the JOIN.

For example:


```
<?php
//query the product table and join to the image table and return any images, if we have any, for each product
$sql = &quot;SELECT * FROM product, image
LEFT OUTER JOIN image ON (product.product_id = image.product_id)&quot;;

$array = $stmt-&gt;fetchAll(PDO::FETCH_ASSOC);

print_r($array);
?>
```


The resulting array will look something like this:

Array
(
&#xA0; &#xA0; [0] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [product_id] =&gt; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [notes] =&gt; &quot;this product...&quot;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [brand] =&gt; &quot;Best Yet&quot;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ...

The fix is to simply specify your field names in the SELECT clause instead of using the * as a wild card, or, you can also specify the field in addition to the *. The following example returns the product_id field correctly:



```
<?php
$sql = &quot;SELECT *, product.product_id FROM product, image
LEFT OUTER JOIN image ON (product.product_id = image.product_id)&quot;;

$array = $stmt-&gt;fetchAll(PDO::FETCH_ASSOC);

print_r($array);
?>
```


The resulting array will look something like this:

Array
(
&#xA0; &#xA0; [0] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [product_id] =&gt; 3
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [notes] =&gt; &quot;this product...&quot;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [brand] =&gt; &quot;Best Yet&quot;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ...

  

#



to fetch rows grouped by primary id or any other field you may use FETCH_GROUP with FETCH_UNIQUE:



```
<?php

//prepare and execute a statement returning multiple rows, on a single one
$stmt = $db-&gt;prepare(&apos;SELECT id,name,role FROM table&apos;);
$stmt-&gt;execute();
var_dump($stmt-&gt;fetchAll(PDO::FETCH_GROUP | PDO::FETCH_UNIQUE));

//returns an array with the first selected field as key containing associative arrays with the row. This mode takes care not to repeat the key in corresponding grouped array.

$result = array
 (1 =&gt; array
&#xA0;&#xA0; (&apos;name&apos;=&gt;&apos;foo&apos;,
&#xA0; &#xA0; &apos;role&apos;=&gt;&apos;sage&apos;,),
&#xA0; 2 =&gt; array
&#xA0;&#xA0; (&apos;name&apos;=&gt;&apos;bar&apos;,
&#xA0; &#xA0; &apos;role&apos;=&gt;&apos;rage&apos;,),);

// &apos;SELECT name,id,role FROM table&apos; would result in that:

$result = array
 (&apos;foo&apos; =&gt; array
&#xA0;&#xA0; (&apos;id&apos;=&gt;1,
&#xA0; &#xA0; &apos;role&apos;=&gt;&apos;sage&apos;,),
&#xA0; &apos;bar&apos; =&gt; array
&#xA0;&#xA0; (&apos;id&apos;=&gt;2,
&#xA0; &#xA0; &apos;role&apos;=&gt;&apos;rage&apos;,),);

?>
```



  

#



If no rows have been returned, fetchAll returns an empty array.

  

#



Note that fetchAll() can be extremely memory inefficient for large data sets. My memory limit was set to 160 MB this is what happened when I tried:





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

&#xA0; &#xA0; echo round(memory_get_usage() / (1024*1024),3) .&apos; MB&lt;br /&gt;&apos;;

&#xA0; &#xA0; // do_other_stuff();

}

// Last line for the same query shows only 28.973 MB usage

?>
```



  

#



There is also another fetch mode supported on Oracle and MSSQL: 
PDO::FETCH_ASSOC

&gt; fetches only column names and omits the numeric index.

If you would like to return all columns from an sql statement with column keys as table headers, it&apos;s as simple as this:



```
<?php
$dbh = new PDO(&quot;DS&quot;, &quot;USERNAME&quot;, &quot;PASSWORD&quot;);
$stmt = $dbh-&gt;prepare(&quot;SELECT * FROM tablename&quot;);
$stmt-&gt;execute();
$arrValues = $stmt-&gt;fetchAll(PDO::FETCH_ASSOC);
// open the table
print &quot;&lt;table wdith=\&quot;100%\&quot;&gt;\n&quot;;
print &quot;&lt;tr&gt;\n&quot;;
// add the table headers
foreach ($arrValues[0] as $key =&gt; $useless){
&#xA0; &#xA0; print &quot;&lt;th&gt;$key&lt;/th&gt;&quot;;
}
print &quot;&lt;/tr&gt;&quot;;
// display data
foreach ($arrValues as $row){
&#xA0; &#xA0; print &quot;&lt;tr&gt;&quot;;
&#xA0; &#xA0; foreach ($row as $key =&gt; $val){
&#xA0; &#xA0; &#xA0; &#xA0; print &quot;&lt;td&gt;$val&lt;/td&gt;&quot;;
&#xA0; &#xA0; }
&#xA0; &#xA0; print &quot;&lt;/tr&gt;\n&quot;;
}
// close the table
print &quot;&lt;/table&gt;\n&quot;;
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.fetchall.php)

**[To root](/README.md)**