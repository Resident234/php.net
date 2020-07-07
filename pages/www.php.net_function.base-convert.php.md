# base_convert





Convert an arbitrarily large number from any base to any base.

string convBase(string $numberInput, string $fromBaseInput, string $toBaseInput)
$numberInput number to convert as a string
$fromBaseInput base of the number to convert as a string
$toBaseInput base the number should be converted to as a string
examples for $fromBaseInput and $toBaseInput
&apos;0123456789ABCDEF&apos; for Hexadecimal (Base16)
&apos;0123456789&apos; for Decimal (Base10)
&apos;01234567&apos; for Octal (Base8)
&apos;01&apos; for Binary (Base2) 
You can really put in whatever you want and the first character is the 0.
Examples:



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
//Convert &apos;355927353784509896715106760&apos; from decimal (base10) to undecimal (base11) using &quot;Christopher&quot; as the numbers.
//result: iihtspiphoeCrCeshhorsrrtrh

convBase(&apos;1C238Ab97132aAC84B72&apos;,&apos;0123456789aAbBcCdD&apos;, &apos;~!@#$%^&amp;*()&apos;);
//Convert&apos;1C238Ab97132aAC84B72&apos; from octodecimal (base18) using &apos;0123456789aAbBcCdD&apos; as the numbers to undecimal (base11) using &apos;~!@#$%^&amp;*()&apos; as the numbers.
//result: !%~!!*&amp;!~^!!&amp;(&amp;!~^@#@@@&amp;

function convBase($numberInput, $fromBaseInput, $toBaseInput)
{
&#xA0; &#xA0; if ($fromBaseInput==$toBaseInput) return $numberInput;
&#xA0; &#xA0; $fromBase = str_split($fromBaseInput,1);
&#xA0; &#xA0; $toBase = str_split($toBaseInput,1);
&#xA0; &#xA0; $number = str_split($numberInput,1);
&#xA0; &#xA0; $fromLen=strlen($fromBaseInput);
&#xA0; &#xA0; $toLen=strlen($toBaseInput);
&#xA0; &#xA0; $numberLen=strlen($numberInput);
&#xA0; &#xA0; $retval=&apos;&apos;;
&#xA0; &#xA0; if ($toBaseInput == &apos;0123456789&apos;)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $retval=0;
&#xA0; &#xA0; &#xA0; &#xA0; for ($i = 1;$i &lt;= $numberLen; $i++)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $retval = bcadd($retval, bcmul(array_search($number[$i-1], $fromBase),bcpow($fromLen,$numberLen-$i)));
&#xA0; &#xA0; &#xA0; &#xA0; return $retval;
&#xA0; &#xA0; }
&#xA0; &#xA0; if ($fromBaseInput != &apos;0123456789&apos;)
&#xA0; &#xA0; &#xA0; &#xA0; $base10=convBase($numberInput, $fromBaseInput, &apos;0123456789&apos;);
&#xA0; &#xA0; else
&#xA0; &#xA0; &#xA0; &#xA0; $base10 = $numberInput;
&#xA0; &#xA0; if ($base10&lt;strlen($toBaseInput))
&#xA0; &#xA0; &#xA0; &#xA0; return $toBase[$base10];
&#xA0; &#xA0; while($base10 != &apos;0&apos;)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $retval = $toBase[bcmod($base10,$toLen)].$retval;
&#xA0; &#xA0; &#xA0; &#xA0; $base10 = bcdiv($base10,$toLen,0);
&#xA0; &#xA0; }
&#xA0; &#xA0; return $retval;
}
?>
```



  

#



Short arabic2roman conveter:





```
<?php

function rome($N){

&#xA0; &#xA0; &#xA0; &#xA0; $c=&apos;IVXLCDM&apos;;

&#xA0; &#xA0; &#xA0; &#xA0; for($a=5,$b=$s=&apos;&apos;;$N;$b++,$a^=7)

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; for($o=$N%$a,$N=$N/$a^0;$o--;$s=$c[$o&gt;2?$b+$N-($N&amp;=-2)+$o=1:$b].$s);

&#xA0; &#xA0; &#xA0; &#xA0; return $s;

}

?>
```




And it works :)

  

#



If you use base_convert to convert a large (eg. 80-bit) hexadecimal to base-36, you might observe:



ABCDEF00001234567890 (hexadecimal) =&gt; 3O47RE02JZSW0KS8 (base-36) =&gt; ABCDEF00001240000000 (hexadecimal)



This is normal and is due to the loss of precision on large numbers.



I have written a string-based function using the built-in BC Math Extension which will overcome this and similar problems.





```
<?php



function str_baseconvert($str, $frombase=10, $tobase=36) {

&#xA0; &#xA0; $str = trim($str);

&#xA0; &#xA0; if (intval($frombase) != 10) {

&#xA0; &#xA0; &#xA0; &#xA0; $len = strlen($str);

&#xA0; &#xA0; &#xA0; &#xA0; $q = 0;

&#xA0; &#xA0; &#xA0; &#xA0; for ($i=0; $i&lt;$len; $i++) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $r = base_convert($str[$i], $frombase, 10);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $q = bcadd(bcmul($q, $frombase), $r);

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; }

&#xA0; &#xA0; else $q = $str;

 

&#xA0; &#xA0; if (intval($tobase) != 10) {

&#xA0; &#xA0; &#xA0; &#xA0; $s = &apos;&apos;;

&#xA0; &#xA0; &#xA0; &#xA0; while (bccomp($q, &apos;0&apos;, 0) &gt; 0) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $r = intval(bcmod($q, $tobase));

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $s = base_convert($r, 10, $tobase) . $s;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $q = bcdiv($q, $tobase, 0);

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; }

