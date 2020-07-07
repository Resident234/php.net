# mysqli_result::fetch_assoc





I often like to have my results sent elsewhere in the format of an array (although keep in mind that if you just plan on traversing through the array in another part of the script, this extra step is just a waste of time).

This is my one-liner for transforming a mysqli_result set into an array.


```
<?php
$sql = new MySQLi($host, $username, $password, $database);

$result = $sql-&gt;query(&quot;SELECT * FROM `$table`;&quot;);
for ($set = array (); $row = $result-&gt;fetch_assoc(); $set[] = $row);
print_r($set);
?>
```


Outputs:
Array
(
&#xA0; &#xA0; [0] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [id] =&gt; 1
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [field2] =&gt; a
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [field3] =&gt; b
&#xA0; &#xA0; &#xA0; &#xA0; ),
&#xA0; &#xA0; [1] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [id] =&gt; 2
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [field2] =&gt; c
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [field3] =&gt; d
&#xA0; &#xA0; &#xA0; &#xA0; )
)

I use other variations to adapt to the situation, i.e. if I am selecting only one field:


```
<?php
$sql = new MySQLi($host, $username, $password, $database);
$result = $sql-&gt;query(&quot;SELECT `field2` FROM `$table`;&quot;);
for ($set = array (); $row = $result-&gt;fetch_assoc(); $set[] = $row[&apos;field2&apos;]);
print_r($set);
?>
```

Outputs:
Array
(
&#xA0; &#xA0; [0] =&gt; a
&#xA0; &#xA0; [1] =&gt; c
)

Or, to make the array associative with the primary index (code assumes primary index is the first field in the table):


```
<?php
$sql = new MySQLi($host, $username, $password, $database);
$result = $sql-&gt;query(&quot;SELECT * FROM `$table`;&quot;);
for ($set = array (); $row = $result-&gt;fetch_assoc(); $set[array_shift($row)] = $row);
print_r($set);
?>
```

Outputs:
Array
(
&#xA0; &#xA0; [1] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [field2] =&gt; a
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [field3] =&gt; b
&#xA0; &#xA0; &#xA0; &#xA0; ),
&#xA0; &#xA0; [2] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [field2] =&gt; c
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [field3] =&gt; d
&#xA0; &#xA0; &#xA0; &#xA0; )
)

  

#



IMPORTANT NOTE:



If you were used to using code like this:





```
<?php

while(false !== ($row = mysql_fetch_assoc($result)))

{

&#xA0; &#xA0; //...

}

?>
```




You must change it to this for mysqli:





```
<?php

while(null !== ($row = mysqli_fetch_assoc($result)))

{

&#xA0; &#xA0; //...

}

?>
```




The former will cause your script to run until max_execution_time is reached.

  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-result.fetch-assoc.php)

**[To root](/README.md)**