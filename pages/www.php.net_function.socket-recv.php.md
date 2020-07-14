# socket_recv



I&apos;ve used socket_select and socket_recv with a while loop and found myself in trouble when remote side closed connection. The code below produced infinite loop and socket_select returned immediately (which lead to high cpu time consumption).<br><br>&lt;?<br><br>socket_set_nonblock($my_socket);<br>$streams = array($my_socket/*, ... */);<br><br>$lastAccess = time();<br>while (socket_select($streams, $write = NULL, $except = NULL, SLEEP_TIME_SECONDS, SLEEP_TIME_MILLISECONDS) !== FALSE) {<br>    if (in_array($my_socket, $streams)) {<br>        while (@socket_recv($my_socket, $data, 8192, 0)) {<br>            echo $data;<br>        }<br>        $lastAccess = time();<br>    } else {<br>        if (time()-$lastAccess &gt; LAST_ACCESS_TIMEOUT) {<br>            break;<br>        }<br>    }<br>    // ...<br>    $streams = array($my_socket/*, ... */);<br>}<br><br>?>
```
<br><br>The solution was simple, but quite hard to find because socket_recv is not documented. socket_recv returns FALSE if there is no data and 0 if the socket is widowed (disconnected by remote side). So I had just to check return value of socket_recv. The problem now sounds stupid, but I&apos;ve spend some time to find it out.<br>I hope this will save some of somebody&apos;s hair ;)  

#

Workaround for the missing MSG_DONTWAIT flag according to the bug report page:<br><br>

```
<?php if(!defined(&apos;MSG_DONTWAIT&apos;)) define(&apos;MSG_DONTWAIT&apos;, 0x40); ?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.socket-recv.php)

**[To root](/README.md)**