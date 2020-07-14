# socket_create



Took me about 20 minutes to figure out the proper arguments to supply for a AF_UNIX socket. Anything else, and I would get a PHP warning about the &apos;type&apos; not being supported. I hope this saves someone else time.<br><br>

```
<?php 
$socket = socket_create(AF_UNIX, SOCK_STREAM, 0);
// code
?>
```
  

#

It took some time to understand how one PHP process can communicate with another by means of unix udp sockets. Examples of &apos;server&apos; and &apos;client&apos; code are given below. Server is assumed to run before client starts.<br><br>&apos;Server&apos; code<br>

```
<?php
if (!extension_loaded(&apos;sockets&apos;)) {
    die(&apos;The sockets extension is not loaded.&apos;);
}
// create unix udp socket
$socket = socket_create(AF_UNIX, SOCK_DGRAM, 0);
if (!$socket)
        die(&apos;Unable to create AF_UNIX socket&apos;);

// same socket will be used in recv_from and send_to
$server_side_sock = dirname(__FILE__)."/server.sock";
if (!socket_bind($socket, $server_side_sock))
        die("Unable to bind to $server_side_sock");

while(1) // server never exits
{
// receive query
if (!socket_set_block($socket))
        die(&apos;Unable to set blocking mode for socket&apos;);
$buf = &apos;&apos;;
$from = &apos;&apos;;
echo "Ready to receive...\n";
// will block to wait client query
$bytes_received = socket_recvfrom($socket, $buf, 65536, 0, $from);
if ($bytes_received == -1)
        die(&apos;An error occured while receiving from the socket&apos;);
echo "Received $buf from $from\n";

$buf .= "-&gt;Response"; // process client query here

// send response
if (!socket_set_nonblock($socket))
        die(&apos;Unable to set nonblocking mode for socket&apos;);
// client side socket filename is known from client request: $from
$len = strlen($buf);
$bytes_sent = socket_sendto($socket, $buf, $len, 0, $from);
if ($bytes_sent == -1)
        die(&apos;An error occured while sending to the socket&apos;);
else if ($bytes_sent != $len)
        die($bytes_sent . &apos; bytes have been sent instead of the &apos; . $len . &apos; bytes expected&apos;);
echo "Request processed\n";
}
?>
```


&apos;Client&apos; code


```
<?php
if (!extension_loaded(&apos;sockets&apos;)) {
    die(&apos;The sockets extension is not loaded.&apos;);
}
// create unix udp socket
$socket = socket_create(AF_UNIX, SOCK_DGRAM, 0);
if (!$socket)
        die(&apos;Unable to create AF_UNIX socket&apos;);

// same socket will be later used in recv_from
// no binding is required if you wish only send and never receive
$client_side_sock = dirname(__FILE__)."/client.sock";
if (!socket_bind($socket, $client_side_sock))
        die("Unable to bind to $client_side_sock");

// use socket to send data
if (!socket_set_nonblock($socket))
        die(&apos;Unable to set nonblocking mode for socket&apos;);
// server side socket filename is known apriori
$server_side_sock = dirname(__FILE__)."/server.sock";
$msg = "Message";
$len = strlen($msg);
// at this point &apos;server&apos; process must be running and bound to receive from serv.sock
$bytes_sent = socket_sendto($socket, $msg, $len, 0, $server_side_sock);
if ($bytes_sent == -1)
        die(&apos;An error occured while sending to the socket&apos;);
else if ($bytes_sent != $len)
        die($bytes_sent . &apos; bytes have been sent instead of the &apos; . $len . &apos; bytes expected&apos;);

// use socket to receive data
if (!socket_set_block($socket))
        die(&apos;Unable to set blocking mode for socket&apos;);
$buf = &apos;&apos;;
$from = &apos;&apos;;
// will block to wait server response
$bytes_received = socket_recvfrom($socket, $buf, 65536, 0, $from);
if ($bytes_received == -1)
        die(&apos;An error occured while receiving from the socket&apos;);
echo "Received $buf from $from\n";

// close socket and delete own .sock file
socket_close($socket);
unlink($client_side_sock);
echo "Client exits\n";
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.socket-create.php)

**[To root](/README.md)**