# chr



Note that if the number is higher than 256, it will return the number mod 256.<br>For example :<br>chr(321)=A because A=65(256)  

#

Another quick and short function to get unicode char by its code.<br><br>

```
<?php
/**
 * Return unicode char by its code
 *
 * @param int $u
 * @return char
 */
function unichr($u) {
    return mb_convert_encoding(&apos;&amp;#&apos; . intval($u) . &apos;;&apos;, &apos;UTF-8&apos;, &apos;HTML-ENTITIES&apos;);
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.chr.php)

**[To root](/README.md)**