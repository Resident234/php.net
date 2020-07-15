# sort



Simple function to sort an array by a specific key. Maintains index association.<br><br>

```
<?php

function array_sort($array, $on, $order=SORT_ASC)
{
    $new_array = array();
    $sortable_array = array();

    if (count($array) &gt; 0) {
        foreach ($array as $k => $v) {
            if (is_array($v)) {
                foreach ($v as $k2 => $v2) {
                    if ($k2 == $on) {
                        $sortable_array[$k] = $v2;
                    }
                }
            } else {
                $sortable_array[$k] = $v;
            }
        }

        switch ($order) {
            case SORT_ASC:
                asort($sortable_array);
            break;
            case SORT_DESC:
                arsort($sortable_array);
            break;
        }

        foreach ($sortable_array as $k => $v) {
            $new_array[$k] = $array[$k];
        }
    }

    return $new_array;
}

$people = array(
    12345 => array(
        'id' => 12345,
        'first_name' => 'Joe',
        'surname' => 'Bloggs',
        'age' => 23,
        'sex' => 'm'
    ),
    12346 => array(
        'id' => 12346,
        'first_name' => 'Adam',
        'surname' => 'Smith',
        'age' => 18,
        'sex' => 'm'
    ),
    12347 => array(
        'id' => 12347,
        'first_name' => 'Amy',
        'surname' => 'Jones',
        'age' => 21,
        'sex' => 'f'
    )
);

print_r(array_sort($people, 'age', SORT_DESC)); // Sort by oldest first
print_r(array_sort($people, 'surname', SORT_ASC)); // Sort by surname

/*
Array
(
    [12345] => Array
        (
            [id] => 12345
            [first_name] => Joe
            [surname] => Bloggs
            [age] => 23
            [sex] => m
        )
 
    [12347] => Array
        (
            [id] => 12347
            [first_name] => Amy
            [surname] => Jones
            [age] => 21
            [sex] => f
        )
 
    [12346] => Array
        (
            [id] => 12346
            [first_name] => Adam
            [surname] => Smith
            [age] => 18
            [sex] => m
        )
 
)
Array
(
    [12345] => Array
        (
            [id] => 12345
            [first_name] => Joe
            [surname] => Bloggs
            [age] => 23
            [sex] => m
        )
 
    [12347] => Array
        (
            [id] => 12347
            [first_name] => Amy
            [surname] => Jones
            [age] => 21
            [sex] => f
        )
 
    [12346] => Array
        (
            [id] => 12346
            [first_name] => Adam
            [surname] => Smith
            [age] => 18
            [sex] => m
        )
 
)
*/

?>
```
  

#

unless you specify the second argument, "regular" comparisons will be used. I quote from the page on comparison operators:<br>"If you compare a number with a string or the comparison involves numerical strings, then each string is converted to a number and the comparison performed numerically."<br>What this means is that "10" &lt; "1a", and "1a" &lt; "2", but "10" &gt; "2". In other words, regular PHP string comparisons are not transitive.<br>This implies that the output of sort() can in rare cases depend on the order of the input array:<br>

```
<?php
function echo_sorted($a)
{
   echo "{$a[0]} {$a[1]} {$a[2]}";
   sort($a);
   echo " => {$a[0]} {$a[1]} {$a[2]}\n";
}
// on PHP 5.2.6:
echo_sorted(array( "10", "1a", "2")); // => 10 1a 2
echo_sorted(array( "10", "2", "1a")); // => 1a 2 10
echo_sorted(array( "1a", "10", "2")); // => 2 10 1a
echo_sorted(array( "1a", "2", "10")); // => 1a 2 10
echo_sorted(array( "2", "10", "1a")); // => 2 10 1a
echo_sorted(array( "2", "1a", "10")); // => 10 1a 2
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.sort.php)

**[To root](/README.md)**