# socket_bind



If you want to reuse address and port, and get rid of error: unable to bind, address already in use, you have to use socket_setopt (check actual spelling for this function in you PHP verison) before calling bind:<br><br>

```
<?php
if (!socket_set_option($sock, SOL_SOCKET, SO_REUSEADDR, 1)) {
    echo socket_strerror(socket_last_error($sock));
    exit;
}
?>
```
<br><br>This solution was found by <br>Christophe Dirac. Thank you Christophe!  

#

[Official documentation page](https://www.php.net/manual/en/function.socket-bind.php)

**[To root](/README.md)**