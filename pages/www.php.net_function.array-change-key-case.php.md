# array_change_key_case



Here is the most compact way to lower case keys in a multidimensional array<br><br>function array_change_key_case_recursive($arr)<br>{<br>    return array_map(function($item){<br>        if(is_array($item))<br>            $item = array_change_key_case_recursive($item);<br>        return $item;<br>    },array_change_key_case($arr));<br>}  

#

Unicode example;<br><br>

```
<?php
function array_change_key_case_unicode($arr, $c = CASE_LOWER) {
    $c = ($c == CASE_LOWER) ? MB_CASE_LOWER : MB_CASE_UPPER;
    foreach ($arr as $k => $v) {
        $ret[mb_convert_case($k, $c, "UTF-8")] = $v;
    }
    return $ret;
}

$arr = array("FirSt" => 1, "ya&#x11F;" => "Oil", "&#x15F;ekER" => "sugar");
print_r(array_change_key_case($arr, CASE_UPPER));
print_r(array_change_key_case_unicode($arr, CASE_UPPER));
?>
```
<br><br>Array<br>(<br>    [FIRST] =&gt; 1<br>    [YA&#x11F;] =&gt; Oil<br>    [&#x15F;EKER] =&gt; sugar<br>)<br>Array<br>(<br>    [FIRST] =&gt; 1<br>    [YA&#x11E;] =&gt; Oil<br>    [&#x15E;EKER] =&gt; sugar<br>)  

#

[Official documentation page](https://www.php.net/manual/en/function.array-change-key-case.php)

**[To root](/README.md)**