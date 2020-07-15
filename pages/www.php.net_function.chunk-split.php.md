# chunk_split



An alternative for unicode strings;<br><br>

```
<?php
function chunk_split_unicode($str, $l = 76, $e = "\r\n") {
    $tmp = array_chunk(
        preg_split("//u", $str, -1, PREG_SPLIT_NO_EMPTY), $l);
    $str = "";
    foreach ($tmp as $t) {
        $str .= join("", $t) . $e;
    }
    return $str;
}

$str = "Yar&#x131;m kilo &#xE7;ay, yar&#x131;m kilo &#x15F;eker";
echo chunk_split($str, 4) ."\n";
echo chunk_split_unicode($str, 4);
?>
```
<br><br>Yar&#xFFFD;<br>&#xFFFD;m k<br>ilo <br>&#xE7;ay<br>, ya<br>r&#x131;m<br> kil<br>o &#x15F;<br>eker<br><br>Yar&#x131;<br>m ki<br>lo &#xE7;<br>ay, <br>yar&#x131;<br>m ki<br>lo &#x15F;<br>eker  

#

As an alternative for  qeremy [atta] gmail [dotta] com<br>There is much shorter way for binarysafe chunking of multibyte string:<br><br>

```
<?php
function word_chunk($str, $len = 76, $end = "\n") {
    $pattern = '~.{1,' . $len . '}~u'; // like "~.{1,76}~u"
    $str = preg_replace($pattern, '$0' . $end, $str);
    return rtrim($str, $end);
}

$str = '&#x440;&#x443;&#x441;&#x441;&#x43A;&#x438;&#x439;';
echo chunk_split($str, 3) ."\n";
echo word_chunk($str, 3) . "\n";
?>
```
<br><br>&#x440;&#xFFFD;<br>&#xFFFD;&#x441;<br>&#x441;&#xFFFD;<br>&#xFFFD;&#x438;<br>&#x439;<br><br>&#x440;&#x443;&#x441;<br>&#x441;&#x43A;&#x438;<br>&#x439;  

#

[Official documentation page](https://www.php.net/manual/en/function.chunk-split.php)

**[To root](/README.md)**