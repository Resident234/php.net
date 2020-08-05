# mysqli_stmt::prepare



Note that if you&apos;re using a question mark as a placeholder for a string value, you don&apos;t surround it with quotation marks in the MySQL query.<br><br>For example, do this:<br><br>mysqli_stmt_prepare($stmt, "SELECT * FROM foo WHERE foo.Date &gt; ?");<br><br>Do not do this:<br><br>mysqli_stmt_prepare($stmt, "SELECT * FROM foo WHERE foo.Date &gt; &apos;?&apos;");<br><br>If you put quotation marks around a question mark in the query, then PHP doesn&apos;t recognize the question mark as a placeholder, and then when you try to use mysqli_stmt_bind_param(), it gives an error to the effect that you have the wrong number of parameters.<br><br>The lack of quotation marks around a string placeholder is implicit in the official example on this page, but it&apos;s not explicitly stated in the docs, and I had trouble figuring it out, so figured it was worth posting.  

---

[Official documentation page](https://www.php.net/manual/en/mysqli-stmt.prepare.php)

**[To root](/README.md)**