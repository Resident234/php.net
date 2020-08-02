# openssl_private_decrypt



Encrypt using public key, decrypt using private key.<br><br>Use this to store stuff in your database: Unless someone<br>has your private key, the database contents are useless.<br><br>Also, use this for sending to a specific individual:  Get<br>their public key, encrypt the message, only they can use<br>their private key to decode it.<br><br>

```
<?php
echo "Source: $source";
$fp=fopen("/path/to/certificate.crt","r");
$pub_key=fread($fp,8192);
fclose($fp);
openssl_get_publickey($pub_key);
/*
 * NOTE:  Here you use the $pub_key value (converted, I guess)
 */
openssl_public_encrypt($source,$crypttext,$pub_key);
echo "String crypted: $crypttext";

$fp=fopen("/path/to/private.key","r");
$priv_key=fread($fp,8192);
fclose($fp);
// $passphrase is required if your key is encoded (suggested)
$res = openssl_get_privatekey($priv_key,$passphrase);
/*
 * NOTE:  Here you use the returned resource value
 */
openssl_private_decrypt($crypttext,$newsource,$res);
echo "String decrypt : $newsource";
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.openssl-private-decrypt.php)

**[To root](/README.md)**