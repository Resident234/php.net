# list



Since PHP 7.1, keys can be specified<br><br>exemple : <br>

```
<?php 
$array = ['locality' => 'Tunis', 'postal_code' => '1110'];

list('postal_code' => $zipCode, 'locality' => $locality) = $array;

print $zipCode; // will output 1110
print $locality; // will output Tunis
 ?>
```
  

---

In PHP 7.1 we can do the following:<br><br>

```
<?php
    [$a, $b, $c] = ['a', 'b', 'c'];
?>
```


Before, we had to do:



```
<?php
    list($a, $b, $c) = ['a', 'b',  'c'];
?>
```
  

---

The example showing that:<br><br>$info = array(&apos;kawa&apos;, &apos;br&#x105;zowa&apos;, &apos;kofeina&apos;);<br>list($a[0], $a[1], $a[2]) = $info;<br>var_dump($a);<br><br>outputs:<br>array(3) {<br>[2]=&gt;<br>string(8) "kofeina"<br>[1]=&gt;<br>string(5) "br&#x105;zowa"<br>[0]=&gt;<br>string(6) "kawa"<br>}<br><br>One thing to note here is that if you define the array earlier, e.g.:<br>$a = [0, 0, 0];<br><br>the indexes will be kept in the correct order:<br><br>array(3) {<br>  [0]=&gt;<br>  string(4) "kawa"<br>  [1]=&gt;<br>  string(8) "br&#x105;zowa"<br>  [2]=&gt;<br>  string(7) "kofeina"<br>}<br><br>Thought that it was worth mentioning.  

---



```
<?php
/**
 * It seems you can skip listed values.
 * Here's an example to show what I mean.
 *
 * FYI works just as well with PHP 7.1 shorthand list syntax.
 * Tested against PHP 5.6.30, 7.1.5
 */
$a = [ 1, 2, 3, 4 ];

// this is quite normal use case for list
echo "Unpack all values\n";
list($v1, $v2, $v3, $v4) = $a;
echo "$v1, $v2, $v3, $v4\n";
unset($v1, $v2, $v3, $v4);

// this is what I mean:
echo "Skip middle\n";
list($v1, , , $v4) = $a;
echo "$v1, $v2, $v3, $v4\n";
unset($v1, $v2, $v3, $v4);

echo "Skip beginning\n";
list( , , $v3, $v4) = $a;
echo "$v1, $v2, $v3, $v4\n";
unset($v1, $v2, $v3, $v4);

echo "Skip end\n";
list($v1, $v2, , ) = $a;
echo "$v1, $v2, $v3, $v4\n";
unset($v1, $v2, $v3, $v4);

echo "Leave middle\n";
list( , $v2, $v3, ) = $a;
echo "$v1, $v2, $v3, $v4\n";
unset($v1, $v2, $v3, $v4);?>
```
  

---

As noted, list() will give an error if the input array is too short. This can be avoided by array_merge()&apos;ing in some default values. For example:<br><br>

```
<?php
$parameter = 'name';
list( $a, $b ) = array_merge( explode( '=', $parameter ), array( true ) );
?>
```


However, you will have to array_merge with an array long enough to ensure there are enough elements (if $parameter is empty, the code above would still error).

An alternate approach would be to use array_pad on the array to ensure its length (if all the defaults you need to add are the same).



```
<?php
    $parameter = 'bob-12345';
    list( $name, $id, $fav_color, $age ) = array_pad( explode( '-', $parameter ), 4, '' );
    var_dump($name, $id, $fav_color, $age);
/* outputs
string(3) "bob"
string(5) "12345"
string(0) ""
string(0) ""
*/
?>
```
  

---

The example states the following:<br>

```
<?php
// list() doesn't work with strings
list($bar) = "abcde";
var_dump($bar); 
// output: NULL
?>
```


If the string is in a variable however, it seems using list() will treat the string as an array:


```
<?php
$string = "abcde";
list($foo) = $string;
var_dump($foo);
// output: string(1) "a"
?>
```
  

---

list() can be used with foreach<br><br>

```
<?php
$array = [[1, 2], [3, 4], [5, 6]];

foreach($array as list($odd, $even)){
    echo "$odd is odd; $even is even", PHP_EOL;
}
?>
```
<br><br>The output:<br>===<br>1 is odd; 2 is even<br>3 is odd; 4 is even<br>5 is odd; 6 is even  

---

The list() definition won&apos;t throw an error if your array is longer then defined list. <br>

```
<?php

list($a, $b, $c) = array("a", "b", "c", "d");

var_dump($a); // a
var_dump($b); // b
var_dump($c); // c
?>
```
  

---

The list construct seems to look for a sequential list of indexes rather taking elements in sequence. What that obscure statement means is that if you unset an element, list will not simply jump to the next element and assign that to the variable but will treat the missing element as a null or empty variable:<br><br>    $test = array("a","b","c","d");<br>    unset($test[1]);<br>    list($a,$b,$c)=$test;<br>    print "\$a=&apos;$a&apos; \$b=&apos;$b&apos; \$c=&apos;$c&apos;&lt;BR&gt;";<br><br>results in:<br>$a=&apos;a&apos; $b=&apos;&apos; $c=&apos;c&apos;<br><br>not:<br>$a=&apos;a&apos; $b=&apos;c&apos; $c=&apos;d&apos;  

---

[Official documentation page](https://www.php.net/manual/en/function.list.php)

**[To root](/README.md)**