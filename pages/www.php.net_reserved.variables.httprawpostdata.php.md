# $HTTP_RAW_POST_DATA





To get the Raw Post Data:





```
<?php $postdata = file_get_contents(&quot;php://input&quot;); ?>
```




Please see the notes here:

http://us.php.net/manual/en/wrappers.php.php

  

#



what is exaclty raw POST data?

Answer:

$_POST can be said as and outcome after splitting the $HTTP_RAW_POST_DATA, php splits the raw post data and formats in the way we see it in the $_POST For example:

&#xA0; &#xA0; $HTTP_RAW_POST_DATA looks something like this

key1=value1&amp;key2=value2

&#xA0; &#xA0; then $_POST would look like this:

$_POST = array(
&#xA0; &#xA0; &quot;key1&quot; =&gt; &quot;value1&quot;,
&#xA0; &#xA0; &quot;key2&quot; =&gt; &quot;value2&quot;,);

  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.httprawpostdata.php)

**[To root](/README.md)**