# array_diff



Again, the function&apos;s description is misleading right now. I sought a function, which (mathematically) computes A - B, or, written differently, A \ B. Or, again in other words, suppose <br><br>A := {a1, ..., an} and B:= {a1, b1, ... , bm}<br><br>=&gt; array_diff(A,B) = {a2, ..., an}<br><br>array_diff(A,B) returns all elements from A, which are not elements of B (= A without B).<br><br>You should include this in the documentation more precisely, I think.  

#

array_diff provides a handy way of deleting array elements by their value, without having to unset it by key, through a lengthy foreach loop and then having to rekey the array.<br><br>

```
<?php

//pass value you wish to delete and the array to delete from
function array_delete( $value, $array)
{
    $array = array_diff( $array, array($value) );
    return $array;
}
?>
```
  

#

If you want a simple way to show values that are in either array, but not both, you can use this:<br><br>

```
<?php
function arrayDiff($A, $B) {
    $intersect = array_intersect($A, $B);
    return array_merge(array_diff($A, $intersect), array_diff($B, $intersect));
}
?>
```
<br><br>If you want to account for keys, use array_diff_assoc() instead; and if you want to remove empty values, use array_filter().  

#

If you just need to know if two arrays&apos; values are exactly the same (regardless of keys and order), then instead of using array_diff, this is a simple method:<br><br>

```
<?php

function identical_values( $arrayA , $arrayB ) {

    sort( $arrayA );
    sort( $arrayB );

    return $arrayA == $arrayB;
}

// Examples:

$array1 = array( "red" , "green" , "blue" );
$array2 = array( "green" , "red" , "blue" );
$array3 = array( "red" , "green" , "blue" , "yellow" );
$array4 = array( "red" , "yellow" , "blue" );
$array5 = array( "x" => "red" , "y" =>  "green" , "z" => "blue" );

identical_values( $array1 , $array2 );  // true
identical_values( $array1 , $array3 );  // false
identical_values( $array1 , $array4 );  // false
identical_values( $array1 , $array5 );  // true

?>
```


The function returns true only if the two arrays contain the same number of values and each value in one array has an exact duplicate in the other array. Everything else will return false.

my alternative method for evaluating if two arrays contain (all) identical values:



```
<?php

sort($a); $sort(b); return $a == $b;

?>
```


may be slightly faster (10-20%) than this array_diff method:



```
<?php

return ( count( $a ) == count( $b ) &amp;&amp; !array_diff( $a , $b ) ? true : false );

?>
```
<br><br>but only when the two arrays contain the same number of values and then only in some cases. Otherwise the latter method will be radically faster due to the use of a count() test before the array_diff().<br><br>Also, if the two arrays contain a different number of values, then which method is faster will depend on whether both arrays need to be sorted or not. Two times sort() is a bit slower than one time array_diff(), but if one of the arrays have already been sorted, then you only have to sort the other array and this will be almost twice as fast as array_diff().<br><br>Basically: 2 x sort() is slower than 1 x array_diff() is slower than 1 x sort().  

#

I just came upon a really good use for array_diff(). When reading a dir(opendir;readdir), I _rarely_ want "." or ".." to be in the array of files I&apos;m creating. Here&apos;s a simple way to remove them:<br><br>

```
<?php
 $someFiles = array();
 $dp = opendir("/some/dir");
 while($someFiles[] = readdir($dp));
 closedir($dp);
 
 $removeDirs = array(".","..");
 $someFiles = array_diff($someFiles, $removeDirs);
 
 foreach($someFiles AS $thisFile) echo $thisFile."\n";
?>
```
<br><br>S  

#

Hello guys,<br><br>I&#xB4;ve been looking for a array_diff that works with recursive arrays, I&#xB4;ve tried the ottodenn at gmail dot com function but to my case it doesn&#xB4;t worked as expected, so I made my own. I&#xB4;ve haven&#xB4;t tested this extensively, but I&#xB4;ll explain my scenario, and this works great at that case :D<br><br>We got 2 arrays like these:<br><br>

```
<?php
$aArray1['marcie'] = array('banana' => 1, 'orange' => 1, 'pasta' => 1);
$aArray1['kenji'] = array('apple' => 1, 'pie' => 1, 'pasta' => 1);

$aArray2['marcie'] = array('banana' => 1, 'orange' => 1);
?>
```


As array_diff, this function returns all the items that is in aArray1 and IS NOT at aArray2, so the result we should expect is:



```
<?php
$aDiff['marcie'] = array('pasta' => 1);
$aDiff['kenji'] = array('apple' => 1, 'pie' => 1, 'pasta' => 1);
?>
```


Ok, now some comments about this function:
 - Different from the PHP array_diff, this function DON&#xB4;T uses the === operator, but the ==, so 0 is equal to '0' or false, but this can be changed with no impacts.
 - This function checks the keys of the arrays, array_diff only compares the values.

I realy hopes that this could help some1 as I&#xB4;ve been helped a lot with some users experiences. (Just please double check if it would work for your case, as I sad I just tested to a scenario like the one I exposed)



```
<?php
function arrayRecursiveDiff($aArray1, $aArray2) {
    $aReturn = array();
   
    foreach ($aArray1 as $mKey => $mValue) {
        if (array_key_exists($mKey, $aArray2)) {
            if (is_array($mValue)) {
                $aRecursiveDiff = arrayRecursiveDiff($mValue, $aArray2[$mKey]);
                if (count($aRecursiveDiff)) { $aReturn[$mKey] = $aRecursiveDiff; }
            } else {
                if ($mValue != $aArray2[$mKey]) {
                    $aReturn[$mKey] = $mValue;
                }
            }
        } else {
            $aReturn[$mKey] = $mValue;
        }
    }
   
    return $aReturn;
}
?>
```
  

#

There is more fast implementation of array_diff, but with some limitations. If you need compare two arrays of integers or strings you can use such function:<br><br>    public static function arrayDiffEmulation($arrayFrom, $arrayAgainst)<br>    {<br>        $arrayAgainst = array_flip($arrayAgainst);<br>        <br>        foreach ($arrayFrom as $key =&gt; $value) {<br>            if(isset($arrayAgainst[$value])) {<br>                unset($arrayFrom[$key]);<br>            }<br>        }<br>        <br>        return $arrayFrom;<br>    }<br><br>It is ~10x faster than array_diff<br><br>php &gt; $t = microtime(true);$a = range(0,25000); $b = range(15000,500000); $c = array_diff($a, $b);echo microtime(true) - $t;<br>4.4335179328918<br>php &gt; $t = microtime(true);$a = range(0,25000); $b = range(15000,500000); $c = arrayDiffEmulation($a, $b);echo microtime(true) - $t;<br>0.37219095230103  

#

[Official documentation page](https://www.php.net/manual/en/function.array-diff.php)

**[To root](/README.md)**