# mysqli_result::fetch_row



It&apos;s worth noting that the MySQLi functions (and, I presume, the MySQL functions) fetch a string regardless of the MySQL data type. E.g. if you fetch a row with an integer column, the corresponding value for that column and row will still be stored as a string in the array returned by mysql_fetch_row.  

---

[Official documentation page](https://www.php.net/manual/en/mysqli-result.fetch-row.php)

**[To root](/README.md)**