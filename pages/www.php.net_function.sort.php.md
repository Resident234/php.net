# sort



Simple function to sort an array by a specific key. Maintains index association.<br><br>

```
<?php

function array_sort($array, $on, $order=SORT_ASC)
{
    $new_array = array();
    $sortable_array = array();

    if (count($array) &gt; 0) {
        foreach ($array as $k =&gt; $v) {
            if (is_array($v)) {
                foreach ($v as $k2 =&gt; $v2) {
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

        foreach ($sortable_array as $k =&gt; $v) {
            $new_array[$k] = $array[$k];
        }
    }

    return $new_array;
}

$people = array(
    12345 =&gt; array(
        &apos;id&apos; =&gt; 12345,
        &apos;first_name&apos; =&gt; &apos;Joe&apos;,
        &apos;surname&apos; =&gt; &apos;Bloggs&apos;,
        &apos;age&apos; =&gt; 23,
        &apos;sex&apos; =&gt; &apos;m&apos;
    ),
    12346 =&gt; array(
        &apos;id&apos; =&gt; 12346,
        &apos;first_name&apos; =&gt; &apos;Adam&apos;,
        &apos;surname&apos; =&gt; &apos;Smith&apos;,
        &apos;age&apos; =&gt; 18,
        &apos;sex&apos; =&gt; &apos;m&apos;
    ),
    12347 =&gt; array(
        &apos;id&apos; =&gt; 12347,
        &apos;first_name&apos; =&gt; &apos;Amy&apos;,
        &apos;surname&apos; =&gt; &apos;Jones&apos;,
        &apos;age&apos; =&gt; 21,
        &apos;sex&apos; =&gt; &apos;f&apos;
    )
);

print_r(array_sort($people, &apos;age&apos;, SORT_DESC)); // Sort by oldest first
print_r(array_sort($people, &apos;surname&apos;, SORT_ASC)); // Sort by surname

/*
Array
(
    [12345] =&gt; Array
        (
            [id] =&gt; 12345
            [first_name] =&gt; Joe
            [surname] =&gt; Bloggs
            [age] =&gt; 23
            [sex] =&gt; m
        )
 
    [12347] =&gt; Array
        (
            [id] =&gt; 12347
            [first_name] =&gt; Amy
            [surname] =&gt; Jones
            [age] =&gt; 21
            [sex] =&gt; f
        )
 
    [12346] =&gt; Array
        (
            [id] =&gt; 12346
            [first_name] =&gt; Adam
            [surname] =&gt; Smith
            [age] =&gt; 18
            [sex] =&gt; m
        )
 
)
Array
(
    [12345] =&gt; Array
        (
            [id] =&gt; 12345
            [first_name] =&gt; Joe
            [surname] =&gt; Bloggs
            [age] =&gt; 23
            [sex] =&gt; m
        )
 
    [12347] =&gt; Array
        (
            [id] =&gt; 12347
            [first_name] =&gt; Amy
            [surname] =&gt; Jones
            [age] =&gt; 21
            [sex] =&gt; f
        )
 
    [12346] =&gt; Array
        (
            [id] =&gt; 12346
            [first_name] =&gt; Adam
            [surname] =&gt; Smith
            [age] =&gt; 18
            [sex] =&gt; m
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
   echo " =&gt; {$a[0]} {$a[1]} {$a[2]}\n";
}
// on PHP 5.2.6:
echo_sorted(array( "10", "1a", "2")); // =&gt; 10 1a 2
echo_sorted(array( "10", "2", "1a")); // =&gt; 1a 2 10
echo_sorted(array( "1a", "10", "2")); // =&gt; 2 10 1a
echo_sorted(array( "1a", "2", "10")); // =&gt; 1a 2 10
echo_sorted(array( "2", "10", "1a")); // =&gt; 2 10 1a
echo_sorted(array( "2", "1a", "10")); // =&gt; 10 1a 2
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.sort.php)

**[To root](/README.md)**