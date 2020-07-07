# elseif/else if





The parser doesn&apos;t handle mixing alternative if syntaxes as reasonably as possible.

The following is illegal (as it should be):

&lt;?
if($a):
&#xA0; &#xA0; echo $a;
else {
&#xA0; &#xA0; echo $c;
}
?>
```


This is also illegal (as it should be):

&lt;?
if($a) {
&#xA0; &#xA0; echo $a;
}
else:
&#xA0; &#xA0; echo $c;
endif;
?>
```


But since the two alternative if syntaxes are not interchangeable, it&apos;s reasonable to expect that the parser wouldn&apos;t try matching else statements using one style to if statement using the alternative style. In other words, one would expect that this would work:

&lt;?
if($a):
&#xA0; &#xA0; echo $a;
&#xA0; &#xA0; if($b) {
&#xA0; &#xA0; &#xA0; echo $b;
&#xA0; &#xA0; }
else:
&#xA0; &#xA0; echo $c;
endif;
?>
```


Instead of concluding that the else statement was intended to match the if($b) statement (and erroring out), the parser could match the else statement to the if($a) statement, which shares its syntax.

While it&apos;s understandable that the PHP developers don&apos;t consider this a bug, or don&apos;t consider it a bug worth their time, jsimlo was right to point out that mixing alternative if syntaxes might lead to unexpected results.

  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.elseif.php)

**[To root](/README.md)**