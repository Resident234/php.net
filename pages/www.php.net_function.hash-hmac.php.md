# hash_hmac



Very important notice, if you pass array to $data, php will generate a Warning, return a NULL and continue your application. Which I think is critical vulnerability as this function used to check authorisation typically.<br><br>Example:<br>

```
<?php
var_dump(hash_hmac(&apos;sha256&apos;, [], &apos;secret&apos;));

WARNING hash_hmac() expects parameter 2 to be string, array given on line number 3
NULL
?>
```
<br>Of course not documented feature.  

#

Please be careful when comparing hashes. In certain cases, information can be leaked by using a timing attack. It takes advantage of the == operator only comparing until it finds a difference in the two strings. To prevent it, you have two options.<br><br>Option 1: hash both hashed strings first - this doesn&apos;t stop the timing difference, but it makes the information useless.<br><br>

```
<?php
    if (md5($hashed_value) === md5($hashed_expected)) {
        echo "hashes match!";
    }
?>
```


Option 2: always compare the whole string.



```
<?php
    if (hash_compare($hashed_value, $hashed_expected)) {
        echo "hashes match!";
    }

    function hash_compare($a, $b) {
        if (!is_string($a) || !is_string($b)) {
            return false;
        }
        
        $len = strlen($a);
        if ($len !== strlen($b)) {
            return false;
        }

        $status = 0;
        for ($i = 0; $i &lt; $len; $i++) {
            $status |= ord($a[$i]) ^ ord($b[$i]);
        }
        return $status === 0;
    }
?>
```
  

#

As  Michael  uggests we should take care not to compare the hash using == (or ===). Since PHP version 5.6 we can now use hash_equals().<br><br>So the example will be:<br><br>

```
<?php
    if (hash_equals($hashed_expected, $hashed_value) ) {
        echo "hashes match!";
    }
?>
```
  

#

For signing an Amazon AWS query, base64-encode the binary value:<br><br>

```
<?php
  $Sig = base64_encode(hash_hmac(&apos;sha256&apos;, $Request, $AmazonSecretKey, true));
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.hash-hmac.php)

**[To root](/README.md)**