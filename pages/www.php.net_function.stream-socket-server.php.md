# stream_socket_server



I&apos;m writing an HTTP server and I need SSL support, but getting this to work correctly with PHP streams took a bit of trial and error. For anyone who is trying to get an HTTP SSL server working with stream_socket_server:<br><br>1) Your SSL context will need to contain &apos;local_cert&apos;. If  you did not include your private key with your local_cert, you&apos;ll also need to specify &apos;local_pk&apos; which is your RSA key.  Your keys and certs should be PEM encoded, which means base-64. If your certificate has intermediary certs, you will need to specify those in the correct order: Your signed cert, intermediary cert 1, intermediary cert 2, etc. Each cert in the list needs to validate the one above it, but you do not need to include the CA Root that your SSL signer provided; that should already be included with the client&apos;s software (i.e. trust root certs).<br><br>You can append your private key in the file with your certs, however I keep mine in its own file. If you see the word "encrypted" when you view your key with a text viewer, you need to enter the correct passphrase and specify the context "passphrase", otherwise you can leave that one out.<br><br>As a server, verify_peer is irrelevant and should be set to false (should always be true if you are acting as an SSL client). Both cafile and capath contexts are not needed for functioning as a SSL/TLS server, but they are needed if you are making SSL connections with PHP as the client.<br><br>Lastly, the &apos;ciphers&apos; context should be set to a list of secure ciphers. Search for "mozilla recommended ciphers" and choose the string of ciphers that works for you, because not all openssl supported ciphers are secure. I went with the "intermediate" list, which provides high security and compatibility.<br><br>2) When you create the binding for stream_socket_server(), make sure that you choose the tcp:// wrapper. DO NOT USE ssl:// or tls://. Anything other than tcp:// will not work correctly AS A SERVER, those transports are what you use when making connections with PHP as a client. <br><br>Remember that the encryption does not start until after an SSL handshake completes, so the server has to listen in non-encrypted mode for new connections, and encryption doesn&apos;t start until certs are exchanged and a cipher is selected. When a new connection arrives you accept it with stream_socket_accept() and then use stream_socket_enable_crypto() to start the SSL session.<br><br>3) Keep in mind that the SSL handshake takes time, and that the stream_socket wrappers are high level and not as responsive as the socket extension due to the additional overhead they incur. For this reason you will need to enable blocking for accepting new connections.<br><br>&lt;enable blocking on ServerListenStream&gt;<br>newConnStream = stream_socket_accept(ServerListenStream);<br>&lt;disable blocking on ServerListenStream&gt;<br><br>&lt;enable blocking on newConnStream &gt;<br>stream_socket_enable_crypto(newConnStream, true, STREAM_CRYPTO_METHOD_SSLv23_SERVER);<br>&lt;disable blocking on newConnStream &gt;<br><br>Note that this is mainly for HTTP. If you are trying to do something like SMTP then your script will have to react to the "starttls" command, but it would be similar to the above except that you would wait for the "starttls" command before invoking the  stream_socket_enable_crypto() function on the client&apos;s stream.<br><br>TLS 1.0 is generally the way to go, SSLv3 is insecure and SSLv2 is buggy. If you use the mozilla recommend cipher list in your context, you&apos;ll be fine. Hope this helps someone out!  

#

In some specialized scenarios, you may want to create an AF_INET socket (UDP or TCP) but let the system select an unused port for you.  This is a standard feature of internet sockets but it doesn&apos;t seem to be documented how to do this for the stream_socket_server function.  It appears you can get this behavior by selecting zero for the port number, for example, my test below printed "127.0.0.1:4960".<br><br>

```
<?php
  $sock = stream_socket_server("udp://127.0.0.1:0"); 
  $name = stream_socket_get_name($sock);
  echo $name;
?>
```
  

#

Just a small example how to use this function and also stream_select() to make a server that accepts more than one connections (can have many clients connected):<br>In master we hold all opened connections. Just before calling stream select we copy the array to $read and then pass it ot stream_select(). In case that we may read from at least one socket, $read will contain socket descriptors. $master is needed not to lose references to the opened connections we have.<br>stream_server.php : <br>

```
<?php

$master = array();
$socket = stream_socket_server("tcp://0.0.0.0:8000", $errno, $errstr);
if (!$socket) {
    echo "$errstr ($errno)<br />\n";
} else {
    $master[] = $socket;
    $read = $master;
    while (1) {
        $read = $master;
        $mod_fd = stream_select($read, $_w = NULL, $_e = NULL, 5);
        if ($mod_fd === FALSE) {
            break;
        }
        for ($i = 0; $i < $mod_fd; ++$i) {
            if ($read[$i] === $socket) {
                $conn = stream_socket_accept($socket);
                fwrite($conn, "Hello! The time is ".date("n/j/Y g:i a")."\n");
                $master[] = $conn;
            } else {
                $sock_data = fread($read[$i], 1024);
                var_dump($sock_data);
                if (strlen($sock_data) === 0) { // connection closed
                    $key_to_del = array_search($read[$i], $master, TRUE);
                    fclose($read[$i]);
                    unset($master[$key_to_del]);
                } else if ($sock_data === FALSE) {
                    echo "Something bad happened";
                    $key_to_del = array_search($read[$i], $master, TRUE);
                    unset($master[$key_to_del]);
                } else {
                    echo "The client has sent :"; var_dump($sock_data);
                    fwrite($read[$i], "You have sent :[".$sock_data."]\n");
                    fclose($read[$i]);
                     unset($master[array_search($read[$i], $master)]);
                }
            }
        }
    }
}
?>
```

stream_client.php:


```
<?php
$fp = stream_socket_client("tcp://127.0.0.1:8000", $errno, $errstr, 30);
if (!$fp) {
    echo "$errstr ($errno)<br />\n";
} else {
    fwrite($fp, "Aloha");
    while (!feof($fp)) {
        var_dump(fgets($fp, 1024));
    }
    fclose($fp);
}
?>
```
<br><br>Thanks  

#

[Official documentation page](https://www.php.net/manual/en/function.stream-socket-server.php)

**[To root](/README.md)**