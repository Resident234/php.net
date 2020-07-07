# openssl_encrypt





There&apos;s a lot of confusion plus some false guidance here on the openssl library. 

The basic tips are:

aes-256-ctr is arguably the best choice for cipher algorithm as of 2016. This avoids potential security issues (so-called padding oracle attacks) and bloat from algorithms that pad data to a certain block size. aes-256-gcm is preferable, but not usable until the openssl library is enhanced, which is due in PHP 7.1

Use different random data for the initialisation vector each time encryption is made with the same key. mcrypt_create_iv() is one choice for random data. AES uses 16 byte blocks, so you need 16 bytes for the iv.

Join the iv data to the encrypted result and extract the iv data again when decrypting.

Pass OPENSSL_RAW_DATA for the flags and encode the result if necessary after adding in the iv data.

Hash the chosen encryption key (the password parameter) using openssl_digest() with a hash function such as sha256, and use the hashed value for the password parameter.

There&apos;s a simple Cryptor class on GitHub called php-openssl-cryptor that demonstrates encryption/decryption and hashing with openssl, along with how to produce and consume the data in base64 and hex as well as binary. It should lay the foundations for better understanding and making effective use of openssl with PHP.

Hopefully it will help anyone looking to get started with this powerful library.

  

#



Many users give up with handilng problem when openssl command line tool cant decrypt php openssl encrypted file which is encrypted with openssl_encrypt function.

For example how beginner is encrypting data:



```
<?php

$string = &apos;It works ? Or not it works ?&apos;;
$pass = &apos;1234&apos;;
$method = &apos;aes128&apos;;

file_put_contents (&apos;./file.encrypted&apos;, openssl_encrypt ($string, $method, $pass));

?>
```


And then how beginner is trying to decrypt data from command line:

# openssl enc -aes-128-cbc -d -in file.encrypted -pass pass:123

Or even if he/she determinates that openssl_encrypt output was base64 and tries:

# openssl enc -aes-128-cbc -d -in file.encrypted -base64 -pass pass:123

Or even if he determinates that base64 encoded file is represented in one line and tries:

# openssl enc -aes-128-cbc -d -in file.encrypted -base64 -A -pass pass:123

Or even if he determinates that IV is needed and adds some string iv as encryption function`s fourth parameter and than adds hex representation of iv as parameter in openssl command line :

# openssl enc -aes-128-cbc -d -in file.encrypted -base64 -pass pass:123 -iv -iv 31323334353637383132333435363738

Or even if he determinates that aes-128 password must be 128 bits there fore 16 bytes and sets $pass = &apos;1234567812345678&apos; and tries:

# openssl enc -aes-128-cbc -d -in file.encrypted -base64 -pass pass:1234567812345678 -iv -iv 31323334353637383132333435363738

All these troubles will have no result in any case.

BECAUSE THE PASSWORD PARAMETER DOCUMENTED HERE IS NOT THE PASSWORD.

It means that the password parameter of the function is not the same string used as [-pass pass:] parameter with openssl cmd tool for file encryption decryption.

IT IS THE KEY !

And now how to correctly encrypt data with php openssl_encrypt and how to correctly decrypt it from openssl command line tool.



```
<?php

&#xA0; &#xA0; function strtohex($x) 
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $s=&apos;&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; foreach (str_split($x) as $c) $s.=sprintf(&quot;%02X&quot;,ord($c));
&#xA0; &#xA0; &#xA0; &#xA0; return($s);
&#xA0; &#xA0; } 
&#xA0; &#xA0; 
&#xA0; &#xA0; $source = &apos;It works !&apos;;

&#xA0; &#xA0; $iv = &quot;1234567812345678&quot;;
&#xA0; &#xA0; $pass = &apos;1234567812345678&apos;;
&#xA0; &#xA0; $method = &apos;aes-128-cbc&apos;;

&#xA0; &#xA0; echo &quot;\niv in hex to use: &quot;.strtohex ($iv);
&#xA0; &#xA0; echo &quot;\nkey in hex to use: &quot;.strtohex ($pass);
&#xA0; &#xA0; echo &quot;\n&quot;;

