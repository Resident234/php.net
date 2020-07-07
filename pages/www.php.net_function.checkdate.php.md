# checkdate





With DateTime you can make the shortest date&amp;time validator for all formats.



```
<?php

function validateDate($date, $format = &apos;Y-m-d H:i:s&apos;)
{
&#xA0; &#xA0; $d = DateTime::createFromFormat($format, $date);
&#xA0; &#xA0; return $d &amp;&amp; $d-&gt;format($format) == $date;
}

var_dump(validateDate(&apos;2012-02-28 12:12:12&apos;)); # true
var_dump(validateDate(&apos;2012-02-30 12:12:12&apos;)); # false
var_dump(validateDate(&apos;2012-02-28&apos;, &apos;Y-m-d&apos;)); # true
var_dump(validateDate(&apos;28/02/2012&apos;, &apos;d/m/Y&apos;)); # true
var_dump(validateDate(&apos;30/02/2012&apos;, &apos;d/m/Y&apos;)); # false
var_dump(validateDate(&apos;14:50&apos;, &apos;H:i&apos;)); # true
var_dump(validateDate(&apos;14:77&apos;, &apos;H:i&apos;)); # false
var_dump(validateDate(14, &apos;H&apos;)); # true
var_dump(validateDate(&apos;14&apos;, &apos;H&apos;)); # true

var_dump(validateDate(&apos;2012-02-28T12:12:12+02:00&apos;, &apos;Y-m-d\TH:i:sP&apos;)); # true
# or
var_dump(validateDate(&apos;2012-02-28T12:12:12+02:00&apos;, DateTime::ATOM)); # true

var_dump(validateDate(&apos;Tue, 28 Feb 2012 12:12:12 +0200&apos;, &apos;D, d M Y H:i:s O&apos;)); # true
# or
var_dump(validateDate(&apos;Tue, 28 Feb 2012 12:12:12 +0200&apos;, DateTime::RSS)); # true
var_dump(validateDate(&apos;Tue, 27 Feb 2012 12:12:12 +0200&apos;, DateTime::RSS)); # false
# ...


  

#

[Official documentation page](https://www.php.net/manual/en/function.checkdate.php)

**[To root](/README.md)**