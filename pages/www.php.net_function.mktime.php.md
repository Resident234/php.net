# mktime



Do remember that, counter-intuitively enough, the arguments for month and day are inversed (or middle-endian). A common mistake for Europeans seems to be to feed the date arguments in the expected order (big endian or little endian).<br><br>It&apos;s clear to see where this weird order comes from (even with the date being big endian the order for all arguments would still be mixed - it&apos;s obviously based on the American date format with the time "prefixed" to allow an easier shorthand) and why this wasn&apos;t changed (passing the values in the wrong order produces a valid, though unexpected, result in most cases), but it continues to be a source of confusion for me whenever I come back to PHP from other languages or libraries.  

#

Just a small thing to think about if you are only trying to pull the month out using mktime and date.  Make sure you place a 1 into day field.  Otherwise you will get incorrect dates when a month is followed by a month with less days when the day of the current month is higher then the max day of the month you are trying to find.. (Such as today being Jan 30th and trying to find the month Feb.)  

#

Be careful passing zeros into mktime, in most cases a zero will count as the previous unit of time. The documentation explains this yet most of the comments here still use zeroes.<br><br>For example, if you pass the year 2013 into mktime, with zeroes for everything else, the outcome is probably not what you are looking for.<br><br>

```
<?php
echo date('F jS, Y g:i:s a', mktime(0, 0, 0, 0, 0, 2013)); 
// November 30th, 2012 12:00:00 am
?>
```


Instead of using 0's, try 1's. This makes more sense (except for minutes/seconds). Maybe not as obvious of a purpose as zeroes to other programmers, though.



```
<?php
echo date('F jS, Y g:i:s a', mktime(1, 1, 1, 1, 1, 2013));
// January 1st, 2013 1:01:01 am
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.mktime.php)

**[To root](/README.md)**