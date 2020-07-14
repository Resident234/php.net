# PostgreSQL Functions



A simple conversion for 1D PostgreSQL array data:<br><br>// =====<br>//Example #1 (An array of IP addresses):<br>

```
<?php
  $pgsqlArr = &apos;{192.168.1.1,10.1.1.1}&apos;;

  preg_match(&apos;/^{(.*)}$/&apos;, $pgsqlArr, $matches);
  $phpArr = str_getcsv($matches[1]);

  print_r($phpArr);
}
// Output:
// Array
// (
//    [0] =&gt; 192.168.1.1
//    [1] =&gt; 10.1.1.1
// )
// =====

// =====
// Example #2 (An array of strings including spaces and commas):
&lt;?php<br>  $pgsqlArr = &apos;{string1,string2,"string,3","string 4"}&apos;;<br><br>  preg_match(&apos;/^{(.*)}$/&apos;, $pgsqlArr, $matches);<br>  $phpArr = str_getcsv($matches[1]);<br><br>  print_r($phpArr);<br>}<br>// Output:<br>// Array<br>// (<br>//    [0] =&gt; string1<br>//    [1] =&gt; string2<br>//    [2] =&gt; string,3<br>//    [3] =&gt; string 4<br>// )<br>// =====  

#

[Official documentation page](https://www.php.net/manual/en/ref.pgsql.php)

**[To root](/README.md)**