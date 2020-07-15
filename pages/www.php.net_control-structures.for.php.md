# for



Looping through letters is possible. I&apos;m amazed at how few people know that.<br><br>for($col = &apos;R&apos;; $col != &apos;AD&apos;; $col++) {<br>    echo $col.&apos; &apos;;<br>}<br><br>returns: R S T U V W X Y Z AA AB AC<br><br>Take note that you can&apos;t use $col &lt; &apos;AD&apos;. It only works with !=<br>Very convenient when working with excel columns.  

#

The point about the speed in loops is, that the middle and the last expression are executed EVERY time it loops.<br>So you should try to take everything that doesn&apos;t change out of the loop.<br>Often you use a function to check the maximum of times it should loop. Like here:<br><br>

```
<?php
for ($i = 0; $i <= somewhat_calcMax(); $i++) {
  somewhat_doSomethingWith($i);
}
?>
```


Faster would be:



```
<?php
$maxI = somewhat_calcMax();
for ($i = 0; $i <= $maxI; $i++) {
  somewhat_doSomethingWith($i);
}
?>
```


And here a little trick:



```
<?php
$maxI = somewhat_calcMax();
for ($i = 0; $i <= $maxI; somewhat_doSomethingWith($i++)) ;
?>
```
<br><br>The $i gets changed after the copy for the function (post-increment).  

#

You can use strtotime with for loops to loop through dates<br><br>

```
<?php
for ($date = strtotime("2014-01-01"); $date < strtotime("2014-02-01"); $date = strtotime("+1 day", $date)) {
    echo date("Y-m-d", $date)."<br />";
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.for.php)

**[To root](/README.md)**