&#xA0; &#xA0; file_put_contents (&apos;./file.encrypted&apos;,openssl_encrypt ($source, $method, $pass, true, $iv));

&#xA0; &#xA0; $exec = &quot;openssl enc -&quot;.$method.&quot; -d -in file.encrypted -nosalt -nopad -K &quot;.strtohex($pass).&quot; -iv &quot;.strtohex($iv);

&#xA0; &#xA0; echo &apos;executing: &apos;.$exec.&quot;\n\n&quot;;
&#xA0; &#xA0; echo exec ($exec);
&#xA0; &#xA0; echo &quot;\n&quot;;

?>
```


IV and Key parameteres passed to openssl command line must be in hex representation of string.

The correct command for decrypting is:

# openssl enc -aes-128-cbc -d -in file.encrypted -nosalt -nopad -K 31323334353637383132333435363738 -iv 31323334353637383132333435363738

As it has no salt has no padding and by setting functions third parameter we have no more base64 encoded file to decode. The command will echo that it works...

: /

  

#



Since the $options are not documented, I&apos;m going to clarify what they mean here in the comments.&#xA0; Behind the scenes, in the source code for /ext/openssl/openssl.c:

&#xA0; &#xA0; EVP_EncryptInit_ex(&amp;cipher_ctx, NULL, NULL, key, (unsigned char *)iv);
&#xA0; &#xA0; if (options &amp; OPENSSL_ZERO_PADDING) {
&#xA0; &#xA0; &#xA0; &#xA0; EVP_CIPHER_CTX_set_padding(&amp;cipher_ctx, 0);
&#xA0; &#xA0; }

And later:

&#xA0; &#xA0; &#xA0; &#xA0; if (options &amp; OPENSSL_RAW_DATA) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; outbuf[outlen] = &apos;\0&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; RETVAL_STRINGL((char *)outbuf, outlen, 0);
&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; int base64_str_len;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; char *base64_str;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; base64_str = (char*)php_base64_encode(outbuf, outlen, &amp;base64_str_len);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; efree(outbuf);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; RETVAL_STRINGL(base64_str, base64_str_len, 0);
&#xA0; &#xA0; &#xA0; &#xA0; }

So as we can see here, OPENSSL_ZERO_PADDING has a direct impact on the OpenSSL context.&#xA0; EVP_CIPHER_CTX_set_padding() enables or disables padding (enabled by default).&#xA0; So, OPENSSL_ZERO_PADDING disables padding for the context, which means that you will have to manually apply your own padding out to the block size.&#xA0; Without using OPENSSL_ZERO_PADDING, you will automatically get PKCS#7 padding.

OPENSSL_RAW_DATA does not affect the OpenSSL context but has an impact on the format of the data returned to the caller.&#xA0; When OPENSSL_RAW_DATA is specified, the returned data is returned as-is.&#xA0; When it is not specified, Base64 encoded data is returned to the caller.

Hope this saves someone a trip to the PHP source code to figure out what the $options do.&#xA0; Pro developer tip:&#xA0; Download and have a copy of the PHP source code locally so that, when the PHP documentation fails to live up to quality expectations, you can see what is actually happening behind the scenes.

  

#



PHP lacks a build-in function to encrypt and decrypt large files. `openssl_encrypt()` can be used to encrypt strings, but loading a huge file into memory is a bad idea.

So we have to write a userland function doing that. This example uses the symmetric AES-128-CBC algorithm to encrypt smaller chunks of a large file and writes them into another file.

# Encrypt Files



```
<?php
/**
 * Define the number of blocks that should be read from the source file for each chunk.
 * For &apos;AES-128-CBC&apos; each block consist of 16 bytes.
 * So if we read 10,000 blocks we load 160kb into memory. You may adjust this value
 * to read/write shorter or longer chunks.
 */
define(&apos;FILE_ENCRYPTION_BLOCKS&apos;, 10000);

