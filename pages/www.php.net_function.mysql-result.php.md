# mysql_result




<div class="phpcode"><span class="html">
here&apos;s a rough replacement using mysqli:<br><br>if (!function_exists(&apos;mysql_result&apos;)) {<br>&#xA0; &#xA0; function mysql_result($result, $number, $field=0) {<br>&#xA0; &#xA0; &#xA0; &#xA0; mysqli_data_seek($result, $number);<br>&#xA0; &#xA0; &#xA0; &#xA0; $row = mysqli_fetch_array($result);<br>&#xA0; &#xA0; &#xA0; &#xA0; return $row[$field];<br>&#xA0; &#xA0; }<br>}</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.mysql-result.php)

**[To root](/README.md)**