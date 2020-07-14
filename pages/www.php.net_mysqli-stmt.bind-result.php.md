# mysqli_stmt::bind_result



lot of people don&apos;t like how bind_result works with prepared statements! it requires you to pass long list of parameters which will be loaded with column value when the function being called.<br><br>To solve this, i used call_user_func_array function and result_metadata functions. which make easy and automatically returns an array of all columns results stored in an array with column names.<br><br>please don&apos;t forget to change setting variables with your own credentials:<br><br>

```
<?php
$host = &apos;localhost&apos;;
$user = &apos;root&apos;;
$pass = &apos;1234&apos;;
$data = &apos;test&apos;;

$mysqli = new mysqli($host, $user, $pass, $data);
/* check connection */
if (mysqli_connect_errno()) {
    printf("Connect failed: %s\n", mysqli_connect_error());
    exit();
}

if ($stmt = $mysqli-&gt;prepare("SELECT * FROM sample WHERE t2 LIKE ?")) {
    $tt2 = &apos;%&apos;;
    
    $stmt-&gt;bind_param("s", $tt2);
    $stmt-&gt;execute();

    $meta = $stmt-&gt;result_metadata();
    while ($field = $meta-&gt;fetch_field())
    {
        $params[] = &amp;$row[$field-&gt;name];
    }

    call_user_func_array(array($stmt, &apos;bind_result&apos;), $params);

    while ($stmt-&gt;fetch()) {
        foreach($row as $key =&gt; $val)
        {
            $c[$key] = $val;
        }
        $result[] = $c;
    }
    
    $stmt-&gt;close();
}
$mysqli-&gt;close();
print_r($result);
?>
```
  

#

I wrote a function that fetches all rows from a result set - either normal or prepared.<br><br>

```
<?php
function fetch($result)
{    
    $array = array();
    
    if($result instanceof mysqli_stmt)
    {
        $result-&gt;store_result();
        
        $variables = array();
        $data = array();
        $meta = $result-&gt;result_metadata();
        
        while($field = $meta-&gt;fetch_field())
            $variables[] = &amp;$data[$field-&gt;name]; // pass by reference
        
        call_user_func_array(array($result, &apos;bind_result&apos;), $variables);
        
        $i=0;
        while($result-&gt;fetch())
        {
            $array[$i] = array();
            foreach($data as $k=&gt;$v)
                $array[$i][$k] = $v;
            $i++;
            
            // don&apos;t know why, but when I tried $array[] = $data, I got the same one result in all rows
        }
    }
    elseif($result instanceof mysqli_result)
    {
        while($row = $result-&gt;fetch_assoc())
            $array[] = $row;
    }
    
    return $array;
}
?>
```
<br><br>Simply call it passing a result set or executed statement and you&apos;ll get all rows fetched.  

#

If you select LOBs use the following order of execution or you risk mysqli allocating more memory that actually used<br><br>1)prepare()<br>2)execute()<br>3)store_result()<br>4)bind_result()<br><br>If you skip 3) or exchange 3) and 4) then mysqli will allocate memory for the maximal length of the column which is 255 for tinyblob, 64k for blob(still ok), 16MByte for MEDIUMBLOB - quite a lot and 4G for LONGBLOB (good if you have so much memory). Queries which use this order a bit slower when there is a LOB but this is the price of not having memory exhaustion in seconds.  

#

A note to people to want to return an array of results - that is, an array of all the results from the query, not just one at a time.<br><br>

```
<?php

// blah blah...
call_user_func_array(array($mysqli_stmt_object, "bind_result"), $byref_array_for_fields);

$results = array();
while ($mysqli_stmt_object-&gt;fetch()) {
    $results[] = $byref_array_for_fields;
}

?>
```

This will NOT work. $results will have a bunch of arrays, but each one will have a reference to $byref.

PHP is optimizing performance here: you aren&apos;t so much copying the $byref array into $results as you are *adding* it. That means $results will have a bunch of $byrefs - the same array repeated multiple times. (So what you see is that $results is all duplicates of the last item from the query.)

hamidhossain (01-Sep-2008) shows how to get around that: inside the loop that fetches results you also have to loop through the list of fields, copying them as you go. In effect, copying everything individually.

Personally, I&apos;d rather use some kind of function that effectively duplicates an array than write my own code. Many of the built-in array functions don&apos;t work, apparently using references rather than copies, but a combination of array_map and create_function does.



```
<?php

// blah blah...
call_user_func_array(array($mysqli_stmt_object, "bind_result"), $byref_array_for_fields);

// returns a copy of a value
$copy = create_function(&apos;$a&apos;, &apos;return $a;&apos;);

$results = array();
while ($mysqli_stmt_object-&gt;fetch()) {
    // array_map will preserve keys when done here and this way
    $results[] = array_map($copy, $byref_array_for_fields);
}

?>
```
<br><br>All these problems would go away if they just implemented a fetch_assoc or even fetch_array for prepared statements...  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-stmt.bind-result.php)

**[To root](/README.md)**