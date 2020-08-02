# PDO::exec



This function cannot be used with any queries that return results.  This includes SELECT, OPTIMIZE TABLE, etc.  

---

It&apos;s worth noting here, that - in addition to the hints given in docs up there - using prepare, bind and execute provides more benefits than multiply querying a statement: performance and security!<br><br>If you insert some binary data (e.g. image file) into database using INSERT INTO ... then it may boost performance of parsing your statement since it is kept small (a few bytes, only, while the image may be several MiBytes) and there is no need to escape/quote the file&apos;s binary data to become a proper string value.<br><br>And, finally and for example, if you want to get a more secure PHP application which isn&apos;t affectable by SQL injection attacks you _have to_ consider using prepare/execute on every statement containing data (like INSERTs or SELECTs with WHERE-clauses). Separating the statement code from related data using prepare, bind and execute is best method - fast and secure! You don&apos;t even need to escape/quote/format-check any data.  

---

[Official documentation page](https://www.php.net/manual/en/pdo.exec.php)

**[To root](/README.md)**