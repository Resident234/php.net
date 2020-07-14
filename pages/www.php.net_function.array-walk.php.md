# array_walk



It&apos;s worth nothing that array_walk can not be used to change keys in the array.<br>The function may be defined as (&amp;$value, $key) but not (&amp;$value, &amp;$key).<br>Even though PHP does not complain/warn, it does not modify the key.  

#

Calling an array Walk inside a class <br><br>If the class is static:<br>array_walk($array, array(&apos;self&apos;, &apos;walkFunction&apos;));<br>or<br>array_walk($array, array(&apos;className&apos;, &apos;walkFunction&apos;));<br><br>Otherwise:<br>array_walk($array, array($this, &apos;walkFunction&apos;));  

#

I noticed that :<br><br>PHP ignored arguments type when using array_walk() even if there was<br> <br>declare(strict_types=1) . <br><br>See this code as an example ...<br><br>

```
<?php
declare(strict_types=1);

$fruits = array("butter" =&gt; 5.3, "meat" =&gt; 7, "banana" =&gt; 3);

function test_print(int $item2, $key) {
    echo "$key: $item2&lt;br /&gt;\n";
}

array_walk($fruits, &apos;test_print&apos;);

?>
```
<br><br>The output is :<br><br>butter: 5<br>meat: 7<br>banana: 3<br><br>whilst the expecting output is :<br><br>Fatal error: Uncaught TypeError: Argument 1 passed to test_print() must be of the type integer<br><br>because "butter" =&gt; 5.3 is float<br><br>I asked someone about it and they said "this was caused by the fact that callbacks called from internal code will always use weak type". But I tried to do some tests and this behavior is not an issue when using call_user_func().  

#

// We can make that with this simple FOREACH loop : <br><br>$fruits = array("d" =&gt; "lemon", "a" =&gt; "orange", "b" =&gt; "banana", "c" =&gt; "apple");<br><br>foreach($fruits as $cls =&gt; $vls)<br>{<br>  $fruits[$cls] = "fruit: ".$vls;<br>}<br><br>Results: <br><br>Array<br>(<br>    [d] =&gt; fruit: lemon<br>    [a] =&gt; fruit: orange<br>    [b] =&gt; fruit: banana<br>    [c] =&gt; fruit: apple<br>)  

#

Note that using array_walk with intval is inappropriate.<br>There are many examples on internet that suggest to use following code to safely escape $_POST arrays of integers:<br>

```
<?php
array_walk($_POST[&apos;something&apos;],&apos;intval&apos;); // does nothing in PHP 5.3.3
?>
```

It works in _some_ older PHP versions (5.2), but is against specifications. Since intval() does not modify it&apos;s arguments, but returns modified result, the code above has no effect on the array and will leave security hole in your website.

You can use following instead:


```
<?php
$_POST[&apos;something&apos;] = array_map(intval,$_POST[&apos;something&apos;]);
?>
```
  

#

It can be very useful to pass the third (optional) parameter by reference while modifying it permanently in callback function. This will cause passing modified parameter to next iteration of array_walk(). The exaple below enumerates items in the array:<br><br>

```
<?php
function enumerate( &amp;$item1, $key, &amp;$startNum ) {
   $item1 = $startNum++ ." $item1";
}

$num = 1;

$fruits = array( "lemon", "orange", "banana", "apple");
array_walk($fruits, &apos;enumerate&apos;, $num );

print_r( $fruits );

echo &apos;$num is: &apos;. $num ."\n";
?>
```


This outputs:

Array
(
    [0] =&gt; 1 lemon
    [1] =&gt; 2 orange
    [2] =&gt; 3 banana
    [3] =&gt; 4 apple
)
$num is: 1

Notice at the last line of output that outside of array_walk() the $num parameter has initial value of 1. This is because array_walk() does not take the third parameter by reference.. so what if we pass the reference as the optional parameter..



```
<?php
$num = 1;

$fruits = array( "lemon", "orange", "banana", "apple");
array_walk($fruits, &apos;enumerate&apos;, &amp;$num ); // reference here

print_r( $fruits );

echo &apos;$num is: &apos;. $num ."\n";
echo "we&apos;ve got ". ($num - 1) ." fruits in the basket!";
?>
```
<br> <br>This outputs:<br>Array<br>(<br>    [0] =&gt; 1 lemon<br>    [1] =&gt; 2 orange<br>    [2] =&gt; 3 banana<br>    [3] =&gt; 4 apple<br>)<br>$num is: 5<br>we&apos;ve got 4 fruits in the basket!<br><br>Now $num has changed so we are able to count the items (without calling count() unnecessarily).<br><br>As a conclusion, using references with array_walk() can be powerful toy but this should be done carefully since modifying third parameter outside the array_walk() is not always what we want.  

#

If you want to unset elements from the callback function, maybe what you really need is array_filter.  

#

Don&apos;t forget about the array_map() function, it may be easier to use!<br><br>Here&apos;s how to lower-case all elements in an array:<br><br>

```
<?php
    $arr = array_map(&apos;strtolower&apos;, $arr);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-walk.php)

**[To root](/README.md)**