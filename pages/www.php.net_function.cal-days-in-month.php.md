# cal_days_in_month





Remember if you just want the days in the current month, use the date function:
$days = date(&quot;t&quot;);

  

#



Here&apos;s a one-line function I just wrote to find the numbers of days in a month that doesn&apos;t depend on any other functions.



The reason I made this is because I just found out I forgot to compile PHP with support for calendars, and a class I&apos;m writing for my website&apos;s open source section was broken. So rather than recompiling PHP (which I will get around to tomorrow I guess), I just wrote this function which should work just as well, and will always work without the requirement of PHP&apos;s calendar extension or any other PHP functions for that matter.



I learned the days of the month using the old knuckle &amp; inbetween knuckle method, so that should explain the mod 7 part. :)





```
<?php

/*

 * days_in_month($month, $year)

 * Returns the number of days in a given month and year, taking into account leap years.

 *

 * $month: numeric month (integers 1-12)

 * $year: numeric year (any integer)

 *

 * Prec: $month is an integer between 1 and 12, inclusive, and $year is an integer.

 * Post: none

 */

// corrected by ben at sparkyb dot net

function days_in_month($month, $year)

{

// calculate number of days in a month

return $month == 2 ? ($year % 4 ? 28 : ($year % 100 ? 29 : ($year % 400 ? 28 : 29))) : (($month - 1) % 7 % 2 ? 30 : 31);

}

?>
```




Enjoy,

David Bindel

  

#

[Official documentation page](https://www.php.net/manual/en/function.cal-days-in-month.php)

**[To root](/README.md)**