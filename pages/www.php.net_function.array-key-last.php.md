# array_key_last



For PHP &lt;= 7.3.0 :<br><br>if (! function_exists("array_key_last")) {<br>    function array_key_last($array) {<br>        if (!is_array($array) || empty($array)) {<br>            return NULL;<br>        }<br>        <br>        return array_keys($array)[count($array)-1];<br>    }<br>}  

#

[Official documentation page](https://www.php.net/manual/en/function.array-key-last.php)

**[To root](/README.md)**