# in_array



Loose checking returns some crazy, counter-intuitive results when used with certain arrays. It is completely correct behaviour, due to PHP&apos;s leniency on variable types, but in "real-life" is almost useless.<br><br>The solution is to use the strict checking option.<br><br>

```
<?php

// Example array

$array = array(
    'egg' => true,
    'cheese' => false,
    'hair' => 765,
    'goblins' => null,
    'ogres' => 'no ogres allowed in this array'
);

// Loose checking -- return values are in comments

// First three make sense, last four do not

in_array(null, $array); // true
in_array(false, $array); // true
in_array(765, $array); // true
in_array(763, $array); // true
in_array('egg', $array); // true
in_array('hhh', $array); // true
in_array(array(), $array); // true

// Strict checking

in_array(null, $array, true); // true
in_array(false, $array, true); // true
in_array(765, $array, true); // true
in_array(763, $array, true); // false
in_array('egg', $array, true); // false
in_array('hhh', $array, true); // false
in_array(array(), $array, true); // false

?>
```
  

---

If you&apos;re working with very large 2 dimensional arrays (eg 20,000+ elements) it&apos;s much faster to do this...<br><br>

```
<?php
$needle = 'test for this';

$flipped_haystack = array_flip($haystack);

if ( isset($flipped_haystack[$needle]) )
{
  print "Yes it's there!";
}
?>
```
<br><br>I had a script that went from 30+ seconds down to 2 seconds (when hunting through a 50,000 element array 50,000 times).<br><br>Remember to only flip it once at the beginning of your code though!  

---



```
<?php

class Method {

    /** @var int current number of inMultiArray() loop */
    private static $currentMultiArrayExec = 0;

    /**
     * Checks if an element is found in an array or one of its subarray.
     * The verification layer of this method is limited to the parameter boundary xdebug.max_nesting_level of your php.ini.
     * @param mixed $element
     * @param array $array
     * @param bool $strict
     * @return bool
     */
    public static function inMultiArray($element, array $array, bool $strict = true) : bool {
        self::$currentMultiArrayExec++;

        if(self::$currentMultiArrayExec >= ini_get("xdebug.max_nesting_level")) return false;

        foreach($array as $key => $value){
            $bool = $strict ? $element === $key : $element == $key;

            if($bool) return true;

            if(is_array($value)){
                $bool = self::inMultiArray($element, $value, $strict);
            } else {
                $bool = $strict ? $element === $value : $element == $value;
            }

            if($bool) return true;
        }

        self::$currentMultiArrayExec = 0;
        return isset($bool) ? $bool : false;
    }
}

$array = array("foo" => array("foo2", "bar"));
$search = "foo";

if(Method::inMultiArray($search, $array, false)){
    echo $search . " it is found in the array or one of its sub array.";
} else {
    echo $search . " was not found.";
}

# foo it is found in the array or one of its sub array.?>
```
  

---

For a case-insensitive in_array(), you can use array_map() to avoid a foreach statement, e.g.:<br><br>

```
<?php
    function in_arrayi($needle, $haystack) {
        return in_array(strtolower($needle), array_map('strtolower', $haystack));
    }
?>
```
  

---

Determine whether an object field matches needle.<br><br>Usage Example:<br>---------------<br><br>

```
<?php
$arr = array( new stdClass(), new stdClass() );
$arr[0]->colour = 'red';
$arr[1]->colour = 'green';
$arr[1]->state  = 'enabled';

if (in_array_field('red', 'colour', $arr))
   echo 'Item exists with colour red.';
if (in_array_field('magenta', 'colour', $arr))
   echo 'Item exists with colour magenta.';
if (in_array_field('enabled', 'state', $arr))
   echo 'Item exists with enabled state.';
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
            if (isset($item->$needle_field) &amp;&amp; $item->$needle_field === $needle)
                return true;
    }
    else {
        foreach ($haystack as $item)
            if (isset($item->$needle_field) &amp;&amp; $item->$needle_field == $needle)
                return true;
    }
    return false;
}
?>
```
  

---

If you found yourself in need of a multidimensional array in_array like function you can use the one below. Works in a fair amount of time<br><br>

```
<?php

    function in_multiarray($elem, $array)
    {
        $top = sizeof($array) - 1;
        $bottom = 0;
        while($bottom <= $top)
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
  

---

[Official documentation page](https://www.php.net/manual/en/function.in-array.php)

**[To root](/README.md)**