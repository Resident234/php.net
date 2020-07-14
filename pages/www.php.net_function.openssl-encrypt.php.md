# openssl_encrypt



There&apos;s a lot of confusion plus some false guidance here on the openssl library. <br><br>The basic tips are:<br><br>aes-256-ctr is arguably the best choice for cipher algorithm as of 2016. This avoids potential security issues (so-called padding oracle attacks) and bloat from algorithms that pad data to a certain block size. aes-256-gcm is preferable, but not usable until the openssl library is enhanced, which is due in PHP 7.1<br><br>Use different random data for the initialisation vector each time encryption is made with the same key. mcrypt_create_iv() is one choice for random data. AES uses 16 byte blocks, so you need 16 bytes for the iv.<br><br>Join the iv data to the encrypted result and extract the iv data again when decrypting.<br><br>Pass OPENSSL_RAW_DATA for the flags and encode the result if necessary after adding in the iv data.<br><br>Hash the chosen encryption key (the password parameter) using openssl_digest() with a hash function such as sha256, and use the hashed value for the password parameter.<br><br>There&apos;s a simple Cryptor class on GitHub called php-openssl-cryptor that demonstrates encryption/decryption and hashing with openssl, along with how to produce and consume the data in base64 and hex as well as binary. It should lay the foundations for better understanding and making effective use of openssl with PHP.<br><br>Hopefully it will help anyone looking to get started with this powerful library.  

#

Many users give up with handilng problem when openssl command line tool cant decrypt php openssl encrypted file which is encrypted with openssl_encrypt function.<br><br>For example how beginner is encrypting data:<br><br>

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

    function strtohex($x) 
    {
        $s=&apos;&apos;;
        foreach (str_split($x) as $c) $s.=sprintf("%02X",ord($c));
        return($s);
    } 
    
    $source = &apos;It works !&apos;;

    $iv = "1234567812345678";
    $pass = &apos;1234567812345678&apos;;
    $method = &apos;aes-128-cbc&apos;;

    echo "\niv in hex to use: ".strtohex ($iv);
    echo "\nkey in hex to use: ".strtohex ($pass);
    echo "\n";

    file_put_contents (&apos;./file.encrypted&apos;,openssl_encrypt ($source, $method, $pass, true, $iv));

    $exec = "openssl enc -".$method." -d -in file.encrypted -nosalt -nopad -K ".strtohex($pass)." -iv ".strtohex($iv);

    echo &apos;executing: &apos;.$exec."\n\n";
    echo exec ($exec);
    echo "\n";

?>
```
<br><br>IV and Key parameteres passed to openssl command line must be in hex representation of string.<br><br>The correct command for decrypting is:<br><br># openssl enc -aes-128-cbc -d -in file.encrypted -nosalt -nopad -K 31323334353637383132333435363738 -iv 31323334353637383132333435363738<br><br>As it has no salt has no padding and by setting functions third parameter we have no more base64 encoded file to decode. The command will echo that it works...<br><br>: /  

#

Since the $options are not documented, I&apos;m going to clarify what they mean here in the comments.  Behind the scenes, in the source code for /ext/openssl/openssl.c:<br><br>    EVP_EncryptInit_ex(&amp;cipher_ctx, NULL, NULL, key, (unsigned char *)iv);<br>    if (options &amp; OPENSSL_ZERO_PADDING) {<br>        EVP_CIPHER_CTX_set_padding(&amp;cipher_ctx, 0);<br>    }<br><br>And later:<br><br>        if (options &amp; OPENSSL_RAW_DATA) {<br>            outbuf[outlen] = &apos;\0&apos;;<br>            RETVAL_STRINGL((char *)outbuf, outlen, 0);<br>        } else {<br>            int base64_str_len;<br>            char *base64_str;<br><br>            base64_str = (char*)php_base64_encode(outbuf, outlen, &amp;base64_str_len);<br>            efree(outbuf);<br>            RETVAL_STRINGL(base64_str, base64_str_len, 0);<br>        }<br><br>So as we can see here, OPENSSL_ZERO_PADDING has a direct impact on the OpenSSL context.  EVP_CIPHER_CTX_set_padding() enables or disables padding (enabled by default).  So, OPENSSL_ZERO_PADDING disables padding for the context, which means that you will have to manually apply your own padding out to the block size.  Without using OPENSSL_ZERO_PADDING, you will automatically get PKCS#7 padding.<br><br>OPENSSL_RAW_DATA does not affect the OpenSSL context but has an impact on the format of the data returned to the caller.  When OPENSSL_RAW_DATA is specified, the returned data is returned as-is.  When it is not specified, Base64 encoded data is returned to the caller.<br><br>Hope this saves someone a trip to the PHP source code to figure out what the $options do.  Pro developer tip:  Download and have a copy of the PHP source code locally so that, when the PHP documentation fails to live up to quality expectations, you can see what is actually happening behind the scenes.  

#

PHP lacks a build-in function to encrypt and decrypt large files. `openssl_encrypt()` can be used to encrypt strings, but loading a huge file into memory is a bad idea.<br><br>So we have to write a userland function doing that. This example uses the symmetric AES-128-CBC algorithm to encrypt smaller chunks of a large file and writes them into another file.<br><br># Encrypt Files<br><br>

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
 * Encrypt the passed file and saves the result in a new file with ".enc" as suffix.
 * 
 * @param string $source Path to file that should be encrypted
 * @param string $key    The key used for the encryption
 * @param string $dest   File name where the encryped file should be written to.
 * @return string|false  Returns the file name that has been created or FALSE if an error occured
 */
function encryptFile($source, $key, $dest)
{
    $key = substr(sha1($key, true), 0, 16);
    $iv = openssl_random_pseudo_bytes(16);

    $error = false;
    if ($fpOut = fopen($dest, &apos;w&apos;)) {
        // Put the initialzation vector to the beginning of the file
        fwrite($fpOut, $iv);
        if ($fpIn = fopen($source, &apos;rb&apos;)) {
            while (!feof($fpIn)) {
                $plaintext = fread($fpIn, 16 * FILE_ENCRYPTION_BLOCKS);
                $ciphertext = openssl_encrypt($plaintext, &apos;AES-128-CBC&apos;, $key, OPENSSL_RAW_DATA, $iv);
                // Use the first 16 bytes of the ciphertext as the next initialization vector
                $iv = substr($ciphertext, 0, 16);
                fwrite($fpOut, $ciphertext);
            }
            fclose($fpIn);
        } else {
            $error = true;
        }
        fclose($fpOut);
    } else {
        $error = true;
    }

    return $error ? false : $dest;
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
 * @param string $key    The key used for the decryption (must be the same as for encryption)
 * @param string $dest   File name where the decryped file should be written to.
 * @return string|false  Returns the file name that has been created or FALSE if an error occured
 */
function decryptFile($source, $key, $dest)
{
    $key = substr(sha1($key, true), 0, 16);

    $error = false;
    if ($fpOut = fopen($dest, &apos;w&apos;)) {
        if ($fpIn = fopen($source, &apos;rb&apos;)) {
            // Get the initialzation vector from the beginning of the file
            $iv = fread($fpIn, 16);
            while (!feof($fpIn)) {
                // we have to read one block more for decrypting than for encrypting
                $ciphertext = fread($fpIn, 16 * (FILE_ENCRYPTION_BLOCKS + 1)); 
                $plaintext = openssl_decrypt($ciphertext, &apos;AES-128-CBC&apos;, $key, OPENSSL_RAW_DATA, $iv);
                // Use the first 16 bytes of the ciphertext as the next initialization vector
                $iv = substr($ciphertext, 0, 16);
                fwrite($fpOut, $plaintext);
            }
            fclose($fpIn);
        } else {
            $error = true;
        }
        fclose($fpOut);
    } else {
        $error = true;
    }

    return $error ? false : $dest;
}
?>
```
<br><br>Source: http://stackoverflow.com/documentation/php/5794/cryptography/25499/  

