# date_create





If you want to create the DateTime object directly from a timestamp use this





```
<?php

$st = 1170288000 //&#xA0; a timestamp

$dt = new DateTime(&quot;@$st&quot;);

?>
```




See also: http://bugs.php.net/bug.php?id=40171

  

#



If you are getting an error like this:

Exception: DateTime::__construct(): Failed to parse time string (13/02/2013) at position 0 (1): Unexpected character in DateTime-&gt;__construct()

Note that when you create a new date object using a format with slashes and dashes (eg 02-02-2012 or 02/02/2012) it must be in the mm/dd/yy(yy) or mm-dd-yy(yy) format (rather than british format dd/mm/yy)! Months always before years (the american style) otherwise you&apos;ll get an incorrect date and may get an error like the one above (where PHP is crashing on trying to decode a 13th month).

Can catch you off guard because everything seems to be working fine and dandy until you hit a value over 12.

  

#

[Official documentation page](https://www.php.net/manual/en/function.date-create.php)

**[To root](/README.md)**