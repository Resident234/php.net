# mysqli_stmt::result_metadata




<div class="phpcode"><span class="html">
If result_metadata() returns false but error/errno/sqlstate tells you no error occurred, this means your query is one that does not produce a result set, i.e. an INSERT/UPDATE/DELETE query instead of a SELECT query.<br><br>This is stated in the documentation where it says &quot;If a statement passed to mysqli_prepare() is one that produces a result set, mysqli_stmt_result_metadata() returns the result object&quot;, but it might not be clear to everyone what this entails exactly. <br><br>Hope this helps.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-stmt.result-metadata.php)

**[⬆ to root](/)**