#

Just a couple of notes about the parameters:<br><br>data - It is interpreted as a binary string<br>method - Regular string, make sure you check openssl_get_cipher_methods() for a list of the ciphers available in your server*<br>password - As biohazard mentioned before, this is actually THE KEY! It should be in hex format.<br>options - As explained in the Parameters section<br>iv - Initialization Vector. Different than biohazard mentioned before, this should be a BINARY string. You should check for your particular implementation.<br><br>To verify the length/format of your IV, you can provide strings of different lengths and check the error log. For example, in PHP 5.5.9 (Ubuntu 14.04 LTS), providing a 32 byte hex string (which would represent a 16 byte binary IV) throws an error.<br>"IV passed is 32 bytes long which is longer than the 16 expected by the selected cipher" (cipher chosen was &apos;aes-256-cbc&apos; which uses an IV of 128 bits, its block size). <br>Alternatively, you can use openssl_cipher_iv_length().<br><br>From the security standpoint, make sure you understand whether your IV needs to be random, secret or encrypted. Many times the IV can be non-secret but it has to be a cryptographically secure random number. Make sure you generate it with an appropriate function like openssl_random_pseudo_bytes(), not mt_rand().<br><br>*Note that the available cipher methods can differ between your dev server and your production server! They will depend on the installation and compilation options used for OpenSSL in your machine(s).  

#

This Is The Most Secure Way To Encrypt And Decrypt Your Data, <br>It Is Almost Impossible To Crack Your Encryption.<br>--------------------------------------------------------<br> --- Create Two Random Keys And Save Them In Your Configuration File ---<br>

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
$second_key = base64_decode(SECONDKEY);    
    
$method = "aes-256-cbc";    
$iv_length = openssl_cipher_iv_length($method);
$iv = openssl_random_pseudo_bytes($iv_length);
        
$first_encrypted = openssl_encrypt($data,$method,$first_key, OPENSSL_RAW_DATA ,$iv);    
$second_encrypted = hash_hmac(&apos;sha3-512&apos;, $first_encrypted, $second_key, TRUE);
            
$output = base64_encode($iv.$second_encrypted.$first_encrypted);    
return $output;        
}
?>
```

--------------------------------------------------------


```
<?php
function secured_decrypt($input)
{
$first_key = base64_decode(FIRSTKEY);
$second_key = base64_decode(SECONDKEY);            
$mix = base64_decode($input);
        
$method = "aes-256-cbc";    
$iv_length = openssl_cipher_iv_length($method);
            
$iv = substr($mix,0,$iv_length);
$second_encrypted = substr($mix,$iv_length,64);
$first_encrypted = substr($mix,$iv_length+64);
            
$data = openssl_decrypt($first_encrypted,$method,$first_key,OPENSSL_RAW_DATA,$iv);
$second_encrypted_new = hash_hmac(&apos;sha3-512&apos;, $first_encrypted, $second_key, TRUE);
    
if (hash_equals($second_encrypted,$second_encrypted_new))
return $data;
    
return false;
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.openssl-encrypt.php)

**[To root](/README.md)**