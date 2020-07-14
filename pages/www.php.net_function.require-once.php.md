# require_once



If your code is running on multiple servers with different environments (locations from where your scripts run) the following idea may be useful to you:<br><br>a. Do not give absolute path to include files on your server.<br>b. Dynamically calculate the full path (absolute path)<br><br>Hints:<br>Use a combination of dirname(__FILE__) and subsequent calls to itself until you reach to the home of your &apos;/index.php&apos;. Then, attach this variable (that contains the path) to your included files.<br><br>One of my typical example is:<br><br>

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
<br><br>After this, if you copy paste your codes to another servers, it will still run, without requiring any further re-configurations.<br><br>[EDIT BY danbrown AT php DOT net: Contains a typofix (missing &apos;)&apos;) provided by &apos;JoeB&apos; on 09-JUN-2011.]  

#

require_once may not work correctly inside repetitive function when storing variable for example:<br><br>file: var.php<br>

```
<?php
$foo = &apos;bar&apos;;
?>
```


file: check.php


```
<?php

function foo(){
    require_once(&apos;var.php&apos;);
    return $foo;
}

for($a=1;$a&lt;=5;$a++){
    echo foo()."&lt;br&gt;";
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
    require(&apos;var.php&apos;);
    return $foo;
}

for($a=1;$a&lt;=5;$a++){
    echo foo()."&lt;br&gt;";
}

&gt; php check2.php<br>result:<br>bar<br>bar<br>bar<br>bar<br>bar  

#

"require_once" and "require" are language constructs and not functions. Therefore they should be written without "()" brackets!  

#

[Official documentation page](https://www.php.net/manual/en/function.require-once.php)

**[To root](/README.md)**