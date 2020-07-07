# foreach





You can also use the alternative syntax for the foreach cycle:





```
<?php

foreach($array as $element):

&#xA0; #do something

endforeach;

?>
```




Just thought it worth mentioning.

  

#



&quot;Reference of a $value and the last array element remain even after the foreach loop. It is recommended to destroy it by unset().&quot;

I cannot stress this point of the documentation enough! Here is a simple example of exactly why this must be done:



```
<?php
$arr1 = array(&quot;a&quot; =&gt; 1, &quot;b&quot; =&gt; 2, &quot;c&quot; =&gt; 3);
$arr2 = array(&quot;x&quot; =&gt; 4, &quot;y&quot; =&gt; 5, &quot;z&quot; =&gt; 6);

foreach ($arr1 as $key =&gt; &amp;$val) {}
foreach ($arr2 as $key =&gt; $val) {}

var_dump($arr1);
var_dump($arr2);
?>
```


The output is:
array(3) { [&quot;a&quot;]=&gt; int(1) [&quot;b&quot;]=&gt; int(2) [&quot;c&quot;]=&gt; &amp;int(6) }
array(3) { [&quot;x&quot;]=&gt; int(4) [&quot;y&quot;]=&gt; int(5) [&quot;z&quot;]=&gt; int(6) }

Notice how the last index in $arr1 is now the value from the last index in $arr2!

  

#



Even though it is not mentioned in this article, you can use &quot;break&quot; control structure to exit from the &quot;foreach&quot; loop.



```
<?php

$array = [ &apos;one&apos;, &apos;two&apos;, &apos;three&apos;, &apos;four&apos;, &apos;five&apos; ];

foreach( $array as $value ){
&#xA0; &#xA0; if( $value == &apos;three&apos; ){
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;Number three was found!&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; }
}

?>
```



  

#



foreach and the while/list/each methods are not completely identical, and there are occasions where one way is beneficial over the other.





```
<?php

$arr = array(1,2,3,4,5,6,7,8,9);



foreach($arr as $key=&gt;$value)

{

&#xA0; &#xA0; unset($arr[$key + 1]);

&#xA0; &#xA0; echo $value . PHP_EOL;

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

&#xA0; &#xA0; unset($arr[$key + 1]);

&#xA0; &#xA0; echo $value . PHP_EOL;

}

?>
```


Output:

1 3 5 7 9





[EDIT BY danbrown AT php DOT net: Contains a typofix by (scissor AT phplabs DOT pl) on 30-JAN-2009.]

  

#



WARNING: Looping through &quot;values by reference&quot; for &quot;extra performance&quot; is an old myth. It&apos;s actually WORSE!



```
<?php

function one($arr) {
&#xA0; &#xA0; foreach($arr as $val) { // Normal Variable
&#xA0; &#xA0; &#xA0; &#xA0; echo $val;
&#xA0; &#xA0; }
}

function two($arr) {
&#xA0; &#xA0; foreach($arr as &amp;$val) { // Reference To Value
&#xA0; &#xA0; &#xA0; &#xA0; echo $val;
&#xA0; &#xA0; }
}

$a = array( &apos;a&apos;, &apos;b&apos;, &apos;c&apos; );
one($a);
two($a);

?>
```


Which do you think is faster?

Lots of people think the answer is two() because it uses &quot;reference to value, which it doesn&apos;t have to copy each value when it loops&quot;.

Well, that&apos;s totally wrong!

Here&apos;s what actually happens:

* one():

- This function takes an array as argument ($arr).
- The array function argument itself isn&apos;t passed by reference, so the function knows it isn&apos;t allowed to modify the original at all.
- Then the foreach loop happens. The array itself wasn&apos;t passed by reference to the function, so PHP knows that it isn&apos;t allowed to modify the outside array, so it therefore makes a copy of the array&apos;s internal iteration offset state (that&apos;s just a simple number which says which item you are currently at during things like foreach()), which costs almost no performance or memory at all since it&apos;s just a small number.
- Next, it uses that copied iteration offset to loop through all key/value pairs of the array (ie 0th key, 1st key, 2nd key, etc...). And the value at the current offset (a PHP &quot;zval&quot;) is assigned to a variable called $val.
- Does $val make a COPY of the value? That&apos;s what MANY people think. But the answer is NO. It DOESN&apos;T. It re-uses the existing value in memory. With zero performance cost. It&apos;s called &quot;copy-on-write&quot; and means that PHP doesn&apos;t make any copies unless you try to MODIFY the value.
- If you try to MODIFY $val, THEN it will allocate a NEW zval in memory and store $val there instead (but it still won&apos;t modify the original array, so you can rest assured).

Alright, so what&apos;s the second version doing? The beloved &quot;iterate values by reference&quot;?

