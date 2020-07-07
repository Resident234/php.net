# Date and Time





I think it&apos;s important to mention with the DateTime class that if you&apos;re trying to create a system that should store UNIX timestamps in UTC/GMT, and then convert them to a desired custom time-zone when they need to be displayed, using the following code is a good idea:



```
<?php
date_default_timezone_set(&apos;UTC&apos;);
?>
```


Even if you use something like:



```
<?php
$date-&gt;setTimezone( new DateTimeZone(&apos;UTC&apos;) );
?>
```


... before you store the value, it doesn&apos;t seem to work because PHP is already trying to convert it to the default timezone.

  

#

[Official documentation page](https://www.php.net/manual/en/book.datetime.php)

**[To root](/README.md)**