/**
 * Encrypt the passed file and saves the result in a new file with &quot;.enc&quot; as suffix.
 * 
 * @param string $source Path to file that should be encrypted
 * @param string $key&#xA0; &#xA0; The key used for the encryption
 * @param string $dest&#xA0;&#xA0; File name where the encryped file should be written to.
 * @return string|false&#xA0; Returns the file name that has been created or FALSE if an error occured
 */
function encryptFile($source, $key, $dest)
{
&#xA0; &#xA0; $key = substr(sha1($key, true), 0, 16);
&#xA0; &#xA0; $iv = openssl_random_pseudo_bytes(16);

&#xA0; &#xA0; $error = false;
&#xA0; &#xA0; if ($fpOut = fopen($dest, &apos;w&apos;)) {
&#xA0; &#xA0; &#xA0; &#xA0; // Put the initialzation vector to the beginning of the file
&#xA0; &#xA0; &#xA0; &#xA0; fwrite($fpOut, $iv);
&#xA0; &#xA0; &#xA0; &#xA0; if ($fpIn = fopen($source, &apos;rb&apos;)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; while (!feof($fpIn)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $plaintext = fread($fpIn, 16 * FILE_ENCRYPTION_BLOCKS);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $ciphertext = openssl_encrypt($plaintext, &apos;AES-128-CBC&apos;, $key, OPENSSL_RAW_DATA, $iv);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Use the first 16 bytes of the ciphertext as the next initialization vector
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $iv = substr($ciphertext, 0, 16);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fwrite($fpOut, $ciphertext);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fclose($fpIn);
&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $error = true;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; fclose($fpOut);
&#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; $error = true;
&#xA0; &#xA0; }

&#xA0; &#xA0; return $error ? false : $dest;
}
?>
```


# Decrypt Files

To decrypt files that have been encrypted with the above function you can use this function.



```
<?php
/**
 * Dencrypt the passed file and saves the result in a new file, removing the
 * last 4 characters from file name.
 * 
 * @param string $source Path to file that should be decrypted
 * @param string $key&#xA0; &#xA0; The key used for the decryption (must be the same as for encryption)
 * @param string $dest&#xA0;&#xA0; File name where the decryped file should be written to.
 * @return string|false&#xA0; Returns the file name that has been created or FALSE if an error occured
 */
