# str_shuffle



Aoccdrnig to rseearch at an Elingsh uinervtisy, it deosn&apos;t mttaer in waht oredr the ltteers in a wrod are, the olny iprmoatnt tihng is that the frist and lsat ltteer is at the rghit pclae. The rset can be a toatl mses and you can sitll raed it wouthit a porbelm. Tihs is bcuseae we do not raed ervey lteter by it slef but the wrod as a wlohe.<br><br>Hree&apos;s a cdoe taht slerbmcas txet in tihs way:<br>

```
<?php
    function scramble_word($word) {
        if (strlen($word) &lt; 2)
            return $word;
        else
            return $word{0} . str_shuffle(substr($word, 1, -1)) . $word{strlen($word) - 1};
    }

    echo preg_replace('/(\w+)/e', 'scramble_word("\1")', 'A quick brown fox jumped over the lazy dog.');
?>
```
<br><br>It may be ufseul if you wnat to cetare an aessblicce CTCPAHA.  

#

This function is affected by srand():<br><br>

```
<?php
srand(12345);
echo str_shuffle('Randomize me') . '&lt;br/&gt;'; // "demmiezr aon"
echo str_shuffle('Randomize me') . '&lt;br/&gt;'; // "izadmeo rmen"

srand(12345);
echo str_shuffle('Randomize me') . '&lt;br/&gt;'; // "demmiezr aon" again
?>
```
  

#

A proper unicode string shuffle;<br><br>

```
<?php
function str_shuffle_unicode($str) {
    $tmp = preg_split("//u", $str, -1, PREG_SPLIT_NO_EMPTY);
    shuffle($tmp);
    return join("", $tmp);
}
?>
```
<br><br>$str = "&#x15E;eker y&#xE2;rim"; // My sweet love<br><br>echo str_shuffle($str); // i&#xFFFD;eymr&#x162;ekr &#xFFFD;<br><br>echo str_shuffle_unicode($str); // &#x15E;r mreyeik&#xE2;  

#

[Official documentation page](https://www.php.net/manual/en/function.str-shuffle.php)

**[To root](/README.md)**