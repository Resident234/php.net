# openssl_csr_new



Not sure whether the "bug" (undocumented behavior) I encountered is common to other people, but this comment might save hours of painful debug:<br>If you can&apos;t generate a new private key using openssl_pkey_new() or openssl_csr_new(), your script hangs during the call of these functions and in case you specified a "private_key_bits" parameter, ensure that you cast the variable to an int. Took me ages to notice that.<br><br>

```
<?php
$SSLcnf = array('config' => '/usr/local/nessy2/share/ssl/openssl.cnf',
        'encrypt_key' => true,
        'private_key_type' => OPENSSL_KEYTYPE_RSA,
        'digest_alg' => 'sha1',
        'x509_extensions' => 'v3_ca',
        'private_key_bits' => $someVariable // ---> bad
        'private_key_bits' => (int)$someVariable // ---> good
        'private_key_bits' => 512 // ---> obviously good
        );
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.openssl-csr-new.php)

**[To root](/README.md)**