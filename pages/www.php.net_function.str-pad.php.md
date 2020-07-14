# str_pad



since the default pad_type is STR_PAD_RIGHT. using STR_PAD_BOTH were always favor in the right pad if the required number of padding characters can&apos;t be evenly divided. <br><br>e.g<br><br>

```
<?php

echo str_pad("input", 10, "pp", STR_PAD_BOTH ); // ppinputppp
echo str_pad("input", 6, "p", STR_PAD_BOTH ); // inputp
echo str_pad("input", 8, "p", STR_PAD_BOTH ); //pinputpp

?>
```
  

#

A proper unicode string padder;<br><br>

```
<?php
mb_internal_encoding(&apos;utf-8&apos;); // @important

function str_pad_unicode($str, $pad_len, $pad_str = &apos; &apos;, $dir = STR_PAD_RIGHT) {
    $str_len = mb_strlen($str);
    $pad_str_len = mb_strlen($pad_str);
    if (!$str_len &amp;&amp; ($dir == STR_PAD_RIGHT || $dir == STR_PAD_LEFT)) {
        $str_len = 1; // @debug
    }
    if (!$pad_len || !$pad_str_len || $pad_len &lt;= $str_len) {
        return $str;
    }
    
    $result = null;
    $repeat = ceil($str_len - $pad_str_len + $pad_len);
    if ($dir == STR_PAD_RIGHT) {
        $result = $str . str_repeat($pad_str, $repeat);
        $result = mb_substr($result, 0, $pad_len);
    } else if ($dir == STR_PAD_LEFT) {
        $result = str_repeat($pad_str, $repeat) . $str;
        $result = mb_substr($result, -$pad_len);
    } else if ($dir == STR_PAD_BOTH) {
        $length = ($pad_len - $str_len) / 2;
        $repeat = ceil($length / $pad_str_len);
        $result = mb_substr(str_repeat($pad_str, $repeat), 0, floor($length)) 
                    . $str 
                       . mb_substr(str_repeat($pad_str, $repeat), 0, ceil($length));
    }
    
    return $result;
}
?>
```


Test;


```
<?php
// needs ie. "test.php" file encoded in "utf-8 without bom"
$s = &apos;...&apos;;
for ($i = 3; $i &lt;= 1000; $i++) {
    $s1 = str_pad($s, $i, &apos;AO&apos;, STR_PAD_BOTH); // can not inculde unicode char!!!
    $s2 = str_pad_unicode($s, $i, &apos;&#xC4;&#xD6;&apos;, STR_PAD_BOTH);
    $sl1 = strlen($s1);
    $sl2 = mb_strlen($s2);
    echo  "len $sl1: $s1 \n";
    echo  "len $sl2: $s2 \n";
    echo  "\n";
    if ($sl1 != $sl2) die("Fail!");
}
?>
```
<br><br>Output;<br>len 3: ... <br>len 3: ... <br><br>len 4: ...A <br>len 4: ...&#xC4; <br><br>len 5: A...A <br>len 5: &#xC4;...&#xC4; <br><br>len 6: A...AO <br>len 6: &#xC4;...&#xC4;&#xD6; <br>...  

#

multibyte version:<br><br>

```
<?php
function mb_str_pad($str, $pad_len, $pad_str = &apos; &apos;, $dir = STR_PAD_RIGHT, $encoding = NULL)
{
    $encoding = $encoding === NULL ? mb_internal_encoding() : $encoding;
    $padBefore = $dir === STR_PAD_BOTH || $dir === STR_PAD_LEFT;
    $padAfter = $dir === STR_PAD_BOTH || $dir === STR_PAD_RIGHT;
    $pad_len -= mb_strlen($str, $encoding);
    $targetLen = $padBefore &amp;&amp; $padAfter ? $pad_len / 2 : $pad_len;
    $strToRepeatLen = mb_strlen($pad_str, $encoding);
    $repeatTimes = ceil($targetLen / $strToRepeatLen);
    $repeatedString = str_repeat($pad_str, max(0, $repeatTimes)); // safe if used with valid utf-8 strings
    $before = $padBefore ? mb_substr($repeatedString, 0, floor($targetLen), $encoding) : &apos;&apos;;
    $after = $padAfter ? mb_substr($repeatedString, 0, ceil($targetLen), $encoding) : &apos;&apos;;
    return $before . $str . $after;
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.str-pad.php)

**[To root](/README.md)**