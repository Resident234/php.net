# foreach



You can also use the alternative syntax for the foreach cycle:<br><br>

```
<?php
foreach($array as $element):
  #do something
endforeach;
?>
```
<br><br>Just thought it worth mentioning.  

#

"Reference of a $value and the last array element remain even after the foreach loop. It is recommended to destroy it by unset()."<br><br>I cannot stress this point of the documentation enough! Here is a simple example of exactly why this must be done:<br><br>

```
<?php
$arr1 = array("a" => 1, "b" => 2, "c" => 3);
$arr2 = array("x" => 4, "y" => 5, "z" => 6);

foreach ($arr1 as $key => &amp;$val) {}
foreach ($arr2 as $key => $val) {}

var_dump($arr1);
var_dump($arr2);
?>
```
<br><br>The output is:<br>array(3) { ["a"]=&gt; int(1) ["b"]=&gt; int(2) ["c"]=&gt; &amp;int(6) }<br>array(3) { ["x"]=&gt; int(4) ["y"]=&gt; int(5) ["z"]=&gt; int(6) }<br><br>Notice how the last index in $arr1 is now the value from the last index in $arr2!  

#

Even though it is not mentioned in this article, you can use "break" control structure to exit from the "foreach" loop.<br><br>

```
<?php

$array = [ 'one', 'two', 'three', 'four', 'five' ];

foreach( $array as $value ){
    if( $value == 'three' ){
        echo "Number three was found!";
        break;
    }
}

?>
```
  

#

foreach and the while/list/each methods are not completely identical, and there are occasions where one way is beneficial over the other.<br><br>

```
<?php
$arr = array(1,2,3,4,5,6,7,8,9);

foreach($arr as $key=>$value)
{
    unset($arr[$key + 1]);
    echo $value . PHP_EOL;
}
?>
```

Output:
1 2 3 4 5 6 7 8 9 



```
<?php
$arr = array(1,2,3,4,5,6,7,8,9);

while (list($key, $value) = each($arr))
{
    unset($arr[$key + 1]);
    echo $value . PHP_EOL;
}
?>
```
<br>Output:<br>1 3 5 7 9<br><br><br>[EDIT BY danbrown AT php DOT net: Contains a typofix by (scissor AT phplabs DOT pl) on 30-JAN-2009.]  

#

WARNING: Looping through "values by reference" for "extra performance" is an old myth. It&apos;s actually WORSE!<br><br>

```
<?php

function one($arr) {
    foreach($arr as $val) { // Normal Variable
        echo $val;
    }
}

function two($arr) {
    foreach($arr as &amp;$val) { // Reference To Value
        echo $val;
    }
}

$a = array( 'a', 'b', 'c' );
one($a);
two($a);

?>
```


Which do you think is faster?

Lots of people think the answer is two() because it uses "reference to value, which it doesn't have to copy each value when it loops".

Well, that's totally wrong!

Here's what actually happens:

* one():

