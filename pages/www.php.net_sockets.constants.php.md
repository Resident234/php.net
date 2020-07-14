# Predefined Constants



We can know sockets constants values with :<br><br>

```
<?php
$a = get_defined_constants(TRUE) ;
foreach ( $a['sockets'] as $constant =&gt; $value ) {
    printf("%-25s %d\r\n", $constant, $value) ;
}
?>
```
<br><br>AF_UNIX                   1<br>AF_INET                   2<br>AF_INET6                  23<br>SOCK_STREAM               1<br>SOCK_DGRAM                2<br>SOCK_RAW                  3<br>SOCK_SEQPACKET            5<br>SOCK_RDM                  4<br>MSG_OOB                   1<br>MSG_WAITALL               0<br>MSG_PEEK                  2<br>MSG_DONTROUTE             4<br>SO_DEBUG                  1<br>SO_REUSEADDR              4<br>SO_KEEPALIVE              8<br>SO_DONTROUTE              16<br>SO_LINGER                 128<br>SO_BROADCAST              32<br>SO_OOBINLINE              256<br>SO_SNDBUF                 4097<br>SO_RCVBUF                 4098<br>SO_SNDLOWAT               4099<br>SO_RCVLOWAT               4100<br>SO_SNDTIMEO               4101<br>SO_RCVTIMEO               4102<br>SO_TYPE                   4104<br>SO_ERROR                  4103<br>SOL_SOCKET                65535<br>SOMAXCONN                 2147483647<br>TCP_NODELAY               1<br>PHP_NORMAL_READ           1<br>PHP_BINARY_READ           2<br>SOCKET_EINTR              10004<br>SOCKET_EBADF              10009<br>SOCKET_EACCES             10013<br>SOCKET_EFAULT             10014<br>SOCKET_EINVAL             10022<br>SOCKET_EMFILE             10024<br>SOCKET_EWOULDBLOCK        10035<br>SOCKET_EINPROGRESS        10036<br>SOCKET_EALREADY           10037<br>SOCKET_ENOTSOCK           10038<br>SOCKET_EDESTADDRREQ       10039<br>SOCKET_EMSGSIZE           10040<br>SOCKET_EPROTOTYPE         10041<br>SOCKET_ENOPROTOOPT        10042<br>SOCKET_EPROTONOSUPPORT    10043<br>SOCKET_ESOCKTNOSUPPORT    10044<br>SOCKET_EOPNOTSUPP         10045<br>SOCKET_EPFNOSUPPORT       10046<br>SOCKET_EAFNOSUPPORT       10047<br>SOCKET_EADDRINUSE         10048<br>SOCKET_EADDRNOTAVAIL      10049<br>SOCKET_ENETDOWN           10050<br>SOCKET_ENETUNREACH        10051<br>SOCKET_ENETRESET          10052<br>SOCKET_ECONNABORTED       10053<br>SOCKET_ECONNRESET         10054<br>SOCKET_ENOBUFS            10055<br>SOCKET_EISCONN            10056<br>SOCKET_ENOTCONN           10057<br>SOCKET_ESHUTDOWN          10058<br>SOCKET_ETOOMANYREFS       10059<br>SOCKET_ETIMEDOUT          10060<br>SOCKET_ECONNREFUSED       10061<br>SOCKET_ELOOP              10062<br>SOCKET_ENAMETOOLONG       10063<br>SOCKET_EHOSTDOWN          10064<br>SOCKET_EHOSTUNREACH       10065<br>SOCKET_ENOTEMPTY          10066<br>SOCKET_EPROCLIM           10067<br>SOCKET_EUSERS             10068<br>SOCKET_EDQUOT             10069<br>SOCKET_ESTALE             10070<br>SOCKET_EREMOTE            10071<br>SOCKET_EDISCON            10101<br>SOCKET_SYSNOTREADY        10091<br>SOCKET_VERNOTSUPPORTED    10092<br>SOCKET_NOTINITIALISED     10093<br>SOCKET_HOST_NOT_FOUND     11001<br>SOCKET_TRY_AGAIN          11002<br>SOCKET_NO_RECOVERY        11003<br>SOCKET_NO_DATA            11004<br>SOCKET_NO_ADDRESS         11004<br>SOL_TCP                   6<br>SOL_UDP                   17  

#

[Official documentation page](https://www.php.net/manual/en/sockets.constants.php)

**[To root](/README.md)**