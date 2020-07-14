# rand



quick way to generate randomish numbers and simple strings.<br>no messing around with functions, so you can just pop the line into the middle of your existing code.<br><br>not the most perfect for sure, but ok for plenty of situations...<br><br>

```
<?php

$random_number = intval( "0" . rand(1,9) . rand(0,9) . rand(0,9) . rand(0,9) . rand(0,9) ); // random(ish) 5 digit int

$random_string = chr(rand(65,90)) . chr(rand(65,90)) . chr(rand(65,90)) . chr(rand(65,90)) . chr(rand(65,90)); // random(ish) 5 character string

?>
```
<br><br>hope someone finds it useful for somthing.<br><br>regards,<br>deeeeeen alxndr0u  

#

Here&apos;s an interesting note about the inferiority of the rand() function. Try, for example, the following code...<br><br>

```
<?php
$r = array(0,0,0,0,0,0,0,0,0,0,0);
for ($i=0;$i&lt;1000000;$i++) {
  $n = rand(0,100000);
  if ($n&lt;=10) {
    $r[$n]++;
  }
}
print_r($r); 
?>
```
<br><br>which produces something similar to the following output (on my windows box, where RAND_MAX is 32768):<br><br>Array<br>(<br>    [0] =&gt; 31<br>    [1] =&gt; 0<br>    [2] =&gt; 0<br>    [3] =&gt; 31<br>    [4] =&gt; 0<br>    [5] =&gt; 0<br>    [6] =&gt; 30<br>    [7] =&gt; 0<br>    [8] =&gt; 0<br>    [9] =&gt; 31<br>    [10] =&gt; 0<br>)<br><br>Within this range only multiples of 3 are being selected. Also note that values that are filled are always 30 or 31 (no other values! really!) <br><br>Now replace rand() with mt_rand() and see the difference...<br><br>Array<br>(<br>    [0] =&gt; 8<br>    [1] =&gt; 8<br>    [2] =&gt; 14<br>    [3] =&gt; 16<br>    [4] =&gt; 9<br>    [5] =&gt; 11<br>    [6] =&gt; 8<br>    [7] =&gt; 9<br>    [8] =&gt; 7<br>    [9] =&gt; 7<br>    [10] =&gt; 9<br>)<br><br>Much more randomly distributed!<br><br>Conclusion: mt_rand() is not just faster, it is a far superior algorithm.  

#

[Official documentation page](https://www.php.net/manual/en/function.rand.php)

**[To root](/README.md)**