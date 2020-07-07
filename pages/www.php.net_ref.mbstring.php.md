# Multibyte String Functions





Please note that all the discussion about mb_str_replace in the comments is pretty pointless. str_replace works just fine with multibyte strings:



```
<?php

$string&#xA0; = &apos;&#x6F22;&#x5B57;&#x306F;&#x30E6;&#x30CB;&#x30B3;&#x30FC;&#x30C9;&apos;;
$needle&#xA0; = &apos;&#x306F;&apos;;
$replace = &apos;Foo&apos;;

echo str_replace($needle, $replace, $string);
// outputs: &#x6F22;&#x5B57;Foo&#x30E6;&#x30CB;&#x30B3;&#x30FC;&#x30C9;

?>
```


The usual problem is that the string is evaluated as binary string, meaning PHP is not aware of encodings at all. Problems arise if you are getting a value &quot;from outside&quot; somewhere (database, POST request) and the encoding of the needle and the haystack is not the same. That typically means the source code is not saved in the same encoding as you are receiving &quot;from outside&quot;. Therefore the binary representations don&apos;t match and nothing happens.

  

#



PHP can input and output Unicode, but a little different from what Microsoft means: when Microsoft says &quot;Unicode&quot;, it unexplicitly means little-endian UTF-16 with BOM(FF FE = chr(255).chr(254)), whereas PHP&apos;s &quot;UTF-16&quot; means big-endian with BOM. For this reason, PHP does not seem to be able to output Unicode CSV file for Microsoft Excel. Solving this problem is quite simple: just put BOM infront of UTF-16LE string.

Example:

$unicode_str_for_Excel = chr(255).chr(254).mb_convert_encoding( $utf8_str, &apos;UTF-16LE&apos;, &apos;UTF-8&apos;);

  

#



Note that some of the multi-byte functions run in O(n) time, rather than constant time as is the case for their single-byte equivalents. This includes any functionality requiring access at a specific index, since random access is not possible in a string whose number of bytes will not necessarily match the number of characters. Affected functions include: mb_substr(), mb_strstr(), mb_strcut(), mb_strpos(), etc.

  

#

[Official documentation page](https://www.php.net/manual/en/ref.mbstring.php)

**[To root](/README.md)**