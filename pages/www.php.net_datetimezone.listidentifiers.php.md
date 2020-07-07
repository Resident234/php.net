# DateTimeZone::listIdentifiers





Even though the manual currently says that the first parameter has to be &quot;One of DateTimeZone class constants&quot;, you may actually combine these constants:



```
<?php
&#xA0; $a = DateTimeZone::listIdentifiers(DateTimeZone::AFRICA); //gives africa time zones
&#xA0; $b = DateTimeZone::listIdentifiers(DateTimeZone::AMERICA); //gives american time zones
&#xA0; $c = DateTimeZone::listIdentifiers(DateTimeZone::AFRICA | DateTimeZone::AMERICA); //gives both african and american time zones
?>
```


Be sure to use |, not ||.

  

#

[Official documentation page](https://www.php.net/manual/en/datetimezone.listidentifiers.php)

**[To root](/README.md)**