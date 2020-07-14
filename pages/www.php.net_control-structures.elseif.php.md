# elseif/else if



The parser doesn&apos;t handle mixing alternative if syntaxes as reasonably as possible.<br><br>The following is illegal (as it should be):<br><br>&lt;?<br>if($a):<br>    echo $a;<br>else {<br>    echo $c;<br>}<br>?>
```
<br><br>This is also illegal (as it should be):<br><br>&lt;?<br>if($a) {<br>    echo $a;<br>}<br>else:<br>    echo $c;<br>endif;<br>?>
```
<br><br>But since the two alternative if syntaxes are not interchangeable, it&apos;s reasonable to expect that the parser wouldn&apos;t try matching else statements using one style to if statement using the alternative style. In other words, one would expect that this would work:<br><br>&lt;?<br>if($a):<br>    echo $a;<br>    if($b) {<br>      echo $b;<br>    }<br>else:<br>    echo $c;<br>endif;<br>?>
```
<br><br>Instead of concluding that the else statement was intended to match the if($b) statement (and erroring out), the parser could match the else statement to the if($a) statement, which shares its syntax.<br><br>While it&apos;s understandable that the PHP developers don&apos;t consider this a bug, or don&apos;t consider it a bug worth their time, jsimlo was right to point out that mixing alternative if syntaxes might lead to unexpected results.  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.elseif.php)

**[To root](/README.md)**