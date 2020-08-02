# socket_recv



I&apos;ve used socket_select and socket_recv with a while loop and found myself in trouble when remote side closed connection. The code below produced infinite loop and socket_select returned immediately (which lead to high cpu time consumption).<br><br>

```
<?php

socket_set_nonblock($my_socket);
$streams = array($my_socket/*, ... */);

$lastAccess = time();
while (socket_select($streams, $write = NULL, $except = NULL, SLEEP_TIME_SECONDS, SLEEP_TIME_MILLISECONDS) !== FALSE) {
    if (in_array($my_socket, $streams)) {
        while (@socket_recv($my_socket, $data, 8192, 0)) {
            echo $data;
        }
        $lastAccess = time();
    } else {
        if (time()-$lastAccess > LAST_ACCESS_TIMEOUT) {
            break;
        }
    }
    // ...
    $streams = array($my_socket/*, ... */);
}

?>
```
<br><br>The solution was simple, but quite hard to find because socket_recv is not documented. socket_recv returns FALSE if there is no data and 0 if the socket is widowed (disconnected by remote side). So I had just to check return value of socket_recv. The problem now sounds stupid, but I&apos;ve spend some time to find it out.<br>I hope this will save some of somebody&apos;s hair ;)  

---

Workaround for the missing MSG_DONTWAIT flag according to the bug report page:<br><br>

```
<?php if(!defined('MSG_DONTWAIT')) define('MSG_DONTWAIT', 0x40); ?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.socket-recv.php)

**[To root](/README.md)**