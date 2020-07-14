# OpenSSL



I was having a heck of a time finding help on making asynchronous encryption/decryption using private key/public key systems working, and I had to have it for creating a credit card module that uses recurring billing.<br><br>You&apos;d be a fool to use normal, &apos;synchronous&apos; or two-way encryption for this, so the whole mcrypt library won&apos;t help.<br><br>But, it turns out OpenSSL is extremely easy to use...yet it is so sparsely documented that it seems it would be incredibly hard.<br><br>So I share my day of hacking with you - I hope you find it helpful!<br><br>

```
<?php

if (isset($_SERVER[&apos;HTTPS&apos;]) )
{
    echo "SECURE: This page is being accessed through a secure connection.&lt;br&gt;&lt;br&gt;";
}
else
{
    echo "UNSECURE: This page is being access through an unsecure connection.&lt;br&gt;&lt;br&gt;";
}

// Create the keypair
$res=openssl_pkey_new();

// Get private key
openssl_pkey_export($res, $privatekey);

// Get public key
$publickey=openssl_pkey_get_details($res);
$publickey=$publickey["key"];

echo "Private Key:&lt;BR&gt;$privatekey&lt;br&gt;&lt;br&gt;Public Key:&lt;BR&gt;$publickey&lt;BR&gt;&lt;BR&gt;";

$cleartext = &apos;1234 5678 9012 3456&apos;;

echo "Clear text:&lt;br&gt;$cleartext&lt;BR&gt;&lt;BR&gt;";

openssl_public_encrypt($cleartext, $crypttext, $publickey);

echo "Crypt text:&lt;br&gt;$crypttext&lt;BR&gt;&lt;BR&gt;";

openssl_private_decrypt($crypttext, $decrypted, $privatekey);

echo "Decrypted text:&lt;BR&gt;$decrypted&lt;br&gt;&lt;br&gt;";
?>
```
<br><br>Many thanks to other contributors in the docs for making this less painful.<br><br>Note that you will want to use these sorts of functions to generate a key ONCE - save your privatekey offline for decryption, and put your public key in your scripts/configuration file. If your data is compromised you don&apos;t care about the encrypted stuff or the public key, it&apos;s only the private key and cleartext that really matter.<br><br>Good luck!  

#

[Official documentation page](https://www.php.net/manual/en/book.openssl.php)

**[To root](/README.md)**