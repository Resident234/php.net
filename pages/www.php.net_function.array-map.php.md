# array_map



I was miffed that array_map didn&apos;t have a way to pass values *and* keys to the callback, but then I realized I could do this:<br><br>function callback($k, $v) { ... }<br><br>array_map( "callback", array_keys($array), $array);  

#

If you need to call a static method from array_map, this will NOT work:<br><br>&lt;?PHP<br>array_map(&apos;myclass::myMethod&apos; , $value);<br>?>
```


Instead, you need to do this:

&lt;?PHP
array_map( array(&apos;myclass&apos;,&apos;myMethod&apos;) , $value);
?>
```
<br><br>It is helpful to remember that this will work with any PHP function which expects a callback argument.  

#

PHP 5.3 enables us to use inline anonymous functions with array_map, cleaning up the syntax slightly.<br><br>

```
<?php
$data = array(
        array(&apos;id&apos; =&gt; 1, &apos;name&apos; =&gt; &apos;Bob&apos;, &apos;position&apos; =&gt; &apos;Clerk&apos;),
        array(&apos;id&apos; =&gt; 2, &apos;name&apos; =&gt; &apos;Alan&apos;, &apos;position&apos; =&gt; &apos;Manager&apos;),
        array(&apos;id&apos; =&gt; 3, &apos;name&apos; =&gt; &apos;James&apos;, &apos;position&apos; =&gt; &apos;Director&apos;)
);

$names = array_map(
        function($person) { return $person[&apos;name&apos;]; },
        $data
);

print_r($names);
?>
```


This was possible (although not recommended) in prior versions of PHP 5, via create_function().



```
<?php
$names = array_map(
        create_function(&apos;$person&apos;, &apos;return $person["name"];&apos;),
        $data
);
?>
```


You&apos;re less likely to catch errors in the latter version because the code is passed as string arguments.

These are alternatives to using a foreach:



```
<?php
$names = array();

foreach ($data as $row) {
        $names[] = $row[&apos;name&apos;];
}
?>
```
  

#

You can use array_map with PHP native functions as well as user functions.  This is very handy if you need to sanitize arrays.  <br><br>

```
<?php

$integers = array_map (&apos;intval&apos;, $integers);
$safeStrings = array_map (&apos;mysql_real_escape_string&apos;, $unsafeStrings);

?>
```
  

#

To transpose rectangular two-dimension array, use the following code:<br><br>array_unshift($array, null);<br>$array = call_user_func_array("array_map", $array);<br><br>If you need to rotate rectangular two-dimension array on 90 degree, add the following line before or after (depending on the rotation direction you need) the code above:<br>$array = array_reverse($array);<br><br>Here is example:<br><br>

```
<?php
$a = array(
  array(1, 2, 3),
  array(4, 5, 6));
array_unshift($a, null);
$a = call_user_func_array("array_map", $a);
print_r($a);
?>
```
<br><br>Output:<br><br>Array<br>(<br>    [0] =&gt; Array<br>        (<br>            [0] =&gt; 1<br>            [1] =&gt; 4<br>        )<br><br>    [1] =&gt; Array<br>        (<br>            [0] =&gt; 2<br>            [1] =&gt; 5<br>        )<br><br>    [2] =&gt; Array<br>        (<br>            [0] =&gt; 3<br>            [1] =&gt; 6<br>        )<br><br>)  

#

Simplest array_map_recursive() implemention.<br><br>

```
<?php
function array_map_recursive(callable $func, array $array) {
    return filter_var($array, \FILTER_CALLBACK, [&apos;options&apos; =&gt; $func]);
}
?>
```
  

#

Let&apos;s assume we have following situation:<br><br>

```
<?php
class MyFilterClass {
    public function filter(array $arr) {
        return array_map(function($value) {
            return $this-&gt;privateFilterMethod($value);
        });
    }

    private function privateFilterMethod($value) {
        if (is_numeric($value)) $value++;
        else $value .= &apos;.&apos;;
    }
}
?>
```
<br><br>This will work, because $this inside anonymous function (unlike for example javascript) is the instance of MyFilterClass inside which we called it.<br>I hope this would be useful for anyone.  

#

Here is how to perform an operation on some of the elements of an array:<br><br>

```
<?php
$an_array = array(
    &apos;item1&apos; =&gt; 0,
    &apos;item2&apos; =&gt; 0,
    &apos;item3&apos; =&gt; 0,
    &apos;item4&apos; =&gt; 0,
    &apos;item5&apos; =&gt; 0,
);

$items_to_modify = array(&apos;item1&apos;, "item3");

 array_map(function ($value) use (&amp;$an_array ) {
     $an_array [$value] = (boolean)$an_array [$value];   //example operation:
 }, $items_to_modify);

?>
```
<br><br>This will take the original array and perform an action only on items specified on the second array items. Use of &amp; symbol in the use statement makes the array_map access the variable as a reference in an outer scope.<br><br>This makes code easily extendable.  

#

array_map() can be applied to assoc arrays without affecting the keys  

#

You may be looking for a method to extract values of a multidimensional array on a conditional basis (i.e. a mixture between array_map and array_filter) other than a for/foreach loop. If so, you can take advantage of the fact that 1) the callback method on array_map returns null if no explicit return value is specified (as with everything else) and 2) array_filter with no arguments removes falsy values. <br><br>So for example, provided you have:<br><br>

