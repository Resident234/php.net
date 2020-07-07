# hexdec





Use this function to convert a hexa decimal color code to its RGB equivalent. Unlike many other functions provided here, it will work correctly with hex color short hand notation.

Also, if a proper hexa decimal color value is given (6 digits), it uses bit wise operations for faster results.

For eg: #FFF and #FFFFFF will produce the same result



```
<?php
/**
 * Convert a hexa decimal color code to its RGB equivalent
 *
 * @param string $hexStr (hexadecimal color value)
 * @param boolean $returnAsString (if set true, returns the value separated by the separator character. Otherwise returns associative array)
 * @param string $seperator (to separate RGB values. Applicable only if second parameter is true.)
 * @return array or string (depending on second parameter. Returns False if invalid hex color value)
 */&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
function hex2RGB($hexStr, $returnAsString = false, $seperator = &apos;,&apos;) {
&#xA0; &#xA0; $hexStr = preg_replace(&quot;/[^0-9A-Fa-f]/&quot;, &apos;&apos;, $hexStr); // Gets a proper hex string
&#xA0; &#xA0; $rgbArray = array();
&#xA0; &#xA0; if (strlen($hexStr) == 6) { //If a proper hex code, convert using bitwise operation. No overhead... faster
&#xA0; &#xA0; &#xA0; &#xA0; $colorVal = hexdec($hexStr);
&#xA0; &#xA0; &#xA0; &#xA0; $rgbArray[&apos;red&apos;] = 0xFF &amp; ($colorVal &gt;&gt; 0x10);
&#xA0; &#xA0; &#xA0; &#xA0; $rgbArray[&apos;green&apos;] = 0xFF &amp; ($colorVal &gt;&gt; 0x8);
&#xA0; &#xA0; &#xA0; &#xA0; $rgbArray[&apos;blue&apos;] = 0xFF &amp; $colorVal;
&#xA0; &#xA0; } elseif (strlen($hexStr) == 3) { //if shorthand notation, need some string manipulations
&#xA0; &#xA0; &#xA0; &#xA0; $rgbArray[&apos;red&apos;] = hexdec(str_repeat(substr($hexStr, 0, 1), 2));
&#xA0; &#xA0; &#xA0; &#xA0; $rgbArray[&apos;green&apos;] = hexdec(str_repeat(substr($hexStr, 1, 1), 2));
&#xA0; &#xA0; &#xA0; &#xA0; $rgbArray[&apos;blue&apos;] = hexdec(str_repeat(substr($hexStr, 2, 1), 2));
&#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; return false; //Invalid hex color code
&#xA0; &#xA0; }
&#xA0; &#xA0; return $returnAsString ? implode($seperator, $rgbArray) : $rgbArray; // returns the rgb string or the associative array
} ?>
```


OUTPUT:

hex2RGB(&quot;#FF0&quot;) -&gt; array( red =&gt;255, green =&gt; 255, blue =&gt; 0)
hex2RGB(&quot;#FFFF00) -&gt; Same as above
hex2RGB(&quot;#FF0&quot;, true) -&gt; 255,255,0
hex2RGB(&quot;#FF0&quot;, true, &quot;:&quot;) -&gt; 255:255:0

  

#

[Official documentation page](https://www.php.net/manual/en/function.hexdec.php)

**[To root](/README.md)**