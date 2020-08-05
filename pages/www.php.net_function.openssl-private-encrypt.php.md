# openssl_private_encrypt



Just a little note on  [P.Peyremorte]&apos;s note.<br><br>"- openssl_private_encrypt can encrypt a maximum of 117 chars at one time."<br><br>This depends on the length of $key:<br><br>- For a 1024 bit key length =&gt; max number of chars (bytes) to encrypt = 1024/8 - 11(when padding used) = 117 chars (bytes).<br>- For a 2048 bit key length =&gt; max number of chars (bytes) to encrypt = 2048/8 - 11(when padding used) = 245 chars (bytes).<br>... and so on<br><br>By the way, if openssl_private_encrypt fails because of data size you won&apos;t get anything but just false as returned value, the same for openssl_public_decrypt() on decryption.<br><br>"- the encrypted output string is always 129 char length. If you use base64_encode on the encrypted output, it will give always 172 chars, with the last always "=" (filler)"<br><br>This again depends on the length of $key:<br><br>- For a 1024 bit key length =&gt; encrypted number of raw bytes is always a block of 128 bytes (1024 bits) by RSA design.<br>- For a 2048 bit key length =&gt; encrypted number of raw bytes is always a block of 256 bytes (2048 bits) by RSA design.<br>... and so on<br><br>About base64_encode output length, it depends on what you encode (meaning it depends on the bytes resulting after encryption), but in general the resulting encoded string will be about a 33% bigger (for 128 bytes bout 170 bytes and for 256 bytes about 340 bytes).<br><br>I would then generalize a little [P.Peyremorte]&apos;s note by:<br>

```
<?php
// given the variables as constants:

  //Block size for encryption block cipher
  private $ENCRYPT_BLOCK_SIZE = 200;// this for 2048 bit key for example, leaving some room

  //Block size for decryption block cipher
  private $DECRYPT_BLOCK_SIZE = 256;// this again for 2048 bit key

         //For encryption we would use:
  function encrypt_RSA($plainData, $privatePEMKey)
  {
    $encrypted = '';
    $plainData = str_split($plainData, $this->ENCRYPT_BLOCK_SIZE);
    foreach($plainData as $chunk)
    {
      $partialEncrypted = '';

      //using for example OPENSSL_PKCS1_PADDING as padding
      $encryptionOk = openssl_private_encrypt($chunk, $partialEncrypted, $privatePEMKey, OPENSSL_PKCS1_PADDING);

      if($encryptionOk === false){return false;}//also you can return and error. If too big this will be false
      $encrypted .= $partialEncrypted;
    }
    return base64_encode($encrypted);//encoding the whole binary String as MIME base 64
  }

         //For decryption we would use:
  protected function decrypt_RSA($publicPEMKey, $data)
  {
    $decrypted = '';

    //decode must be done before spliting for getting the binary String
    $data = str_split(base64_decode($data), $this->DECRYPT_BLOCK_SIZE);

    foreach($data as $chunk)
    {
      $partial = '';

      //be sure to match padding
      $decryptionOK = openssl_public_decrypt($chunk, $partial, $publicPEMKey, OPENSSL_PKCS1_PADDING);

      if($decryptionOK === false){return false;}//here also processed errors in decryption. If too big this will be false
      $decrypted .= $partial;
    }
    return $decrypted;
  }
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.openssl-private-encrypt.php)

**[To root](/README.md)**