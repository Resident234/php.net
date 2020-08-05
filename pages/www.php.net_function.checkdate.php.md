# checkdate



With DateTime you can make the shortest date&amp;time validator for all formats.<br><br>

```
<?php

function validateDate($date, $format = 'Y-m-d H:i:s')
{
    $d = DateTime::createFromFormat($format, $date);
    return $d &amp;&amp; $d->format($format) == $date;
}

var_dump(validateDate('2012-02-28 12:12:12')); # true
var_dump(validateDate('2012-02-30 12:12:12')); # false
var_dump(validateDate('2012-02-28', 'Y-m-d')); # true
var_dump(validateDate('28/02/2012', 'd/m/Y')); # true
var_dump(validateDate('30/02/2012', 'd/m/Y')); # false
var_dump(validateDate('14:50', 'H:i')); # true
var_dump(validateDate('14:77', 'H:i')); # false
var_dump(validateDate(14, 'H')); # true
var_dump(validateDate('14', 'H')); # true

var_dump(validateDate('2012-02-28T12:12:12+02:00', 'Y-m-d\TH:i:sP')); # true
# or
var_dump(validateDate('2012-02-28T12:12:12+02:00', DateTime::ATOM)); # true

var_dump(validateDate('Tue, 28 Feb 2012 12:12:12 +0200', 'D, d M Y H:i:s O')); # true
# or
var_dump(validateDate('Tue, 28 Feb 2012 12:12:12 +0200', DateTime::RSS)); # true
var_dump(validateDate('Tue, 27 Feb 2012 12:12:12 +0200', DateTime::RSS)); # false
# ...?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.checkdate.php)

**[To root](/README.md)**