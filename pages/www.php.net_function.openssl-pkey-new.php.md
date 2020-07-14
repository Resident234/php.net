# openssl_pkey_new



Working example:<br><br>$config = array(<br>    "digest_alg" =&gt; "sha512",<br>    "private_key_bits" =&gt; 4096,<br>    "private_key_type" =&gt; OPENSSL_KEYTYPE_RSA,<br>);<br>    <br>// Create the private and public key<br>$res = openssl_pkey_new($config);<br><br>// Extract the private key from $res to $privKey<br>openssl_pkey_export($res, $privKey);<br><br>// Extract the public key from $res to $pubKey<br>$pubKey = openssl_pkey_get_details($res);<br>$pubKey = $pubKey["key"];<br><br>$data = &apos;plaintext data goes here&apos;;<br><br>// Encrypt the data to $encrypted using the public key<br>openssl_public_encrypt($data, $encrypted, $pubKey);<br><br>// Decrypt the data using the private key and store the results in $decrypted<br>openssl_private_decrypt($encrypted, $decrypted, $privKey);<br><br>echo $decrypted;  

#

If you try and generate a new key using openssl_pkey_new(), and need to specify the size of the key, the key MUST be type-bound to integer<br><br>// works<br>$keysize = 1024;<br>$ssl = openssl_pkey_new (array(&apos;private_key_bits&apos; =&gt; $keysize));<br><br>// fails<br>$keysize = "1024";<br>$ssl = openssl_pkey_new (array(&apos;private_key_bits&apos; =&gt; $keysize));<br><br>// works (force to int)<br>$keysize = "1024";<br>$ssl = openssl_pkey_new (array(&apos;private_key_bits&apos; =&gt; (int)$keysize));  

#

[Official documentation page](https://www.php.net/manual/en/function.openssl-pkey-new.php)

**[To root](/README.md)**