# inet_pton



Be careful, address with leading 0 return false.<br><br>Example : <br>

```
<?php
inet_pton(&apos;172.27.1.04&apos;); // return false
inet_pton(&apos;172.27.1.4&apos;) ;// return the good result
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.inet-pton.php)

**[To root](/README.md)**