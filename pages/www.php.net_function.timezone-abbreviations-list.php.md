# timezone_abbreviations_list



This was driving me nuts so I&apos;m adding a note here.<br><br>Took a while to get the simple time zone abbreviation for a given time zone. If you have the name just do this:<br><br>

```
<?php
$dateTime = new DateTime();
$dateTime->setTimeZone(new DateTimeZone('America/Los_Angeles'));
return $dateTime->format('T');
?>
```
<br><br>Will return PST<br><br>[red.: Or PDT is the current date/time is during Daylight Savings Time]  

#

[Official documentation page](https://www.php.net/manual/en/function.timezone-abbreviations-list.php)

**[To root](/README.md)**