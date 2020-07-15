# strspn



you can use this function with strlen to check illegal characters, string lenght must be the same than strspn (characters from my string contained in another)<br><br>

```
<?php

$digits='0123456789';

if (strlen($phone) != strspn($phone,$digits))
 echo "illegal characters";

?>
```
  

#

It took me some time to understand the way this function works&#x2026;<br>I&#x2019;ve compiled my own explanation with my own words that is more understandable for me personally than the official one or those that can be found in different tutorials on the web.<br>Perhaps, it will save someone several minutes&#x2026;<br><br>

```
<?php 
strspn(string $haystack, string $char_list [, int $start [, int $length]])
?>
```
<br><br>The way it works:<br> -   searches for a segment of $haystack that consists entirely from supplied through the second argument chars <br> -   $haystack must start from one of the chars supplied through $char_list, otherwise the function will find nothing<br> -   as soon as the function encounters a char that was not mentioned in $chars it understands that the segment is over and stops (it doesn&#x2019;t search for the second, third and so on segments)<br> -   finally, it measures the segment&#x2019;s length and return it (i.e. length)<br><br>In other words it finds a span (only the first one) in the string that consists entirely form chars supplied in $chars_list and returns its length  

#

[Official documentation page](https://www.php.net/manual/en/function.strspn.php)

**[To root](/README.md)**