* two():

- This function takes an array as argument ($arr).
- The array function argument itself isn&apos;t passed by reference, so the function knows it isn&apos;t allowed to modify the original at all.
- Then the foreach loop happens. The array itself wasn&apos;t passed by reference to the function, so PHP knows that it isn&apos;t allowed to modify the outside array.
- But it also sees that you want to look at all VALUES by reference (&amp;$val), so PHP says &quot;Uh oh, this is dangerous. If we just give them references to the original array&apos;s values, and they assign some new value to their reference, they would destroy the original array which they aren&apos;t allowed to touch!&quot;.
- So PHP makes a FULL COPY of the ENTIRE array and ALL VALUES before it starts iterating. YIKES!

Therefore: STOP using the old, mythological &quot;&amp;$val&quot; iteration method! It&apos;s almost always BAD! With worse performance, and risks of bugs and quirks as is demonstrated in the manual.

You can always manually write array assignments explicitly, without references, like this:



```
<?php

$a = array(1, 2, 3);
foreach($a as $key =&gt; $val) {
&#xA0;&#xA0; $a[$key] = $val * 10;
}
// $a is now [10, 20, 30]

?>
```


The main lesson is this: DON&apos;T blindly iterate through values by reference! Telling PHP that you want direct references will force PHP to need to copy the WHOLE array to protect its original values! So instead, just loop normally and trust the fact that PHP *is* actually smart enough to never copy your original array&apos;s values! PHP uses &quot;copy-on-write&quot;, which means that attempting to assign something new to $val is the ONLY thing that causes a copying, and only of that SINGLE element! :-) But you never do that anyway, when iterating without reference. If you ever want to modify something, you use the &quot;$a[$key] = 123;&quot; method of updating the value.

Enjoy and good luck with your code! :-)

  

#



in foreach if you want to iterate through a specific column in a nested arrays for example:

$arr = array(
&#xA0; &#xA0;&#xA0; [1, 2, 3,&#xA0;&#xA0; 4],
&#xA0; &#xA0;&#xA0; [14, 6, 7,&#xA0; 6],
&#xA0; &#xA0;&#xA0; [10, 2 ,3 , 2],
);

when we want to iterate on the third column we can use:

foreach( $arr as list( , , $a)) {
&#xA0; &#xA0; echo &quot;$a\n&quot;;
}

this will print:
3
7
3

  

#



If you want to use the list for multidimension arrays, you can nest several lists:



```
<?php
$array = [
&#xA0; &#xA0; [1, 2, array(3, 4)],
&#xA0; &#xA0; [3, 4, array(5, 6)],
];

foreach ($array as list($a, $b, list($c, $d))) {
&#xA0; &#xA0; echo &quot;A: $a; B: $b; C: $c; D: $d;&lt;br&gt;&quot;;
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
&#xA0; &#xA0; [1, 2, array(3, array(4, 5))],
&#xA0; &#xA0; [3, 4, array(5, array(6, 7))],
];

foreach ($array as list($a, $b, list($c, list($d, $e)))) {
&#xA0; &#xA0; echo &quot;A: $a; B: $b; C: $c; D: $d; E: $e;&lt;br&gt;&quot;;
};
Will output:
A: 1; B: 2; C: 3; D: 4; E: 5;
A: 3; B: 4; C: 5; D: 6; E: 7;
?>
```



  

#



What happened to this note:
&quot;Unless the array is referenced, foreach operates on a copy of the specified array and not the array itself. foreach has some side effects on the array pointer. Don&apos;t rely on the array pointer during or after the foreach without resetting it.&quot;

Is this no longer the case?
It seems only to remain in the Serbian documentation: http://php.net/manual/sr/control-structures.foreach.php

  

#



For those who&apos;d like to traverse an array including just added elements (within this very foreach), here&apos;s a workaround:





```
<?php

$values = array(1 =&gt; &apos;a&apos;, 2 =&gt; &apos;b&apos;, 3 =&gt; &apos;c&apos;);

while (list($key, $value) = each($values)) {

&#xA0; &#xA0; echo &quot;$key =&gt; $value \r\n&quot;;

&#xA0; &#xA0; if ($key == 3) {

&#xA0; &#xA0; &#xA0; &#xA0; $values[4] = &apos;d&apos;; 

&#xA0; &#xA0; }

&#xA0; &#xA0; if ($key == 4) {

&#xA0; &#xA0; &#xA0; &#xA0; $values[5] = &apos;e&apos;;

&#xA0; &#xA0; }

}

?>
```




the code above will output:



1 =&gt; a

2 =&gt; b

3 =&gt; c

4 =&gt; d

5 =&gt; e

  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.foreach.php)

**[To root](/README.md)**