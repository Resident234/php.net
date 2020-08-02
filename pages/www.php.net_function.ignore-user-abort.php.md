# ignore_user_abort



Comment from Anonymous is not 100% valid. Time from sleep function is not counted to execution time because sleep delays program execution (see http://www.php.net/manual/en/function.sleep.php and comments). We tested it and it&apos;s true. Try this:<br><br>

```
<?php

set_time_limit(2);
sleep(4);
echo 'hi!';
sleep(4);
echo 'bye bye!';

?>
```
<br><br>It means, that if the loop most of the time will be at sleep (and in this case it probably be), then this script may be active for months or years even if you set time limit to one day.  

---

If you want to simulate a crontask you must call this script once and it will keep running forever (during server uptime) in the background while "doing something" every specified seconds (= $interval):<br><br>

```
<?php
ignore_user_abort(1); // run script in background
set_time_limit(0); // run script forever
$interval=60*15; // do every 15 minutes...
do{
   // add the script that has to be ran every 15 minutes here
   // ...
   sleep($interval); // wait 15 minutes
}while(true);
?>
```
  

---

use the spiritual-coder&apos;s code below with exreme caution and do not use it unless you are an advanced user.<br><br>first of all, such a code with no bypass point can cause infinite loops and ghost threads in server. there must be a trick to break out the loop. <br><br>i.e. you can use  if (file_exists(dirname(__FILE__)."stop.txt")) break; in the loop so if you create "stop.txt", she script will stop execution.<br><br>also if you use set_time_limit(86400); instead of set_time_limit(0); your script will stop after one day.  

---

[Official documentation page](https://www.php.net/manual/en/function.ignore-user-abort.php)

**[To root](/README.md)**