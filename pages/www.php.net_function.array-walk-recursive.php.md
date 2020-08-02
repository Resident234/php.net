# array_walk_recursive



Since this is only mentioned in the footnote of the output of one of the examples, I feel it should be spelled out:<br><br>* THIS FUNCTION ONLY VISITS LEAF NODES *<br><br>That is to say that if you have a tree of arrays with subarrays of subarrays, only the plain values at the leaves of the tree will be visited by the callback function.  The callback function isn&apos;t ever called for a nodes in the tree that subnodes (i.e., a subarray).  This has the effect as to make this function unusable for most practical situations.  

---

If you are wanting to change the values of an existing multi-dimensional array, as it says above in the note, you need to specify the first argument as a reference. All that means is, be sure to precede the $item variable with an ampersand (&amp;) as in the good_example below. <br><br>Unfortunately the PHP example given doesn&apos;t do this. It actually took me a while to figure out why my function wasn&apos;t changing the original array, even though I was passing by reference. <br><br>Here&apos;s the tip: Don&apos;t return any value from the function! Just change the value of $item that you passed in by reference. This is rather counter-intuitive since the vast majority of functions return a value.<br><br>

```
<?php
// array_walk_recursive fails to change your array unless you pass by reference.
// Don't return values from your filter function, even though it's quite logical at a glance!
function bad_example($item,$key){
   if($key=='test'){
       return 'PHP Rocks';  // Don't do it
   }else{
      return $item;  // Don't do this either
   }
}

// array_walk_recursive pass-by-reference example
function good_example(&amp;$item,$key){
   if($key=='test'){
        $item='PHP Rocks'; // Do This!
   }
}

$arr = array('a'=>'1','b'=>'2','test'=>'Replace This');

array_walk_recursive($arr,'bad_example');
var_dump($arr);
/**
 * no errors, but prints...
 * array('a'=>'1','b'=>'2','test'=>'Replace This');
 */

array_walk_recursive($arr,'good_example');
var_dump($arr);
/**
 * prints...
 * array('a'=>'1','b'=>'2','test'=>'PHP Rocks');
 */

?>
```


Returning a value from your function does work if you pass by reference and modify $item before you return, but you will eat up memory very, very fast if you try it, even on an example as small as the one here.

One other silly thing you might try first is something like this:



```
<?php
// Resist the urge to do this, it doesn't work.
$filtered = array_walk_recursive($unfiltered,'filter_function');
?>
```
<br><br>Of course, $filtered is just TRUE afterwards, not the filtered results you were wanting. Oh, it ran your function recursively alright, but changed all the values in the local function scope only and returns a boolean as the documentation states.  

---

I use RecursiveIteratorIterator with parameter CATCH_GET_CHILD to iterate on leafs AND nodes instead of array_walk_recursive function :<br><br>

```
<?php
// Iteration on leafs AND nodes
foreach (new RecursiveIteratorIterator(new RecursiveArrayIterator($candidate), RecursiveIteratorIterator::CATCH_GET_CHILD) as $key => $value) {
    echo 'My node ' . $key . ' with value ' . $value . PHP_EOL;
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.array-walk-recursive.php)

**[To root](/README.md)**