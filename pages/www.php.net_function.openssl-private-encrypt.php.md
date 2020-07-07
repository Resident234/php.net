# openssl_private_encrypt





Just a little note on&#xA0; [P.Peyremorte]&apos;s note.

&quot;- openssl_private_encrypt can encrypt a maximum of 117 chars at one time.&quot;

This depends on the length of $key:

- For a 1024 bit key length =&gt; max number of chars (bytes) to encrypt = 1024/8 - 11(when padding used) = 117 chars (bytes).
- For a 2048 bit key length =&gt; max number of chars (bytes) to encrypt = 2048/8 - 11(when padding used) = 245 chars (bytes).
... and so on

By the way, if openssl_private_encrypt fails because of data size you won&apos;t get anything but just false as returned value, the same for openssl_public_decrypt() on decryption.

&quot;- the encrypted output string is always 129 char length. If you use base64_encode on the encrypted output, it will give always 172 chars, with the last always &quot;=&quot; (filler)&quot;

This again depends on the length of $key:

- For a 1024 bit key length =&gt; encrypted number of raw bytes is always a block of 128 bytes (1024 bits) by RSA design.
- For a 2048 bit key length =&gt; encrypted number of raw bytes is always a block of 256 bytes (2048 bits) by RSA design.
... and so on

About base64_encode output length, it depends on what you encode (meaning it depends on the bytes resulting after encryption), but in general the resulting encoded string will be about a 33% bigger (for 128 bytes bout 170 bytes and for 256 bytes about 340 bytes).

I would then generalize a little [P.Peyremorte]&apos;s note by:


```
<?php
// given the variables as constants:

&#xA0; //Block size for encryption block cipher
&#xA0; private $ENCRYPT_BLOCK_SIZE = 200;// this for 2048 bit key for example, leaving some room

&#xA0; //Block size for decryption block cipher
&#xA0; private $DECRYPT_BLOCK_SIZE = 256;// this again for 2048 bit key

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; //For encryption we would use:
&#xA0; function encrypt_RSA($plainData, $privatePEMKey)
&#xA0; {
&#xA0; &#xA0; $encrypted = &apos;&apos;;
&#xA0; &#xA0; $plainData = str_split($plainData, $this-&gt;ENCRYPT_BLOCK_SIZE);
&#xA0; &#xA0; foreach($plainData as $chunk)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; $partialEncrypted = &apos;&apos;;

&#xA0; &#xA0; &#xA0; //using for example OPENSSL_PKCS1_PADDING as padding
&#xA0; &#xA0; &#xA0; $encryptionOk = openssl_private_encrypt($chunk, $partialEncrypted, $privatePEMKey, OPENSSL_PKCS1_PADDING);

&#xA0; &#xA0; &#xA0; if($encryptionOk === false){return false;}//also you can return and error. If too big this will be false
&#xA0; &#xA0; &#xA0; $encrypted .= $partialEncrypted;
&#xA0; &#xA0; }
&#xA0; &#xA0; return base64_encode($encrypted);//encoding the whole binary String as MIME base 64
&#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; //For decryption we would use:
&#xA0; protected function decrypt_RSA($publicPEMKey, $data)
&#xA0; {
&#xA0; &#xA0; $decrypted = &apos;&apos;;

&#xA0; &#xA0; //decode must be done before spliting for getting the binary String
&#xA0; &#xA0; $data = str_split(base64_decode($data), $this-&gt;DECRYPT_BLOCK_SIZE);

&#xA0; &#xA0; foreach($data as $chunk)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; $partial = &apos;&apos;;

&#xA0; &#xA0; &#xA0; //be sure to match padding
&#xA0; &#xA0; &#xA0; $decryptionOK = openssl_public_decrypt($chunk, $partial, $publicPEMKey, OPENSSL_PKCS1_PADDING);

&#xA0; &#xA0; &#xA0; if($decryptionOK === false){return false;}//here also processed errors in decryption. If too big this will be false
&#xA0; &#xA0; &#xA0; $decrypted .= $partial;
&#xA0; &#xA0; }
&#xA0; &#xA0; return $decrypted;
&#xA0; }
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.openssl-private-encrypt.php)

**[To root](/README.md)**