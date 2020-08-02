# natsort



There&apos;s no need to include your own API code to natsort an associative array by key.  PHP&apos;s in-built functions (other than natsort) can do the job just fine:<br><br>

```
<?php
  uksort($myArray, "strnatcmp");
?>
```
  

---

About the reverse natsort.. Maybe simpler to do :<br><br>function strrnatcmp ($a, $b) {<br>    return strnatcmp ($b, $a);<br>}  

---

Be careful of the new behaviour in 5.2.10 version.<br>See the following sample:<br><br>

```
<?php

$array = array('1 bis', '10 ter', '0 PHP', '0', '01', '01 Ver', '0 ', '1 ', '1');

natsort($array);
echo '';
print_r($array);
echo '';
?>
```
<br><br>5.2.6-1 will output:<br>Array<br>(<br>    [3] =&gt; 0<br>    [6] =&gt; 0 <br>    [2] =&gt; 0 OP<br>    [4] =&gt; 01<br>    [5] =&gt; 01 Ver<br>    [8] =&gt; 1<br>    [7] =&gt; 1 <br>    [0] =&gt; 1 bis<br>    [1] =&gt; 10 ter<br>)<br><br>5.2.10 will output:<br>Array<br>(<br>    [6] =&gt; 0 <br>    [3] =&gt; 0<br>    [8] =&gt; 1<br>    [4] =&gt; 01<br>    [7] =&gt; 1 <br>    [5] =&gt; 01 Ver<br>    [0] =&gt; 1 bis<br>    [1] =&gt; 10 ter<br>    [2] =&gt; 0 OP<br>)<br><br>Greetings  

---

[Official documentation page](https://www.php.net/manual/en/function.natsort.php)

**[To root](/README.md)**