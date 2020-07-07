# require





Remember, when using require that it is a statement, not a function. It&apos;s not necessary to write:


```
<?php
 require(&apos;somefile.php&apos;);
?>
```


The following:


```
<?php
require &apos;somefile.php&apos;;
?>
```


Is preferred, it will prevent your peers from giving you a hard time and a trivial conversation about what require really is.

  

#

[Official documentation page](https://www.php.net/manual/en/function.require.php)

**[To root](/README.md)**