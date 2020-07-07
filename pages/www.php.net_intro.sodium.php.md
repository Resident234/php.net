# Introduction







```
<?php 
//Simple Usage

/**
 * Encrypt a message
 * 
 * @param string $message - message to encrypt
 * @param string $key - encryption key
 * @return string
 */
function safeEncrypt($message, $key)
{
&#xA0; &#xA0; $nonce = random_bytes(
&#xA0; &#xA0; &#xA0; &#xA0; SODIUM_CRYPTO_SECRETBOX_NONCEBYTES
&#xA0; &#xA0; );

&#xA0; &#xA0; $cipher = base64_encode(
&#xA0; &#xA0; &#xA0; &#xA0; $nonce.
&#xA0; &#xA0; &#xA0; &#xA0; sodium_crypto_secretbox(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $message,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $nonce,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $key
&#xA0; &#xA0; &#xA0; &#xA0; )
&#xA0; &#xA0; );
&#xA0; &#xA0; sodium_memzero($message);
&#xA0; &#xA0; sodium_memzero($key);
&#xA0; &#xA0; return $cipher;
}

/**
 * Decrypt a message
 * 
 * @param string $encrypted - message encrypted with safeEncrypt()
 * @param string $key - encryption key
 * @return string
 */
function safeDecrypt($encrypted, $key)
{&#xA0;&#xA0; 
&#xA0; &#xA0; $decoded = base64_decode($encrypted);
&#xA0; &#xA0; if ($decoded === false) {
&#xA0; &#xA0; &#xA0; &#xA0; throw new Exception(&apos;Scream bloody murder, the encoding failed&apos;);
&#xA0; &#xA0; }
&#xA0; &#xA0; if (mb_strlen($decoded, &apos;8bit&apos;) &lt; (SODIUM_CRYPTO_SECRETBOX_NONCEBYTES + SODIUM_CRYPTO_SECRETBOX_MACBYTES)) {
&#xA0; &#xA0; &#xA0; &#xA0; throw new Exception(&apos;Scream bloody murder, the message was truncated&apos;);
&#xA0; &#xA0; }
&#xA0; &#xA0; $nonce = mb_substr($decoded, 0, SODIUM_CRYPTO_SECRETBOX_NONCEBYTES, &apos;8bit&apos;);
&#xA0; &#xA0; $ciphertext = mb_substr($decoded, SODIUM_CRYPTO_SECRETBOX_NONCEBYTES, null, &apos;8bit&apos;);

&#xA0; &#xA0; $plain = sodium_crypto_secretbox_open(
&#xA0; &#xA0; &#xA0; &#xA0; $ciphertext,
&#xA0; &#xA0; &#xA0; &#xA0; $nonce,
&#xA0; &#xA0; &#xA0; &#xA0; $key
&#xA0; &#xA0; );
&#xA0; &#xA0; if ($plain === false) {
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; throw new Exception(&apos;the message was tampered with in transit&apos;);
&#xA0; &#xA0; }
&#xA0; &#xA0; sodium_memzero($ciphertext);
&#xA0; &#xA0; sodium_memzero($key);
&#xA0; &#xA0; return $plain;
}
//Encrypt &amp; Decrypt your message
$key = sodium_crypto_secretbox_keygen();
$enc = safeEncrypt(&apos;Abdul Rafay Hingoro&apos;, $key); //generates random&#xA0; encrypted string (Base64 related)
echo $enc;
echo &apos;&lt;br&gt;&apos;;
$dec = safeDecrypt($enc, $key); //decrypts encoded string generated via safeEncrypt function 
echo $dec;
?>
```


//Output
DEx9ATXEg/eRq8GWD3NT5BatB3m31WEDEYLK2V4L0Am5GZGoa2rvYWUpoUeCrm7W/pdgLJrNoE6AA8U=
Abdul Rafay Hingoro

  

#

[Official documentation page](https://www.php.net/manual/en/intro.sodium.php)

**[To root](/README.md)**