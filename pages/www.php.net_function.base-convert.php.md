# base_convert



Convert an arbitrarily large number from any base to any base.<br><br>string convBase(string $numberInput, string $fromBaseInput, string $toBaseInput)<br>$numberInput number to convert as a string<br>$fromBaseInput base of the number to convert as a string<br>$toBaseInput base the number should be converted to as a string<br>examples for $fromBaseInput and $toBaseInput<br>&apos;0123456789ABCDEF&apos; for Hexadecimal (Base16)<br>&apos;0123456789&apos; for Decimal (Base10)<br>&apos;01234567&apos; for Octal (Base8)<br>&apos;01&apos; for Binary (Base2) <br>You can really put in whatever you want and the first character is the 0.<br>Examples:<br><br>

```
<?php 
convBase(&apos;123&apos;, &apos;0123456789&apos;, &apos;01234567&apos;); 
//Convert &apos;123&apos; from decimal (base10) to octal (base8).
//result: 173

convBase(&apos;70B1D707EAC2EDF4C6389F440C7294B51FFF57BB&apos;, &apos;0123456789ABCDEF&apos;, &apos;01&apos;);
//Convert &apos;70B1D707EAC2EDF4C6389F440C7294B51FFF57BB&apos; from hexadecimal (base16) to binary (base2).
//result: 
//111000010110001110101110000011111101010110000101110
//110111110100110001100011100010011111010001000000110
//001110010100101001011010100011111111111110101011110
//111011

convBase(&apos;1324523453243154324542341524315432113200203012&apos;, &apos;012345&apos;, &apos;0123456789ABCDEF&apos;);
//Convert &apos;1324523453243154324542341524315432113200203012&apos; from senary (base6) to hexadecimal (base16).
//result: 1F9881BAD10454A8C23A838EF00F50

convBase(&apos;355927353784509896715106760&apos;,&apos;0123456789&apos;,&apos;Christopher&apos;);
//Convert &apos;355927353784509896715106760&apos; from decimal (base10) to undecimal (base11) using "Christopher" as the numbers.
//result: iihtspiphoeCrCeshhorsrrtrh

convBase(&apos;1C238Ab97132aAC84B72&apos;,&apos;0123456789aAbBcCdD&apos;, &apos;~!@#$%^&amp;*()&apos;);
//Convert&apos;1C238Ab97132aAC84B72&apos; from octodecimal (base18) using &apos;0123456789aAbBcCdD&apos; as the numbers to undecimal (base11) using &apos;~!@#$%^&amp;*()&apos; as the numbers.
//result: !%~!!*&amp;!~^!!&amp;(&amp;!~^@#@@@&amp;

function convBase($numberInput, $fromBaseInput, $toBaseInput)
{
    if ($fromBaseInput==$toBaseInput) return $numberInput;
    $fromBase = str_split($fromBaseInput,1);
    $toBase = str_split($toBaseInput,1);
    $number = str_split($numberInput,1);
    $fromLen=strlen($fromBaseInput);
    $toLen=strlen($toBaseInput);
    $numberLen=strlen($numberInput);
    $retval=&apos;&apos;;
    if ($toBaseInput == &apos;0123456789&apos;)
    {
        $retval=0;
        for ($i = 1;$i &lt;= $numberLen; $i++)
            $retval = bcadd($retval, bcmul(array_search($number[$i-1], $fromBase),bcpow($fromLen,$numberLen-$i)));
        return $retval;
    }
    if ($fromBaseInput != &apos;0123456789&apos;)
        $base10=convBase($numberInput, $fromBaseInput, &apos;0123456789&apos;);
    else
        $base10 = $numberInput;
    if ($base10&lt;strlen($toBaseInput))
        return $toBase[$base10];
    while($base10 != &apos;0&apos;)
    {
        $retval = $toBase[bcmod($base10,$toLen)].$retval;
        $base10 = bcdiv($base10,$toLen,0);
    }
    return $retval;
}
?>
```
  

#

Short arabic2roman conveter:<br><br>

```
<?php
function rome($N){
        $c=&apos;IVXLCDM&apos;;
        for($a=5,$b=$s=&apos;&apos;;$N;$b++,$a^=7)
                for($o=$N%$a,$N=$N/$a^0;$o--;$s=$c[$o&gt;2?$b+$N-($N&amp;=-2)+$o=1:$b].$s);
        return $s;
}
?>
```
<br><br>And it works :)  

