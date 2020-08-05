# range



To create a range array like<br><br>Array<br>(<br>    [11] =&gt; 1<br>    [12] =&gt; 2<br>    [13] =&gt; 3<br>    [14] =&gt; 4<br>)<br><br>combine two range arrays using array_combine:<br><br>array_combine(range(11,14),range(1,4))  

---

So with the introduction of single-character ranges to the range() function, the internal function tries to be "smart", and (I am inferring from behavior here) apparently checks the type of the incoming values. If one is numeric, including numeric string, then the other is treated as numeric; if it is a non-numeric string, it is treated as zero. <br><br>But.<br><br>If you pass in a numeric string in such a way that is is forced to be recognized as type string and not type numeric, range() will function quite differently.<br><br>Compare:<br><br>

```
<?php
echo implode("",range(9,"Q"));
// prints 9876543210

echo implode("",range("9 ","Q"));  //space after the 9
// prints 9:;<=>?@ABCDEFGHIJKLMNOPQ

echo implode("",range("q","9 "));
// prints qponmlkjihgfedcba`_^]\[ZYXWVUTSRQPONMLKJIHGFEDCBA@?>=<;:987654
?>
```
<br><br>I wouldn&apos;t call this a bug, because IMO it is even more useful than the stock usage of the function.  

---

foreach(range()) whilst efficiant in other languages, such as python, it is not (compared to a for) in php*.<br><br>php is a C-inspired language and thus for is entirely in-keeping with the lanuage aethetic to use it<br><br>

```
<?php
//efficiant
for($i = $start; $i < $end; $i+=$step) 
{
        //do something with array 
}

//inefficiant
foreach(range($start, $end, $step) as $i)
{
        //do something with array 
}
?>
```
<br><br>That the officiant documentation doesnt mention the for loop is strange.<br><br>Note however, that in PHP5 foreach is faster than for when iterating without incrementing a variable.<br><br>* My tests using microtime and 100 000 iterations consistently (~10 times) show that for is 4x faster than foreach(range()).  

---

So, I needed a quick and dirty way to create a dropdown select for hours, minutes and seconds using 2 digit formatting, and to create those arrays of data, I combined range with array merge..<br><br>

```
<?php
$prepend = array('00','01','02','03','04','05','06','07','08','09');
$hours     = array_merge($prepend,range(10, 23));
$minutes     = array_merge($prepend,range(10, 59));
$seconds     = $minutes;
?>
```
<br><br>Super simple.  

---

[Official documentation page](https://www.php.net/manual/en/function.range.php)

**[To root](/README.md)**