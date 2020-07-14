# list



Since PHP 7.1, keys can be specified<br><br>exemple : <br>

```
<?php 
$array = [&apos;locality&apos; =&gt; &apos;Tunis&apos;, &apos;postal_code&apos; =&gt; &apos;1110&apos;];

list(&apos;postal_code&apos; =&gt; $zipCode, &apos;locality&apos; =&gt; $locality) = $array;

print $zipCode; // will output 1110
print $locality; // will output Tunis
 ?>
```
  

#

In PHP 7.1 we can do the following:<br><br>

```
<?php
    [$a, $b, $c] = [&apos;a&apos;, &apos;b&apos;, &apos;c&apos;];
?>
```


Before, we had to do:



```
<?php
    list($a, $b, $c) = [&apos;a&apos;, &apos;b&apos;,  &apos;c&apos;];
?>
```
  

#

The example showing that:<br><br>$info = array(&apos;kawa&apos;, &apos;br&#x105;zowa&apos;, &apos;kofeina&apos;);<br>list($a[0], $a[1], $a[2]) = $info;<br>var_dump($a);<br><br>outputs:<br>array(3) {<br>[2]=&gt;<br>string(8) "kofeina"<br>[1]=&gt;<br>string(5) "br&#x105;zowa"<br>[0]=&gt;<br>string(6) "kawa"<br>}<br><br>One thing to note here is that if you define the array earlier, e.g.:<br>$a = [0, 0, 0];<br><br>the indexes will be kept in the correct order:<br><br>array(3) {<br>  [0]=&gt;<br>  string(4) "kawa"<br>  [1]=&gt;<br>  string(8) "br&#x105;zowa"<br>  [2]=&gt;<br>  string(7) "kofeina"<br>}<br><br>Thought that it was worth mentioning.  

#



```
<?php<br>/**<br> * It seems you can skip listed values.<br> * Here&apos;s an example to show what I mean.<br> *<br> * FYI works just as well with PHP 7.1 shorthand list syntax.<br> * Tested against PHP 5.6.30, 7.1.5<br> */<br>$a = [ 1, 2, 3, 4 ];<br><br>// this is quite normal use case for list<br>echo "Unpack all values\n";<br>list($v1, $v2, $v3, $v4) = $a;<br>echo "$v1, $v2, $v3, $v4\n";<br>unset($v1, $v2, $v3, $v4);<br><br>// this is what I mean:<br>echo "Skip middle\n";<br>list($v1, , , $v4) = $a;<br>echo "$v1, $v2, $v3, $v4\n";<br>unset($v1, $v2, $v3, $v4);<br><br>echo "Skip beginning\n";<br>list( , , $v3, $v4) = $a;<br>echo "$v1, $v2, $v3, $v4\n";<br>unset($v1, $v2, $v3, $v4);<br><br>echo "Skip end\n";<br>list($v1, $v2, , ) = $a;<br>echo "$v1, $v2, $v3, $v4\n";<br>unset($v1, $v2, $v3, $v4);<br><br>echo "Leave middle\n";<br>list( , $v2, $v3, ) = $a;<br>echo "$v1, $v2, $v3, $v4\n";<br>unset($v1, $v2, $v3, $v4);  

#

As noted, list() will give an error if the input array is too short. This can be avoided by array_merge()&apos;ing in some default values. For example:<br><br>

```
<?php
$parameter = &apos;name&apos;;
list( $a, $b ) = array_merge( explode( &apos;=&apos;, $parameter ), array( true ) );
?>
```


However, you will have to array_merge with an array long enough to ensure there are enough elements (if $parameter is empty, the code above would still error).

An alternate approach would be to use array_pad on the array to ensure its length (if all the defaults you need to add are the same).



```
<?php
    $parameter = &apos;bob-12345&apos;;
    list( $name, $id, $fav_color, $age ) = array_pad( explode( &apos;-&apos;, $parameter ), 4, &apos;&apos; );
    var_dump($name, $id, $fav_color, $age);
/* outputs
string(3) "bob"
string(5) "12345"
string(0) ""
string(0) ""
*/
?>
```
  

#

The example states the following:<br>

```
<?php
// list() doesn&apos;t work with strings
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
  

#

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

#

The list() definition won&apos;t throw an error if your array is longer then defined list. <br>

```
<?php

list($a, $b, $c) = array("a", "b", "c", "d");

var_dump($a); // a
var_dump($b); // b
var_dump($c); // c
?>
```
  

#

The list construct seems to look for a sequential list of indexes rather taking elements in sequence. What that obscure statement means is that if you unset an element, list will not simply jump to the next element and assign that to the variable but will treat the missing element as a null or empty variable:<br><br>    $test = array("a","b","c","d");<br>    unset($test[1]);<br>    list($a,$b,$c)=$test;<br>    print "\$a=&apos;$a&apos; \$b=&apos;$b&apos; \$c=&apos;$c&apos;&lt;BR&gt;";<br><br>results in:<br>$a=&apos;a&apos; $b=&apos;&apos; $c=&apos;c&apos;<br><br>not:<br>$a=&apos;a&apos; $b=&apos;c&apos; $c=&apos;d&apos;  

#

[Official documentation page](https://www.php.net/manual/en/function.list.php)

**[To root](/README.md)**