# hash_hmac





Very important notice, if you pass array to $data, php will generate a Warning, return a NULL and continue your application. Which I think is critical vulnerability as this function used to check authorisation typically.

Example:


```
<?php
var_dump(hash_hmac(&apos;sha256&apos;, [], &apos;secret&apos;));

WARNING hash_hmac() expects parameter 2 to be string, array given on line number 3
NULL
?>
```

Of course not documented feature.

  

#



Please be careful when comparing hashes. In certain cases, information can be leaked by using a timing attack. It takes advantage of the == operator only comparing until it finds a difference in the two strings. To prevent it, you have two options.



Option 1: hash both hashed strings first - this doesn&apos;t stop the timing difference, but it makes the information useless.





```
<?php

&#xA0; &#xA0; if (md5($hashed_value) === md5($hashed_expected)) {

&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;hashes match!&quot;;

&#xA0; &#xA0; }

?>
```




Option 2: always compare the whole string.





```
<?php

&#xA0; &#xA0; if (hash_compare($hashed_value, $hashed_expected)) {

&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;hashes match!&quot;;

&#xA0; &#xA0; }



&#xA0; &#xA0; function hash_compare($a, $b) {

&#xA0; &#xA0; &#xA0; &#xA0; if (!is_string($a) || !is_string($b)) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return false;

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; $len = strlen($a);

&#xA0; &#xA0; &#xA0; &#xA0; if ($len !== strlen($b)) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return false;

&#xA0; &#xA0; &#xA0; &#xA0; }



&#xA0; &#xA0; &#xA0; &#xA0; $status = 0;

&#xA0; &#xA0; &#xA0; &#xA0; for ($i = 0; $i &lt; $len; $i++) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $status |= ord($a[$i]) ^ ord($b[$i]);

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; return $status === 0;

&#xA0; &#xA0; }

?>
```



  

#



As&#xA0; Michael&#xA0; uggests we should take care not to compare the hash using == (or ===). Since PHP version 5.6 we can now use hash_equals().

So the example will be:



```
<?php
&#xA0; &#xA0; if (hash_equals($hashed_expected, $hashed_value) ) {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;hashes match!&quot;;
&#xA0; &#xA0; }
?>
```



  

#



For signing an Amazon AWS query, base64-encode the binary value:



```
<?php
&#xA0; $Sig = base64_encode(hash_hmac(&apos;sha256&apos;, $Request, $AmazonSecretKey, true));
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.hash-hmac.php)

**[To root](/README.md)**