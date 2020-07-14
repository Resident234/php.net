# Examples



You can easily extend the first example to handle any number of connections instead of jsut one<br><br>#!/usr/bin/env php<br>

```
<?php
error_reporting(E_ALL);

/* Permitir al script esperar para conexiones. */
set_time_limit(0);

/* Activar el volcado de salida impl&#xED;cito, as&#xED; veremos lo que estamo obteniendo
 * mientras llega. */
ob_implicit_flush();

$address = &apos;127.0.0.1&apos;;
$port = 10000;

if (($sock = socket_create(AF_INET, SOCK_STREAM, SOL_TCP)) === false) {
    echo "socket_create() fall&#xF3;: raz&#xF3;n: " . socket_strerror(socket_last_error()) . "\n";
}

if (socket_bind($sock, $address, $port) === false) {
    echo "socket_bind() fall&#xF3;: raz&#xF3;n: " . socket_strerror(socket_last_error($sock)) . "\n";
}

if (socket_listen($sock, 5) === false) {
    echo "socket_listen() fall&#xF3;: raz&#xF3;n: " . socket_strerror(socket_last_error($sock)) . "\n";
}

//clients array
$clients = array();

do {
    $read = array();
    $read[] = $sock;
    
    $read = array_merge($read,$clients);
    
    // Set up a blocking call to socket_select
    if(socket_select($read,$write = NULL, $except = NULL, $tv_sec = 5) &lt; 1)
    {
        //    SocketServer::debug("Problem blocking socket_select?");
        continue;
    }
    
    // Handle new Connections
    if (in_array($sock, $read)) {        
        
        if (($msgsock = socket_accept($sock)) === false) {
            echo "socket_accept() fall&#xF3;: raz&#xF3;n: " . socket_strerror(socket_last_error($sock)) . "\n";
            break;
        }
        $clients[] = $msgsock;
        $key = array_keys($clients, $msgsock);
        /* Enviar instrucciones. */
        $msg = "\nBienvenido al Servidor De Prueba de PHP. \n" .
        "Usted es el cliente numero: {$key[0]}\n" .
        "Para salir, escriba &apos;quit&apos;. Para cerrar el servidor escriba &apos;shutdown&apos;.\n";
        socket_write($msgsock, $msg, strlen($msg));
        
    }
    
    // Handle Input
    foreach ($clients as $key =&gt; $client) { // for each client        
        if (in_array($client, $read)) {
            if (false === ($buf = socket_read($client, 2048, PHP_NORMAL_READ))) {
                echo "socket_read() fall&#xF3;: raz&#xF3;n: " . socket_strerror(socket_last_error($client)) . "\n";
                break 2;
            }
            if (!$buf = trim($buf)) {
                continue;
            }
            if ($buf == &apos;quit&apos;) {
                unset($clients[$key]);
                socket_close($client);
                break;
            }
            if ($buf == &apos;shutdown&apos;) {
                socket_close($client);
                break 2;
            }
            $talkback = "Cliente {$key}: Usted dijo &apos;$buf&apos;.\n";
            socket_write($client, $talkback, strlen($talkback));
            echo "$buf\n";
        }
        
    }        
} while (true);

socket_close($sock);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/sockets.examples.php)

**[To root](/README.md)**