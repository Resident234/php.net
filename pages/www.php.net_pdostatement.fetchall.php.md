# PDOStatement::fetchAll



I still don&apos;t understand why FETCH_KEY_PAIR is not documented here (http://php.net/manual/fr/pdo.constants.php), because it could be very useful!<br><br>

```
<?php
  var_dump($pdo->query('select id, name from table')->fetchAll(PDO::FETCH_KEY_PAIR));
?>
```
<br><br>This will display:<br>array(2) {<br>  [2]=&gt;<br>  string(10) "name2"<br>  [5]=&gt;<br>  string(10) "name5"<br>}  

#

Getting foreach to play nicely with some data from PDO FetchAll()<br>I was not understanding to use the $value part of the foreach properly, I hope this helps someone else.<br>Example:<br>

```
<?php 
$stmt = $this->db->prepare('SELECT title, FMarticle_id FROM articles WHERE domain_name =:domain_name');
            $stmt->bindValue(':domain_name', $domain);
            $stmt->execute();
            $article_list = $stmt->fetchAll(PDO::FETCH_ASSOC);
?>
```

which gives:

array (size=2)
  0 => 
    array (size=2)
      'title' => string 'About Cats Really Long title for the article' (length=44)
      'FMarticle_id' => string '7CAEBB15-6784-3A41-909A-1B6D12667499' (length=36)
  1 => 
    array (size=2)
      'title' => string 'another cat story' (length=17)
      'FMarticle_id' => string '0BB86A06-2A79-3145-8A02-ECF6EA5C405C' (length=36)

Then use:


```
<?php
foreach ($article_list as $row => $link) {
  echo  '&lt;a href="'.  $link['FMarticle_id'].'"&gt;' . $link['title']. '&lt;/a&gt;&lt;/br&gt;';
  }
?>
```
  

#

You might find yourself wanting to use FETCH_GROUP and FETCH_ASSOC at the same time, to get your table&apos;s primary key as the array key:<br>

```
<?php
// $stmt is some query like "SELECT rowid, username, comment"
$results = $stmt->fetchAll(PDO::FETCH_GROUP|PDO::FETCH_ASSOC);

// It does work, but not as you might expect:
$results = array(
    1234 => array(0 => array('username' => 'abc', 'comment' => '[...]')),
    1235 => array(0 => array('username' => 'def', 'comment' => '[...]')),
);

// ...but you can at least strip the useless numbered array out easily:
$results = array_map('reset', $results);
?>
```
  

#

Interestingly enough, when you use fetchAll, the constructor for your object is called AFTER the properties are assigned. For example:<br><br>

```
<?php
class person {
    public $name;

    function __construct() {
       $this->name = $this->name . " is my name.";
    }
}

# set up select from a database here with PDO
$obj = $STH->fetchALL(PDO::FETCH_CLASS, 'person');
?>
```


Will result in ' is my name' being appended to all the name columns. However if you call it slightly differently:



```
<?php
$obj = $obj = $STH->fetchAll(PDO::FETCH_CLASS | PDO::FETCH_PROPS_LATE, 'person');
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

$array = $stmt->fetchAll(PDO::FETCH_ASSOC);

print_r($array);
?>
```


The resulting array will look something like this:

Array
(
    [0] => Array
        (
            [product_id] => 
            [notes] => "this product..."
            [brand] => "Best Yet"
            ...

The fix is to simply specify your field names in the SELECT clause instead of using the * as a wild card, or, you can also specify the field in addition to the *. The following example returns the product_id field correctly:



```
<?php
$sql = "SELECT *, product.product_id FROM product, image
LEFT OUTER JOIN image ON (product.product_id = image.product_id)";

$array = $stmt->fetchAll(PDO::FETCH_ASSOC);

print_r($array);
?>
```
<br><br>The resulting array will look something like this:<br><br>Array<br>(<br>    [0] =&gt; Array<br>        (<br>            [product_id] =&gt; 3<br>            [notes] =&gt; "this product..."<br>            [brand] =&gt; "Best Yet"<br>            ...  

#

to fetch rows grouped by primary id or any other field you may use FETCH_GROUP with FETCH_UNIQUE:<br><br>

```
<?php

//prepare and execute a statement returning multiple rows, on a single one
$stmt = $db->prepare('SELECT id,name,role FROM table');
$stmt->execute();
var_dump($stmt->fetchAll(PDO::FETCH_GROUP | PDO::FETCH_UNIQUE));

//returns an array with the first selected field as key containing associative arrays with the row. This mode takes care not to repeat the key in corresponding grouped array.

$result = array
 (1 => array
   ('name'=>'foo',
    'role'=>'sage',),
  2 => array
   ('name'=>'bar',
    'role'=>'rage',),);

// 'SELECT name,id,role FROM table' would result in that:

$result = array
 ('foo' => array
   ('id'=>1,
    'role'=>'sage',),
  'bar' => array
   ('id'=>2,
    'role'=>'rage',),);

?>
```
  

#

If no rows have been returned, fetchAll returns an empty array.  

#

Note that fetchAll() can be extremely memory inefficient for large data sets. My memory limit was set to 160 MB this is what happened when I tried:<br><br>

```
<?php
$arr = $stmt->fetchAll();
// Fatal error: Allowed memory size of 16777216 bytes exhausted
?>
```


If you are going to loop through the output array of fetchAll(), instead use fetch() to minimize memory usage as follows:



```
<?php
while ($arr = $stmt->fetch()) {
    echo round(memory_get_usage() / (1024*1024),3) .' MB&lt;br /&gt;';
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
$stmt = $dbh->prepare("SELECT * FROM tablename");
$stmt->execute();
$arrValues = $stmt->fetchAll(PDO::FETCH_ASSOC);
// open the table
print "&lt;table wdith=\"100%\"&gt;\n";
print "&lt;tr&gt;\n";
// add the table headers
foreach ($arrValues[0] as $key => $useless){
    print "&lt;th&gt;$key&lt;/th&gt;";
}
print "&lt;/tr&gt;";
// display data
foreach ($arrValues as $row){
    print "&lt;tr&gt;";
    foreach ($row as $key => $val){
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