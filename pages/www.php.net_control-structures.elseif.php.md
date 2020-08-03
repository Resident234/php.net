# elseif/else if



The parser doesn&apos;t handle mixing alternative if syntaxes as reasonably as possible.<br><br>The following is illegal (as it should be):<br><br>

```
<?php
if($a):
    echo $a;
else {
    echo $c;
}
?>
```


This is also illegal (as it should be):



```
<?php
if($a) {
    echo $a;
}
else:
    echo $c;
endif;
?>
```


But since the two alternative if syntaxes are not interchangeable, it's reasonable to expect that the parser wouldn't try matching else statements using one style to if statement using the alternative style. In other words, one would expect that this would work:



```
<?php
if($a):
    echo $a;
    if($b) {
      echo $b;
    }
else:
    echo $c;
endif;
?>
```
<br><br>Instead of concluding that the else statement was intended to match the if($b) statement (and erroring out), the parser could match the else statement to the if($a) statement, which shares its syntax.<br><br>While it&apos;s understandable that the PHP developers don&apos;t consider this a bug, or don&apos;t consider it a bug worth their time, jsimlo was right to point out that mixing alternative if syntaxes might lead to unexpected results.  

---

[Official documentation page](https://www.php.net/manual/en/control-structures.elseif.php)

**[To root](/README.md)**