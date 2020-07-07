# What References Are Not





What References are not: References.

References are opaque things that are like pointers, except A) smarter and B) referring to HLL objects, rather than memory addresses. PHP doesn&apos;t have references. PHP has a syntax for creating *aliases* which are multiple names for the same object. PHP has a few pieces of syntax for calling and returning &quot;by reference&quot;, which really just means inhibiting copying. At no point in this &quot;references&quot; section of the manual are there any references.

  

#



The example given in the text:



```
<?php
function foo(&amp;$var)
{
&#xA0; &#xA0; $var =&amp; $GLOBALS[&quot;baz&quot;];
}
foo($bar); 
?>
```


illustrates (to my mind anyway) why the = and &amp; should be written together as a new kind of replacement operator and not apart as in C, like&#xA0; $var = &amp;$GLOBALS[&quot;baz&quot;];

Using totally new terminology:

To me the result of this function is not surprising because the =&amp; means &apos;change the &quot;destination&quot; of $var from wherever it was to the same as the destination of $GLOBALS[&quot;baz&quot;]. Well it &apos;was&apos; the actual parameter $bar, but now it will be the global at &quot;baz&quot;.

If you simply remove the &amp; in the the replacement, it will place the value of $GLOBALS[&quot;baz&apos;] into the destination of $var, which is $bar (unless $bar was already a reference, then the value goes into that destination.)

To summarize, =, replaces the &apos;destination&apos;s value; =&amp;, changes the destination.

  

#

[Official documentation page](https://www.php.net/manual/en/language.references.arent.php)

**[To root](/README.md)**