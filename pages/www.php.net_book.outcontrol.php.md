# Output Buffering Control





The manual is a little shy on explaining that output buffers are nested, and that &quot;turns off output buffering&quot; means turning off the highest nested buffer.&#xA0; See ob_get_level (for a useful function, but still no explanation)





```
<?php

&#xA0; &#xA0; ob_start();

&#xA0; &#xA0; echo &quot;1:blah\n&quot;;

&#xA0; &#xA0; ob_start();

&#xA0; &#xA0; echo &quot;2:blah&quot;;

&#xA0; &#xA0; // ob_get_clean() returns the contents of the last buffer opened.&#xA0; The first &quot;blah&quot; and the output of var_dump are flushed from the top buffer on exit

&#xA0; &#xA0; var_dump(ob_get_clean());

&#xA0; &#xA0; exit;

?>
```




puts:

&#xA0; &#xA0; 1:blah

&#xA0; &#xA0; string(6) &quot;2:blah&quot;



Prior to realising this, I had thought PHP&apos;s ob functionality left more to be desired.&#xA0; I *really* wish I knew earlier.

  

#

[Official documentation page](https://www.php.net/manual/en/book.outcontrol.php)

**[To root](/README.md)**