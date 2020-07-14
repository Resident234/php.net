# random_bytes



I used below function to create random token, and also a salt from the token. I used it in my application to prevent CSRF attack.<br><br>

```
<?php
function RandomToken($length = 32){
    if(!isset($length) || intval($length) &lt;= 8 ){
      $length = 32;
    }
    if (function_exists(&apos;random_bytes&apos;)) {
        return bin2hex(random_bytes($length));
    }
    if (function_exists(&apos;mcrypt_create_iv&apos;)) {
        return bin2hex(mcrypt_create_iv($length, MCRYPT_DEV_URANDOM));
    } 
    if (function_exists(&apos;openssl_random_pseudo_bytes&apos;)) {
        return bin2hex(openssl_random_pseudo_bytes($length));
    }
}

function Salt(){
    return substr(strtr(base64_encode(hex2bin(RandomToken(32))), &apos;+&apos;, &apos;.&apos;), 0, 44);
}

echo (RandomToken());
echo "\n";
echo Salt();
echo "\n";

/*
This function is same as above but its only used for debugging
*/
function RandomTokenDebug($length = 32){
    if(!isset($length) || intval($length) &lt;= 8 ){
      $length = 32;
    }
    $randoms = array();
    if (function_exists(&apos;random_bytes&apos;)) {
        $randoms[&apos;random_bytes&apos;] = bin2hex(random_bytes($length));
    }
    if (function_exists(&apos;mcrypt_create_iv&apos;)) {
        $randoms[&apos;mcrypt_create_iv&apos;] = bin2hex(mcrypt_create_iv($length, MCRYPT_DEV_URANDOM));
    }
    if (function_exists(&apos;openssl_random_pseudo_bytes&apos;)) {
        $randoms[&apos;openssl_random_pseudo_bytes&apos;] = bin2hex(openssl_random_pseudo_bytes($length));
    }
    
    return $randoms;
}
echo "\n";
print_r (RandomTokenDebug());

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.random-bytes.php)

**[To root](/README.md)**