# mysql_result



here&apos;s a rough replacement using mysqli:<br><br>if (!function_exists(&apos;mysql_result&apos;)) {<br>    function mysql_result($result, $number, $field=0) {<br>        mysqli_data_seek($result, $number);<br>        $row = mysqli_fetch_array($result);<br>        return $row[$field];<br>    }<br>}  

#

[Official documentation page](https://www.php.net/manual/en/function.mysql-result.php)

**[To root](/README.md)**