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
    $nonce = random_bytes(
        SODIUM_CRYPTO_SECRETBOX_NONCEBYTES
    );

    $cipher = base64_encode(
        $nonce.
        sodium_crypto_secretbox(
            $message,
            $nonce,
            $key
        )
    );
    sodium_memzero($message);
    sodium_memzero($key);
    return $cipher;
}

/**
 * Decrypt a message
 * 
 * @param string $encrypted - message encrypted with safeEncrypt()
 * @param string $key - encryption key
 * @return string
 */
function safeDecrypt($encrypted, $key)
{   
    $decoded = base64_decode($encrypted);
    if ($decoded === false) {
        throw new Exception(&apos;Scream bloody murder, the encoding failed&apos;);
    }
    if (mb_strlen($decoded, &apos;8bit&apos;) &lt; (SODIUM_CRYPTO_SECRETBOX_NONCEBYTES + SODIUM_CRYPTO_SECRETBOX_MACBYTES)) {
        throw new Exception(&apos;Scream bloody murder, the message was truncated&apos;);
    }
    $nonce = mb_substr($decoded, 0, SODIUM_CRYPTO_SECRETBOX_NONCEBYTES, &apos;8bit&apos;);
    $ciphertext = mb_substr($decoded, SODIUM_CRYPTO_SECRETBOX_NONCEBYTES, null, &apos;8bit&apos;);

    $plain = sodium_crypto_secretbox_open(
        $ciphertext,
        $nonce,
        $key
    );
    if ($plain === false) {
         throw new Exception(&apos;the message was tampered with in transit&apos;);
    }
    sodium_memzero($ciphertext);
    sodium_memzero($key);
    return $plain;
}
//Encrypt &amp; Decrypt your message
$key = sodium_crypto_secretbox_keygen();
$enc = safeEncrypt(&apos;Abdul Rafay Hingoro&apos;, $key); //generates random  encrypted string (Base64 related)
echo $enc;
echo &apos;&lt;br&gt;&apos;;
$dec = safeDecrypt($enc, $key); //decrypts encoded string generated via safeEncrypt function 
echo $dec;
?>
```
<br><br>//Output<br>DEx9ATXEg/eRq8GWD3NT5BatB3m31WEDEYLK2V4L0Am5GZGoa2rvYWUpoUeCrm7W/pdgLJrNoE6AA8U=<br>Abdul Rafay Hingoro  

#

[Official documentation page](https://www.php.net/manual/en/intro.sodium.php)

**[To root](/README.md)**