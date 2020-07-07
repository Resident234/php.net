# strtotime





I&apos;ve had a little trouble with this function in the past because (as some people have pointed out) you can&apos;t really set a locale for strtotime. If you&apos;re American, you see 11/12/10 and think &quot;12 November, 2010&quot;. If you&apos;re Australian (or European), you think it&apos;s 11 December, 2010. If you&apos;re a sysadmin who reads in ISO, it looks like 10th December 2011.



The best way to compensate for this is by modifying your joining characters. Forward slash (/) signifies American M/D/Y formatting, a dash (-) signifies European D-M-Y and a period (.) signifies ISO Y.M.D.



Observe:





```
<?php

echo date(&quot;jS F, Y&quot;, strtotime(&quot;11.12.10&quot;));

// outputs 10th December, 2011



echo date(&quot;jS F, Y&quot;, strtotime(&quot;11/12/10&quot;));

// outputs 12th November, 2010



echo date(&quot;jS F, Y&quot;, strtotime(&quot;11-12-10&quot;));

// outputs 11th December, 2010&#xA0; 

?>
```




Hope this helps someone!

  

#



The &quot;+1 month&quot; issue with strtotime
===================================
As noted in several blogs, strtotime() solves the &quot;+1 month&quot; (&quot;next month&quot;) issue on days that do not exist in the subsequent month differently than other implementations like for example MySQL.



```
<?php
echo date( &quot;Y-m-d&quot;, strtotime( &quot;2009-01-31 +1 month&quot; ) ); // PHP:&#xA0; 2009-03-03
echo date( &quot;Y-m-d&quot;, strtotime( &quot;2009-01-31 +2 month&quot; ) ); // PHP:&#xA0; 2009-03-31
?>
```




```
<?php
SELECT DATE_ADD( &apos;2009-01-31&apos;, INTERVAL 1 MONTH ); // MySQL:&#xA0; 2009-02-28
?>
```



  

#



UK dates (eg. 27/05/1990) won&apos;t work with strotime, even with timezone properly set. 



/*

However, if you just replace &quot;/&quot; with &quot;-&quot; it will work fine.



```
<?php

$timestamp = strtotime(str_replace(&apos;/&apos;, &apos;-&apos;, &apos;27/05/1990&apos;));

?>
```


*/



[red., derick]: What you instead should do is:





```
<?php

$date = date_create_from_format(&apos;d/m/y&apos;, &apos;27/05/1990&apos;);

?>
```




That does not make it a timestamp, but a DateTime object, which is much more versatile instead.

  

#



WARNING when using &quot;next month&quot;, &quot;last month&quot;, &quot;+1 month&quot;,&#xA0; &quot;-1 month&quot; or any combination of +/-X months. It will give non-intuitive results on Jan 30th and 31st. 

As described at : http://derickrethans.nl/obtaining-the-next-month-in-php.html



```
<?php
$d = new DateTime( &apos;2010-01-31&apos; );
$d-&gt;modify( &apos;next month&apos; );
echo $d-&gt;format( &apos;F&apos; ), &quot;\n&quot;;
?>
```


In the above, using &quot;next month&quot; on January 31 will output &quot;March&quot; even though you might want it to output &quot;February&quot;. (&quot;+1 month&quot; will give the same result. &quot;last month&quot;, &quot;-1 month&quot; are similarly affected, but the results would be seen at beginning of March.)

The way to get what people would generally be looking for when they say &quot;next month&quot; even on Jan 30 and Jan 31 is to use &quot;first day of next month&quot;:



```
<?php
$d = new DateTime( &apos;2010-01-08&apos; );
$d-&gt;modify( &apos;first day of next month&apos; );
echo $d-&gt;format( &apos;F&apos; ), &quot;\n&quot;;
?>
```




```
<?php
$d = new DateTime( &apos;2010-01-08&apos; );
$d-&gt;modify( &apos;first day of +1 month&apos; );
echo $d-&gt;format( &apos;F&apos; ), &quot;\n&quot;;
?>
```



  

#



I tried using sams most popular example but got incorrect results.

Incorrect:


```
<?php 
echo date(&quot;jS F, Y&quot;, strtotime(&quot;11.12.10&quot;)); 
// outputs 10th December, 2011 

echo date(&quot;jS F, Y&quot;, strtotime(&quot;11/12/10&quot;)); 
// outputs 12th November, 2010 

echo date(&quot;jS F, Y&quot;, strtotime(&quot;11-12-10&quot;)); 
// outputs 11th December, 2010&#xA0; 
?>
```
 

Then I read the notes which said:
if the separator is a slash (/), then the American m/d/y is assumed; whereas if the separator is a dash (-) or a dot (.), then the European d-m-y format is assumed. ***If, however, the year is given in a two digit format and the separator is a dash (-), the date string is parsed as y-m-d.***

Therefore, the above code does not work on 2 digit years - only 4 digit years

  

#



A useful testing tool for strtotime() and unix timestamp conversion:
http://strtotime.co.uk/

  

#



strtotime() also returns time by year and weeknumber. (I use PHP 5.2.8, PHP 4 does not support it.) Queries can be in two forms:

- &quot;yyyyWww&quot;, where yyyy is 4-digit year, W is literal and ww is 2-digit weeknumber. Returns timestamp for first day of week (for me Monday)

- &quot;yyyy-Www-d&quot;, where yyyy is 4-digit year, W is literal, ww is 2-digit weeknumber and dd is day of week (1 for Monday, 7 for Sunday) 





```
<?php

// Get timestamp of 32nd week in 2009.

strtotime(&apos;2009W32&apos;); // returns timestamp for Mon, 03 Aug 2009 00:00:00

// Weeknumbers &lt; 10 must be padded with zero:

strtotime(&apos;2009W01&apos;); // returns timestamp for Mon, 29 Dec 2008 00:00:00

// strtotime(&apos;2009W1&apos;); // error! returns false



// See timestamp for Tuesday in 5th week of 2008

strtotime(&apos;2008-W05-2&apos;); // returns timestamp for Tue, 29 Jan 2008 00:00:00

?>
```




Weeknumbers are (probably) computed according to ISO-8601 specification, so doing date(&apos;W&apos;) on given timestamps should return passed weeknumber.

  

#

[Official documentation page](https://www.php.net/manual/en/function.strtotime.php)

**[To root](/README.md)**