# mysqli_stmt::result_metadata



If result_metadata() returns false but error/errno/sqlstate tells you no error occurred, this means your query is one that does not produce a result set, i.e. an INSERT/UPDATE/DELETE query instead of a SELECT query.<br><br>This is stated in the documentation where it says "If a statement passed to mysqli_prepare() is one that produces a result set, mysqli_stmt_result_metadata() returns the result object", but it might not be clear to everyone what this entails exactly. <br><br>Hope this helps.  

---

[Official documentation page](https://www.php.net/manual/en/mysqli-stmt.result-metadata.php)

**[To root](/README.md)**