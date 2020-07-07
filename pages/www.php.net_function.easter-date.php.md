# easter_date





To compute the correct Easter date for Eastern Orthodox Churches I made a function based on the Meeus Julian algorithm:





```
<?php

function orthodox_eastern($year) {

&#xA0; &#xA0; $a = $year % 4;

&#xA0; &#xA0; $b = $year % 7;

&#xA0; &#xA0; $c = $year % 19;

&#xA0; &#xA0; $d = (19 * $c + 15) % 30;

&#xA0; &#xA0; $e = (2 * $a + 4 * $b - $d + 34) % 7;

&#xA0; &#xA0; $month = floor(($d + $e + 114) / 31);

&#xA0; &#xA0; $day = (($d + $e + 114) % 31) + 1;

&#xA0; &#xA0; 

&#xA0; &#xA0; $de = mktime(0, 0, 0, $month, $day + 13, $year);

&#xA0; &#xA0; 

&#xA0; &#xA0; return $de;

}

?>
```



  

#



I recently had to write a function that allows me to know if today is a holiday.

And in France, we have some holidays which depends on the easter date. Maybe this will be helpful to someone.

Just modify in the $holidays array the actual holidays dates of your country.



```
<?php
/**
 * This function returns an array of timestamp corresponding to french holidays
 */
protected static function getHolidays($year = null)
{
&#xA0; if ($year === null)
&#xA0; {
&#xA0; &#xA0; $year = intval(date(&apos;Y&apos;));
&#xA0; }
&#xA0; &#xA0; 
&#xA0; $easterDate&#xA0; = easter_date($year);
&#xA0; $easterDay&#xA0;&#xA0; = date(&apos;j&apos;, $easterDate);
&#xA0; $easterMonth = date(&apos;n&apos;, $easterDate);
&#xA0; $easterYear&#xA0;&#xA0; = date(&apos;Y&apos;, $easterDate);

&#xA0; $holidays = array(
&#xA0; &#xA0; // These days have a fixed date
&#xA0; &#xA0; mktime(0, 0, 0, 1,&#xA0; 1,&#xA0; $year),&#xA0; // 1er janvier
&#xA0; &#xA0; mktime(0, 0, 0, 5,&#xA0; 1,&#xA0; $year),&#xA0; // F&#xEA;te du travail
&#xA0; &#xA0; mktime(0, 0, 0, 5,&#xA0; 8,&#xA0; $year),&#xA0; // Victoire des alli&#xE9;s
&#xA0; &#xA0; mktime(0, 0, 0, 7,&#xA0; 14, $year),&#xA0; // F&#xEA;te nationale
&#xA0; &#xA0; mktime(0, 0, 0, 8,&#xA0; 15, $year),&#xA0; // Assomption
&#xA0; &#xA0; mktime(0, 0, 0, 11, 1,&#xA0; $year),&#xA0; // Toussaint
&#xA0; &#xA0; mktime(0, 0, 0, 11, 11, $year),&#xA0; // Armistice
&#xA0; &#xA0; mktime(0, 0, 0, 12, 25, $year),&#xA0; // Noel

&#xA0; &#xA0; // These days have a date depending on easter
&#xA0; &#xA0; mktime(0, 0, 0, $easterMonth, $easterDay + 2,&#xA0; $easterYear),
&#xA0; &#xA0; mktime(0, 0, 0, $easterMonth, $easterDay + 40, $easterYear),
&#xA0; &#xA0; mktime(0, 0, 0, $easterMonth, $easterDay + 50, $easterYear),
&#xA0; );

&#xA0; sort($holidays);
&#xA0; 
&#xA0; return $holidays;
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.easter-date.php)

**[To root](/README.md)**