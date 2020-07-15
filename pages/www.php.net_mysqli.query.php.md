# mysqli::query



This may or may not be obvious to people but perhaps it will help someone.<br><br>When running joins in SQL you may encounter a problem if you are trying to pull two columns with the same name. mysqli returns the last in the query when called by name. So to get what you need you can use an alias.<br><br>Below I am trying to join a user id with a user role. in the first table (tbl_usr), role is a number and in the second is a  text name (tbl_memrole is a lookup table). If I call them both as role I get the text as it is the last "role" in the query. If I use an alias then I get both as desired as shown below.<br><br>

```
<?php
$sql = "SELECT a.uid, a.role AS roleid, b.role, 
            FROM tbl_usr a
            INNER JOIN tbl_memrole b
            ON a.role = b.id
            ";

    if ($result = $mysqli->query($sql)) {
        while($obj = $result->fetch_object()){
            $line.=$obj->uid;
            $line.=$obj->role;
            $line.=$obj->roleid;
        }
    }
    $result->close();
    unset($obj);
    unset($sql);
    unset($query);
    
?>
```
<br>In this situation I guess I could have just renamed the role column in the first table roleid and that would have taken care of it, but it was a learning experience.  

#

When calling multiple stored procedures, you can run into the following error: "Commands out of sync; you can&apos;t run this command now".<br>This can happen even when using the close() function on the result object between calls. <br>To fix the problem, remember to call the next_result() function on the mysqli object after each stored procedure call. See example below:<br><br>

```
<?php
// New Connection
$db = new mysqli('localhost','user','pass','database');

// Check for errors
if(mysqli_connect_errno()){
 echo mysqli_connect_error();
}

// 1st Query
$result = $db->query("call getUsers()");
if($result){
     // Cycle through results
    while ($row = $result->fetch_object()){
        $user_arr[] = $row;
    }
    // Free result set
    $result->close();
    $db->next_result();
}

// 2nd Query
$result = $db->query("call getGroups()");
if($result){
     // Cycle through results
    while ($row = $result->fetch_object()){
        $group_arr[] = $row;
    }
     // Free result set
     $result->close();
     $db->next_result();
}
else echo($db->error);

// Close connection
$db->close();
?>
```
  

#

The cryptic "Couldn&apos;t fetch mysqli" error message can mean any number of things, including:<br><br>1. You&apos;re trying to use a database object that you&apos;ve already closed (as noted by ceo at l-i-e dot com). Reopen your database connection, or find the call to 

```
<?php mysqli_close($db); ?>
```
 or 

```
<?php $db->close(); ?>
```
 and remove it.
2. Your MySQLi object has been serialized and unserialized for some reason. Define a wakeup function to re-create your database connection. http://php.net/__wakeup 
3. Something besides you closed your mysqli connection (in particular, see http://bugs.php.net/bug.php?id=33772)
4. You mixed OOP and functional calls to the database object. (So, you have 

```
<?php $db->query() ?>
```
 in the same program as 

```
<?php mysqli_query($db) ?>
```
).  

#

Here is an example of a clean query into a html table<br><br>&lt;table&gt;<br>   &lt;tr&gt;<br>     &lt;th&gt;First Name&lt;/th&gt;<br>     &lt;th&gt;Last Name&lt;/th&gt;<br>     &lt;th&gt;City&lt;/th&gt;<br>   &lt;/tr&gt;<br>   

```
<?php while ($row = $myquery->fetch_assoc()) { ?>
```

   <tr>
     <td>

```
<?php echo $row["firstname"]; ?>
```
</td>
     <td>

```
<?php echo $row["lastname"]; ?>
```
</td>
     <td>

```
<?php echo $row["city"];?>
```
</td>
   </tr>
   

```
<?php } ?>
```
<br> &lt;/table&gt;  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.query.php)

**[To root](/README.md)**