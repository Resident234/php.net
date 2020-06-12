# mysqli_stmt::$affected_rows




<div class="phpcode"><span class="html">
It appears that an UPDATE prepared statement which contains the same data as that already in the database returns 0 for affected_rows.&#xA0; I was expecting it to return 1, but it must be comparing the input values with the existing values and determining that no UPDATE has occurred.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-stmt.affected-rows.php)

**[⬆ to root](/)**