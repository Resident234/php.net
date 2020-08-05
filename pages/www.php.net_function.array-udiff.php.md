# array_udiff



I think the example given here using classes is convoluting things too much to demonstrate what this function does.<br><br>array_udiff() will walk through array_values($a) and array_values($b) and compare each value by using the passed in callback function.<br><br>To put it another way, array_udiff() compares $a[0] to $b[0], $b[1], $b[2], and $b[3] using the provided callback function.  If the callback returns zero for any of the comparisons then $a[0] will not be in the returned array from array_udiff().  It then compares $a[1] to $b[0], $b[1], $b[2], and $b[3].  Then, finally, $a[2] to $b[0], $b[1], $b[2], and $b[3].<br><br>For example, compare_ids($a[0], $b[0]) === -5 while compare_ids($a[1], $b[1]) === 0.  Therefore, $a[1] is not returned from array_udiff() since it is present in $b.<br><br>

```
<?php
$a = array(
        array(
                'id' => 10,
                'name' => 'John',
                'color' => 'red',
        ),
        array(
                'id' => 20,
                'name' => 'Elise',
                'color' => 'blue',
        ),
        array(
                'id' => 30,
                'name' => 'Mark',
                'color' => 'red',
        ),
);

$b = array(
        array(
                'id' => 15,
                'name' => 'Nancy',
                'color' => 'black',
        ),
        array(
                'id' => 20,
                'name' => 'Elise',
                'color' => 'blue',
        ),
        array(
                'id' => 30,
                'name' => 'Mark',
                'color' => 'red',
        ),
        array(
                'id' => 40,
                'name' => 'John',
                'color' => 'orange',
        ),
);

function compare_ids($a, $b)
{
    return ($a['id'] - $b['id']);
}
function compare_names($a, $b)
{
    return strcmp($a['name'], $b['name']);
}

$ret = array_udiff($a, $b, 'compare_ids');
var_dump($ret);

$ret = array_udiff($b, $a, 'compare_ids');
var_dump($ret);

$ret = array_udiff($a, $b, 'compare_names');
var_dump($ret);
?>
```


Which returns the following.

In the first return we see that $b has no entry in it with an id of 10.


```
<?php
array(1) {
  [0]=>
  array(3) {
    ["id"]=>
    int(10)
    ["name"]=>
    string(4) "John"
    ["color"]=>
    string(3) "red"
  }
}
?>
```


In the second return we see that $a has no entry in it with an id of 15 or 40.


```
<?php
array(2) {
  [0]=>
  array(3) {
    ["id"]=>
    int(15)
    ["name"]=>
    string(5) "Nancy"
    ["color"]=>
    string(5) "black"
  }
  [3]=>
  array(3) {
    ["id"]=>
    int(40)
    ["name"]=>
    string(4) "John"
    ["color"]=>
    string(6) "orange"
  }
}
?>
```


In third return we see that all names in $a are in $b (even though the entry in $b whose name is 'John' is different, the anonymous function is only comparing names).


```
<?php
array(0) {
}
?>
```
  

---

Note that the compare function is used also internally, to order the arrays and choose which element compare against in the next round.<br><br>If your compare function is not really comparing (ie. returns 0 if elements are equals, 1 otherwise), you will receive an unexpected result.  

---

It is not stated, by this function also diffs array1 to itself, removing any duplicate values...  

---

Quick example for using array_udiff to do a multi-dimensional diff<br><br>Returns values of $arr1 that are not in $arr2<br><br>

```
<?php
$arr1 = array( array('Bob', 42), array('Phil', 37), array('Frank', 39) );
        
$arr2 = array( array('Phil', 37), array('Mark', 45) );
        
$arr3 = array_udiff($arr1, $arr2, create_function(
    '$a,$b',
    'return strcmp( implode("", $a), implode("", $b) ); ')
    );
        
print_r($arr3);
?>
```
<br><br>Output:<br><br>Array<br>(<br>    [0] =&gt; Array<br>        (<br>            [0] =&gt; Bob<br>            [1] =&gt; 42<br>        )<br> <br>    [2] =&gt; Array<br>        (<br>            [0] =&gt; Frank<br>            [1] =&gt; 39<br>        )<br> <br>)<br>1<br><br>Hope this helps someone  

---

[Official documentation page](https://www.php.net/manual/en/function.array-udiff.php)

**[To root](/README.md)**