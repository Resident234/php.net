# mysqli_result::fetch_array



Putting multiple rows into an array:<br><br>

```
<?php
$mysqli = new mysqli("localhost", "my_user", "my_password", "world");

/* check connection */
if (mysqli_connect_errno()) {
    printf("Connect failed: %s\n", mysqli_connect_error());
    exit();
}

$query = "SELECT Name, CountryCode FROM City ORDER by ID LIMIT 3";
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