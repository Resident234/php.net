# Supported Date and Time Formats



When you&apos;ve got external inputs that do not strictly follow the formatting and disambiguation rules, you may still be able to use the static method ::createFromFormat() to create a usable DateTime object<br><br>

```
<?php 
/**
 * Date values separated by slash are assumed to be in American order: m/d/y
 * Date values separated by dash are assumed to be in European order: d-m-y
 * Exact formats for date/time strings can be injected with ::createFromFormat()
 */
error_reporting(E_ALL);

// THIS IS INVALID, WOULD IMPLY MONTH == 19
$external = "19/10/2016 14:48:21";

// HOWEVER WE CAN INJECT THE FORMATTING WHEN WE DECODE THE DATE
$format = "d/m/Y H:i:s";
$dateobj = DateTime::createFromFormat($format, $external);

$iso_datetime = $dateobj->format(Datetime::ATOM);
echo "SUCCESS: $external EQUALS ISO-8601 $iso_datetime";

// MAN PAGE: http://php.net/manual/en/datetime.createfromformat.php?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/datetime.formats.php)

**[To root](/README.md)**