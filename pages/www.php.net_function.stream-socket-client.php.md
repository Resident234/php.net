# stream_socket_client



For those wanting to use stream_socket_client() to connect to a local UNIX socket who can&apos;t find documentation on how to do it, here&apos;s a (rough) example:<br><br>

```
<?php

$sock = stream_socket_client('unix:///full/path/to/my/socket.sock', $errno, $errstr);

fwrite($sock, 'SOME COMMAND'."\r\n");

echo fread($sock, 4096)."\n";

fclose($sock);

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.stream-socket-client.php)

**[To root](/README.md)**