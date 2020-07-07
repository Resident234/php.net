# timezone_abbreviations_list





This was driving me nuts so I&apos;m adding a note here.



Took a while to get the simple time zone abbreviation for a given time zone. If you have the name just do this:





```
<?php

$dateTime = new DateTime();

$dateTime-&gt;setTimeZone(new DateTimeZone(&apos;America/Los_Angeles&apos;));

return $dateTime-&gt;format(&apos;T&apos;);

?>
```




Will return PST



[red.: Or PDT is the current date/time is during Daylight Savings Time]

  

#

[Official documentation page](https://www.php.net/manual/en/function.timezone-abbreviations-list.php)

**[To root](/README.md)**