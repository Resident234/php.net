# return





for those of you who think that using return in a script is the same as using exit note that: using return just exits the execution of the current script, exit the whole execution.

look at that example:

a.php


```
<?php
include(&quot;b.php&quot;);
echo &quot;a&quot;;
?>
```


b.php


```
<?php
echo &quot;b&quot;;
return;
?>
```


(executing a.php:) will echo &quot;ba&quot;.

whereas (b.php modified):

a.php


```
<?php
include(&quot;b.php&quot;);
echo &quot;a&quot;;
?>
```


b.php


```
<?php
echo &quot;b&quot;;
exit;
?>
```


(executing a.php:) will echo &quot;b&quot;.

  

#



Note that because PHP processes the file before running it, any functions defined in an included file will still be available, even if the file is not executed.



Example:



a.php



```
<?php

include &apos;b.php&apos;;



foo();

?>
```




b.php



```
<?php

return;



function foo() {

&#xA0; &#xA0;&#xA0; echo &apos;foo&apos;;

}

?>
```




Executing a.php will output &quot;foo&quot;.

  

#

[Official documentation page](https://www.php.net/manual/en/function.return.php)

**[To root](/README.md)**