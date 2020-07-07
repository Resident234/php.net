# openssl_private_decrypt





Encrypt using public key, decrypt using private key.

Use this to store stuff in your database: Unless someone
has your private key, the database contents are useless.

Also, use this for sending to a specific individual:&#xA0; Get
their public key, encrypt the message, only they can use
their private key to decode it.



```
<?php
echo &quot;Source: $source&quot;;
$fp=fopen(&quot;/path/to/certificate.crt&quot;,&quot;r&quot;);
$pub_key=fread($fp,8192);
fclose($fp);
openssl_get_publickey($pub_key);
/*
 * NOTE:&#xA0; Here you use the $pub_key value (converted, I guess)
 */
openssl_public_encrypt($source,$crypttext,$pub_key);
echo &quot;String crypted: $crypttext&quot;;

$fp=fopen(&quot;/path/to/private.key&quot;,&quot;r&quot;);
$priv_key=fread($fp,8192);
fclose($fp);
// $passphrase is required if your key is encoded (suggested)
$res = openssl_get_privatekey($priv_key,$passphrase);
/*
 * NOTE:&#xA0; Here you use the returned resource value
 */
openssl_private_decrypt($crypttext,$newsource,$res);
echo &quot;String decrypt : $newsource&quot;;
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.openssl-private-decrypt.php)

**[To root](/README.md)**