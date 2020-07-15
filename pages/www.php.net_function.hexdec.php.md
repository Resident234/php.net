# hexdec



Use this function to convert a hexa decimal color code to its RGB equivalent. Unlike many other functions provided here, it will work correctly with hex color short hand notation.<br><br>Also, if a proper hexa decimal color value is given (6 digits), it uses bit wise operations for faster results.<br><br>For eg: #FFF and #FFFFFF will produce the same result<br><br>

```
<?php
/**
 * Convert a hexa decimal color code to its RGB equivalent
 *
 * @param string $hexStr (hexadecimal color value)
 * @param boolean $returnAsString (if set true, returns the value separated by the separator character. Otherwise returns associative array)
 * @param string $seperator (to separate RGB values. Applicable only if second parameter is true.)
 * @return array or string (depending on second parameter. Returns False if invalid hex color value)
 */                                                                                                 
function hex2RGB($hexStr, $returnAsString = false, $seperator = ',') {
    $hexStr = preg_replace("/[^0-9A-Fa-f]/", '', $hexStr); // Gets a proper hex string
    $rgbArray = array();
    if (strlen($hexStr) == 6) { //If a proper hex code, convert using bitwise operation. No overhead... faster
        $colorVal = hexdec($hexStr);
        $rgbArray['red'] = 0xFF &amp; ($colorVal &gt;&gt; 0x10);
        $rgbArray['green'] = 0xFF &amp; ($colorVal &gt;&gt; 0x8);
        $rgbArray['blue'] = 0xFF &amp; $colorVal;
    } elseif (strlen($hexStr) == 3) { //if shorthand notation, need some string manipulations
        $rgbArray['red'] = hexdec(str_repeat(substr($hexStr, 0, 1), 2));
        $rgbArray['green'] = hexdec(str_repeat(substr($hexStr, 1, 1), 2));
        $rgbArray['blue'] = hexdec(str_repeat(substr($hexStr, 2, 1), 2));
    } else {
        return false; //Invalid hex color code
    }
    return $returnAsString ? implode($seperator, $rgbArray) : $rgbArray; // returns the rgb string or the associative array
} ?>
```
<br><br>OUTPUT:<br><br>hex2RGB("#FF0") -&gt; array( red =&gt;255, green =&gt; 255, blue =&gt; 0)<br>hex2RGB("#FFFF00) -&gt; Same as above<br>hex2RGB("#FF0", true) -&gt; 255,255,0<br>hex2RGB("#FF0", true, ":") -&gt; 255:255:0  

#

[Official documentation page](https://www.php.net/manual/en/function.hexdec.php)

**[To root](/README.md)**