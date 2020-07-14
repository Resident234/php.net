# Memcached::addServer



Important to not call -&gt;addServers() every run -- only call it if no servers exist (check getServerList() ); otherwise, since addServers() does not check for dups, it will let you add the same server again and again and again, resultings in hundreds if not thousands of connections to the MC daemon. Specially when using FastCGI.<br><br>Example:<br><br>

```
<?php
class Cache {
        private $id;
        private $obj;

        function __construct($id){
                $this-&gt;id = $id;
                $this-&gt;obj = new Memcached($id);
        }

        public function connect($host , $port){
                $servers = $this-&gt;obj-&gt;getServerList();
                if(is_array($servers)) {
                        foreach ($servers as $server)
                                if($server[&apos;host&apos;] == $host and $server[&apos;port&apos;] == $port)
                                        return true;
                }
                return $this-&gt;obj-&gt;addServer($host , $port);
        }

}
?>
```
  

#

As of version 2.0.0b1 you can use Unix socket.<br><br>

```
<?php
$m = new Memcached();
$m-&gt;addServer(&apos;/path/to/socket&apos;,0);
?>
```
<br><br>Not to be confused with Memcache that use &apos;unix:///path/to/socket&apos;  

#

[Official documentation page](https://www.php.net/manual/en/memcached.addserver.php)

**[To root](/README.md)**