# session_destroy



If you want to change the session id on each log in, make sure to use session_regenerate_id(true) during the log in process.<br><br>

```
<?php
session_start();
session_regenerate_id(true);
?>
```
<br><br>[Edited by moderator (googleguy at php dot net)]  

#

It took me a while to figure out how to destroy a particular session in php. Note I&apos;m not sure if solution provided below is perfect but it seems work for me. Please feel free to post any easier way to destroy a particular session. Because it&apos;s quite useful for functionality of force an user offline.<br><br>1. If you&apos;re using db or memcached to manage session, you can always delete that session entry directly from db or memcached.<br><br>2. Using generic php session methods to delete a particular session(by session id).<br><br>

```
<?php
$session_id_to_destroy = 'nill2if998vhplq9f3pj08vjb1';
// 1. commit session if it's started.
if (session_id()) {
    session_commit();
}

// 2. store current session id
session_start();
$current_session_id = session_id();
session_commit();

// 3. hijack then destroy session specified.
session_id($session_id_to_destroy);
session_start();
session_destroy();
session_commit();

// 4. restore current session id. If don't restore it, your current session will refer to the session you just destroyed!
session_id($current_session_id);
session_start();
session_commit();

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.session-destroy.php)

**[To root](/README.md)**