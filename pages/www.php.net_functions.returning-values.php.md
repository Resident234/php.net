# Returning values





PHP 7.1 allows for void and null return types by preceding the type declaration with a ? -- (e.g. function canReturnNullorString(): ?string) 

However resource is not allowed as a return type:



```
<?php
function fileOpen(string $fileName, string $mode): resource
{
&#xA0; &#xA0; $handle = fopen($fileName, $mode);
&#xA0; &#xA0; if ($handle !== false)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; return $handle;
&#xA0; &#xA0; }
}

$resourceHandle = fileOpen(&quot;myfile.txt&quot;, &quot;r&quot;);
?>
```


Errors with:
Fatal error: Uncaught TypeError: Return value of fileOpen() must be an instance of resource, resource returned.

  

#



Developers with a C background may expect pass by reference semantics for arrays. It may be surprising that&#xA0; pass by value is used for arrays just like scalars. Objects are implicitly passed by reference.



```
<?php

# (1) Objects are always passed by reference and returned by reference

class Obj {
&#xA0; &#xA0; public $x;
}

function obj_inc_x($obj) {
&#xA0; &#xA0; $obj-&gt;x++;
&#xA0; &#xA0; return $obj;
}

$obj = new Obj();
$obj-&gt;x = 1;

$obj2 = obj_inc_x($obj);
obj_inc_x($obj2);

print $obj-&gt;x . &apos;, &apos; . $obj2-&gt;x . &quot;\n&quot;;

# (2) Scalars are not passed by reference or returned as such

function scalar_inc_x($x) {
&#xA0; &#xA0; $x++;
&#xA0; &#xA0; return $x;
}

$x = 1;

$x2 = scalar_inc_x($x);
scalar_inc_x($x2);

print $x . &apos;, &apos; . $x2 . &quot;\n&quot;;

# (3) You have to force pass by reference and return by reference on scalars

function &amp;scalar_ref_inc_x(&amp;$x) {
&#xA0; &#xA0; $x++;
&#xA0; &#xA0; return $x;
}

$x = 1;

$x2 =&amp; scalar_ref_inc_x($x);&#xA0; &#xA0; # Need reference here as well as the function sig
scalar_ref_inc_x($x2);

print $x . &apos;, &apos; . $x2 . &quot;\n&quot;;

# (4) Arrays use pass by value sematics just like scalars

function array_inc_x($array) {
&#xA0; &#xA0; $array{&apos;x&apos;}++;
&#xA0; &#xA0; return $array;
}

$array = array();
$array[&apos;x&apos;] = 1;

$array2 = array_inc_x($array);
array_inc_x($array2);

print $array[&apos;x&apos;] . &apos;, &apos; . $array2[&apos;x&apos;] . &quot;\n&quot;;

# (5) You have to force pass by reference and return by reference on arrays

function &amp;array_ref_inc_x(&amp;$array) {
&#xA0; &#xA0; $array{&apos;x&apos;}++;
&#xA0; &#xA0; return $array;
}

$array = array();
$array[&apos;x&apos;] = 1;

$array2 =&amp; array_ref_inc_x($array); # Need reference here as well as the function sig
array_ref_inc_x($array2);

print $array[&apos;x&apos;] . &apos;, &apos; . $array2[&apos;x&apos;] . &quot;\n&quot;;


  

#



Be careful about using &quot;do this thing or die()&quot; logic in your return lines.&#xA0; It doesn&apos;t work as you&apos;d expect:



```
<?php
function myfunc1() {
&#xA0; &#xA0; return(&apos;thingy&apos; or die(&apos;otherthingy&apos;));
}
function myfunc2() {
&#xA0; &#xA0; return &apos;thingy&apos; or die(&apos;otherthingy&apos;);
}
function myfunc3() {
&#xA0; &#xA0; return(&apos;thingy&apos;) or die(&apos;otherthingy&apos;);
}
function myfunc4() {
&#xA0; &#xA0; return &apos;thingy&apos; or &apos;otherthingy&apos;;
}
function myfunc5() {
&#xA0; &#xA0; $x = &apos;thingy&apos; or &apos;otherthingy&apos;; return $x;
}
echo myfunc1(). &quot;\n&quot;. myfunc2(). &quot;\n&quot;. myfunc3(). &quot;\n&quot;. myfunc4(). &quot;\n&quot;. myfunc5(). &quot;\n&quot;;
?>
```


Only myfunc5() returns &apos;thingy&apos; - the rest return 1.

  

#

[Official documentation page](https://www.php.net/manual/en/functions.returning-values.php)

**[To root](/README.md)**