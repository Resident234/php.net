# usort



Just wanted to show off the beauty of PHPs spaceship operator in this use case.<br><br>

```
<?php // tested on PHP 7.1
$a = [2, 1, 3, 6, 5, 4, 7];
$asc = $desc = $a;
usort($asc, function (int $a, int $b) { return ($a <=> $b); });
usort($desc, function (int $a, int $b) { return -($a <=> $b); });
print_r([ $a, $asc, $desc ]);

/**
 * Getting ahead of myself but... If arrow function syntax was possible:
 * usort($asc, (int $a, int $b) => ($a <=> $b));
 * usort($desc, (int $a, int $b) => -($a <=> $b));
 */
?>
```
  

---

When trying to do some custom sorting with objects and an anonymous function it wasn&apos;t entirely clear how this usort function works. I think it probably uses a quicksort in the background. Basically it actually moves the $b variable up or down in respect to the $a variable. It does NOT move the $a variable inside the callback function. This is key to getting your logic right in the comparisons.<br><br>If you return -1 that moves the $b variable down the array, return 1 moves $b up the array and return 0 keeps $b in the same place. <br><br>To test I cut down my code to sorting a simple array from highest priority to lowest.<br><br>

```
<?php
$priorities = array(5, 8, 3, 7, 3);

usort($priorities, function($a, $b)
{
    if ($a == $b)
    {
        echo "a ($a) is same priority as b ($b), keeping the same\n";
        return 0;
    }
    else if ($a > $b)
    {
        echo "a ($a) is higher priority than b ($b), moving b down array\n";
        return -1;
    }
    else {
        echo "b ($b) is higher priority than a ($a), moving b up array\n";                
        return 1;
    }
});

echo "Sorted priorities:\n";
var_dump($priorities);
?>
```
<br><br>Output:<br><br>b (8) is higher priority than a (3), moving b up array<br>b (5) is higher priority than a (3), moving b up array<br>b (7) is higher priority than a (3), moving b up array<br>a (3) is same priority as b (3), keeping the same<br>a (8) is higher priority than b (3), moving b down array<br>b (8) is higher priority than a (7), moving b up array<br>b (8) is higher priority than a (5), moving b up array<br>b (8) is higher priority than a (3), moving b up array<br>a (5) is higher priority than b (3), moving b down array<br>a (7) is higher priority than b (5), moving b down array<br><br>Sorted priorities:<br>array(5) {<br>  [0]=&gt; int(8)<br>  [1]=&gt; int(7)<br>  [2]=&gt; int(5)<br>  [3]=&gt; int(3)<br>  [4]=&gt; int(3)<br>}  

---

this is a new multisort function for sorting on multiple subfield like it will be in sql : &apos;ORDER BY field1, field2&apos;<br>number of sort field is undefined<br>

```
<?php

$array[] = array('soc' => 3, 'code'=>1);
$array[] = array('soc' => 2, 'code'=>1);
$array[] = array('soc' => 1, 'code'=>1);
$array[] = array('soc' => 1, 'code'=>1);
$array[] = array('soc' => 2, 'code'=>5);
$array[] = array('soc' => 1, 'code'=>2);
$array[] = array('soc' => 3, 'code'=>2);

//usage
print_r(multiSort($array, 'soc', 'code'));

function multiSort() {
    //get args of the function
    $args = func_get_args();
    $c = count($args);
    if ($c < 2) {
        return false;
    }
    //get the array to sort
    $array = array_splice($args, 0, 1);
    $array = $array[0];
    //sort with an anoymous function using args
    usort($array, function($a, $b) use($args) {

        $i = 0;
        $c = count($args);
        $cmp = 0;
        while($cmp == 0 &amp;&amp; $i < $c)
        {
            $cmp = strcmp($a[ $args[ $i ] ], $b[ $args[ $i ] ]);
            $i++;
        }

        return $cmp;

    });

    return $array;

}
?>
```
<br><br>output:<br>Array<br>(<br>    [0] =&gt; Array<br>        (<br>            [soc] =&gt; 1<br>            [code] =&gt; 1<br>        )<br><br>    [1] =&gt; Array<br>        (<br>            [soc] =&gt; 1<br>            [code] =&gt; 1<br>        )<br><br>    [2] =&gt; Array<br>        (<br>            [soc] =&gt; 1<br>            [code] =&gt; 2<br>        )<br><br>    [3] =&gt; Array<br>        (<br>            [soc] =&gt; 2<br>            [code] =&gt; 1<br>        )<br><br>    [4] =&gt; Array<br>        (<br>            [soc] =&gt; 2<br>            [code] =&gt; 5<br>        )<br><br>    [5] =&gt; Array<br>        (<br>            [soc] =&gt; 3<br>            [code] =&gt; 1<br>        )<br><br>    [6] =&gt; Array<br>        (<br>            [soc] =&gt; 3<br>            [code] =&gt; 2<br>        )<br><br>)  

