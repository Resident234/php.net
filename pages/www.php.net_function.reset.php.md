# reset



GOTCHA: If your first element is false, you don&apos;t know whether it was empty or not.<br><br>

```
<?php

$a = array();
$b = array(false, true, true);
var_dump(reset($a) === reset($b)); //bool(true)

?>
```
<br><br>So don&apos;t count on a false return being an empty array.  

#

[Official documentation page](https://www.php.net/manual/en/function.reset.php)

**[To root](/README.md)**