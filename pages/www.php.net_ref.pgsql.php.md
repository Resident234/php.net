# PostgreSQL Functions





A simple conversion for 1D PostgreSQL array data:

// =====
//Example #1 (An array of IP addresses):


```
<?php
&#xA0; $pgsqlArr = &apos;{192.168.1.1,10.1.1.1}&apos;;

&#xA0; preg_match(&apos;/^{(.*)}$/&apos;, $pgsqlArr, $matches);
&#xA0; $phpArr = str_getcsv($matches[1]);

&#xA0; print_r($phpArr);
}
// Output:
// Array
// (
//&#xA0; &#xA0; [0] =&gt; 192.168.1.1
//&#xA0; &#xA0; [1] =&gt; 10.1.1.1
// )
// =====

// =====
// Example #2 (An array of strings including spaces and commas):
&lt;?php
&#xA0; $pgsqlArr = &apos;{string1,string2,&quot;string,3&quot;,&quot;string 4&quot;}&apos;;

&#xA0; preg_match(&apos;/^{(.*)}$/&apos;, $pgsqlArr, $matches);
&#xA0; $phpArr = str_getcsv($matches[1]);

&#xA0; print_r($phpArr);
}
// Output:
// Array
// (
//&#xA0; &#xA0; [0] =&gt; string1
//&#xA0; &#xA0; [1] =&gt; string2
//&#xA0; &#xA0; [2] =&gt; string,3
//&#xA0; &#xA0; [3] =&gt; string 4
// )
// =====


  

#

[Official documentation page](https://www.php.net/manual/en/ref.pgsql.php)

**[To root](/README.md)**