---

I wrote a wrapper for usort that lets you use something similar to an SQL ORDER BY clause. It can sort arrays of associative arrays and arrays of objects and I think it would work with some hybrid case.<br><br>Example of how the function works:<br><br>

```
<?php
$testAry = array(
  array('a' => 1, 'b' => 2, 'c' => 3),
  array('a' => 2, 'b' => 1, 'c' => 3),
  array('a' => 3, 'b' => 2, 'c' => 1),
  array('a' => 1, 'b' => 3, 'c' => 2),
  array('a' => 2, 'b' => 3, 'c' => 1),
  array('a' => 3, 'b' => 1, 'c' => 2)
);

Utility::orderBy($testAry, 'a ASC, b DESC');

//Result:
$testAry = array(
  array('a' => 1, 'b' => 3, 'c' => 2),
  array('a' => 1, 'b' => 2, 'c' => 3),
  array('a' => 2, 'b' => 3, 'c' => 1),
  array('a' => 2, 'b' => 1, 'c' => 3),
  array('a' => 3, 'b' => 2, 'c' => 1),
  array('a' => 3, 'b' => 1, 'c' => 2)
);
?>
```


To sort an array of objects you would do something like:
Utility::orderBy($objectAry, 'getCreationDate() DESC, getSubOrder() ASC');

This would sort an array of objects that have methods getCreationDate() and getSubOrder().

Here is the function:



```
<?php
class Utility {
    /*
    * @param array $ary the array we want to sort
    * @param string $clause a string specifying how to sort the array similar to SQL ORDER BY clause
    * @param bool $ascending that default sorts fall back to when no direction is specified
    * @return null
    */
    public static function orderBy(&amp;$ary, $clause, $ascending = true) {
        $clause = str_ireplace('order by', '', $clause);
        $clause = preg_replace('/\s+/', ' ', $clause);
        $keys = explode(',', $clause);
        $dirMap = array('desc' => 1, 'asc' => -1);
        $def = $ascending ? -1 : 1;

        $keyAry = array();
        $dirAry = array();
        foreach($keys as $key) {
            $key = explode(' ', trim($key));
            $keyAry[] = trim($key[0]);
            if(isset($key[1])) {
                $dir = strtolower(trim($key[1]));
                $dirAry[] = $dirMap[$dir] ? $dirMap[$dir] : $def;
            } else {
                $dirAry[] = $def;
            }
        }

        $fnBody = '';
        for($i = count($keyAry) - 1; $i >= 0; $i--) {
            $k = $keyAry[$i];
            $t = $dirAry[$i];
            $f = -1 * $t;
            $aStr = '$a[\''.$k.'\']';
            $bStr = '$b[\''.$k.'\']';
            if(strpos($k, '(') !== false) {
                $aStr = '$a->'.$k;
                $bStr = '$b->'.$k;
            }

            if($fnBody == '') {
                $fnBody .= "if({$aStr} == {$bStr}) { return 0; }\n";
                $fnBody .= "return ({$aStr} < {$bStr}) ? {$t} : {$f};\n";                
            } else {
                $fnBody = "if({$aStr} == {$bStr}) {\n" . $fnBody;
                $fnBody .= "}\n";
                $fnBody .= "return ({$aStr} < {$bStr}) ? {$t} : {$f};\n";
            }
        }

        if($fnBody) {
            $sortFn = create_function('$a,$b', $fnBody);
            usort($ary, $sortFn);        
        }
    }
}
?>
```
  

