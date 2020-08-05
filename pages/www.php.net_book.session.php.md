# Session Handling



There is a nuance we found with session timing out although the user is still active in the session.  The problem has to do with never modifying the session variable.<br><br>The GC will clear the session data files based on their last modification time.  Thus if you never modify the session, you simply read from it, then the GC will eventually clean up.<br><br>To prevent this you need to ensure that your session is modified within the GC delete time.  You can accomplish this like below.<br><br>

```
<?php
if( !isset($_SESSION['last_access']) || (time() - $_SESSION['last_access']) > 60 )
  $_SESSION['last_access'] = time();
?>
```
<br><br>This will update the session every 60s to ensure that the modification date is altered.  

---

[Official documentation page](https://www.php.net/manual/en/book.session.php)

**[To root](/README.md)**