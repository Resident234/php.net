# $_SESSION



Creating New Session<br>==========================<br>

```
<?php 
session_start();
/*session is started if you don&apos;t write this line can&apos;t use $_Session  global variable*/
$_SESSION["newsession"]=$value;
?>
```

Getting Session
==========================


```
<?php 
session_start();
/*session is started if you don&apos;t write this line can&apos;t use $_Session  global variable*/
$_SESSION["newsession"]=$value;
/*session created*/
echo $_SESSION["newsession"];
/*session was getting*/
?>
```

Updating Session
==========================


```
<?php 
session_start();
/*session is started if you don&apos;t write this line can&apos;t use $_Session  global variable*/
$_SESSION["newsession"]=$value;
/*it is my new session*/
$_SESSION["newsession"]=$updatedvalue;
/*session updated*/
?>
```

Deleting Session
==========================


```
<?php 
session_start();
/*session is started if you don&apos;t write this line can&apos;t use $_Session  global variable*/
$_SESSION["newsession"]=$value;
unset($_SESSION["newsession"]);
/*session deleted. if you try using this you&apos;ve got an error*/
?>
```
<br><br>Reference: http://gencbilgin.net/php-session-kullanimi.html  

#

Please note that if you have register_globals to On, global variables associated to $_SESSION variables are references, so this may lead to some weird situations.<br><br>

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
    foreach ($_SESSION as $key=&gt;$value)
    {
        if (isset($GLOBALS[$key]))
            unset($GLOBALS[$key]);
    }
}

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.session.php)

**[To root](/README.md)**