# in_array



Loose checking returns some crazy, counter-intuitive results when used with certain arrays. It is completely correct behaviour, due to PHP&apos;s leniency on variable types, but in "real-life" is almost useless.<br><br>The solution is to use the strict checking option.<br><br>

```
<?php

// Example array

$array = array(
    &apos;egg&apos; =&gt; true,
    &apos;cheese&apos; =&gt; false,
    &apos;hair&apos; =&gt; 765,
    &apos;goblins&apos; =&gt; null,
    &apos;ogres&apos; =&gt; &apos;no ogres allowed in this array&apos;
);

// Loose checking -- return values are in comments

// First three make sense, last four do not

in_array(null, $array); // true
in_array(false, $array); // true
in_array(765, $array); // true
in_array(763, $array); // true
in_array(&apos;egg&apos;, $array); // true
in_array(&apos;hhh&apos;, $array); // true
in_array(array(), $array); // true

// Strict checking

in_array(null, $array, true); // true
in_array(false, $array, true); // true
in_array(765, $array, true); // true
in_array(763, $array, true); // false
in_array(&apos;egg&apos;, $array, true); // false
in_array(&apos;hhh&apos;, $array, true); // false
in_array(array(), $array, true); // false

?>
```
  

#

If you&apos;re working with very large 2 dimensional arrays (eg 20,000+ elements) it&apos;s much faster to do this...<br><br>

```
<?php
$needle = &apos;test for this&apos;;

$flipped_haystack = array_flip($haystack);

if ( isset($flipped_haystack[$needle]) )
{
  print "Yes it&apos;s there!";
}
?>
```
<br><br>I had a script that went from 30+ seconds down to 2 seconds (when hunting through a 50,000 element array 50,000 times).<br><br>Remember to only flip it once at the beginning of your code though!  

#



```
<?php<br><br>class Method {<br><br>    /** @var int current number of inMultiArray() loop */<br>    private static $currentMultiArrayExec = 0;<br><br>    /**<br>     * Checks if an element is found in an array or one of its subarray.<br>     * The verification layer of this method is limited to the parameter boundary xdebug.max_nesting_level of your php.ini.<br>     * @param mixed $element<br>     * @param array $array<br>     * @param bool $strict<br>     * @return bool<br>     */<br>    public static function inMultiArray($element, array $array, bool $strict = true) : bool {<br>        self::$currentMultiArrayExec++;<br><br>        if(self::$currentMultiArrayExec &gt;= ini_get("xdebug.max_nesting_level")) return false;<br><br>        foreach($array as $key =&gt; $value){<br>            $bool = $strict ? $element === $key : $element == $key;<br><br>            if($bool) return true;<br><br>            if(is_array($value)){<br>                $bool = self::inMultiArray($element, $value, $strict);<br>            } else {<br>                $bool = $strict ? $element === $value : $element == $value;<br>            }<br><br>            if($bool) return true;<br>        }<br><br>        self::$currentMultiArrayExec = 0;<br>        return isset($bool) ? $bool : false;<br>    }<br>}<br><br>$array = array("foo" =&gt; array("foo2", "bar"));<br>$search = "foo";<br><br>if(Method::inMultiArray($search, $array, false)){<br>    echo $search . " it is found in the array or one of its sub array.";<br>} else {<br>    echo $search . " was not found.";<br>}<br><br># foo it is found in the array or one of its sub array.  

#

For a case-insensitive in_array(), you can use array_map() to avoid a foreach statement, e.g.:<br><br>

```
<?php
    function in_arrayi($needle, $haystack) {
        return in_array(strtolower($needle), array_map(&apos;strtolower&apos;, $haystack));
    }
?>
```
  

#

Determine whether an object field matches needle.<br><br>Usage Example:<br>---------------<br><br>

```
<?php
$arr = array( new stdClass(), new stdClass() );
$arr[0]-&gt;colour = &apos;red&apos;;
$arr[1]-&gt;colour = &apos;green&apos;;
$arr[1]-&gt;state  = &apos;enabled&apos;;

if (in_array_field(&apos;red&apos;, &apos;colour&apos;, $arr))
   echo &apos;Item exists with colour red.&apos;;
if (in_array_field(&apos;magenta&apos;, &apos;colour&apos;, $arr))
   echo &apos;Item exists with colour magenta.&apos;;
if (in_array_field(&apos;enabled&apos;, &apos;state&apos;, $arr))
   echo &apos;Item exists with enabled state.&apos;;
?>
```


Output:
--------
Item exists with colour red.
Item exists with enabled state.



```
<?php
function in_array_field($needle, $needle_field, $haystack, $strict = false) {
    if ($strict) {
        foreach ($haystack as $item)
            if (isset($item-&gt;$needle_field) &amp;&amp; $item-&gt;$needle_field === $needle)
                return true;
    }
    else {
        foreach ($haystack as $item)
            if (isset($item-&gt;$needle_field) &amp;&amp; $item-&gt;$needle_field == $needle)
                return true;
    }
    return false;
}
?>
```
  

#

If you found yourself in need of a multidimensional array in_array like function you can use the one below. Works in a fair amount of time<br><br>

```
<?php

    function in_multiarray($elem, $array)
    {
        $top = sizeof($array) - 1;
        $bottom = 0;
        while($bottom &lt;= $top)
        {
            if($array[$bottom] == $elem)
                return true;
            else 
                if(is_array($array[$bottom]))
                    if(in_multiarray($elem, ($array[$bottom])))
                        return true;
                    
            $bottom++;
        }        
        return false;
    }
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.in-array.php)

**[To root](/README.md)**