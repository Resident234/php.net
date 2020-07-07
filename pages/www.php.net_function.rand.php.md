# rand





quick way to generate randomish numbers and simple strings.
no messing around with functions, so you can just pop the line into the middle of your existing code.

not the most perfect for sure, but ok for plenty of situations...



```
<?php

$random_number = intval( &quot;0&quot; . rand(1,9) . rand(0,9) . rand(0,9) . rand(0,9) . rand(0,9) ); // random(ish) 5 digit int

$random_string = chr(rand(65,90)) . chr(rand(65,90)) . chr(rand(65,90)) . chr(rand(65,90)) . chr(rand(65,90)); // random(ish) 5 character string

?>
```


hope someone finds it useful for somthing.

regards,
deeeeeen alxndr0u

  

#



Here&apos;s an interesting note about the inferiority of the rand() function. Try, for example, the following code...



```
<?php
$r = array(0,0,0,0,0,0,0,0,0,0,0);
for ($i=0;$i&lt;1000000;$i++) {
&#xA0; $n = rand(0,100000);
&#xA0; if ($n&lt;=10) {
&#xA0; &#xA0; $r[$n]++;
&#xA0; }
}
print_r($r); 
?>
```


which produces something similar to the following output (on my windows box, where RAND_MAX is 32768):

Array
(
&#xA0; &#xA0; [0] =&gt; 31
&#xA0; &#xA0; [1] =&gt; 0
&#xA0; &#xA0; [2] =&gt; 0
&#xA0; &#xA0; [3] =&gt; 31
&#xA0; &#xA0; [4] =&gt; 0
&#xA0; &#xA0; [5] =&gt; 0
&#xA0; &#xA0; [6] =&gt; 30
&#xA0; &#xA0; [7] =&gt; 0
&#xA0; &#xA0; [8] =&gt; 0
&#xA0; &#xA0; [9] =&gt; 31
&#xA0; &#xA0; [10] =&gt; 0
)

Within this range only multiples of 3 are being selected. Also note that values that are filled are always 30 or 31 (no other values! really!) 

Now replace rand() with mt_rand() and see the difference...

Array
(
&#xA0; &#xA0; [0] =&gt; 8
&#xA0; &#xA0; [1] =&gt; 8
&#xA0; &#xA0; [2] =&gt; 14
&#xA0; &#xA0; [3] =&gt; 16
&#xA0; &#xA0; [4] =&gt; 9
&#xA0; &#xA0; [5] =&gt; 11
&#xA0; &#xA0; [6] =&gt; 8
&#xA0; &#xA0; [7] =&gt; 9
&#xA0; &#xA0; [8] =&gt; 7
&#xA0; &#xA0; [9] =&gt; 7
&#xA0; &#xA0; [10] =&gt; 9
)

Much more randomly distributed!

Conclusion: mt_rand() is not just faster, it is a far superior algorithm.

  

#

[Official documentation page](https://www.php.net/manual/en/function.rand.php)

**[To root](/README.md)**