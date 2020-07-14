# Relative Formats



Note that expressions such as &#x201C;last day of&#x201D; and &#x201C;first day of&#x201D; imply a day of a month, not, for example of the year or week.<br><br>So, expressions, such as &#x201C;first day of this year&#x201D; will give the first day of this month, with no apparent regard for the year.<br><br>As powerful as the parser is, it can lead to disappointing or confusing results.  

#

April 1st, 2012 is Sunday. You might expect to get April 2nd, 2012 with &apos;Monday next week&apos;, however this:<br><br>

```
<?php
    echo date(&apos;F jS, Y&apos;, strtotime(&apos;Monday next week 2012-04-01&apos;));
?>
```


returns April 9th, 2012. To get April 2nd, you need to use this:



```
<?php
    echo date(&apos;F jS, Y&apos;, strtotime(&apos;next Monday 2012-04-01&apos;));
?>
```


Apparently &apos;next week&apos; advances the week if and only if the day is Sunday. This:



```
<?php
    echo date(&apos;F jS, Y&apos;, strtotime(&apos;Monday next week 2012-03-31&apos;));
?>
```
<br><br>would still return April 2nd.  

#

[Official documentation page](https://www.php.net/manual/en/datetime.formats.relative.php)

**[To root](/README.md)**