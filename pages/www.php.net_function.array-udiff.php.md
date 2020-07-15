# array_udiff



I think the example given here using classes is convoluting things too much to demonstrate what this function does.<br><br>array_udiff() will walk through array_values($a) and array_values($b) and compare each value by using the passed in callback function.<br><br>To put it another way, array_udiff() compares $a[0] to $b[0], $b[1], $b[2], and $b[3] using the provided callback function.  If the callback returns zero for any of the comparisons then $a[0] will not be in the returned array from array_udiff().  It then compares $a[1] to $b[0], $b[1], $b[2], and $b[3].  Then, finally, $a[2] to $b[0], $b[1], $b[2], and $b[3].<br><br>For example, compare_ids($a[0], $b[0]) === -5 while compare_ids($a[1], $b[1]) === 0.  Therefore, $a[1] is not returned from array_udiff() since it is present in $b.<br><br>&lt;?<br>$a = array(<br>        array(<br>                &apos;id&apos; =&gt; 10,<br>                &apos;name&apos; =&gt; &apos;John&apos;,<br>                &apos;color&apos; =&gt; &apos;red&apos;,<br>        ),<br>        array(<br>                &apos;id&apos; =&gt; 20,<br>                &apos;name&apos; =&gt; &apos;Elise&apos;,<br>                &apos;color&apos; =&gt; &apos;blue&apos;,<br>        ),<br>        array(<br>                &apos;id&apos; =&gt; 30,<br>                &apos;name&apos; =&gt; &apos;Mark&apos;,<br>                &apos;color&apos; =&gt; &apos;red&apos;,<br>        ),<br>);<br><br>$b = array(<br>        array(<br>                &apos;id&apos; =&gt; 15,<br>                &apos;name&apos; =&gt; &apos;Nancy&apos;,<br>                &apos;color&apos; =&gt; &apos;black&apos;,<br>        ),<br>        array(<br>                &apos;id&apos; =&gt; 20,<br>                &apos;name&apos; =&gt; &apos;Elise&apos;,<br>                &apos;color&apos; =&gt; &apos;blue&apos;,<br>        ),<br>        array(<br>                &apos;id&apos; =&gt; 30,<br>                &apos;name&apos; =&gt; &apos;Mark&apos;,<br>                &apos;color&apos; =&gt; &apos;red&apos;,<br>        ),<br>        array(<br>                &apos;id&apos; =&gt; 40,<br>                &apos;name&apos; =&gt; &apos;John&apos;,<br>                &apos;color&apos; =&gt; &apos;orange&apos;,<br>        ),<br>);<br><br>function compare_ids($a, $b)<br>{<br>    return ($a[&apos;id&apos;] - $b[&apos;id&apos;]);<br>}<br>function compare_names($a, $b)<br>{<br>    return strcmp($a[&apos;name&apos;], $b[&apos;name&apos;]);<br>}<br><br>$ret = array_udiff($a, $b, &apos;compare_ids&apos;);<br>var_dump($ret);<br><br>$ret = array_udiff($b, $a, &apos;compare_ids&apos;);<br>var_dump($ret);<br><br>$ret = array_udiff($a, $b, &apos;compare_names&apos;);<br>var_dump($ret);<br>?>
```
<br><br>Which returns the following.<br><br>In the first return we see that $b has no entry in it with an id of 10.<br>&lt;?<br>array(1) {<br>  [0]=&gt;<br>  array(3) {<br>    ["id"]=&gt;<br>    int(10)<br>    ["name"]=&gt;<br>    string(4) "John"<br>    ["color"]=&gt;<br>    string(3) "red"<br>  }<br>}<br>?>
```
<br><br>In the second return we see that $a has no entry in it with an id of 15 or 40.<br>&lt;?<br>array(2) {<br>  [0]=&gt;<br>  array(3) {<br>    ["id"]=&gt;<br>    int(15)<br>    ["name"]=&gt;<br>    string(5) "Nancy"<br>    ["color"]=&gt;<br>    string(5) "black"<br>  }<br>  [3]=&gt;<br>  array(3) {<br>    ["id"]=&gt;<br>    int(40)<br>    ["name"]=&gt;<br>    string(4) "John"<br>    ["color"]=&gt;<br>    string(6) "orange"<br>  }<br>}<br>?>
```
<br><br>In third return we see that all names in $a are in $b (even though the entry in $b whose name is &apos;John&apos; is different, the anonymous function is only comparing names).<br>&lt;?<br>array(0) {<br>}<br>?>
```
  

#

Note that the compare function is used also internally, to order the arrays and choose which element compare against in the next round.<br><br>If your compare function is not really comparing (ie. returns 0 if elements are equals, 1 otherwise), you will receive an unexpected result.  

#

It is not stated, by this function also diffs array1 to itself, removing any duplicate values...  

#

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

#

[Official documentation page](https://www.php.net/manual/en/function.array-udiff.php)

**[To root](/README.md)**