```
<?php
$data = [
    [
        "name" =&gt; "John",
        "smoker" =&gt; false
    ],
    [
        "name" =&gt; "Mary",
        "smoker" =&gt; true
    ],
    [
        "name" =&gt; "Peter",
        "smoker" =&gt; false
    ],
    [
        "name" =&gt; "Tony",
        "smoker" =&gt; true
    ]
];
?>
```


You can extract the names of all the non-smokers with the following one-liner:



```
<?php
$names = array_filter(array_map(function($n) { if(!$n[&apos;smoker&apos;]) return $n[&apos;name&apos;]; }, $data));
?>
```
<br><br>It&apos;s not necessarily better than a for/foreach loop, but the occasional one-liner for trivial tasks can help keep your code cleaner.  

#

Find an interesting thing that in array_map&apos;s callable function, late static binding does not work:<br>

```
<?php
class A {
    public static function foo($name) {
        return &apos;In A: &apos;.$name;
    }

    public static function test($names) {
        return array_map(function($n) {return static::foo($n);}, $names);
    }
}

class B extends A{
    public static function foo($name) {
        return &apos;In B: &apos;.$name;
    }
}

$result = B::test([&apos;alice&apos;, &apos;bob&apos;]);
var_dump($result);
?>
```


the result is:
array (size=2)
  0 =&gt; string &apos;In A: alice&apos; (length=11)
  1 =&gt; string &apos;In A: bob&apos; (length=9)

if I change A::test to


```
<?php
    public static function test($names) {
        return array_map([get_called_class(), &apos;foo&apos;], $names);
    }
?>
```
<br><br>Then the result is as expected:<br>array (size=2)<br>  0 =&gt; string &apos;In B: alice&apos; (length=11)<br>  1 =&gt; string &apos;In B: bob&apos; (length=9)  

#

In case of you need to recursively bypass a function over the itens of an array, you can use it<br><br>

```
<?php
    function array_map_recursive($callback, $array) {
        foreach ($array as $key =&gt; $value) {
            if (is_array($array[$key])) {
                $array[$key] = array_map_recursive($callback, $array[$key]);
            }
            else {
                $array[$key] = call_user_func($callback, $array[$key]);
            }
        }
        return $array;
    }
?>
```


-----------------------------------------------------------------------



```
<?php
    $strings = array(
        &apos;The&apos;,
        array(
            &apos;quick&apos;,
            &apos;fox&apos;,
            array(
                &apos;brown&apos;,
                &apos;jumps&apos;,
                array(
                    &apos;over&apos;,
                    array(
                        &apos;the&apos;,
                        array(
                            &apos;lazy&apos;,
                            array(
                                &apos;dog&apos;
                            )
                        )
                    )
                )
            )
        )
    );

    print_r($strings);
    $hashedString = array_map_recursive(&apos;md5&apos;, $strings);
    print_r($hashedString);
?>
```


------------------------------------------------------------------------
Testing it, you&apos;ll obtain



```
<?php

/* Original array */

array (
  0 =&gt; &apos;The&apos;,
  1 =&gt;
  array (
    0 =&gt; &apos;quick&apos;,
    1 =&gt; &apos;fox&apos;,
    2 =&gt;
    array (
      0 =&gt; &apos;brown&apos;,
      1 =&gt; &apos;jumps&apos;,
      2 =&gt;
      array (
        0 =&gt; &apos;over&apos;,
        1 =&gt;
        array (
          0 =&gt; &apos;the&apos;,
          1 =&gt;
          array (
            0 =&gt; &apos;lazy&apos;,
            1 =&gt;
            array (
              0 =&gt; &apos;dog&apos;,
            ),
          ),
        ),
      ),
    ),
  ),
);

/* Recursived array */
array (
  0 =&gt; &apos;a4704fd35f0308287f2937ba3eccf5fe&apos;,
  1 =&gt;
  array (
    0 =&gt; &apos;1df3746a4728276afdc24f828186f73a&apos;,
    1 =&gt; &apos;2b95d1f09b8b66c5c43622a4d9ec9a04&apos;,
    2 =&gt;
    array (
      0 =&gt; &apos;6ff47afa5dc7daa42cc705a03fca8a9b&apos;,
      1 =&gt; &apos;55947829059f255e4ba2f536a2ae99fe&apos;,
      2 =&gt;
      array (
        0 =&gt; &apos;3b759a9ca80234563d87672350659b2b&apos;,
        1 =&gt;
        array (
          0 =&gt; &apos;8fc42c6ddf9966db3b09e84365034357&apos;,
          1 =&gt;
          array (
            0 =&gt; &apos;0ffe34b4e04c2b282c5a388b1ad8aa7a&apos;,
            1 =&gt;
            array (
              0 =&gt; &apos;06d80eb0c50b49a509b49f2424e8c805&apos;,
            ),
          ),
        ),
      ),
    ),
  ),
);

?>
```
<br><br>Hope it helps you.<br><br>Cheers.  

#

[Official documentation page](https://www.php.net/manual/en/function.array-map.php)

**[To root](/README.md)**