function decryptFile($source, $key, $dest)
{
&#xA0; &#xA0; $key = substr(sha1($key, true), 0, 16);

&#xA0; &#xA0; $error = false;
&#xA0; &#xA0; if ($fpOut = fopen($dest, &apos;w&apos;)) {
&#xA0; &#xA0; &#xA0; &#xA0; if ($fpIn = fopen($source, &apos;rb&apos;)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Get the initialzation vector from the beginning of the file
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $iv = fread($fpIn, 16);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; while (!feof($fpIn)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // we have to read one block more for decrypting than for encrypting
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $ciphertext = fread($fpIn, 16 * (FILE_ENCRYPTION_BLOCKS + 1)); 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $plaintext = openssl_decrypt($ciphertext, &apos;AES-128-CBC&apos;, $key, OPENSSL_RAW_DATA, $iv);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Use the first 16 bytes of the ciphertext as the next initialization vector
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $iv = substr($ciphertext, 0, 16);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fwrite($fpOut, $plaintext);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fclose($fpIn);
&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $error = true;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; fclose($fpOut);
&#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; $error = true;
&#xA0; &#xA0; }

&#xA0; &#xA0; return $error ? false : $dest;
}
?>
```


Source: http://stackoverflow.com/documentation/php/5794/cryptography/25499/

  

#



Just a couple of notes about the parameters:

data - It is interpreted as a binary string
method - Regular string, make sure you check openssl_get_cipher_methods() for a list of the ciphers available in your server*
password - As biohazard mentioned before, this is actually THE KEY! It should be in hex format.
options - As explained in the Parameters section
iv - Initialization Vector. Different than biohazard mentioned before, this should be a BINARY string. You should check for your particular implementation.

To verify the length/format of your IV, you can provide strings of different lengths and check the error log. For example, in PHP 5.5.9 (Ubuntu 14.04 LTS), providing a 32 byte hex string (which would represent a 16 byte binary IV) throws an error.
&quot;IV passed is 32 bytes long which is longer than the 16 expected by the selected cipher&quot; (cipher chosen was &apos;aes-256-cbc&apos; which uses an IV of 128 bits, its block size). 
Alternatively, you can use openssl_cipher_iv_length().

From the security standpoint, make sure you understand whether your IV needs to be random, secret or encrypted. Many times the IV can be non-secret but it has to be a cryptographically secure random number. Make sure you generate it with an appropriate function like openssl_random_pseudo_bytes(), not mt_rand().

*Note that the available cipher methods can differ between your dev server and your production server! They will depend on the installation and compilation options used for OpenSSL in your machine(s).

  

#



This Is The Most Secure Way To Encrypt And Decrypt Your Data, 
It Is Almost Impossible To Crack Your Encryption.
--------------------------------------------------------
 --- Create Two Random Keys And Save Them In Your Configuration File ---


```
<?php
// Create The First Key
echo base64_encode(openssl_random_pseudo_bytes(32));

// Create The Second Key
echo base64_encode(openssl_random_pseudo_bytes(64));
?>
```

--------------------------------------------------------


```
<?php
// Save The Keys In Your Configuration File
define(&apos;FIRSTKEY&apos;,&apos;Lk5Uz3slx3BrAghS1aaW5AYgWZRV0tIX5eI0yPchFz4=&apos;);
define(&apos;SECONDKEY&apos;,&apos;EZ44mFi3TlAey1b2w4Y7lVDuqO+SRxGXsa7nctnr/JmMrA2vN6EJhrvdVZbxaQs5jpSe34X3ejFK/o9+Y5c83w==&apos;);
?>
```

--------------------------------------------------------


```
<?php
function secured_encrypt($data)
{
$first_key = base64_decode(FIRSTKEY);
$second_key = base64_decode(SECONDKEY);&#xA0; &#xA0; 
&#xA0; &#xA0; 
$method = &quot;aes-256-cbc&quot;;&#xA0; &#xA0; 
$iv_length = openssl_cipher_iv_length($method);
$iv = openssl_random_pseudo_bytes($iv_length);
&#xA0; &#xA0; &#xA0; &#xA0; 
$first_encrypted = openssl_encrypt($data,$method,$first_key, OPENSSL_RAW_DATA ,$iv);&#xA0; &#xA0; 
$second_encrypted = hash_hmac(&apos;sha3-512&apos;, $first_encrypted, $second_key, TRUE);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
$output = base64_encode($iv.$second_encrypted.$first_encrypted);&#xA0; &#xA0; 
return $output;&#xA0; &#xA0; &#xA0; &#xA0; 
}
?>
```

--------------------------------------------------------


```
<?php
function secured_decrypt($input)
{
$first_key = base64_decode(FIRSTKEY);
$second_key = base64_decode(SECONDKEY);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
$mix = base64_decode($input);
&#xA0; &#xA0; &#xA0; &#xA0; 
$method = &quot;aes-256-cbc&quot;;&#xA0; &#xA0; 
$iv_length = openssl_cipher_iv_length($method);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
$iv = substr($mix,0,$iv_length);
$second_encrypted = substr($mix,$iv_length,64);
$first_encrypted = substr($mix,$iv_length+64);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
$data = openssl_decrypt($first_encrypted,$method,$first_key,OPENSSL_RAW_DATA,$iv);
$second_encrypted_new = hash_hmac(&apos;sha3-512&apos;, $first_encrypted, $second_key, TRUE);
&#xA0; &#xA0; 
if (hash_equals($second_encrypted,$second_encrypted_new))
return $data;
&#xA0; &#xA0; 
return false;
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.openssl-encrypt.php)

**[To root](/README.md)**