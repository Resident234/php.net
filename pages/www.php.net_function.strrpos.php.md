# strrpos



The documentation for &apos;offset&apos; is misleading.<br><br>It says, "offset may be specified to begin searching an arbitrary number of characters into the string. Negative values will stop searching at an arbitrary point prior to the end of the string."<br><br>This is confusing if you think of strrpos as starting at the end of the string and working backwards.<br><br>A better way to think of offset is:<br><br>- If offset is positive, then strrpos only operates on the part of the string from offset to the end. This will usually have the same results as not specifying an offset, unless the only occurences of needle are before offset (in which case specifying the offset won&apos;t find the needle).<br><br>- If offset is negative, then strrpos only operates on that many characters at the end of the string. If the needle is farther away from the end of the string, it won&apos;t be found.<br><br>If, for example, you want to find the last space in a string before the 50th character, you&apos;ll need to do something like this:<br><br>strrpos($text, " ", -(strlen($text) - 50));<br><br>If instead you used strrpos($text, " ", 50), then you would find the last space between the 50th character and the end of the string, which may not have been what you were intending.  

#

[Official documentation page](https://www.php.net/manual/en/function.strrpos.php)

**[To root](/README.md)**