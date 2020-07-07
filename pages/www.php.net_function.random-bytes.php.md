# random_bytes





I used below function to create random token, and also a salt from the token. I used it in my application to prevent CSRF attack.



```
<?php
function RandomToken($length = 32){
&#xA0; &#xA0; if(!isset($length) || intval($length) &lt;= 8 ){
&#xA0; &#xA0; &#xA0; $length = 32;
&#xA0; &#xA0; }
&#xA0; &#xA0; if (function_exists(&apos;random_bytes&apos;)) {
&#xA0; &#xA0; &#xA0; &#xA0; return bin2hex(random_bytes($length));
&#xA0; &#xA0; }
&#xA0; &#xA0; if (function_exists(&apos;mcrypt_create_iv&apos;)) {
&#xA0; &#xA0; &#xA0; &#xA0; return bin2hex(mcrypt_create_iv($length, MCRYPT_DEV_URANDOM));
&#xA0; &#xA0; } 
&#xA0; &#xA0; if (function_exists(&apos;openssl_random_pseudo_bytes&apos;)) {
&#xA0; &#xA0; &#xA0; &#xA0; return bin2hex(openssl_random_pseudo_bytes($length));
&#xA0; &#xA0; }
}

function Salt(){
&#xA0; &#xA0; return substr(strtr(base64_encode(hex2bin(RandomToken(32))), &apos;+&apos;, &apos;.&apos;), 0, 44);
}

echo (RandomToken());
echo &quot;\n&quot;;
echo Salt();
echo &quot;\n&quot;;

/*
This function is same as above but its only used for debugging
*/
function RandomTokenDebug($length = 32){
&#xA0; &#xA0; if(!isset($length) || intval($length) &lt;= 8 ){
&#xA0; &#xA0; &#xA0; $length = 32;
&#xA0; &#xA0; }
&#xA0; &#xA0; $randoms = array();
&#xA0; &#xA0; if (function_exists(&apos;random_bytes&apos;)) {
&#xA0; &#xA0; &#xA0; &#xA0; $randoms[&apos;random_bytes&apos;] = bin2hex(random_bytes($length));
&#xA0; &#xA0; }
&#xA0; &#xA0; if (function_exists(&apos;mcrypt_create_iv&apos;)) {
&#xA0; &#xA0; &#xA0; &#xA0; $randoms[&apos;mcrypt_create_iv&apos;] = bin2hex(mcrypt_create_iv($length, MCRYPT_DEV_URANDOM));
&#xA0; &#xA0; }
&#xA0; &#xA0; if (function_exists(&apos;openssl_random_pseudo_bytes&apos;)) {
&#xA0; &#xA0; &#xA0; &#xA0; $randoms[&apos;openssl_random_pseudo_bytes&apos;] = bin2hex(openssl_random_pseudo_bytes($length));
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; return $randoms;
}
echo &quot;\n&quot;;
print_r (RandomTokenDebug());

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.random-bytes.php)

**[To root](/README.md)**