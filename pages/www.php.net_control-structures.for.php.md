# for





Looping through letters is possible. I&apos;m amazed at how few people know that.

for($col = &apos;R&apos;; $col != &apos;AD&apos;; $col++) {
&#xA0; &#xA0; echo $col.&apos; &apos;;
}

returns: R S T U V W X Y Z AA AB AC

Take note that you can&apos;t use $col &lt; &apos;AD&apos;. It only works with !=
Very convenient when working with excel columns.

  

#



The point about the speed in loops is, that the middle and the last expression are executed EVERY time it loops.

So you should try to take everything that doesn&apos;t change out of the loop.

Often you use a function to check the maximum of times it should loop. Like here:





```
<?php

for ($i = 0; $i &lt;= somewhat_calcMax(); $i++) {

&#xA0; somewhat_doSomethingWith($i);

}

?>
```




Faster would be:





```
<?php

$maxI = somewhat_calcMax();

for ($i = 0; $i &lt;= $maxI; $i++) {

&#xA0; somewhat_doSomethingWith($i);

}

?>
```




And here a little trick:





```
<?php

$maxI = somewhat_calcMax();

for ($i = 0; $i &lt;= $maxI; somewhat_doSomethingWith($i++)) ;

?>
```




The $i gets changed after the copy for the function (post-increment).

  

#



You can use strtotime with for loops to loop through dates



```
<?php
for ($date = strtotime(&quot;2014-01-01&quot;); $date &lt; strtotime(&quot;2014-02-01&quot;); $date = strtotime(&quot;+1 day&quot;, $date)) {
&#xA0; &#xA0; echo date(&quot;Y-m-d&quot;, $date).&quot;&lt;br /&gt;&quot;;
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.for.php)

**[To root](/README.md)**