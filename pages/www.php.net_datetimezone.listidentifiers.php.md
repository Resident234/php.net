# DateTimeZone::listIdentifiers



Even though the manual currently says that the first parameter has to be "One of DateTimeZone class constants", you may actually combine these constants:<br><br>

```
<?php
  $a = DateTimeZone::listIdentifiers(DateTimeZone::AFRICA); //gives africa time zones
  $b = DateTimeZone::listIdentifiers(DateTimeZone::AMERICA); //gives american time zones
  $c = DateTimeZone::listIdentifiers(DateTimeZone::AFRICA | DateTimeZone::AMERICA); //gives both african and american time zones
?>
```
<br><br>Be sure to use |, not ||.  

---

[Official documentation page](https://www.php.net/manual/en/datetimezone.listidentifiers.php)

**[To root](/README.md)**