# Socket Functions



After many non-sleep nights I got the most simple multi-client server written in PHP that really works. Ctrl+C and Ctrl+V... use as command line to test it. Enjoy it.<br><br>

```
<?php

ini_set('error_reporting', E_ALL ^ E_NOTICE);
ini_set('display_errors', 1);

// Set time limit to indefinite execution
set_time_limit (0);

// Set the ip and port we will listen on
$address = '10.203.9.67';
$port = 6901;

// Create a TCP Stream socket
$sock = socket_create(AF_INET, SOCK_STREAM, 0);

// Bind the socket to an address/port
socket_bind($sock, $address, $port) or die('Could not bind to address');

// Start listening for connections
socket_listen($sock);

// Non block socket type
socket_set_nonblock($sock);

// Loop continuously
while (true)
{
    unset($read);

    $j = 0;

    if (count($client))
    {
        foreach ($client AS $k =&gt; $v)
        {
            $read[$j] = $v;

            $j++;
        }
    }

    $client = $read;

    if ($newsock = @socket_accept($sock))
    {
        if (is_resource($newsock))
        {
            socket_write($newsock, "$j&gt;", 2).chr(0);
            
            echo "New client connected $j";

            $client[$j] = $newsock;

            $j++;
        }
    }

    if (count($client))
    {
        foreach ($client AS $k =&gt; $v)
        {
            if (@socket_recv($v, $string, 1024, MSG_DONTWAIT) === 0)
            {
                unset($client[$k]);

                socket_close($v);
            }
            else
            {
                if ($string)
                {
                    echo "$k: $string\n";
                }
            }
        }
    }

    //echo ".";

    sleep(1);
}

// Close the master sockets
socket_close($sock);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/ref.sockets.php)

**[To root](/README.md)**