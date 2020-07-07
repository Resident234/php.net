# mcrypt_encrypt





If you&apos;re writing code to encrypt/encrypt data in 2015, you should use openssl_encrypt() and openssl_decrypt(). The underlying library (libmcrypt) has been abandoned since 2007, and performs far worse than OpenSSL (which leverages AES-NI on modern processors and is cache-timing safe).

Also, MCRYPT_RIJNDAEL_256 is not AES-256, it&apos;s a different variant of the Rijndael block cipher. If you want AES-256 in mcrypt, you have to use MCRYPT_RIJNDAEL_128 with a 32-byte key. OpenSSL makes it more obvious which mode you are using (i.e. &apos;aes-128-cbc&apos; vs &apos;aes-256-ctr&apos;).

OpenSSL also uses PKCS7 padding with CBC mode rather than mcrypt&apos;s NULL byte padding. Thus, mcrypt is more likely to make your code vulnerable to padding oracle attacks than OpenSSL.

Finally, if you are not authenticating your ciphertexts (Encrypt Then MAC), you&apos;re doing it wrong.

Further reading:

https://paragonie.com/blog/2015/05/using-encryption-and-authentication-correctly

https://paragonie.com/blog/2015/05/if-you-re-typing-word-mcrypt-into-your-code-you-re-doing-it-wrong

  

#



Solving 3DES incompatibilities with .NET&apos;s TripleDESCryptoServiceProvider

mcrypt&apos;s 3DES only accepts 192 bit keys, but Microsoft&apos;s .NET and many other tools accept both 128 and 192 bit keys.
If your key is too short, mcrypt will &apos;helpfully&apos; pad null characters onto the end, but .NET refuses to use a key where the last third is all null (this is a Bad Key). This prevents you from emulating mcrypt&apos;s &quot;short key&quot; behaviour in .NET.

How to reconcile this? A little DES theory is in order
3DES runs the DES algorithm three times, using each third of your 192 bit key as the 64 bit DES key

Encrypt Key1 -&gt; Decrypt Key2 -&gt; Encrypt Key3

and both .NET and PHP&apos;s mcrypt do this the same way.
The problem arises in short key mode on .NET, since 128 bits is only two 64 bit DES keys
The algorithm that they use then is:

Encrypt Key1 -&gt; Decrypt Key2 -&gt; Encrypt Key1

mcrypt does not have this mode of operation natively.
but before you go and start running DES three times yourself, here&apos;s a Quick Fix


```
<?php
$my_key = &quot;12345678abcdefgh&quot;; // a 128 bit (16 byte) key
$my_key .= substr($my_key,0,8); // append the first 8 bytes onto the end
$secret = mcrypt_encrypt(MCRYPT_3DES, $my_key, $data, MCRYPT_MODE_CBC, $iv);&#xA0; //CBC is the default mode in .NET
?>
```


And, like magic, it works.

There&apos;s one more caveat: Data padding
mcrypt always pads data will the null character
but .NET has two padding modes: &quot;Zeros&quot; and &quot;PKCS7&quot;
Zeros is identical to the mcrypt scheme, but PKCS7 is the default.
PKCS7 isn&apos;t much more complex, though:
instead of nulls, it appends the total number of padding bytes (which means, for 3DES, it can be a value from 0x01 to 0x07)
if your plaintext is &quot;ABC&quot;, it will be padded into:
0x41 0x42 0x43 0x05 0x05 0x05 0x05 0x05

You can remove these from a decrypted string in PHP by counting the number of times that last character appears, and if it matches it&apos;s ordinal value, truncating the string by that many characters:


```
<?php
&#xA0; &#xA0; $block = mcrypt_get_block_size(&apos;tripledes&apos;, &apos;cbc&apos;);
&#xA0; &#xA0; $packing = ord($text{strlen($text) - 1});
&#xA0; &#xA0; if($packing and ($packing &lt; $block)){
&#xA0; &#xA0; &#xA0; for($P = strlen($text) - 1; $P &gt;= strlen($text) - $packing; $P--){
&#xA0; &#xA0; if(ord($text{$P}) != $packing){
&#xA0; &#xA0; &#xA0; $packing = 0;
&#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; $text = substr($text,0,strlen($text) - $packing);
?>
```


And to pad a string that you intend to decrypt with .NET, just add the chr() value of the number of padding bytes:


```
<?php
&#xA0; &#xA0; $block = mcrypt_get_block_size(&apos;tripledes&apos;, &apos;cbc&apos;);
&#xA0; &#xA0; $len = strlen($dat);
&#xA0; &#xA0; $padding = $block - ($len % $block);
&#xA0; &#xA0; $dat .= str_repeat(chr($padding),$padding);
?>
```


That&apos;s all there is to it.
Knowing this, you can encrypt, decrypt, and duplicate exactly any .NET 3DES behaviour in PHP.

  

#

[Official documentation page](https://www.php.net/manual/en/function.mcrypt-encrypt.php)

**[To root](/README.md)**