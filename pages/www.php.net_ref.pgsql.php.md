# PostgreSQL Functions



A simple conversion for 1D PostgreSQL array data:<br><br>// =====<br>//Example #1 (An array of IP addresses):<br>

```
<?php
  $pgsqlArr = '{192.168.1.1,10.1.1.1}';

  preg_match('/^{(.*)}$/', $pgsqlArr, $matches);
  $phpArr = str_getcsv($matches[1]);

  print_r($phpArr);
}
// Output:
// Array
// (
//    [0] => 192.168.1.1
//    [1] => 10.1.1.1
// )
// =====

// =====
// Example #2 (An array of strings including spaces and commas):


```
<?phpphp
  $pgsqlArr = '{string1,string2,"string,3","string 4"}';

  preg_match('/^{(.*)}$/', $pgsqlArr, $matches);
  $phpArr = str_getcsv($matches[1]);

  print_r($phpArr);
}
// Output:
// Array
// (
//    [0] => string1
//    [1] => string2
//    [2] => string,3
//    [3] => string 4
// )
// =====?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/ref.pgsql.php)

**[To root](/README.md)**