# openssl_pkey_new





Working example:

$config = array(
&#xA0; &#xA0; &quot;digest_alg&quot; =&gt; &quot;sha512&quot;,
&#xA0; &#xA0; &quot;private_key_bits&quot; =&gt; 4096,
&#xA0; &#xA0; &quot;private_key_type&quot; =&gt; OPENSSL_KEYTYPE_RSA,
);
&#xA0; &#xA0; 
// Create the private and public key
$res = openssl_pkey_new($config);

// Extract the private key from $res to $privKey
openssl_pkey_export($res, $privKey);

// Extract the public key from $res to $pubKey
$pubKey = openssl_pkey_get_details($res);
$pubKey = $pubKey[&quot;key&quot;];

$data = &apos;plaintext data goes here&apos;;

// Encrypt the data to $encrypted using the public key
openssl_public_encrypt($data, $encrypted, $pubKey);

// Decrypt the data using the private key and store the results in $decrypted
openssl_private_decrypt($encrypted, $decrypted, $privKey);

echo $decrypted;

  

#



If you try and generate a new key using openssl_pkey_new(), and need to specify the size of the key, the key MUST be type-bound to integer

// works
$keysize = 1024;
$ssl = openssl_pkey_new (array(&apos;private_key_bits&apos; =&gt; $keysize));

// fails
$keysize = &quot;1024&quot;;
$ssl = openssl_pkey_new (array(&apos;private_key_bits&apos; =&gt; $keysize));

// works (force to int)
$keysize = &quot;1024&quot;;
$ssl = openssl_pkey_new (array(&apos;private_key_bits&apos; =&gt; (int)$keysize));

  

#

[Official documentation page](https://www.php.net/manual/en/function.openssl-pkey-new.php)

**[To root](/README.md)**