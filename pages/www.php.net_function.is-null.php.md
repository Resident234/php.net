# is_null



Micro optimization isn&apos;t worth it.<br><br>You had to do it ten million times to notice a difference, a little more than 2 seconds<br><br>$a===NULL; Took: 1.2424390316s<br> is_null($a); Took: 3.70693397522s<br><br>difference = 2.46449494362<br>difference/10,000,000 = 0.000000246449494362<br><br>The execution time difference between ===NULL and is_null is less than 250 nanoseconds. Go optimize something that matters.  

---

See how php parses different values. $var is the variable.<br><br>$var        =    NULL    ""    0    "0"    1<br><br>strlen($var)    =    0    0    1    1    1<br>is_null($var)    =    TRUE    FALSE    FALSE    FALSE    FALSE<br>$var == ""    =    TRUE    TRUE    TRUE    FALSE    FALSE<br>!$var        =    TRUE    TRUE    TRUE    TRUE    FALSE<br>!is_null($var)    =    FALSE    TRUE    TRUE    TRUE    TRUE<br>$var != ""    =    FALSE    FALSE    FALSE    TRUE    TRUE<br>$var        =    FALSE    FALSE    FALSE    FALSE    TRUE<br><br>Peace!  

---

In PHP 7 (phpng), is_null is actually marginally faster than ===, although the performance difference between the two is far smaller.<br><br>PHP 5.5.9<br>is_null - float(2.2381200790405)<br>===     - float(1.0024659633636)<br>=== faster by ~100ns per call<br><br>PHP 7.0.0-dev (built: May 19 2015 10:16:06)<br>is_null - float(1.4121870994568)<br>===     - float(1.4577329158783)<br>is_null faster by ~5ns per call  

---

[Official documentation page](https://www.php.net/manual/en/function.is-null.php)

**[To root](/README.md)**