# mysqli_result::fetch_array





Putting multiple rows into an array:



```
<?php
$mysqli = new mysqli(&quot;localhost&quot;, &quot;my_user&quot;, &quot;my_password&quot;, &quot;world&quot;);

/* check connection */
if (mysqli_connect_errno()) {
&#xA0; &#xA0; printf(&quot;Connect failed: %s\n&quot;, mysqli_connect_error());
&#xA0; &#xA0; exit();
}

$query = &quot;SELECT Name, CountryCode FROM City ORDER by ID LIMIT 3&quot;;
$result = $mysqli-&gt;query($query);

while($row = $result-&gt;fetch_array())
{
$rows[] = $row;
}

foreach($rows as $row)
{
echo $row[&apos;CountryCode&apos;];
}

/* free result set */
$result-&gt;close();

/* close connection */
$mysqli-&gt;close();
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-result.fetch-array.php)

**[To root](/README.md)**