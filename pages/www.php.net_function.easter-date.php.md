# easter_date



To compute the correct Easter date for Eastern Orthodox Churches I made a function based on the Meeus Julian algorithm:<br><br>

```
<?php
function orthodox_eastern($year) {
    $a = $year % 4;
    $b = $year % 7;
    $c = $year % 19;
    $d = (19 * $c + 15) % 30;
    $e = (2 * $a + 4 * $b - $d + 34) % 7;
    $month = floor(($d + $e + 114) / 31);
    $day = (($d + $e + 114) % 31) + 1;
    
    $de = mktime(0, 0, 0, $month, $day + 13, $year);
    
    return $de;
}
?>
```
  

#

I recently had to write a function that allows me to know if today is a holiday.<br><br>And in France, we have some holidays which depends on the easter date. Maybe this will be helpful to someone.<br><br>Just modify in the $holidays array the actual holidays dates of your country.<br><br>

```
<?php
/**
 * This function returns an array of timestamp corresponding to french holidays
 */
protected static function getHolidays($year = null)
{
  if ($year === null)
  {
    $year = intval(date(&apos;Y&apos;));
  }
    
  $easterDate  = easter_date($year);
  $easterDay   = date(&apos;j&apos;, $easterDate);
  $easterMonth = date(&apos;n&apos;, $easterDate);
  $easterYear   = date(&apos;Y&apos;, $easterDate);

  $holidays = array(
    // These days have a fixed date
    mktime(0, 0, 0, 1,  1,  $year),  // 1er janvier
    mktime(0, 0, 0, 5,  1,  $year),  // F&#xEA;te du travail
    mktime(0, 0, 0, 5,  8,  $year),  // Victoire des alli&#xE9;s
    mktime(0, 0, 0, 7,  14, $year),  // F&#xEA;te nationale
    mktime(0, 0, 0, 8,  15, $year),  // Assomption
    mktime(0, 0, 0, 11, 1,  $year),  // Toussaint
    mktime(0, 0, 0, 11, 11, $year),  // Armistice
    mktime(0, 0, 0, 12, 25, $year),  // Noel

    // These days have a date depending on easter
    mktime(0, 0, 0, $easterMonth, $easterDay + 2,  $easterYear),
    mktime(0, 0, 0, $easterMonth, $easterDay + 40, $easterYear),
    mktime(0, 0, 0, $easterMonth, $easterDay + 50, $easterYear),
  );

  sort($holidays);
  
  return $holidays;
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.easter-date.php)

**[To root](/README.md)**