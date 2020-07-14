# Array Operators



The union operator did not behave as I thought it would on first glance. It implements a union (of sorts) based on the keys of the array, not on the values.<br><br>For instance:<br>

```
<?php
$a = array('one','two');
$b=array('three','four','five');

//not a union of arrays' values
echo '$a + $b : ';
print_r ($a + $b);

//a union of arrays' values
echo "array_unique(array_merge($a,$b)):";
// cribbed from http://oreilly.com/catalog/progphp/chapter/ch05.html
print_r (array_unique(array_merge($a,$b)));
?>
```
<br><br>//output<br><br>$a + $b : Array<br>(<br>    [0] =&gt; one<br>    [1] =&gt; two<br>    [2] =&gt; five<br>)<br>array_unique(array_merge(Array,Array)):Array<br>(<br>    [0] =&gt; one<br>    [1] =&gt; two<br>    [2] =&gt; three<br>    [3] =&gt; four<br>    [4] =&gt; five<br>)  

#

The example may get u into thinking that the identical operator returns true because the key of apple is a string but that is not the case, cause if a string array key is the standart representation of a integer it&apos;s gets a numeral key automaticly. <br><br>The identical operator just requires that the keys are in the same order in both arrays:<br><br>

```
<?php
$a = array (0 => "apple", 1 => "banana");
$b = array (1 => "banana", 0 => "apple");

var_dump($a === $b); // prints bool(false) as well

$b = array ("0" => "apple", "1" => "banana");

var_dump($a === $b); // prints bool(true)
?>
```
  

#

It should be mentioned that the array union operator functions almost identically to array_replace with the exception that precedence of arguments is reversed.  

#

Note that + will not renumber numeric array keys.  If you have two numeric arrays, and their indices overlap, + will use the first array&apos;s values for each numeric key, adding the 2nd array&apos;s values only where the first doesn&apos;t already have a value for that index.  Example:<br><br>$a = array(&apos;red&apos;, &apos;orange&apos;);<br>$b = array(&apos;yellow&apos;, &apos;green&apos;, &apos;blue&apos;);<br>$both = $a + $b;<br>var_dump($both);<br><br>Produces the output:<br><br>array(3) { [0]=&gt;  string(3) "red" [1]=&gt;  string(6) "orange" [2]=&gt;  string(4) "blue" }<br><br>To get a 5-element array, use array_merge.<br><br>    Dan  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.array.php)

**[To root](/README.md)**