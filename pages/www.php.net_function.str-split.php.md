# str_split



A proper unicode string split;<br><br>

```
<?php
function str_split_unicode($str, $l = 0) {
    if ($l > 0) {
        $ret = array();
        $len = mb_strlen($str, "UTF-8");
        for ($i = 0; $i < $len; $i += $l) {
            $ret[] = mb_substr($str, $i, $l, "UTF-8");
        }
        return $ret;
    }
    return preg_split("//u", $str, -1, PREG_SPLIT_NO_EMPTY);
}
?>
```
<br><br>$s = "Il&#x131;k s&#xFC;t"; // Mild milk<br><br>print_r(str_split($s, 3));<br>print_r(str_split_unicode($s, 3));<br><br>Array<br>(<br>    [0] =&gt; Il&#xFFFD;<br>    [1] =&gt; &#xFFFD;k <br>    [2] =&gt; s&#xFC;<br>    [3] =&gt; t<br>)<br><br>Array<br>(<br>    [0] =&gt; Il&#x131;<br>    [1] =&gt; k s<br>    [2] =&gt; &#xFC;t<br>)  

#

A new version of "str_split_unicode" prev.<br><br>

```
<?php
function str_split_unicode($str, $length = 1) {
    $tmp = preg_split('~~u', $str, -1, PREG_SPLIT_NO_EMPTY);
    if ($length > 1) {
        $chunks = array_chunk($tmp, $length);
        foreach ($chunks as $i => $chunk) {
            $chunks[$i] = join('', (array) $chunk);
        }
        $tmp = $chunks;
    }
    return $tmp;
}
?>
```
<br><br>$s = &apos;&#xD6;zg&#xFC;r Yaz&#x131;l&#x131;m!&apos;; // Open Source!<br><br>print_r(str_split_unicode($s));<br>print_r(str_split_unicode($s, 3));<br><br>Array<br>(<br>    [0] =&gt; &#xD6;<br>    [1] =&gt; z<br>    [2] =&gt; g<br>    [3] =&gt; &#xFC;<br>    [4] =&gt; r<br>    [5] =&gt;  <br>    [6] =&gt; Y<br>    [7] =&gt; a<br>    [8] =&gt; z<br>    [9] =&gt; &#x131;<br>    [10] =&gt; l<br>    [11] =&gt; &#x131;<br>    [12] =&gt; m<br>    [13] =&gt; !<br>)<br>Array<br>(<br>    [0] =&gt; &#xD6;zg<br>    [1] =&gt; &#xFC;r <br>    [2] =&gt; Yaz<br>    [3] =&gt; &#x131;l&#x131;<br>    [4] =&gt; m!<br>)  

#

Version of str_split by rlpvandenberg at hotmail dot com is god-damn inefficient and when $i+$j &gt; strlen($text) [last part of string] throws a lot of notice errors. This should work better:<br><br>    if(! function_exists(&apos;str_split&apos;))<br>    {<br>        function str_split($text, $split = 1)<br>        {<br>            $array = array();<br>            <br>            for ($i = 0; $i &lt; strlen($text);)<br>            {<br>                $array[] = substr($text, $i, $split);<br>                $i += $split;<br>            }<br>            <br>            return $array;<br>        }<br>    }  

#

[Official documentation page](https://www.php.net/manual/en/function.str-split.php)

**[To root](/README.md)**