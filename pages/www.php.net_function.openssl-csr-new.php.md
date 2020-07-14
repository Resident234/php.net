# openssl_csr_new



Not sure whether the "bug" (undocumented behavior) I encountered is common to other people, but this comment might save hours of painful debug:<br>If you can&apos;t generate a new private key using openssl_pkey_new() or openssl_csr_new(), your script hangs during the call of these functions and in case you specified a "private_key_bits" parameter, ensure that you cast the variable to an int. Took me ages to notice that.<br><br>

```
<?php
$SSLcnf = array(&apos;config&apos; =&gt; &apos;/usr/local/nessy2/share/ssl/openssl.cnf&apos;,
        &apos;encrypt_key&apos; =&gt; true,
        &apos;private_key_type&apos; =&gt; OPENSSL_KEYTYPE_RSA,
        &apos;digest_alg&apos; =&gt; &apos;sha1&apos;,
        &apos;x509_extensions&apos; =&gt; &apos;v3_ca&apos;,
        &apos;private_key_bits&apos; =&gt; $someVariable // ---&gt; bad
        &apos;private_key_bits&apos; =&gt; (int)$someVariable // ---&gt; good
        &apos;private_key_bits&apos; =&gt; 512 // ---&gt; obviously good
        );
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.openssl-csr-new.php)

**[To root](/README.md)**