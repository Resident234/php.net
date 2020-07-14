# uasort



a quick reminder on the syntax if you want to use uasort in a Class or Object:<br><br>

```
<?php

// procedural:
uasort($collection, &apos;my_sort_function&apos;);

// Object Oriented
uasort($collection, array($this, &apos;mySortMethod&apos;));

// Objet Oriented with static method
uasort($collection, array(&apos;self&apos;, &apos;myStaticSortMethod&apos;));

?>
```
  

#

If you want to keep the order when two members compare as equal, use this.<br>

```
<?php

function stable_uasort(&amp;$array, $cmp_function) {
    if(count($array) &lt; 2) {
        return;
    }
    $halfway = count($array) / 2;
    $array1 = array_slice($array, 0, $halfway, TRUE);
    $array2 = array_slice($array, $halfway, NULL, TRUE);

    stable_uasort($array1, $cmp_function);
    stable_uasort($array2, $cmp_function);
    if(call_user_func($cmp_function, end($array1), reset($array2)) &lt; 1) {
        $array = $array1 + $array2;
        return;
    }
    $array = array();
    reset($array1);
    reset($array2);
    while(current($array1) &amp;&amp; current($array2)) {
        if(call_user_func($cmp_function, current($array1), current($array2)) &lt; 1) {
            $array[key($array1)] = current($array1);
            next($array1);
        } else {
            $array[key($array2)] = current($array2);
            next($array2);
        }
    }
    while(current($array1)) {
        $array[key($array1)] = current($array1);
        next($array1);
    }
    while(current($array2)) {
        $array[key($array2)] = current($array2);
        next($array2);
    }
    return;
}

function cmp($a, $b) {
    if($a[&apos;n&apos;] == $b[&apos;n&apos;]) {
        return 0;
    }
    return ($a[&apos;n&apos;] &gt; $b[&apos;n&apos;]) ? -1 : 1;
}

$a = $b = array(
    &apos;a&apos; =&gt; array("l" =&gt; "A", "n" =&gt; 1),
    &apos;b&apos; =&gt; array("l" =&gt; "B", "n" =&gt; 2),
    &apos;c&apos; =&gt; array("l" =&gt; "C", "n" =&gt; 1),
    &apos;d&apos; =&gt; array("l" =&gt; "D", "n" =&gt; 2),
    &apos;e&apos; =&gt; array("l" =&gt; "E", "n" =&gt; 2),
);

uasort($a, &apos;cmp&apos;);
print_r($a);

stable_uasort($b, &apos;cmp&apos;);
print_r($b);
?>
```
<br><br>returns<br><br>Array<br>(<br>    [e] =&gt; Array<br>        (<br>            [l] =&gt; E<br>            [n] =&gt; 2<br>        )<br><br>    [b] =&gt; Array<br>        (<br>            [l] =&gt; B<br>            [n] =&gt; 2<br>        )<br><br>    [d] =&gt; Array<br>        (<br>            [l] =&gt; D<br>            [n] =&gt; 2<br>        )<br><br>    [c] =&gt; Array<br>        (<br>            [l] =&gt; C<br>            [n] =&gt; 1<br>        )<br><br>    [a] =&gt; Array<br>        (<br>            [l] =&gt; A<br>            [n] =&gt; 1<br>        )<br><br>)<br>Array<br>(<br>    [b] =&gt; Array<br>        (<br>            [l] =&gt; B<br>            [n] =&gt; 2<br>        )<br><br>    [d] =&gt; Array<br>        (<br>            [l] =&gt; D<br>            [n] =&gt; 2<br>        )<br><br>    [e] =&gt; Array<br>        (<br>            [l] =&gt; E<br>            [n] =&gt; 2<br>        )<br><br>    [a] =&gt; Array<br>        (<br>            [l] =&gt; A<br>            [n] =&gt; 1<br>        )<br><br>    [c] =&gt; Array<br>        (<br>            [l] =&gt; C<br>            [n] =&gt; 1<br>        )<br>)<br><br>https://bugs.php.net/bug.php?id=53341  

#

An Example using anonymous function.<br>Anonymous functions make some time the code easier to understand.<br>

```
<?php
$fruits = array(&apos;Orange9&apos;,&apos;Orange11&apos;,&apos;Orange10&apos;,&apos;Orange6&apos;,&apos;Orange15&apos;);
uasort ( $fruits , function ($a, $b) {
            return strnatcmp($a,$b); // or other function/code
        }
    );
print_r($fruits);
?>
```
<br>returns<br>Array<br>(<br>    [3] =&gt; Orange6<br>    [0] =&gt; Orange9<br>    [2] =&gt; Orange10<br>    [1] =&gt; Orange11<br>    [4] =&gt; Orange15<br>)  

#

[Official documentation page](https://www.php.net/manual/en/function.uasort.php)

**[To root](/README.md)**