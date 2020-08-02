# floatval



This function takes the last comma or dot (if any) to make a clean float, ignoring thousand separator, currency or any other letter :<br><br>function tofloat($num) {<br>    $dotPos = strrpos($num, &apos;.&apos;);<br>    $commaPos = strrpos($num, &apos;,&apos;);<br>    $sep = (($dotPos &gt; $commaPos) &amp;&amp; $dotPos) ? $dotPos : <br>        ((($commaPos &gt; $dotPos) &amp;&amp; $commaPos) ? $commaPos : false);<br>   <br>    if (!$sep) {<br>        return floatval(preg_replace("/[^0-9]/", "", $num));<br>    } <br><br>    return floatval(<br>        preg_replace("/[^0-9]/", "", substr($num, 0, $sep)) . &apos;.&apos; .<br>        preg_replace("/[^0-9]/", "", substr($num, $sep+1, strlen($num)))<br>    );<br>}<br><br>$num = &apos;1.999,369&#x20AC;&apos;;<br>var_dump(tofloat($num)); // float(1999.369)<br>$otherNum = &apos;126,564,789.33 m&#xB2;&apos;;<br>var_dump(tofloat($otherNum)); // float(126564789.33)<br><br>Demo : http://codepad.org/NW4e9hQH  

---

you can also use typecasting instead of functions:<br><br>(float) $value;  

---

To view the very large and very small numbers (eg from a database DECIMAL), without displaying scientific notation, or leading zeros.<br><br>FR : Pour afficher les tr&#xE8;s grand et tr&#xE8;s petits nombres (ex. depuis une base de donn&#xE9;es DECIMAL), sans afficher la notation scientifique, ni les z&#xE9;ros non significatifs.<br><br>

```
<?php
function floattostr( $val )
{
    preg_match( "#^([\+\-]|)([0-9]*)(\.([0-9.*?)|)(0*)$#", trim($val), $o );
    return $o[1].sprintf('%d',$o[2]).($o[3]!='.'?$o[3]:'');
}
?>
```




```
<?php
echo floattostr("0000000000000001");
echo floattostr("1.00000000000000");
echo floattostr("0.00000000001000");
echo floattostr("0000.00010000000");
echo floattostr("000000010000000000.00000000000010000000000");
echo floattostr("-0000000000000.1");
echo floattostr("-00000001.100000");

// result
// 1
// 1
// 0.00000000001
// 0.0001
// 10000000000.0000000000001
// -0.1
// -1.1

?>
```
  

---

Easier-to-grasp-function for the &apos;,&apos; problem.<br><br>

```
<?php
function Getfloat($str) {
  if(strstr($str, ",")) {
    $str = str_replace(".", "", $str); // replace dots (thousand seps) with blancs
    $str = str_replace(",", ".", $str); // replace ',' with '.'
  }
  
  if(preg_match("#([0-9\.]+)#", $str, $match)) { // search for number that may contain '.'
    return floatval($match[0]);
  } else {
    return floatval($str); // take some last chances with floatval
  }
}

echo Getfloat("$ 19.332,35-"); // will print: 19332.35
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.floatval.php)

**[To root](/README.md)**