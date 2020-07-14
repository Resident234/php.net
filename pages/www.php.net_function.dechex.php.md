# dechex



Be very careful calling dechex on a number if it&apos;s stored in a string.<br><br>For instance:<br><br>The max number it can handle is 4294967295 which in hex is FFFFFFFF, as it says in the documentation.<br><br>dechex(4294967295) =&gt; FFFFFFFF //CORRECT<br><br>BUT, if you call it on a string of a number, it casts to int, and automatically gives you the largest int it can handle.<br><br>dechex(&apos;4294967295&apos;) =&gt; 7FFFFFFF //WRONG!<br><br>so you&apos;ll need to cast to a float:<br><br>dechex((float) &apos;4294967295&apos;) =&gt; FFFFFFFF //CORRECT<br><br>This took me FOREVER to figure out, so hopefully I just saved someone some time.  

#

Here are two functions that will convert large dec numbers to hex and vice versa. And I really mean LARGE, much larger than any function posted earlier.<br><br>&lt;pre&gt;<br>// Input: A decimal number as a String.<br>// Output: The equivalent hexadecimal number as a String.<br>function dec2hex($number)<br>{<br>    $hexvalues = array(&apos;0&apos;,&apos;1&apos;,&apos;2&apos;,&apos;3&apos;,&apos;4&apos;,&apos;5&apos;,&apos;6&apos;,&apos;7&apos;,<br>               &apos;8&apos;,&apos;9&apos;,&apos;A&apos;,&apos;B&apos;,&apos;C&apos;,&apos;D&apos;,&apos;E&apos;,&apos;F&apos;);<br>    $hexval = &apos;&apos;;<br>     while($number != &apos;0&apos;)<br>     {<br>        $hexval = $hexvalues[bcmod($number,&apos;16&apos;)].$hexval;<br>        $number = bcdiv($number,&apos;16&apos;,0);<br>    }<br>    return $hexval;<br>}<br><br>// Input: A hexadecimal number as a String.<br>// Output: The equivalent decimal number as a String.<br>function hex2dec($number)<br>{<br>    $decvalues = array(&apos;0&apos; =&gt; &apos;0&apos;, &apos;1&apos; =&gt; &apos;1&apos;, &apos;2&apos; =&gt; &apos;2&apos;,<br>               &apos;3&apos; =&gt; &apos;3&apos;, &apos;4&apos; =&gt; &apos;4&apos;, &apos;5&apos; =&gt; &apos;5&apos;,<br>               &apos;6&apos; =&gt; &apos;6&apos;, &apos;7&apos; =&gt; &apos;7&apos;, &apos;8&apos; =&gt; &apos;8&apos;,<br>               &apos;9&apos; =&gt; &apos;9&apos;, &apos;A&apos; =&gt; &apos;10&apos;, &apos;B&apos; =&gt; &apos;11&apos;,<br>               &apos;C&apos; =&gt; &apos;12&apos;, &apos;D&apos; =&gt; &apos;13&apos;, &apos;E&apos; =&gt; &apos;14&apos;,<br>               &apos;F&apos; =&gt; &apos;15&apos;);<br>    $decval = &apos;0&apos;;<br>    $number = strrev($number);<br>    for($i = 0; $i &lt; strlen($number); $i++)<br>    {<br>        $decval = bcadd(bcmul(bcpow(&apos;16&apos;,$i,0),$decvalues[$number{$i}]), $decval);<br>    }<br>    return $decval;<br>}<br>&lt;/pre&gt;  

#

[Official documentation page](https://www.php.net/manual/en/function.dechex.php)

**[To root](/README.md)**