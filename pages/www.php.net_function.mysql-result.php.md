# mysql_result





here&apos;s a rough replacement using mysqli:

if (!function_exists(&apos;mysql_result&apos;)) {
&#xA0; &#xA0; function mysql_result($result, $number, $field=0) {
&#xA0; &#xA0; &#xA0; &#xA0; mysqli_data_seek($result, $number);
&#xA0; &#xA0; &#xA0; &#xA0; $row = mysqli_fetch_array($result);
&#xA0; &#xA0; &#xA0; &#xA0; return $row[$field];
&#xA0; &#xA0; }
}

  

#

[Official documentation page](https://www.php.net/manual/en/function.mysql-result.php)

**[To root](/README.md)**