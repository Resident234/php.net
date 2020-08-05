# array_intersect



A clearer example of the key preservation of this function:<br><br>

```
<?php

$array1 = array(2, 4, 6, 8, 10, 12);
$array2 = array(1, 2, 3, 4, 5, 6);

var_dump(array_intersect($array1, $array2));
var_dump(array_intersect($array2, $array1));

?>
```
<br><br>yields the following:<br><br>array(3) {<br>  [0]=&gt; int(2)<br>  [1]=&gt; int(4)<br>  [2]=&gt; int(6)<br>}<br><br>array(3) {<br>  [1]=&gt; int(2)<br>  [3]=&gt; int(4)<br>  [5]=&gt; int(6)<br>}<br><br>This makes it important to remember which way round you passed the arrays to the function if these keys are relied on later in the script.  

---

Here is a array_union($a, $b):<br><br>

```
<?php
                                        //  $a = 1 2 3 4
    $union =                            //  $b =   2   4 5 6
        array_merge(
            array_intersect($a, $b),    //         2   4
            array_diff($a, $b),         //       1   3
            array_diff($b, $a)          //               5 6
        );                              //  $u = 1 2 3 4 5 6
?>
```
  

---

If you need to supply arbitrary number of arguments <br>to array_intersect() or other array function, <br>use following function:<br><br>$full=call_user_func_array(&apos;array_intersect&apos;, $any_number_of_arrays_here);  

---

Note that array_intersect and array_unique doesnt work well with multidimensional arrays.<br>If you have, for example, <br><br>

```
<?php

$orders_today[0] = array('John Doe', 'PHP Book');
$orders_today[1] = array('Jack Smith', 'Coke');

$orders_yesterday[0] = array('Miranda Jones', 'Digital Watch');
$orders_yesterday[1] = array('John Doe', 'PHP Book');
$orders_yesterday[2] = array('Z&#xFFFD; da Silva', 'BMW Car');

?>
```


and wants to know if the same person bought the same thing today and yesterday and use array_intersect($orders_today, $orders_yesterday) you'll get as result:



```
<?php

Array
(
    [0] => Array
        (
            [0] => John Doe
            [1] => PHP Book
        )

    [1] => Array
        (
            [0] => Jack Smith
            [1] => Coke
        )

)

?>
```


but we can get around that by serializing the inner arrays:


```
<?php

$orders_today[0] = serialize(array('John Doe', 'PHP Book'));
$orders_today[1] = serialize(array('Jack Smith', 'Coke'));

$orders_yesterday[0] = serialize(array('Miranda Jones', 'Digital Watch'));
$orders_yesterday[1] = serialize(array('John Doe', 'PHP Book'));
$orders_yesterday[2] = serialize(array('Z&#xFFFD; da Silva', 'Uncle Tungsten'));

?>
```


so that array_map("unserialize", array_intersect($orders_today, $orders_yesterday)) will return:



```
<?php

Array
(
    [0] => Array
        (
            [0] => John Doe
            [1] => PHP Book
        )

)

?>
```
<br><br>showing us who bought the same thing today and yesterday =)<br><br>[]s  

---

[Official documentation page](https://www.php.net/manual/en/function.array-intersect.php)

**[To root](/README.md)**