# Other filters



Here is an example, since I cannot find one through out php.net"<br><br>

```
<?php<br><br>/**<br> * Strip whitespace (or other non-printable characters) from the beginning and end of a string<br> * @param string $value<br> */<br>function trimString($value)<br>{<br>    return trim($value);<br>}<br><br>$loginname = filter_input(INPUT_POST, &apos;loginname&apos;, FILTER_CALLBACK, array(&apos;options&apos; =&gt; &apos;trimString&apos;));  

#

[Official documentation page](https://www.php.net/manual/en/filter.filters.misc.php)

**[To root](/README.md)**