---

You can also sort multi-dimensional array for multiple values like as<br><br>

```
<?php 
$arr = [
    [
        "name"=> "Sally",
        "nick_name"=> "sal",
        "availability"=> "0",
        "is_fav"=> "0"
    ],
    [
        "name"=> "David",
        "nick_name"=> "dav07",
        "availability"=> "0",
        "is_fav"=> "1"
    ],
    [
        "name"=> "Zen",
        "nick_name"=> "zen",
        "availability"=> "1",
        "is_fav"=> "0"
    ],
    [
        "name"=> "Jackson",
        "nick_name"=> "jack",
        "availability"=> "1",
        "is_fav"=> "1"
    ],
    [
        "name"=> "Rohit",
        "nick_name"=> "rod",
        "availability"=> "0",
        "is_fav"=> "0"
    ],

];

usort($arr,function($a,$b){
    $c = $b['is_fav'] - $a['is_fav'];
    $c .= $b['availability'] - $a['availability'];
    $c .= strcmp($a['nick_name'],$b['nick_name']);
    return $c;
});

print_r($arr);
?>
```
<br><br>Output:<br><br>Array<br>(<br>    [0] =&gt; Array<br>        (<br>            [name] =&gt; Jackson<br>            [nick_name] =&gt; jack<br>            [availability] =&gt; 1<br>            [is_fav] =&gt; 1<br>        )<br><br>    [1] =&gt; Array<br>        (<br>            [name] =&gt; David<br>            [nick_name] =&gt; dav07<br>            [availability] =&gt; 0<br>            [is_fav] =&gt; 1<br>        )<br><br>    [2] =&gt; Array<br>        (<br>            [name] =&gt; Zen<br>            [nick_name] =&gt; zen<br>            [availability] =&gt; 1<br>            [is_fav] =&gt; 0<br>        )<br><br>    [3] =&gt; Array<br>        (<br>            [name] =&gt; Rohit<br>            [nick_name] =&gt; rod<br>            [availability] =&gt; 0<br>            [is_fav] =&gt; 0<br>        )<br><br>    [4] =&gt; Array<br>        (<br>            [name] =&gt; Sally<br>            [nick_name] =&gt; sal<br>            [availability] =&gt; 0<br>            [is_fav] =&gt; 0<br>        )<br><br>)  

---

You can also sort multi-dimensional array for multiple values like as<br>

