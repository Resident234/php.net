# ord



As ord() doesn&apos;t work with utf-8, and if you do not have access to mb_* functions, the following function will work well:<br>

```
<?php
function ordutf8($string, &amp;$offset) {
    $code = ord(substr($string, $offset,1)); 
    if ($code &gt;= 128) {        //otherwise 0xxxxxxx
        if ($code &lt; 224) $bytesnumber = 2;                //110xxxxx
        else if ($code &lt; 240) $bytesnumber = 3;        //1110xxxx
        else if ($code &lt; 248) $bytesnumber = 4;    //11110xxx
        $codetemp = $code - 192 - ($bytesnumber &gt; 2 ? 32 : 0) - ($bytesnumber &gt; 3 ? 16 : 0);
        for ($i = 2; $i &lt;= $bytesnumber; $i++) {
            $offset ++;
            $code2 = ord(substr($string, $offset, 1)) - 128;        //10xxxxxx
            $codetemp = $codetemp*64 + $code2;
        }
        $code = $codetemp;
    }
    $offset += 1;
    if ($offset &gt;= strlen($string)) $offset = -1;
    return $code;
}
?>
```

$offset is a reference, as it is not easy to split a utf-8 char-by-char. Useful to iterate on a string:


```
<?php
$text = "abc&#xE0;&#xEA;&#xDF;&#x20AC;abc";
$offset = 0;
while ($offset &gt;= 0) {
    echo $offset.": ".ordutf8($text, $offset)."\n";
}
/* returns:
0: 97
1: 98
2: 99
3: 224
5: 234
7: 223
9: 8364
12: 97
13: 98
14: 99
*/
?>
```
<br>Feel free to adapt my code to fit your needs.  

#

Regarding character sets, and whether or not this is "ASCII". Firstly, there is no such thing as "8-bit ASCII", so if it were ASCII it would only ever return integers up to 127. 8-bit ASCII-compatible encodings include the ISO 8859 family of encodings, which map various common characters to the values from 128 to 255. UTF-8 is also designed so that characters representable in 7-bit ASCII are coded the same; byte values higher than 127 in a UTF-8 string represent the beginning of a multi-byte character.<br><br>In fact, like most of PHP&apos;s string functions, this function isn&apos;t doing anything to do with character encoding at all - it is just interpreting a binary byte from a string as an unsigned integer. That is, ord(chr(200)) will always return 200, but what character chr(200) *means* will vary depending on what character encoding it is *interpreted* as part of (e.g. during display).<br><br>A technically correct description would be "Returns an integer representation of the first byte of a string, from 0 to 255. For single-byte encodings such as (7-bit) ASCII and the ISO 8859 family, this will correspond to the first character, and will be the position of that character in the encoding&apos;s mapping table. For multi-byte encodings, such as UTF-8 or UTF-16, the byte may not represent a complete character."<br><br>The link to asciitable.com should also be replaced by one which explains what character encoding it is displaying, as "Extended ASCII" is an ambiguous and misleading name.  

#

I did not found a unicode/multibyte capable &apos;ord&apos; function, so...<br><br>

```
<?php
function uniord($u) {
    $k = mb_convert_encoding($u, 'UCS-2LE', 'UTF-8');
    $k1 = ord(substr($k, 0, 1));
    $k2 = ord(substr($k, 1, 1));
    return $k2 * 256 + $k1;
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.ord.php)

**[To root](/README.md)**