# com_create_guid



The phunction PHP framework (http://sourceforge.net/projects/phunction/) uses the following function to generate valid version 4 UUIDs:<br><br>

```
<?php

function GUID()
{
    if (function_exists('com_create_guid') === true)
    {
        return trim(com_create_guid(), '{}');
    }

    return sprintf('%04X%04X-%04X-%04X-%04X-%04X%04X%04X', mt_rand(0, 65535), mt_rand(0, 65535), mt_rand(0, 65535), mt_rand(16384, 20479), mt_rand(32768, 49151), mt_rand(0, 65535), mt_rand(0, 65535), mt_rand(0, 65535));
}

?>
```
<br><br>The output generated by the sprintf() and mt_rand() calls is identical to com_create_guid() results.  

---

Here&apos;s my final version of a GUIDv4 function (based on others work here) that should work on all platforms and gracefully fallback to less cryptographically secure version if others are not supported...<br><br>

```
<?php
/**
 * Returns a GUIDv4 string
 *
 * Uses the best cryptographically secure method 
 * for all supported pltforms with fallback to an older, 
 * less secure version.
 *
 * @param bool $trim
 * @return string
 */
function GUIDv4 ($trim = true)
{
    // Windows
    if (function_exists('com_create_guid') === true) {
        if ($trim === true)
            return trim(com_create_guid(), '{}');
        else
            return com_create_guid();
    }

    // OSX/Linux
    if (function_exists('openssl_random_pseudo_bytes') === true) {
        $data = openssl_random_pseudo_bytes(16);
        $data[6] = chr(ord($data[6]) &amp; 0x0f | 0x40);    // set version to 0100
        $data[8] = chr(ord($data[8]) &amp; 0x3f | 0x80);    // set bits 6-7 to 10
        return vsprintf('%s%s-%s-%s-%s-%s%s%s', str_split(bin2hex($data), 4));
    }

    // Fallback (PHP 4.2+)
    mt_srand((double)microtime() * 10000);
    $charid = strtolower(md5(uniqid(rand(), true)));
    $hyphen = chr(45);                  // "-"
    $lbrace = $trim ? "" : chr(123);    // "{"
    $rbrace = $trim ? "" : chr(125);    // "}"
    $guidv4 = $lbrace.
              substr($charid,  0,  8).$hyphen.
              substr($charid,  8,  4).$hyphen.
              substr($charid, 12,  4).$hyphen.
              substr($charid, 16,  4).$hyphen.
              substr($charid, 20, 12).
              $rbrace;
    return $guidv4;
}
?>
```
  

---

Use more cryptographically strong algorithm to generate pseudo-random bytes and format it as GUID v4 string<br><br>function guidv4()<br>{<br>    if (function_exists(&apos;com_create_guid&apos;) === true)<br>        return trim(com_create_guid(), &apos;{}&apos;);<br><br>    $data = openssl_random_pseudo_bytes(16);<br>    $data[6] = chr(ord($data[6]) &amp; 0x0f | 0x40); // set version to 0100<br>    $data[8] = chr(ord($data[8]) &amp; 0x3f | 0x80); // set bits 6-7 to 10<br>    return vsprintf(&apos;%s%s-%s-%s-%s-%s%s%s&apos;, str_split(bin2hex($data), 4));<br>}  

---

[Official documentation page](https://www.php.net/manual/en/function.com-create-guid.php)

**[To root](/README.md)**