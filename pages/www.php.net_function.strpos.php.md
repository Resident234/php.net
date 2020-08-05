# strpos



WARNING<br><br>As strpos may return either FALSE (substring absent) or 0 (substring at start of string), strict versus loose equivalency operators must be used very carefully.<br><br>To know that a substring is absent, you must use:  <br><br>=== FALSE<br><br>To know that a substring is present (in any position including 0), you can use either of:<br><br>!== FALSE  (recommended)<br> &gt; -1  (note: or greater than any negative number)<br><br>To know that a substring is at the start of the string, you must use:  <br><br>=== 0<br><br>To know that a substring is in any position other than the start, you can use any of: <br><br> &gt; 0  (recommended)<br>!= 0  (note: but not !== 0 which also equates to FALSE)<br>!= FALSE  (disrecommended as highly confusing)<br><br>Also note that you cannot compare a value of "" to the returned value of strpos. With a loose equivalence operator (== or !=) it will return results which don&apos;t distinguish between the substring&apos;s presence versus position. With a strict equivalence operator (=== or !==) it will always return false.  

---

This is a function I wrote to find all occurrences of a string, using strpos recursively.<br><br>

```
<?php
function strpos_recursive($haystack, $needle, $offset = 0, &amp;$results = array()) {                
    $offset = strpos($haystack, $needle, $offset);
    if($offset === false) {
        return $results;            
    } else {
        $results[] = $offset;
        return strpos_recursive($haystack, $needle, ($offset + 1), $results);
    }
}
?>
```


This is how you use it:



```
<?php
$string = 'This is some string';
$search = 'a';
$found = strpos_recursive($string, $search);

if($found) {
    foreach($found as $pos) {
        echo 'Found "'.$search.'" in string "'.$string.'" at position <b>'.$pos.'</b><br />';
    }    
} else {
    echo '"'.$search.'" not found in "'.$string.'"';
}
?>
```
  

---

It is interesting to be aware of the behavior when the treatment of strings with characters using different encodings.<br><br>

```
<?php
# Works like expected. There is no accent
var_dump(strpos("Fabio", 'b'));
#int(2)

# The "&#xE1;" letter is occupying two positions
var_dump(strpos("F&#xE1;bio", 'b')) ;
#int(3)

# Now, encoding the string "F&#xE1;bio" to utf8, we get some "unexpected" outputs. Every letter that is no in regular ASCII table, will use 4 positions(bytes). The starting point remains like before.
# We cant find the characted, because the haystack string is now encoded.
var_dump(strpos(utf8_encode("F&#xE1;bio"), '&#xE1;'));
#bool(false)

# To get the expected result, we need to encode the needle too
var_dump(strpos(utf8_encode("F&#xE1;bio"), utf8_encode('&#xE1;')));
#int(1) 

# And, like said before, "&#xE1;" occupies 4 positions(bytes)
var_dump(strpos(utf8_encode("F&#xE1;bio"), 'b'));
#int(5)?>
```
  

---

I lost an hour before I noticed that strpos only returns FALSE as a boolean, never TRUE.. This means that<br><br>strpos() !== false <br><br>is a different beast then:<br><br>strpos() === true<br><br>since the latter will never be true. After I found out, The warning in the documentation made a lot more sense.  

---

when you want to know how much of substring occurrences, you&apos;ll use "substr_count".<br>But, retrieve their positions, will be harder.<br>So, you can do it by starting with the last occurrence :<br><br>function strpos_r($haystack, $needle)<br>{<br>    if(strlen($needle) &gt; strlen($haystack))<br>        trigger_error(sprintf("%s: length of argument 2 must be &lt;= argument 1", __FUNCTION__), E_USER_WARNING);<br><br>    $seeks = array();<br>    while($seek = strrpos($haystack, $needle))<br>    {<br>        array_push($seeks, $seek);<br>        $haystack = substr($haystack, 0, $seek);<br>    }<br>    return $seeks;<br>}<br><br>it will return an array of all occurrences a the substring in the string<br><br>Example : <br><br>$test = "this is a test for testing a test function... blah blah";<br>var_dump(strpos_r($test, "test"));<br><br>// output <br><br>array(3) {<br>  [0]=&gt;<br>  int(29)<br>  [1]=&gt;<br>  int(19)<br>  [2]=&gt;<br>  int(10)<br>}<br><br>Paul-antoine<br>Mal&#xE9;zieux.  

---

My version of strpos with needles as an array. Also allows for a string, or an array inside an array.<br><br>

```
<?php
function strpos_array($haystack, $needles) {
    if ( is_array($needles) ) {
        foreach ($needles as $str) {
            if ( is_array($str) ) {
                $pos = strpos_array($haystack, $str);
            } else {
                $pos = strpos($haystack, $str);
            }
            if ($pos !== FALSE) {
                return $pos;
            }
        }
    } else {
        return strpos($haystack, $needles);
    }
}

// Test
echo strpos_array('This is a test', array('test', 'drive')); // Output is 10

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.strpos.php)

**[To root](/README.md)**