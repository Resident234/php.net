# What References Are



it just likes a person who has two different names.  

#

Unlike in C, PHP references are not treated as pre-dereferenced pointers, but as complete aliases.<br><br>The data that they are aliasing ("referencing") will not become available for garbage collection until all references to it have been removed. <br><br>"Regular" variables are themselves considered references, and are not treated differently from variables assigned using =&amp; for the purposes of garbage collection.<br><br>The following examples are provided for clarification.<br><br>1) When treated as a variable containing a value, references behave as expected. However, they are in fact objects that *reference* the original data.<br><br>

```
<?php 
var = "foo";
$ref1 =&amp; $var; // new object that references $var
$ref2 =&amp; $ref1; // references $var directly, not $ref1!!!!!

echo $ref; // >foo

unset($ref);

echo $ref1; // >Notice:  Undefined variable: ref1
echo $ref2; // >foo
echo $var; // >foo
?>
```


2) When accessed via reference, the original data will not be removed until *all* references to it have been removed. This includes both references and "regular" variables assigned without the &amp; operator, and there are no distinctions made between the two for the purpose of garbage collection.



```
<?php 
$var = "foo";
$ref =&amp; $var;

unset($var);

echo $var; // >Notice:  Undefined variable: var
echo $ref; // >foo
?>
```


3) To remove the original data without removing all references to it, simply set it to null.



```
<?php 
$var = "foo";
$ref =&amp; $var;

$ref = NULL;

echo $var; // Value is NULL, so nothing prints
echo $ref; // Value is NULL, so nothing prints
?>
```
<br><br>4) Placing data in an array also counts as adding one more reference to it, for the purposes of garbage collection.<br><br>For more info, see http://php.net/manual/en/features.gc.refcounting-basics.php  

#

[Official documentation page](https://www.php.net/manual/en/language.references.whatare.php)

**[To root](/README.md)**