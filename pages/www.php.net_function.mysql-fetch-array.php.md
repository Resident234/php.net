# mysql_fetch_array





Benchmark on a table with 38567 rows:

mysql_fetch_array
MYSQL_BOTH: 6.01940000057 secs
MYSQL_NUM: 3.22173595428 secs 
MYSQL_ASSOC: 3.92950594425 secs 

mysql_fetch_row: 2.35096800327 secs 
mysql_fetch_assoc: 2.92349803448 secs 

As you can see, it&apos;s twice as effecient to fetch either an array or a hash, rather than getting both.&#xA0; it&apos;s even faster to use fetch_row rather than passing fetch_array MYSQL_NUM, or fetch_assoc rather than fetch_array MYSQL_ASSOC.&#xA0; Don&apos;t fetch BOTH unless you really need them, and most of the time you don&apos;t.

  

#

[Official documentation page](https://www.php.net/manual/en/function.mysql-fetch-array.php)

**[To root](/README.md)**