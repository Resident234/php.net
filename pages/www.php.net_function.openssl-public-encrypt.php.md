# openssl_public_encrypt



I figured it out.  This function is not intended for general encryption and decryption.  For that, you want openssl_seal() and openssl_open().  

#

[Official documentation page](https://www.php.net/manual/en/function.openssl-public-encrypt.php)

**[To root](/README.md)**