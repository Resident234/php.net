# preg_quote



Wondering why your preg_replace fails, even if you have used preg_quote?<br><br>Try adding the delimiter / - preg_quote($string, &apos;/&apos;);  

#

To escape characters with special meaning, like: .-[]() and so on, use \Q and \E.<br><br>For example:<br><br>

```
<?php echo ( preg_match(&apos;/^&apos;.( $myvar = &apos;te.t&apos; ).&apos;$/i&apos;, &apos;test&apos;) ? &apos;match&apos; : &apos;nomatch&apos; ); ?>
```


Will result in: match

But:



```
<?php echo ( preg_match(&apos;/^\Q&apos;.( $myvar = &apos;te.t&apos; ).&apos;\E$/i&apos;, &apos;test&apos;) ? &apos;match&apos; : &apos;nomatch&apos; ); ?>
```
<br><br>Will result in: nomatch  

#

[Official documentation page](https://www.php.net/manual/en/function.preg-quote.php)

**[To root](/README.md)**