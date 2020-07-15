# array_fill



This is what I recently did to quickly create a two dimensional array (10x10), initialized to 0:<br><br>

```
<?php
  $a = array_fill(0, 10, array_fill(0, 10, 0));
?>
```
<br><br>This should work for as many dimensions as you want, each time passing to array_fill() (as the 3rd argument) another array_fill() function.  

#

If you need negative indices:<br>

```
<?php
$b = array_fill(-2, 4, 'pear');//this is not what we want
$c = array_fill_keys(range(-2,1),'pear');//these are negative indices
print_r($b);
print_r($c);
?>
```
<br>Here is result of the code above:<br>Array<br>(<br>    [-2] =&gt; pear<br>    [0] =&gt; pear<br>    [1] =&gt; pear<br>    [2] =&gt; pear<br>)<br>Array<br>(<br>    [-2] =&gt; pear<br>    [-1] =&gt; pear<br>    [0] =&gt; pear<br>    [1] =&gt; pear<br>)  

#

Using objects with array_fill may cause unexpected results. Consider the following:<br><br>

```
<?php
class Foo {
   public $bar = "banana";
}

//fill an array with objects
$array = array_fill(0, 2, new Foo());

var_dump($array);
/*
array(2) {
  [0]=>
  object(Foo)#1 (1) {
    ["bar"]=>
    string(6) "banana"
  }
  [1]=>
  object(Foo)#1 (1) {
    ["bar"]=>
    string(6) "banana"
  }
} */

//now we change the attribute of the object stored in index 0
//this actually changes the attribute for EACH object in the ENTIRE array
$array[0]->bar = "apple";

var_dump($array);
/*
array(2) {
  [0]=>
  object(Foo)#1 (1) {
    ["bar"]=>
    string(5) "apple"
  }
  [1]=>
  object(Foo)#1 (1) {
    ["bar"]=>
    string(5) "apple"
  }
}
 */
?>
```
<br><br>Objects are filled in the array BY REFERENCE. They are not copied for each element in the array.  

#

[Official documentation page](https://www.php.net/manual/en/function.array-fill.php)

**[To root](/README.md)**