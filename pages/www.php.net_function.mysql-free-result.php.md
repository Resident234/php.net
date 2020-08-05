# mysql_free_result



yes this function may increase the memory usage if you use unbuffered querys and if you have not fetched all the data from mysql. in this case the mysql api has a problem: you want to free the result but do not want to close the connection. now mysql will only accept another query if all data has been fetched, so the api now must fetch the rest of the data when calling mysql_free_result().<br><br>so only use unbuffered querys if you fetch all the data (and need it).  

---

mysql_query() also returns a resource for "OPTIMIZE TABLE" statements!  

---

[Official documentation page](https://www.php.net/manual/en/function.mysql-free-result.php)

**[To root](/README.md)**