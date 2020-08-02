# arsort



I have two servers; one running 5.6 and another that is running 7.  Using this function on the two servers gets me different results when all of the values are the same.  <br><br>

```
<?php

$list = json_decode('{"706":2,"703":2,"702":2,"696":2,"658":2}', true);

print_r($list);

arsort($list);
echo "<br>";

print_r($list);

?>
```
<br><br>PHP 5.6 results:<br>Array ( [706] =&gt; 2 [703] =&gt; 2 [702] =&gt; 2 [696] =&gt; 2 [658] =&gt; 2 ) <br>Array ( [658] =&gt; 2 [696] =&gt; 2 [702] =&gt; 2 [703] =&gt; 2 [706] =&gt; 2 )<br><br>PHP 7 results:<br>Array ( [706] =&gt; 2 [703] =&gt; 2 [702] =&gt; 2 [696] =&gt; 2 [658] =&gt; 2 ) <br>Array ( [706] =&gt; 2 [703] =&gt; 2 [702] =&gt; 2 [696] =&gt; 2 [658] =&gt; 2 )  

---

If you need to sort a multi-demension array, for example, an array such as <br><br>$TeamInfo[$TeamID]["WinRecord"] <br>$TeamInfo[$TeamID]["LossRecord"] <br>$TeamInfo[$TeamID]["TieRecord"] <br>$TeamInfo[$TeamID]["GoalDiff"]<br>$TeamInfo[$TeamID]["TeamPoints"] <br><br>and you have say, 100 teams here, and want to sort by "TeamPoints":<br><br>first, create your multi-dimensional array. Now, create another, single dimension array populated with the scores from the first array, and with indexes of corresponding team_id... ie<br>$foo[25] = 14<br>$foo[47] = 42<br>or whatever.<br>Now, asort or arsort the second array.<br>Since the array is now sorted by score or wins/losses or whatever you put in it, the indices are all hoopajooped.<br>If you just walk through the array, grabbing the index of each entry, (look at the asort example. that for loop does just that) then the index you get will point right back to one of the values of the multi-dimensional array.<br>Not sure if that&apos;s clear, but mail me if it isn&apos;t...<br>-mo  

---

[Official documentation page](https://www.php.net/manual/en/function.arsort.php)

**[To root](/README.md)**