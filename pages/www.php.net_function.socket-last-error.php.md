# socket_last_error



This is a bit long, but personally I prefer to use the standard C defines in my code.<br><br>

```
<?php

define(&apos;ENOTSOCK&apos;,      88);    /* Socket operation on non-socket */
define(&apos;EDESTADDRREQ&apos;,  89);    /* Destination address required */
define(&apos;EMSGSIZE&apos;,      90);    /* Message too long */
define(&apos;EPROTOTYPE&apos;,    91);    /* Protocol wrong type for socket */
define(&apos;ENOPROTOOPT&apos;,   92);    /* Protocol not available */
define(&apos;EPROTONOSUPPORT&apos;, 93);  /* Protocol not supported */
define(&apos;ESOCKTNOSUPPORT&apos;, 94);  /* Socket type not supported */
define(&apos;EOPNOTSUPP&apos;,    95);    /* Operation not supported on transport endpoint */
define(&apos;EPFNOSUPPORT&apos;,  96);    /* Protocol family not supported */
define(&apos;EAFNOSUPPORT&apos;,  97);    /* Address family not supported by protocol */
define(&apos;EADDRINUSE&apos;,    98);    /* Address already in use */
define(&apos;EADDRNOTAVAIL&apos;, 99);    /* Cannot assign requested address */
define(&apos;ENETDOWN&apos;,      100);   /* Network is down */
define(&apos;ENETUNREACH&apos;,   101);   /* Network is unreachable */
define(&apos;ENETRESET&apos;,     102);   /* Network dropped connection because of reset */
define(&apos;ECONNABORTED&apos;,  103);   /* Software caused connection abort */
define(&apos;ECONNRESET&apos;,    104);   /* Connection reset by peer */
define(&apos;ENOBUFS&apos;,       105);   /* No buffer space available */
define(&apos;EISCONN&apos;,       106);   /* Transport endpoint is already connected */
define(&apos;ENOTCONN&apos;,      107);   /* Transport endpoint is not connected */
define(&apos;ESHUTDOWN&apos;,     108);   /* Cannot send after transport endpoint shutdown */
define(&apos;ETOOMANYREFS&apos;,  109);   /* Too many references: cannot splice */
define(&apos;ETIMEDOUT&apos;,     110);   /* Connection timed out */
define(&apos;ECONNREFUSED&apos;,  111);   /* Connection refused */
define(&apos;EHOSTDOWN&apos;,     112);   /* Host is down */
define(&apos;EHOSTUNREACH&apos;,  113);   /* No route to host */
define(&apos;EALREADY&apos;,      114);   /* Operation already in progress */
define(&apos;EINPROGRESS&apos;,   115);   /* Operation now in progress */
define(&apos;EREMOTEIO&apos;,     121);   /* Remote I/O error */
define(&apos;ECANCELED&apos;,     125);   /* Operation Canceled */
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.socket-last-error.php)

**[To root](/README.md)**