&#xA0; &#xA0; else $s = $q;

 

&#xA0; &#xA0; return $s;

}



?>
```




Typical use-cases:

1.&#xA0; Convert a large arbitrary precision number to base-36.

2.&#xA0; Convert a 32-char hexadecimal UUID (128-bit) to a 25-char base-36 unique key, and vice versa.



Examples:





```
<?php

$b16 = &apos;ABCDEF00001234567890&apos;;

$b36 = str_baseconvert($b16, 16, 36);

echo (&quot;$b16 (hexadecimal) = $b36 (base-36) \\n&quot;);



$uuid = &apos;ABCDEF01234567890123456789ABCDEF&apos;;

$ukey = str_baseconvert($uuid, 16, 36);

echo (&quot;$uuid (hexadecimal) = $ukey (base-36) \\n&quot;);

?>
```




ABCDEF00001234567890 (hexadecimal) = 3o47re02jzqisvio (base-36) 

ABCDEF01234567890123456789ABCDEF (hexadecimal) = a65xa07491kf5zyfpvbo76g33 (base-36)

  

#



If you need to use base_convert with numbers larger then 32 bit, the following gmp implementation of base_convert should work.



```
<?php

/*use gmp library to convert base. gmp will convert numbers &gt; 32bit*/
function gmp_convert($num, $base_a, $base_b)
{
&#xA0; &#xA0; &#xA0; &#xA0; return gmp_strval ( gmp_init($num, $base_a), $base_b );
}

?>
```



  

#



If you would like to convert numbers into just the uppercase alphabet base and vice-versa (e.g. the column names in a Microsoft Windows Excel sheet..A-Z, AA-ZZ, AAA-ZZZ, ...), the following functions will do this.



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
&#xA0; &#xA0; $r = &apos;&apos;;
&#xA0; &#xA0; for ($i = 1; $n &gt;= 0 &amp;&amp; $i &lt; 10; $i++) {
&#xA0; &#xA0; &#xA0; &#xA0; $r = chr(0x41 + ($n % pow(26, $i) / pow(26, $i - 1))) . $r;
&#xA0; &#xA0; &#xA0; &#xA0; $n -= pow(26, $i);
&#xA0; &#xA0; }
&#xA0; &#xA0; return $r;
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
&#xA0; &#xA0; $r = 0;
&#xA0; &#xA0; $l = strlen($a);
&#xA0; &#xA0; for ($i = 0; $i &lt; $l; $i++) {
&#xA0; &#xA0; &#xA0; &#xA0; $r += pow(26, $i) * (ord($a[$l - $i - 1]) - 0x40);
&#xA0; &#xA0; }
&#xA0; &#xA0; return $r - 1;
}

?>
```


Microsoft Windows Excel stops at IV (255), but this function can handle much larger. However, English words will start to form after a while and some may be offensive, so be careful.

  

#

[Official documentation page](https://www.php.net/manual/en/function.base-convert.php)

**[To root](/README.md)**