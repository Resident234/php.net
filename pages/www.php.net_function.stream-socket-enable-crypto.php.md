# stream_socket_enable_crypto



Constants added in PHP 5.6 :<br><br>STREAM_CRYPTO_METHOD_ANY_CLIENT<br>STREAM_CRYPTO_METHOD_TLSv1_0_CLIENT<br>STREAM_CRYPTO_METHOD_TLSv1_1_CLIENT<br>STREAM_CRYPTO_METHOD_TLSv1_2_CLIENT<br>STREAM_CRYPTO_METHOD_ANY_SERVER<br>STREAM_CRYPTO_METHOD_TLSv1_0_SERVER<br>STREAM_CRYPTO_METHOD_TLSv1_1_SERVER<br>STREAM_CRYPTO_METHOD_TLSv1_2_SERVER<br><br>Now, be careful because since PHP 5.6.7, STREAM_CRYPTO_METHOD_TLS_CLIENT (same for _SERVER) no longer means any tls version but tls 1.0 only (for "backward compatibility"...).<br><br>Before PHP 5.6.7 :<br>STREAM_CRYPTO_METHOD_SSLv23_CLIENT = STREAM_CRYPTO_METHOD_SSLv2_CLIENT|STREAM_CRYPTO_METHOD_SSLv3_CLIENT<br>STREAM_CRYPTO_METHOD_TLS_CLIENT = STREAM_CRYPTO_METHOD_TLSv1_0_CLIENT|STREAM_CRYPTO_METHOD_TLSv1_1_CLIENT|STREAM_CRYPTO_METHOD_TLSv1_2_CLIENT<br><br>PHP &gt;= 5.6.7<br>STREAM_CRYPTO_METHOD_SSLv23_CLIENT = STREAM_CRYPTO_METHOD_TLSv1_0_CLIENT|STREAM_CRYPTO_METHOD_TLSv1_1_CLIENT|STREAM_CRYPTO_METHOD_TLSv1_2_CLIENT<br>STREAM_CRYPTO_METHOD_TLS_CLIENT = STREAM_CRYPTO_METHOD_TLSv1_0_CLIENT<br><br>PHP bug : https://bugs.php.net/bug.php?id=69195<br>Commit : https://github.com/php/php-src/commit/10bc5fd4c4c8e1dd57bd911b086e9872a56300a0<br><br>STREAM_CRYPTO_METHOD_SSLv23_CLIENT is not safe to use because before php 5.6.7, it means sslv2 or sslv3. So, you should do this :<br>

```
<?php
$crypto_method = STREAM_CRYPTO_METHOD_TLS_CLIENT;

if (defined('STREAM_CRYPTO_METHOD_TLSv1_2_CLIENT')) {
    $crypto_method |= STREAM_CRYPTO_METHOD_TLSv1_2_CLIENT;
    $crypto_method |= STREAM_CRYPTO_METHOD_TLSv1_1_CLIENT;
}

stream_socket_enable_crypto($socket, true, $crypto_method);
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.stream-socket-enable-crypto.php)

**[To root](/README.md)**