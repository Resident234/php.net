# Returning values



PHP 7.1 allows for void and null return types by preceding the type declaration with a ? -- (e.g. function canReturnNullorString(): ?string) <br><br>However resource is not allowed as a return type:<br><br>

```
<?php
function fileOpen(string $fileName, string $mode): resource
{
    $handle = fopen($fileName, $mode);
    if ($handle !== false)
    {
        return $handle;
    }
}

$resourceHandle = fileOpen("myfile.txt", "r");
?>
```
<br><br>Errors with:<br>Fatal error: Uncaught TypeError: Return value of fileOpen() must be an instance of resource, resource returned.  

#

Developers with a C background may expect pass by reference semantics for arrays. It may be surprising that  pass by value is used for arrays just like scalars. Objects are implicitly passed by reference.<br><br>

```
<?php

# (1) Objects are always passed by reference and returned by reference

class Obj {
    public $x;
}

function obj_inc_x($obj) {
    $obj->x++;
    return $obj;
}

$obj = new Obj();
$obj->x = 1;

$obj2 = obj_inc_x($obj);
obj_inc_x($obj2);

print $obj->x . ', ' . $obj2->x . "\n";

# (2) Scalars are not passed by reference or returned as such

function scalar_inc_x($x) {
    $x++;
    return $x;
}

$x = 1;

$x2 = scalar_inc_x($x);
scalar_inc_x($x2);

print $x . ', ' . $x2 . "\n";

# (3) You have to force pass by reference and return by reference on scalars

function &amp;scalar_ref_inc_x(&amp;$x) {
    $x++;
    return $x;
}

$x = 1;

$x2 =&amp; scalar_ref_inc_x($x);    # Need reference here as well as the function sig
scalar_ref_inc_x($x2);

print $x . ', ' . $x2 . "\n";

# (4) Arrays use pass by value sematics just like scalars

function array_inc_x($array) {
    $array{'x'}++;
    return $array;
}

$array = array();
$array['x'] = 1;

$array2 = array_inc_x($array);
array_inc_x($array2);

print $array['x'] . ', ' . $array2['x'] . "\n";

# (5) You have to force pass by reference and return by reference on arrays

function &amp;array_ref_inc_x(&amp;$array) {
    $array{'x'}++;
    return $array;
}

$array = array();
$array['x'] = 1;

$array2 =&amp; array_ref_inc_x($array); # Need reference here as well as the function sig
array_ref_inc_x($array2);

print $array['x'] . ', ' . $array2['x'] . "\n";?>
```
  

#

Be careful about using "do this thing or die()" logic in your return lines.  It doesn&apos;t work as you&apos;d expect:<br><br>

```
<?php
function myfunc1() {
    return('thingy' or die('otherthingy'));
}
function myfunc2() {
    return 'thingy' or die('otherthingy');
}
function myfunc3() {
    return('thingy') or die('otherthingy');
}
function myfunc4() {
    return 'thingy' or 'otherthingy';
}
function myfunc5() {
    $x = 'thingy' or 'otherthingy'; return $x;
}
echo myfunc1(). "\n". myfunc2(). "\n". myfunc3(). "\n". myfunc4(). "\n". myfunc5(). "\n";
?>
```
<br><br>Only myfunc5() returns &apos;thingy&apos; - the rest return 1.  

#

[Official documentation page](https://www.php.net/manual/en/functions.returning-values.php)

**[To root](/README.md)**