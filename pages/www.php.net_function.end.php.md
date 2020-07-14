# end



It&apos;s interesting to note that when creating an array with numeric keys in no particular order, end() will still only return the value that was the last one to be created. So, if you have something like this:<br><br>    

```
<?php
    $a = array();
    $a[1] = 1;
    $a[0] = 0;
    echo end($a);
    ?>
```
<br><br>This will print "0".  

#

If you need to get a reference on the first or last element of an array, use these functions because reset() and end() only return you a copy that you cannot dereference directly:<br><br>

```
<?php
function first(&amp;$array) {
if (!is_array($array)) return &amp;$array;
if (!count($array)) return null;
reset($array);
return &amp;$array[key($array)];
}

function last(&amp;$array) {
if (!is_array($array)) return &amp;$array;
if (!count($array)) return null;
end($array);
return &amp;$array[key($array)];
}
?>
```
  

#

This function returns the value at the end of the array, but you may sometimes be interested in the key at the end of the array, particularly when working with non integer indexed arrays:<br><br>

```
<?php
// Returns the key at the end of the array
function endKey($array){
 end($array);
 return key($array);
}
?>
```


Usage example:


```
<?php
$a = array("one" =&gt; "apple", "two" =&gt; "orange", "three" =&gt; "pear");
echo endKey($a); // will output "three"
?>
```
  

#

If all you want is the last item of the array without affecting the internal array pointer just do the following:<br><br>

```
<?php

function endc( $array ) { return end( $array ); }

$items = array( &apos;one&apos;, &apos;two&apos;, &apos;three&apos; );
$lastItem = endc( $items ); // three
$current = current( $items ); // one
?>
```
<br><br>This works because the parameter to the function is being sent as a copy, not as a reference to the original variable.  

#

[Official documentation page](https://www.php.net/manual/en/function.end.php)

**[To root](/README.md)**