#

If you use base_convert to convert a large (eg. 80-bit) hexadecimal to base-36, you might observe:<br><br>ABCDEF00001234567890 (hexadecimal) =&gt; 3O47RE02JZSW0KS8 (base-36) =&gt; ABCDEF00001240000000 (hexadecimal)<br><br>This is normal and is due to the loss of precision on large numbers.<br><br>I have written a string-based function using the built-in BC Math Extension which will overcome this and similar problems.<br><br>

```
<?php

function str_baseconvert($str, $frombase=10, $tobase=36) {
    $str = trim($str);
    if (intval($frombase) != 10) {
        $len = strlen($str);
        $q = 0;
        for ($i=0; $i&lt;$len; $i++) {
            $r = base_convert($str[$i], $frombase, 10);
            $q = bcadd(bcmul($q, $frombase), $r);
        }
    }
    else $q = $str;
 
    if (intval($tobase) != 10) {
        $s = &apos;&apos;;
        while (bccomp($q, &apos;0&apos;, 0) &gt; 0) {
            $r = intval(bcmod($q, $tobase));
            $s = base_convert($r, 10, $tobase) . $s;
            $q = bcdiv($q, $tobase, 0);
        }
    }
    else $s = $q;
 
    return $s;
}

?>
```


Typical use-cases:
1.  Convert a large arbitrary precision number to base-36.
2.  Convert a 32-char hexadecimal UUID (128-bit) to a 25-char base-36 unique key, and vice versa.

Examples:



```
<?php
$b16 = &apos;ABCDEF00001234567890&apos;;
$b36 = str_baseconvert($b16, 16, 36);
echo ("$b16 (hexadecimal) = $b36 (base-36) \\n");

$uuid = &apos;ABCDEF01234567890123456789ABCDEF&apos;;
$ukey = str_baseconvert($uuid, 16, 36);
echo ("$uuid (hexadecimal) = $ukey (base-36) \\n");
?>
```
<br><br>ABCDEF00001234567890 (hexadecimal) = 3o47re02jzqisvio (base-36) <br>ABCDEF01234567890123456789ABCDEF (hexadecimal) = a65xa07491kf5zyfpvbo76g33 (base-36)  

#

If you need to use base_convert with numbers larger then 32 bit, the following gmp implementation of base_convert should work.<br><br>

```
<?php

/*use gmp library to convert base. gmp will convert numbers &gt; 32bit*/
function gmp_convert($num, $base_a, $base_b)
{
        return gmp_strval ( gmp_init($num, $base_a), $base_b );
}

?>
```
  

#

If you would like to convert numbers into just the uppercase alphabet base and vice-versa (e.g. the column names in a Microsoft Windows Excel sheet..A-Z, AA-ZZ, AAA-ZZZ, ...), the following functions will do this.<br><br>

```
<?php

/**
 * Converts an integer into the alphabet base (A-Z).
 *
 * @param int $n This is the number to convert.
 * @return string The converted number.
 * @author Theriault
 * 
 */
function num2alpha($n) {
    $r = &apos;&apos;;
    for ($i = 1; $n &gt;= 0 &amp;&amp; $i &lt; 10; $i++) {
        $r = chr(0x41 + ($n % pow(26, $i) / pow(26, $i - 1))) . $r;
        $n -= pow(26, $i);
    }
    return $r;
}
/**
 * Converts an alphabetic string into an integer.
 *
 * @param int $n This is the number to convert.
 * @return string The converted number.
 * @author Theriault
 * 
 */
function alpha2num($a) {
    $r = 0;
    $l = strlen($a);
    for ($i = 0; $i &lt; $l; $i++) {
        $r += pow(26, $i) * (ord($a[$l - $i - 1]) - 0x40);
    }
    return $r - 1;
}

?>
```
<br><br>Microsoft Windows Excel stops at IV (255), but this function can handle much larger. However, English words will start to form after a while and some may be offensive, so be careful.  

#

[Official documentation page](https://www.php.net/manual/en/function.base-convert.php)

**[To root](/README.md)**