- This function takes an array as argument ($arr).
- The array function argument itself isn't passed by reference, so the function knows it isn't allowed to modify the original at all.
- Then the foreach loop happens. The array itself wasn't passed by reference to the function, so PHP knows that it isn't allowed to modify the outside array, so it therefore makes a copy of the array's internal iteration offset state (that's just a simple number which says which item you are currently at during things like foreach()), which costs almost no performance or memory at all since it's just a small number.
- Next, it uses that copied iteration offset to loop through all key/value pairs of the array (ie 0th key, 1st key, 2nd key, etc...). And the value at the current offset (a PHP "zval") is assigned to a variable called $val.
- Does $val make a COPY of the value? That's what MANY people think. But the answer is NO. It DOESN'T. It re-uses the existing value in memory. With zero performance cost. It's called "copy-on-write" and means that PHP doesn't make any copies unless you try to MODIFY the value.
- If you try to MODIFY $val, THEN it will allocate a NEW zval in memory and store $val there instead (but it still won't modify the original array, so you can rest assured).

Alright, so what's the second version doing? The beloved "iterate values by reference"?

* two():

- This function takes an array as argument ($arr).
- The array function argument itself isn't passed by reference, so the function knows it isn't allowed to modify the original at all.
- Then the foreach loop happens. The array itself wasn't passed by reference to the function, so PHP knows that it isn't allowed to modify the outside array.
- But it also sees that you want to look at all VALUES by reference (&amp;$val), so PHP says "Uh oh, this is dangerous. If we just give them references to the original array's values, and they assign some new value to their reference, they would destroy the original array which they aren't allowed to touch!".
- So PHP makes a FULL COPY of the ENTIRE array and ALL VALUES before it starts iterating. YIKES!

Therefore: STOP using the old, mythological "&amp;$val" iteration method! It's almost always BAD! With worse performance, and risks of bugs and quirks as is demonstrated in the manual.

You can always manually write array assignments explicitly, without references, like this:



```
<?php

$a = array(1, 2, 3);
foreach($a as $key => $val) {
   $a[$key] = $val * 10;
}
// $a is now [10, 20, 30]

?>
```
<br><br>The main lesson is this: DON&apos;T blindly iterate through values by reference! Telling PHP that you want direct references will force PHP to need to copy the WHOLE array to protect its original values! So instead, just loop normally and trust the fact that PHP *is* actually smart enough to never copy your original array&apos;s values! PHP uses "copy-on-write", which means that attempting to assign something new to $val is the ONLY thing that causes a copying, and only of that SINGLE element! :-) But you never do that anyway, when iterating without reference. If you ever want to modify something, you use the "$a[$key] = 123;" method of updating the value.<br><br>Enjoy and good luck with your code! :-)  

#

in foreach if you want to iterate through a specific column in a nested arrays for example:<br><br>$arr = array(<br>     [1, 2, 3,   4],<br>     [14, 6, 7,  6],<br>     [10, 2 ,3 , 2],<br>);<br><br>when we want to iterate on the third column we can use:<br><br>foreach( $arr as list( , , $a)) {<br>    echo "$a\n";<br>}<br><br>this will print:<br>3<br>7<br>3  

#

If you want to use the list for multidimension arrays, you can nest several lists:<br><br>

```
<?php
$array = [
    [1, 2, array(3, 4)],
    [3, 4, array(5, 6)],
];

foreach ($array as list($a, $b, list($c, $d))) {
    echo "A: $a; B: $b; C: $c; D: $d;<br>";
};
?>
```


Will output:
A: 1; B: 2; C: 3; D: 4;
A: 3; B: 4; C: 5; D: 6;

And:



```
<?php
$array = [
    [1, 2, array(3, array(4, 5))],
    [3, 4, array(5, array(6, 7))],
];

foreach ($array as list($a, $b, list($c, list($d, $e)))) {
    echo "A: $a; B: $b; C: $c; D: $d; E: $e;<br>";
};
Will output:
A: 1; B: 2; C: 3; D: 4; E: 5;
A: 3; B: 4; C: 5; D: 6; E: 7;
?>
```
  

#

What happened to this note:<br>"Unless the array is referenced, foreach operates on a copy of the specified array and not the array itself. foreach has some side effects on the array pointer. Don&apos;t rely on the array pointer during or after the foreach without resetting it."<br><br>Is this no longer the case?<br>It seems only to remain in the Serbian documentation: http://php.net/manual/sr/control-structures.foreach.php  

#

For those who&apos;d like to traverse an array including just added elements (within this very foreach), here&apos;s a workaround:<br><br>

```
<?php
$values = array(1 => 'a', 2 => 'b', 3 => 'c');
while (list($key, $value) = each($values)) {
    echo "$key => $value \r\n";
    if ($key == 3) {
        $values[4] = 'd'; 
    }
    if ($key == 4) {
        $values[5] = 'e';
    }
}
?>
```
<br><br>the code above will output:<br><br>1 =&gt; a<br>2 =&gt; b<br>3 =&gt; c<br>4 =&gt; d<br>5 =&gt; e  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.foreach.php)

**[To root](/README.md)**