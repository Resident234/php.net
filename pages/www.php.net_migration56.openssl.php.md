# OpenSSL changes in PHP 5.6.x



To get back to the "old behavior" with self-signed ("snakeoil") certificates to possibly incorrect name, you need to set both options.<br><br>

```
<?php
            $streamContext = stream_context_create([
                'ssl' => [
                    'verify_peer'      => false,
                    'verify_peer_name' => false
                ]
            ]);
            $contents = file_get_contents('https://url', false, $streamContext);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/migration56.openssl.php)

**[To root](/README.md)**