```
<?php
$arr = array(
    array('id'=>1, 'age'=>1, 'sex'=>6, 'name'=>'a'),
    array('id'=>2, 'age'=>3, 'sex'=>1, 'name'=>'c'),
    array('id'=>3, 'age'=>3, 'sex'=>1, 'name'=>'b'),
    array('id'=>4, 'age'=>2, 'sex'=>1, 'name'=>'d'),
);

print_r(arrayOrderBy($arr, 'age asc,sex asc,name desc'));

function arrayOrderBy(array &amp;$arr, $order = null) {
    if (is_null($order)) {
        return $arr;
    }
    $orders = explode(',', $order);
    usort($arr, function($a, $b) use($orders) {
        $result = array();
        foreach ($orders as $value) {
            list($field, $sort) = array_map('trim', explode(' ', trim($value)));
            if (!(isset($a[$field]) &amp;&amp; isset($b[$field]))) {
                continue;
            }
            if (strcasecmp($sort, 'desc') === 0) {
                $tmp = $a;
                $a = $b;
                $b = $tmp;
            }
            if (is_numeric($a[$field]) &amp;&amp; is_numeric($b[$field]) ) {
                $result[] = $a[$field] - $b[$field];
            } else {
                $result[] = strcmp($a[$field], $b[$field]);
            }
        }
        return implode('', $result);
    });
    return $arr;
}
?>
```
<br><br>output:<br>Array<br>(<br>    [0] =&gt; Array<br>        (<br>            [id] =&gt; 1<br>            [age] =&gt; 1<br>            [sex] =&gt; 6<br>            [name] =&gt; a<br>        )<br><br>    [1] =&gt; Array<br>        (<br>            [id] =&gt; 4<br>            [age] =&gt; 2<br>            [sex] =&gt; 1<br>            [name] =&gt; d<br>        )<br><br>    [2] =&gt; Array<br>        (<br>            [id] =&gt; 2<br>            [age] =&gt; 3<br>            [sex] =&gt; 1<br>            [name] =&gt; c<br>        )<br><br>    [3] =&gt; Array<br>        (<br>            [id] =&gt; 3<br>            [age] =&gt; 3<br>            [sex] =&gt; 1<br>            [name] =&gt; b<br>        )<br><br>)  

---

As the manual says, "If two members compare as equal, their order in the sorted array is undefined." This means that the sort used is not "stable" and may change the order of elements that compare equal.<br><br>Sometimes you really do need a stable sort. For example, if you sort a list by one field, then sort it again by another field, but don&apos;t want to lose the ordering from the previous field. In that case it is better to use usort with a comparison function that takes both fields into account, but if you can&apos;t do that then use the function below. It is a merge sort, which is guaranteed O(n*log(n)) complexity, which means it stays reasonably fast even when you use larger lists (unlike bubblesort and insertion sort, which are O(n^2)). <br><br>

```
<?php
function mergesort(&amp;$array, $cmp_function = 'strcmp') {
    // Arrays of size < 2 require no action.
    if (count($array) < 2) return;
    // Split the array in half
    $halfway = count($array) / 2;
    $array1 = array_slice($array, 0, $halfway);
    $array2 = array_slice($array, $halfway);
    // Recurse to sort the two halves
    mergesort($array1, $cmp_function);
    mergesort($array2, $cmp_function);
    // If all of $array1 is <= all of $array2, just append them.
    if (call_user_func($cmp_function, end($array1), $array2[0]) < 1) {
        $array = array_merge($array1, $array2);
        return;
    }
    // Merge the two sorted arrays into a single sorted array
    $array = array();
    $ptr1 = $ptr2 = 0;
    while ($ptr1 < count($array1) &amp;&amp; $ptr2 < count($array2)) {
        if (call_user_func($cmp_function, $array1[$ptr1], $array2[$ptr2]) < 1) {
            $array[] = $array1[$ptr1++];
        }
        else {
            $array[] = $array2[$ptr2++];
        }
    }
    // Merge the remainder
    while ($ptr1 < count($array1)) $array[] = $array1[$ptr1++];
    while ($ptr2 < count($array2)) $array[] = $array2[$ptr2++];
    return;
}
?>
```
  

---

If you want to sort an array according to another array acting as a priority list, you can use this function.<br><br>

```
<?php
function listcmp($a, $b)
{
  global $order;

  foreach($order as $key => $value)
    {
      if($a==$value)
        {
          return 0;
          break;
        }

      if($b==$value)
        {
          return 1;
          break;
        }
    }
}

$order[0] = "first";
$order[1] = "second";
$order[2] = "third";

$array[0] = "second";
$array[1] = "first";
$array[2] = "third";
$array[3] = "fourth";
$array[4] = "second";
$array[5] = "first";
$array[6] = "second";

usort($array, "listcmp");

print_r($array);
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.usort.php)

**[To root](/README.md)**