# What References Are Not



What References are not: References.<br><br>References are opaque things that are like pointers, except A) smarter and B) referring to HLL objects, rather than memory addresses. PHP doesn&apos;t have references. PHP has a syntax for creating *aliases* which are multiple names for the same object. PHP has a few pieces of syntax for calling and returning "by reference", which really just means inhibiting copying. At no point in this "references" section of the manual are there any references.  

#

The example given in the text:<br><br>

```
<?php
function foo(&amp;$var)
{
    $var =&amp; $GLOBALS["baz"];
}
foo($bar); 
?>
```
<br><br>illustrates (to my mind anyway) why the = and &amp; should be written together as a new kind of replacement operator and not apart as in C, like  $var = &amp;$GLOBALS["baz"];<br><br>Using totally new terminology:<br><br>To me the result of this function is not surprising because the =&amp; means &apos;change the "destination" of $var from wherever it was to the same as the destination of $GLOBALS["baz"]. Well it &apos;was&apos; the actual parameter $bar, but now it will be the global at "baz".<br><br>If you simply remove the &amp; in the the replacement, it will place the value of $GLOBALS["baz&apos;] into the destination of $var, which is $bar (unless $bar was already a reference, then the value goes into that destination.)<br><br>To summarize, =, replaces the &apos;destination&apos;s value; =&amp;, changes the destination.  

#

[Official documentation page](https://www.php.net/manual/en/language.references.arent.php)

**[To root](/README.md)**