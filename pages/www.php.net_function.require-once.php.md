# require_once





If your code is running on multiple servers with different environments (locations from where your scripts run) the following idea may be useful to you:



a. Do not give absolute path to include files on your server.

b. Dynamically calculate the full path (absolute path)



Hints:

Use a combination of dirname(__FILE__) and subsequent calls to itself until you reach to the home of your &apos;/index.php&apos;. Then, attach this variable (that contains the path) to your included files.



One of my typical example is:





```
<?php

define(&apos;__ROOT__&apos;, dirname(dirname(__FILE__)));

require_once(__ROOT__.&apos;/config.php&apos;);

?>
```




instead of:



```
<?php require_once(&apos;/var/www/public_html/config.php&apos;); ?>
```




After this, if you copy paste your codes to another servers, it will still run, without requiring any further re-configurations.



[EDIT BY danbrown AT php DOT net: Contains a typofix (missing &apos;)&apos;) provided by &apos;JoeB&apos; on 09-JUN-2011.]

  

#



require_once may not work correctly inside repetitive function when storing variable for example:

file: var.php


```
<?php
$foo = &apos;bar&apos;;
?>
```


file: check.php


```
<?php

function foo(){
&#xA0; &#xA0; require_once(&apos;var.php&apos;);
&#xA0; &#xA0; return $foo;
}

for($a=1;$a&lt;=5;$a++){
&#xA0; &#xA0; echo foo().&quot;&lt;br&gt;&quot;;
}

&gt; php check.php
result: 
bar
&lt;empty line&gt;
&lt;empty line&gt;
&lt;empty line&gt;
&lt;empty line&gt;

to make sure variable bar available at each function call, replace require once with require. eg situation: https://stackoverflow.com/questions/29898199/variables-not-defined-inside-function-on-second-time-at-foreach

Solution:

file: check2.php
&lt;?php

function foo(){
&#xA0; &#xA0; require(&apos;var.php&apos;);
&#xA0; &#xA0; return $foo;
}

for($a=1;$a&lt;=5;$a++){
&#xA0; &#xA0; echo foo().&quot;&lt;br&gt;&quot;;
}

&gt; php check2.php
result:
bar
bar
bar
bar
bar


  

#



&quot;require_once&quot; and &quot;require&quot; are language constructs and not functions. Therefore they should be written without &quot;()&quot; brackets!

  

#

[Official documentation page](https://www.php.net/manual/en/function.require-once.php)

**[To root](/README.md)**