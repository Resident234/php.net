# mysqli::query





This may or may not be obvious to people but perhaps it will help someone.



When running joins in SQL you may encounter a problem if you are trying to pull two columns with the same name. mysqli returns the last in the query when called by name. So to get what you need you can use an alias.



Below I am trying to join a user id with a user role. in the first table (tbl_usr), role is a number and in the second is a&#xA0; text name (tbl_memrole is a lookup table). If I call them both as role I get the text as it is the last &quot;role&quot; in the query. If I use an alias then I get both as desired as shown below.





```
<?php

$sql = &quot;SELECT a.uid, a.role AS roleid, b.role, 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; FROM tbl_usr a

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; INNER JOIN tbl_memrole b

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ON a.role = b.id

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;;



&#xA0; &#xA0; if ($result = $mysqli-&gt;query($sql)) {

&#xA0; &#xA0; &#xA0; &#xA0; while($obj = $result-&gt;fetch_object()){

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $line.=$obj-&gt;uid;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $line.=$obj-&gt;role;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $line.=$obj-&gt;roleid;

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; }

&#xA0; &#xA0; $result-&gt;close();

&#xA0; &#xA0; unset($obj);

&#xA0; &#xA0; unset($sql);

&#xA0; &#xA0; unset($query);

&#xA0; &#xA0; 

?>
```


In this situation I guess I could have just renamed the role column in the first table roleid and that would have taken care of it, but it was a learning experience.

  

#



When calling multiple stored procedures, you can run into the following error: &quot;Commands out of sync; you can&apos;t run this command now&quot;.
This can happen even when using the close() function on the result object between calls. 
To fix the problem, remember to call the next_result() function on the mysqli object after each stored procedure call. See example below:



```
<?php
// New Connection
$db = new mysqli(&apos;localhost&apos;,&apos;user&apos;,&apos;pass&apos;,&apos;database&apos;);

// Check for errors
if(mysqli_connect_errno()){
 echo mysqli_connect_error();
}

// 1st Query
$result = $db-&gt;query(&quot;call getUsers()&quot;);
if($result){
&#xA0; &#xA0;&#xA0; // Cycle through results
&#xA0; &#xA0; while ($row = $result-&gt;fetch_object()){
&#xA0; &#xA0; &#xA0; &#xA0; $user_arr[] = $row;
&#xA0; &#xA0; }
&#xA0; &#xA0; // Free result set
&#xA0; &#xA0; $result-&gt;close();
&#xA0; &#xA0; $db-&gt;next_result();
}

// 2nd Query
$result = $db-&gt;query(&quot;call getGroups()&quot;);
if($result){
&#xA0; &#xA0;&#xA0; // Cycle through results
&#xA0; &#xA0; while ($row = $result-&gt;fetch_object()){
&#xA0; &#xA0; &#xA0; &#xA0; $group_arr[] = $row;
&#xA0; &#xA0; }
&#xA0; &#xA0;&#xA0; // Free result set
&#xA0; &#xA0;&#xA0; $result-&gt;close();
&#xA0; &#xA0;&#xA0; $db-&gt;next_result();
}
else echo($db-&gt;error);

// Close connection
$db-&gt;close();
?>
```



  

#



The cryptic &quot;Couldn&apos;t fetch mysqli&quot; error message can mean any number of things, including:

1. You&apos;re trying to use a database object that you&apos;ve already closed (as noted by ceo at l-i-e dot com). Reopen your database connection, or find the call to 

```
<?php mysqli_close($db); ?>
```
 or 

```
<?php $db-&gt;close(); ?>
```
 and remove it.
2. Your MySQLi object has been serialized and unserialized for some reason. Define a wakeup function to re-create your database connection. http://php.net/__wakeup 
3. Something besides you closed your mysqli connection (in particular, see http://bugs.php.net/bug.php?id=33772)
4. You mixed OOP and functional calls to the database object. (So, you have 

```
<?php $db-&gt;query() ?>
```
 in the same program as 

```
<?php mysqli_query($db) ?>
```
).

  

#



Here is an example of a clean query into a html table

&lt;table&gt;
&#xA0;&#xA0; &lt;tr&gt;
&#xA0; &#xA0;&#xA0; &lt;th&gt;First Name&lt;/th&gt;
&#xA0; &#xA0;&#xA0; &lt;th&gt;Last Name&lt;/th&gt;
&#xA0; &#xA0;&#xA0; &lt;th&gt;City&lt;/th&gt;
&#xA0;&#xA0; &lt;/tr&gt;
&#xA0;&#xA0; 

```
<?php while ($row = $myquery-&gt;fetch_assoc()) { ?>
```

&#xA0;&#xA0; &lt;tr&gt;
&#xA0; &#xA0;&#xA0; &lt;td&gt;

```
<?php echo $row[&quot;firstname&quot;]; ?>
```
&lt;/td&gt;
&#xA0; &#xA0;&#xA0; &lt;td&gt;

```
<?php echo $row[&quot;lastname&quot;]; ?>
```
&lt;/td&gt;
&#xA0; &#xA0;&#xA0; &lt;td&gt;

```
<?php echo $row[&quot;city&quot;];?>
```
&lt;/td&gt;
&#xA0;&#xA0; &lt;/tr&gt;
&#xA0;&#xA0; 

```
<?php } ?>
```

 &lt;/table&gt;

  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.query.php)

**[To root](/README.md)**