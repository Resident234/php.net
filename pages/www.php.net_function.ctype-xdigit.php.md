# ctype_xdigit



This function shows its usefulness on a web site where a user is asked to entered a hexidecimal color code for a color. To prevent breaking W3C standard and having them enter in "neon-green" or the wrong type of code like 355511235.<br><br>In conjunction with strlen()  you could create a function like this:<br><br>function check_valid_colorhex($colorCode) {<br>    // If user accidentally passed along the # sign, strip it off<br>    $colorCode = ltrim($colorCode, &apos;#&apos;);<br><br>    if (<br>          ctype_xdigit($colorCode) &amp;&amp;<br>          (strlen($colorCode) == 6 || strlen($colorCode) == 3))<br>               return true;<br><br>    else return false;<br>}  

---

[Official documentation page](https://www.php.net/manual/en/function.ctype-xdigit.php)

**[To root](/README.md)**