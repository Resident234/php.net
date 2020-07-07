# What References Do





I ran into something when using an expanded version of the example of pbaltz at NO_SPAM dot cs dot NO_SPAM dot wisc dot edu below.
This could be somewhat confusing although it is perfectly clear if you have read the manual carfully. It makes the fact that references always point to the content of a variable perfectly clear (at least to me).



```
<?php
$a = 1;
$c = 2;
$b =&amp; $a; // $b points to 1
$a =&amp; $c; // $a points now to 2, but $b still to 1;
echo $a, &quot; &quot;, $b;
// Output: 2 1
?>
```



  

#



Watch out for this:

foreach ($somearray as &amp;$i) {
&#xA0; // update some $i...
}
...
foreach ($somearray as $i) {
&#xA0; // last element of $somearray is mysteriously overwritten!
}

Problem is $i contians reference to last element of $somearray after the first foreach, and the second foreach happily assigns to it!

  

#



It appears that references can have side-effects.&#xA0; Below are two examples.&#xA0; Both are simply copying one array to another.&#xA0; In the second example, a reference is made to a value in the first array before the copy.&#xA0; In the first example the value at index 0 points to two separate memory locations. In the second example, the value at index 0 points to the same memory location. 

I won&apos;t say this is a bug, because I don&apos;t know what the designed behavior of PHP is, but I don&apos;t think ANY developers would expect this behavior, so look out.

An example of where this could cause problems is if you do an array copy in a script and expect on type of behavior, but then later add a reference to a value in the array earlier in the script, and then find that the array copy behavior has unexpectedly changed.



```
<?php
// Example one
$arr1 = array(1);
echo &quot;\nbefore:\n&quot;;
echo &quot;\$arr1[0] == {$arr1[0]}\n&quot;;
$arr2 = $arr1;
$arr2[0]++;
echo &quot;\nafter:\n&quot;;
echo &quot;\$arr1[0] == {$arr1[0]}\n&quot;;
echo &quot;\$arr2[0] == {$arr2[0]}\n&quot;;

// Example two
$arr3 = array(1);
$a =&amp; $arr3[0];
echo &quot;\nbefore:\n&quot;;
echo &quot;\$a == $a\n&quot;;
echo &quot;\$arr3[0] == {$arr3[0]}\n&quot;;
$arr4 = $arr3;
$arr4[0]++;
echo &quot;\nafter:\n&quot;;
echo &quot;\$a == $a\n&quot;;
echo &quot;\$arr3[0] == {$arr3[0]}\n&quot;;
echo &quot;\$arr4[0] == {$arr4[0]}\n&quot;;
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/language.references.whatdo.php)

**[To root](/README.md)**