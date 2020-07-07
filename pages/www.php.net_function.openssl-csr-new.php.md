# openssl_csr_new





Not sure whether the &quot;bug&quot; (undocumented behavior) I encountered is common to other people, but this comment might save hours of painful debug:
If you can&apos;t generate a new private key using openssl_pkey_new() or openssl_csr_new(), your script hangs during the call of these functions and in case you specified a &quot;private_key_bits&quot; parameter, ensure that you cast the variable to an int. Took me ages to notice that.



```
<?php
$SSLcnf = array(&apos;config&apos; =&gt; &apos;/usr/local/nessy2/share/ssl/openssl.cnf&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &apos;encrypt_key&apos; =&gt; true,
&#xA0; &#xA0; &#xA0; &#xA0; &apos;private_key_type&apos; =&gt; OPENSSL_KEYTYPE_RSA,
&#xA0; &#xA0; &#xA0; &#xA0; &apos;digest_alg&apos; =&gt; &apos;sha1&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &apos;x509_extensions&apos; =&gt; &apos;v3_ca&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &apos;private_key_bits&apos; =&gt; $someVariable // ---&gt; bad
&#xA0; &#xA0; &#xA0; &#xA0; &apos;private_key_bits&apos; =&gt; (int)$someVariable // ---&gt; good
&#xA0; &#xA0; &#xA0; &#xA0; &apos;private_key_bits&apos; =&gt; 512 // ---&gt; obviously good
&#xA0; &#xA0; &#xA0; &#xA0; );
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.openssl-csr-new.php)

**[To root](/README.md)**