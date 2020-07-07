# dechex





Be very careful calling dechex on a number if it&apos;s stored in a string.

For instance:

The max number it can handle is 4294967295 which in hex is FFFFFFFF, as it says in the documentation.

dechex(4294967295) =&gt; FFFFFFFF //CORRECT

BUT, if you call it on a string of a number, it casts to int, and automatically gives you the largest int it can handle.

dechex(&apos;4294967295&apos;) =&gt; 7FFFFFFF //WRONG!

so you&apos;ll need to cast to a float:

dechex((float) &apos;4294967295&apos;) =&gt; FFFFFFFF //CORRECT

This took me FOREVER to figure out, so hopefully I just saved someone some time.

  

#



Here are two functions that will convert large dec numbers to hex and vice versa. And I really mean LARGE, much larger than any function posted earlier.

&lt;pre&gt;
// Input: A decimal number as a String.
// Output: The equivalent hexadecimal number as a String.
function dec2hex($number)
{
&#xA0; &#xA0; $hexvalues = array(&apos;0&apos;,&apos;1&apos;,&apos;2&apos;,&apos;3&apos;,&apos;4&apos;,&apos;5&apos;,&apos;6&apos;,&apos;7&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; &apos;8&apos;,&apos;9&apos;,&apos;A&apos;,&apos;B&apos;,&apos;C&apos;,&apos;D&apos;,&apos;E&apos;,&apos;F&apos;);
&#xA0; &#xA0; $hexval = &apos;&apos;;
&#xA0; &#xA0;&#xA0; while($number != &apos;0&apos;)
&#xA0; &#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $hexval = $hexvalues[bcmod($number,&apos;16&apos;)].$hexval;
&#xA0; &#xA0; &#xA0; &#xA0; $number = bcdiv($number,&apos;16&apos;,0);
&#xA0; &#xA0; }
&#xA0; &#xA0; return $hexval;
}

// Input: A hexadecimal number as a String.
// Output: The equivalent decimal number as a String.
function hex2dec($number)
{
&#xA0; &#xA0; $decvalues = array(&apos;0&apos; =&gt; &apos;0&apos;, &apos;1&apos; =&gt; &apos;1&apos;, &apos;2&apos; =&gt; &apos;2&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; &apos;3&apos; =&gt; &apos;3&apos;, &apos;4&apos; =&gt; &apos;4&apos;, &apos;5&apos; =&gt; &apos;5&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; &apos;6&apos; =&gt; &apos;6&apos;, &apos;7&apos; =&gt; &apos;7&apos;, &apos;8&apos; =&gt; &apos;8&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; &apos;9&apos; =&gt; &apos;9&apos;, &apos;A&apos; =&gt; &apos;10&apos;, &apos;B&apos; =&gt; &apos;11&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; &apos;C&apos; =&gt; &apos;12&apos;, &apos;D&apos; =&gt; &apos;13&apos;, &apos;E&apos; =&gt; &apos;14&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; &apos;F&apos; =&gt; &apos;15&apos;);
&#xA0; &#xA0; $decval = &apos;0&apos;;
&#xA0; &#xA0; $number = strrev($number);
&#xA0; &#xA0; for($i = 0; $i &lt; strlen($number); $i++)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $decval = bcadd(bcmul(bcpow(&apos;16&apos;,$i,0),$decvalues[$number{$i}]), $decval);
&#xA0; &#xA0; }
&#xA0; &#xA0; return $decval;
}
&lt;/pre&gt;

  

#

[Official documentation page](https://www.php.net/manual/en/function.dechex.php)

**[To root](/README.md)**