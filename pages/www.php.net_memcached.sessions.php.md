# Sessions support



If you want to use &apos;memcacheD&apos; extention not &apos;memcache&apos; (there are two diffrent extentions) for session control,  you should pay attention to modify php.ini<br><br>Most web resource from google is based on memcache because It&apos;s earlier version than memcacheD. They will say as following<br><br>session.save_handler = memcache<br>session.save_path = "tcp://localhost:11211"<br><br>But it&apos;s not valid when it comes to memcacheD<br><br>you should modify php.ini like that<br><br>session.save_handler = memcached<br>session.save_path = "localhost:11211"<br><br>Look, there is no protocol indentifier  

#

If you are setting data to the session and it immediately disappears and you aren&apos;t getting any warnings in your PHP error log, it&apos;s probably because your sessions expired sometime in the 1970s.<br><br>Somewhere between memcached 1.0.2 and 2.1.0, the memcached session handler became sensitive to the 30-day TTL gotcha (aka "transparent failover").  If your session.gc_maxlifetime is greater than 2592000 (30 days), the value is treated as a unix timestamp instead of a relative seconds count.<br><br>This issue is likely to impact anyone with long-running sessions who is upgrading from Ubuntu 12.04 to 14.04.  

#

The documentation is not complete, you can also pass the weight of each server and you can use sockets if you want. In your PHP ini:<br><br>

```
<?php

// Sockets with weight in the format socket_path:port:weight
session.save_path = "/path/to/socket:0:42"

// Or more than one so that weight makes sense?
session.save_path = "/path/to/socket_x:0:42,/path/to/socket_y:0:666"

?>
```


And if you should ever want to access these servers in PHP:



```
<?php

$servers = explode(",", ini_get("session.save_path"));
$c = count($servers);
for ($i = 0; $i &lt; $c; ++$i) {
  $servers[$i] = explode(":", $servers[$i]);
}
$memcached = new \Memcached();
call_user_func_array([ $memcached, "addServers" ], $servers);
print_r($memcached-&gt;getAllKeys());

?>
```
  

#

If you are using the memcache class for session handling your key is the PHP session ID.  This is different than when using the  memcached class.<br><br>Example with memcache:<br>GET nphu2u8eo5niltfgdbc33ajb62<br><br>Example with memcached:<br>GET memc.sess.key.nphu2u8eo5niltfgdbc33ajb62<br><br>For memcached, the prefix is set in the config:<br>memcached.sess_prefix = "memc.sess.key."  

#

[Official documentation page](https://www.php.net/manual/en/memcached.sessions.php)

**[To root](/README.md)**