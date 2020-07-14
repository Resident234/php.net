# chop



Rather use rtrim(). Usage of chop() is not very clear nor consistent for people reading the code after you.  

#

If you are searching for a function that does the same trick as chop in PERL, then you should just do the following code:<br>

```
<?php
   $str = substr($str, 0, -1);
?>
```
<br><br>The question is: why isn&apos;t chop() an alias of the code above, rather than something which will trap developpers?  

#

[Official documentation page](https://www.php.net/manual/en/function.chop.php)

**[To root](/README.md)**