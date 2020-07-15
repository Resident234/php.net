# DateTimeZone::listAbbreviations



This method returns an associative array containing some &apos;major&apos; timezones (like CEST), which on their own contain more specific &apos;geographic&apos; timezones (like Europe/Amsterdam).<br><br>If you&apos;re using these timezones and their offset/DST information, it&apos;s extremely important to realize the following:<br><br>*It seems like ALL DIFFERENT OFFSET/DST CONFIGURATIONS (including historical configurations) of each timezone are included!*<br><br>For example, Europe/Amsterdam can be found six times in the output of this function. Two occurrences (offset 1172/4772) are for the Amsterdam time used until 1937; two (1200/4800) are for the time that was used between 1937 and 1940; and two (3600/4800) are for the time used since 1940.<br><br>*Therefore, YOU CANNOT RELY ON THE OFFSET/DST INFORMATION RETURNED BY THIS FUNCTION as being currently correct/in use!*<br><br>If you want to know the current offset/DST of a certain timezone, you&apos;ll have to do something like this:<br><br>

```
<?php
$now = new DateTime(null, new DateTimeZone('Europe/Amsterdam'));
echo $now->getOffset();
?>
```
<br><br>P.S. I&apos;m sorry for my use of caps lock in this post, but as this behavior is not described in the documentation, I considered it to be important enough to shout. Normally I don&apos;t do such things :)  

#

[Official documentation page](https://www.php.net/manual/en/datetimezone.listabbreviations.php)

**[To root](/README.md)**