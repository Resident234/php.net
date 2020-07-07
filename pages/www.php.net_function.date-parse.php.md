# date_parse





A warning to others. Some keys will return with a default value where others will return as false if the date string has it omitted. Unsure if this is a bug or feature, but hopefully this will save someone some time.


```
<?php
///Example
$input = &quot;Feb 2010&quot;;
$info = date_parse($input);
var_dump($info);

/*Returns:
array(12) { 
&#xA0; &#xA0; [&quot;year&quot;]=&gt; int(2010)
&#xA0; &#xA0; [&quot;month&quot;]=&gt; int(2)
&#xA0; &#xA0; [&quot;day&quot;]=&gt; int(1)&#xA0; &#xA0; //&lt;---expected false like below
&#xA0; &#xA0; [&quot;hour&quot;]=&gt; bool(false)
&#xA0; &#xA0; [&quot;minute&quot;]=&gt; bool(false)
&#xA0; &#xA0; [&quot;second&quot;]=&gt; bool(false)
&#xA0; &#xA0; [&quot;fraction&quot;]=&gt; bool(false)
&#xA0; &#xA0; [&quot;warning_count&quot;]=&gt; int(0)
&#xA0; &#xA0; [&quot;warnings&quot;]=&gt; array(0) { }
&#xA0; &#xA0; [&quot;error_count&quot;]=&gt; int(0)
&#xA0; &#xA0; [&quot;errors&quot;]=&gt; array(0) { }
&#xA0; &#xA0; [&quot;is_localtime&quot;]=&gt; bool(false)
}*/
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.date-parse.php)

**[To root](/README.md)**