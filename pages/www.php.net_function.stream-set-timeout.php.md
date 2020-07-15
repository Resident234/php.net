# stream_set_timeout



In case anyone is puzzled, stream_set_timeout DOES NOT work for sockets created with socket_create or socket_accept. Use socket_set_option instead.<br><br>Instead of:<br>

```
<?php
stream_set_timeout($socket,$sec,$usec);
?>
```


Use:


```
<?php
socket_set_option($socket, SOL_SOCKET, SO_RCVTIMEO, array('sec'=>$sec, 'usec'=>$usec));
socket_set_option($socket, SOL_SOCKET, SO_SNDTIMEO, array('sec'=>$sec, 'usec'=>$usec));
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.stream-set-timeout.php)

**[To root](/README.md)**