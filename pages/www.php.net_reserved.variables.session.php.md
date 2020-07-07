# $_SESSION





Creating New Session
==========================


```
<?php 
session_start();
/*session is started if you don&apos;t write this line can&apos;t use $_Session&#xA0; global variable*/
$_SESSION[&quot;newsession&quot;]=$value;
?>
```

Getting Session
==========================


```
<?php 
session_start();
/*session is started if you don&apos;t write this line can&apos;t use $_Session&#xA0; global variable*/
$_SESSION[&quot;newsession&quot;]=$value;
/*session created*/
echo $_SESSION[&quot;newsession&quot;];
/*session was getting*/
?>
```

Updating Session
==========================


```
<?php 
session_start();
/*session is started if you don&apos;t write this line can&apos;t use $_Session&#xA0; global variable*/
$_SESSION[&quot;newsession&quot;]=$value;
/*it is my new session*/
$_SESSION[&quot;newsession&quot;]=$updatedvalue;
/*session updated*/
?>
```

Deleting Session
==========================


```
<?php 
session_start();
/*session is started if you don&apos;t write this line can&apos;t use $_Session&#xA0; global variable*/
$_SESSION[&quot;newsession&quot;]=$value;
unset($_SESSION[&quot;newsession&quot;]);
/*session deleted. if you try using this you&apos;ve got an error*/
?>
```


Reference: http://gencbilgin.net/php-session-kullanimi.html

  

#



Please note that if you have register_globals to On, global variables associated to $_SESSION variables are references, so this may lead to some weird situations.



```
<?php

session_start();

$_SESSION[&apos;test&apos;] = 42;
$test = 43;
echo $_SESSION[&apos;test&apos;];

?>
```


Load the page, OK it displays 42, reload the page... it displays 43.

The solution is to do this after each time you do a session_start() :



```
<?php

if (ini_get(&apos;register_globals&apos;))
{
&#xA0; &#xA0; foreach ($_SESSION as $key=&gt;$value)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if (isset($GLOBALS[$key]))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; unset($GLOBALS[$key]);
&#xA0; &#xA0; }
}

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.session.php)

**[To root](/README.md)**