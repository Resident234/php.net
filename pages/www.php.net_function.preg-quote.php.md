# preg_quote



Wondering why your preg_replace fails, even if you have used preg_quote?<br><br>Try adding the delimiter / - preg_quote($string, &apos;/&apos;);  

---

To escape characters with special meaning, like: .-[]() and so on, use \Q and \E.<br><br>For example:<br><br>

```
<?php echo ( preg_match('/^'.( $myvar = 'te.t' ).'$/i', 'test') ? 'match' : 'nomatch' ); ?>
```


Will result in: match

But:



```
<?php echo ( preg_match('/^\Q'.( $myvar = 'te.t' ).'\E$/i', 'test') ? 'match' : 'nomatch' ); ?>
```
<br><br>Will result in: nomatch  

---

[Official documentation page](https://www.php.net/manual/en/function.preg-quote.php)

**[To root](/README.md)**