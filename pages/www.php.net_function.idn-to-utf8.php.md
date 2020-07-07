# idn_to_utf8







```
<?php
// for those who has PHP older than version 5.3
class IDN {
&#xA0; &#xA0; // adapt bias for punycode algorithm
&#xA0; &#xA0; private static function punyAdapt(
&#xA0; &#xA0; &#xA0; &#xA0; $delta,
&#xA0; &#xA0; &#xA0; &#xA0; $numpoints,
&#xA0; &#xA0; &#xA0; &#xA0; $firsttime
&#xA0; &#xA0; ) {
&#xA0; &#xA0; &#xA0; &#xA0; $delta = $firsttime ? $delta / 700 : $delta / 2; 
&#xA0; &#xA0; &#xA0; &#xA0; $delta += $delta / $numpoints;
&#xA0; &#xA0; &#xA0; &#xA0; for ($k = 0; $delta &gt; 455; $k += 36)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $delta = intval($delta / 35);
&#xA0; &#xA0; &#xA0; &#xA0; return $k + (36 * $delta) / ($delta + 38);
&#xA0; &#xA0; }
 
&#xA0; &#xA0; // translate character to punycode number
&#xA0; &#xA0; private static function decodeDigit($cp) {
&#xA0; &#xA0; &#xA0; &#xA0; $cp = strtolower($cp);
&#xA0; &#xA0; &#xA0; &#xA0; if ($cp &gt;= &apos;a&apos; &amp;&amp; $cp &lt;= &apos;z&apos;)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return ord($cp) - ord(&apos;a&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; elseif ($cp &gt;= &apos;0&apos; &amp;&amp; $cp &lt;= &apos;9&apos;)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return ord($cp) - ord(&apos;0&apos;)+26;
&#xA0; &#xA0; }
 
&#xA0; &#xA0; // make utf8 string from unicode codepoint number
&#xA0; &#xA0; private static function utf8($cp) {
&#xA0; &#xA0; &#xA0; &#xA0; if ($cp &lt; 128) return chr($cp);
&#xA0; &#xA0; &#xA0; &#xA0; if ($cp &lt; 2048) 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return chr(192+($cp &gt;&gt; 6)).chr(128+($cp &amp; 63));
&#xA0; &#xA0; &#xA0; &#xA0; if ($cp &lt; 65536) return 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; chr(224+($cp &gt;&gt; 12)).
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; chr(128+(($cp &gt;&gt; 6) &amp; 63)).
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; chr(128+($cp &amp; 63));
&#xA0; &#xA0; &#xA0; &#xA0; if ($cp &lt; 2097152) return 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; chr(240+($cp &gt;&gt; 18)).
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; chr(128+(($cp &gt;&gt; 12) &amp; 63)).
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; chr(128+(($cp &gt;&gt; 6) &amp; 63)).
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; chr(128+($cp &amp; 63));
&#xA0; &#xA0; &#xA0; &#xA0; // it should never get here 
&#xA0; &#xA0; }
 
&#xA0; &#xA0; // main decoding function
&#xA0; &#xA0; private static function decodePart($input) {
&#xA0; &#xA0; &#xA0; &#xA0; if (substr($input,0,4) != &quot;xn--&quot;) // prefix check...
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $input;
&#xA0; &#xA0; &#xA0; &#xA0; $input = substr($input,4); // discard prefix
&#xA0; &#xA0; &#xA0; &#xA0; $a = explode(&quot;-&quot;,$input);
&#xA0; &#xA0; &#xA0; &#xA0; if (count($a) &gt; 1) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $input = str_split(array_pop($a));
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $output = str_split(implode(&quot;-&quot;,$a));
&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $output = array();
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $input = str_split($input);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; $n = 128; $i = 0; $bias = 72; // init punycode vars
&#xA0; &#xA0; &#xA0; &#xA0; while (!empty($input)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $oldi = $i;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $w = 1;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; for ($k = 36;;$k += 36) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $digit = IDN::decodeDigit(array_shift($input));
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $i += $digit * $w;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ($k &lt;= $bias) $t = 1;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; elseif ($k &gt;= $bias + 26) $t = 26;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else $t = $k - $bias;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ($digit &lt; $t) break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $w *= intval(36 - $t);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bias = IDN::punyAdapt(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $i-$oldi,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; count($output)+1,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $oldi == 0
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; );
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $n += intval($i / (count($output) + 1));
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $i %= count($output) + 1;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; array_splice($output,$i,0,array(IDN::utf8($n)));
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $i++;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; return implode(&quot;&quot;,$output);
&#xA0; &#xA0; }
 
&#xA0; &#xA0; public static function decodeIDN($name) {
&#xA0; &#xA0; &#xA0; &#xA0; // split it, parse it and put it back together
&#xA0; &#xA0; &#xA0; &#xA0; return 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; implode(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;.&quot;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; array_map(&quot;IDN::decodePart&quot;,explode(&quot;.&quot;,$name))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; );
&#xA0; &#xA0; }
 
}
echo IDN::decodeIDN($_SERVER[&apos;HTTP_HOST&apos;]);
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.idn-to-utf8.php)

**[To root](/README.md)**