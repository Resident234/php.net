# What References Are





it just likes a person who has two different names.

  

#



Unlike in C, PHP references are not treated as pre-dereferenced pointers, but as complete aliases.

The data that they are aliasing (&quot;referencing&quot;) will not become available for garbage collection until all references to it have been removed. 

&quot;Regular&quot; variables are themselves considered references, and are not treated differently from variables assigned using =&amp; for the purposes of garbage collection.

The following examples are provided for clarification.

1) When treated as a variable containing a value, references behave as expected. However, they are in fact objects that *reference* the original data.



```
<?php 
var = &quot;foo&quot;;
$ref1 =&amp; $var; // new object that references $var
$ref2 =&amp; $ref1; // references $var directly, not $ref1!!!!!

echo $ref; // &gt;foo

unset($ref);

echo $ref1; // &gt;Notice:&#xA0; Undefined variable: ref1
echo $ref2; // &gt;foo
echo $var; // &gt;foo
?>
```


2) When accessed via reference, the original data will not be removed until *all* references to it have been removed. This includes both references and &quot;regular&quot; variables assigned without the &amp; operator, and there are no distinctions made between the two for the purpose of garbage collection.



```
<?php 
$var = &quot;foo&quot;;
$ref =&amp; $var;

unset($var);

echo $var; // &gt;Notice:&#xA0; Undefined variable: var
echo $ref; // &gt;foo
?>
```


3) To remove the original data without removing all references to it, simply set it to null.



```
<?php 
$var = &quot;foo&quot;;
$ref =&amp; $var;

$ref = NULL;

echo $var; // Value is NULL, so nothing prints
echo $ref; // Value is NULL, so nothing prints
?>
```


4) Placing data in an array also counts as adding one more reference to it, for the purposes of garbage collection.

For more info, see http://php.net/manual/en/features.gc.refcounting-basics.php

  

#

[Official documentation page](https://www.php.net/manual/en/language.references.whatare.php)

**[To root](/README.md)**