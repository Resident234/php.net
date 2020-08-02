# strtotime



I&apos;ve had a little trouble with this function in the past because (as some people have pointed out) you can&apos;t really set a locale for strtotime. If you&apos;re American, you see 11/12/10 and think "12 November, 2010". If you&apos;re Australian (or European), you think it&apos;s 11 December, 2010. If you&apos;re a sysadmin who reads in ISO, it looks like 10th December 2011.<br><br>The best way to compensate for this is by modifying your joining characters. Forward slash (/) signifies American M/D/Y formatting, a dash (-) signifies European D-M-Y and a period (.) signifies ISO Y.M.D.<br><br>Observe:<br><br>

```
<?php
echo date("jS F, Y", strtotime("11.12.10"));
// outputs 10th December, 2011

echo date("jS F, Y", strtotime("11/12/10"));
// outputs 12th November, 2010

echo date("jS F, Y", strtotime("11-12-10"));
// outputs 11th December, 2010  
?>
```
<br><br>Hope this helps someone!  

---

The "+1 month" issue with strtotime<br>===================================<br>As noted in several blogs, strtotime() solves the "+1 month" ("next month") issue on days that do not exist in the subsequent month differently than other implementations like for example MySQL.<br><br>

```
<?php
echo date( "Y-m-d", strtotime( "2009-01-31 +1 month" ) ); // PHP:  2009-03-03
echo date( "Y-m-d", strtotime( "2009-01-31 +2 month" ) ); // PHP:  2009-03-31
?>
```




```
<?php
SELECT DATE_ADD( '2009-01-31', INTERVAL 1 MONTH ); // MySQL:  2009-02-28
?>
```
  

---

UK dates (eg. 27/05/1990) won&apos;t work with strotime, even with timezone properly set. <br><br>/*<br>However, if you just replace "/" with "-" it will work fine.<br>

```
<?php
$timestamp = strtotime(str_replace('/', '-', '27/05/1990'));
?>
```

*/

[red., derick]: What you instead should do is:



```
<?php
$date = date_create_from_format('d/m/y', '27/05/1990');
?>
```
<br><br>That does not make it a timestamp, but a DateTime object, which is much more versatile instead.  

---

WARNING when using "next month", "last month", "+1 month",  "-1 month" or any combination of +/-X months. It will give non-intuitive results on Jan 30th and 31st. <br><br>As described at : http://derickrethans.nl/obtaining-the-next-month-in-php.html<br><br>

```
<?php
$d = new DateTime( '2010-01-31' );
$d->modify( 'next month' );
echo $d->format( 'F' ), "\n";
?>
```


In the above, using "next month" on January 31 will output "March" even though you might want it to output "February". ("+1 month" will give the same result. "last month", "-1 month" are similarly affected, but the results would be seen at beginning of March.)

The way to get what people would generally be looking for when they say "next month" even on Jan 30 and Jan 31 is to use "first day of next month":



```
<?php
$d = new DateTime( '2010-01-08' );
$d->modify( 'first day of next month' );
echo $d->format( 'F' ), "\n";
?>
```




```
<?php
$d = new DateTime( '2010-01-08' );
$d->modify( 'first day of +1 month' );
echo $d->format( 'F' ), "\n";
?>
```
  

---

I tried using sams most popular example but got incorrect results.<br><br>Incorrect:<br>

```
<?php 
echo date("jS F, Y", strtotime("11.12.10")); 
// outputs 10th December, 2011 

echo date("jS F, Y", strtotime("11/12/10")); 
// outputs 12th November, 2010 

echo date("jS F, Y", strtotime("11-12-10")); 
// outputs 11th December, 2010  
?>
```
 <br><br>Then I read the notes which said:<br>if the separator is a slash (/), then the American m/d/y is assumed; whereas if the separator is a dash (-) or a dot (.), then the European d-m-y format is assumed. ***If, however, the year is given in a two digit format and the separator is a dash (-), the date string is parsed as y-m-d.***<br><br>Therefore, the above code does not work on 2 digit years - only 4 digit years  

---

A useful testing tool for strtotime() and unix timestamp conversion:<br>http://strtotime.co.uk/  

---

strtotime() also returns time by year and weeknumber. (I use PHP 5.2.8, PHP 4 does not support it.) Queries can be in two forms:<br>- "yyyyWww", where yyyy is 4-digit year, W is literal and ww is 2-digit weeknumber. Returns timestamp for first day of week (for me Monday)<br>- "yyyy-Www-d", where yyyy is 4-digit year, W is literal, ww is 2-digit weeknumber and dd is day of week (1 for Monday, 7 for Sunday) <br><br>

```
<?php
// Get timestamp of 32nd week in 2009.
strtotime('2009W32'); // returns timestamp for Mon, 03 Aug 2009 00:00:00
// Weeknumbers < 10 must be padded with zero:
strtotime('2009W01'); // returns timestamp for Mon, 29 Dec 2008 00:00:00
// strtotime('2009W1'); // error! returns false

// See timestamp for Tuesday in 5th week of 2008
strtotime('2008-W05-2'); // returns timestamp for Tue, 29 Jan 2008 00:00:00
?>
```
<br><br>Weeknumbers are (probably) computed according to ISO-8601 specification, so doing date(&apos;W&apos;) on given timestamps should return passed weeknumber.  

---

[Official documentation page](https://www.php.net/manual/en/function.strtotime.php)

**[To root](/README.md)**