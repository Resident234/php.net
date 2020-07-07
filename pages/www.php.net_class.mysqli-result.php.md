# The mysqli_result class





Converting an old project from using the mysql extension to the mysqli extension, I found the most annoying change to be the lack of a corresponding mysql_result function in mysqli. While mysql_result is a generally terrible function, it was useful for fetching a single result field *value* from a result set (for example, if looking up a user&apos;s ID).



The behavior of mysql_result is approximated here, though you may want to name it something other than mysqli_result so as to avoid thinking it&apos;s an actual, built-in function.





```
<?php

function mysqli_result($res, $row, $field=0) {

&#xA0; &#xA0; $res-&gt;data_seek($row);

&#xA0; &#xA0; $datarow = $res-&gt;fetch_array();

&#xA0; &#xA0; return $datarow[$field];

}

?>
```




Implementing it via the OO interface is left as an exercise to the reader.

  

#

[Official documentation page](https://www.php.net/manual/en/class.mysqli-result.php)

**[To root](/README.md)**