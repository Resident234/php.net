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
<?php<br><br># (1) Objects are always passed by reference and returned by reference<br><br>class Obj {<br>    public $x;<br>}<br><br>function obj_inc_x($obj) {<br>    $obj-&gt;x++;<br>    return $obj;<br>}<br><br>$obj = new Obj();<br>$obj-&gt;x = 1;<br><br>$obj2 = obj_inc_x($obj);<br>obj_inc_x($obj2);<br><br>print $obj-&gt;x . &apos;, &apos; . $obj2-&gt;x . "\n";<br><br># (2) Scalars are not passed by reference or returned as such<br><br>function scalar_inc_x($x) {<br>    $x++;<br>    return $x;<br>}<br><br>$x = 1;<br><br>$x2 = scalar_inc_x($x);<br>scalar_inc_x($x2);<br><br>print $x . &apos;, &apos; . $x2 . "\n";<br><br># (3) You have to force pass by reference and return by reference on scalars<br><br>function &amp;scalar_ref_inc_x(&amp;$x) {<br>    $x++;<br>    return $x;<br>}<br><br>$x = 1;<br><br>$x2 =&amp; scalar_ref_inc_x($x);    # Need reference here as well as the function sig<br>scalar_ref_inc_x($x2);<br><br>print $x . &apos;, &apos; . $x2 . "\n";<br><br># (4) Arrays use pass by value sematics just like scalars<br><br>function array_inc_x($array) {<br>    $array{&apos;x&apos;}++;<br>    return $array;<br>}<br><br>$array = array();<br>$array[&apos;x&apos;] = 1;<br><br>$array2 = array_inc_x($array);<br>array_inc_x($array2);<br><br>print $array[&apos;x&apos;] . &apos;, &apos; . $array2[&apos;x&apos;] . "\n";<br><br># (5) You have to force pass by reference and return by reference on arrays<br><br>function &amp;array_ref_inc_x(&amp;$array) {<br>    $array{&apos;x&apos;}++;<br>    return $array;<br>}<br><br>$array = array();<br>$array[&apos;x&apos;] = 1;<br><br>$array2 =&amp; array_ref_inc_x($array); # Need reference here as well as the function sig<br>array_ref_inc_x($array2);<br><br>print $array[&apos;x&apos;] . &apos;, &apos; . $array2[&apos;x&apos;] . "\n";  

#

Be careful about using "do this thing or die()" logic in your return lines.  It doesn&apos;t work as you&apos;d expect:<br><br>

```
<?php
function myfunc1() {
    return(&apos;thingy&apos; or die(&apos;otherthingy&apos;));
}
function myfunc2() {
    return &apos;thingy&apos; or die(&apos;otherthingy&apos;);
}
function myfunc3() {
    return(&apos;thingy&apos;) or die(&apos;otherthingy&apos;);
}
function myfunc4() {
    return &apos;thingy&apos; or &apos;otherthingy&apos;;
}
function myfunc5() {
    $x = &apos;thingy&apos; or &apos;otherthingy&apos;; return $x;
}
echo myfunc1(). "\n". myfunc2(). "\n". myfunc3(). "\n". myfunc4(). "\n". myfunc5(). "\n";
?>
```
<br><br>Only myfunc5() returns &apos;thingy&apos; - the rest return 1.  

#

[Official documentation page](https://www.php.net/manual/en/functions.returning-values.php)

**[To root](/README.md)**