# mysql_fetch_array



Benchmark on a table with 38567 rows:<br><br>mysql_fetch_array<br>MYSQL_BOTH: 6.01940000057 secs<br>MYSQL_NUM: 3.22173595428 secs <br>MYSQL_ASSOC: 3.92950594425 secs <br><br>mysql_fetch_row: 2.35096800327 secs <br>mysql_fetch_assoc: 2.92349803448 secs <br><br>As you can see, it&apos;s twice as effecient to fetch either an array or a hash, rather than getting both.  it&apos;s even faster to use fetch_row rather than passing fetch_array MYSQL_NUM, or fetch_assoc rather than fetch_array MYSQL_ASSOC.  Don&apos;t fetch BOTH unless you really need them, and most of the time you don&apos;t.  

---

[Official documentation page](https://www.php.net/manual/en/function.mysql-fetch-array.php)

**[To root](/README.md)**