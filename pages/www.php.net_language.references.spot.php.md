# Spotting References





Hi,

If you want to check if two variables are referencing each other (i.e. point to the same memory) you can use a function like this:



```
<?php

function same_type(&amp;$var1, &amp;$var2){
&#xA0; &#xA0; return gettype($var1) === gettype($var2);
}

function is_ref(&amp;$var1, &amp;$var2) {
&#xA0; &#xA0; //If a reference exists, the type IS the same
&#xA0; &#xA0; if(!same_type($var1, $var2)) {
&#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; }

&#xA0; &#xA0; $same = false;

&#xA0; &#xA0; //We now only need to ask for var1 to be an array ;-)
&#xA0; &#xA0; if(is_array($var1)) {
&#xA0; &#xA0; &#xA0; &#xA0; //Look for an unused index in $var1
&#xA0; &#xA0; &#xA0; &#xA0; do {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $key = uniqid(&quot;is_ref_&quot;, true);
&#xA0; &#xA0; &#xA0; &#xA0; } while(array_key_exists($key, $var1));

&#xA0; &#xA0; &#xA0; &#xA0; //The two variables differ in content ... They can&apos;t be the same
&#xA0; &#xA0; &#xA0; &#xA0; if(array_key_exists($key, $var2)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; //The arrays point to the same data if changes are reflected in $var2
&#xA0; &#xA0; &#xA0; &#xA0; $data = uniqid(&quot;is_ref_data_&quot;, true);
&#xA0; &#xA0; &#xA0; &#xA0; $var1[$key] =&amp; $data;
&#xA0; &#xA0; &#xA0; &#xA0; //There seems to be a modification ...
&#xA0; &#xA0; &#xA0; &#xA0; if(array_key_exists($key, $var2)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if($var2[$key] === $data) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $same = true;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; //Undo our changes ...
&#xA0; &#xA0; &#xA0; &#xA0; unset($var1[$key]);
&#xA0; &#xA0; } elseif(is_object($var1)) {
&#xA0; &#xA0; &#xA0; &#xA0; //The same objects are required to have equal class names ;-)
&#xA0; &#xA0; &#xA0; &#xA0; if(get_class($var1) !== get_class($var2)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; $obj1 = array_keys(get_object_vars($var1));
&#xA0; &#xA0; &#xA0; &#xA0; $obj2 = array_keys(get_object_vars($var2));

&#xA0; &#xA0; &#xA0; &#xA0; //Look for an unused index in $var1
&#xA0; &#xA0; &#xA0; &#xA0; do {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $key = uniqid(&quot;is_ref_&quot;, true);
&#xA0; &#xA0; &#xA0; &#xA0; } while(in_array($key, $obj1));

&#xA0; &#xA0; &#xA0; &#xA0; //The two variables differ in content ... They can&apos;t be the same
&#xA0; &#xA0; &#xA0; &#xA0; if(in_array($key, $obj2)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; //The arrays point to the same data if changes are reflected in $var2
&#xA0; &#xA0; &#xA0; &#xA0; $data = uniqid(&quot;is_ref_data_&quot;, true);
&#xA0; &#xA0; &#xA0; &#xA0; $var1-&gt;$key =&amp; $data;
&#xA0; &#xA0; &#xA0; &#xA0; //There seems to be a modification ...
&#xA0; &#xA0; &#xA0; &#xA0; if(isset($var2-&gt;$key)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if($var2[$key] === $data) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $same = true;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; //Undo our changes ...
&#xA0; &#xA0; &#xA0; &#xA0; unset($var1-&gt;$key);
&#xA0; &#xA0; } elseif (is_resource($var1)) {
&#xA0; &#xA0; &#xA0; &#xA0; if(get_resource_type($var1) !== get_resource_type($var2)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; return ((string) $var1) === ((string) $var2);
&#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; //Simple variables ...
&#xA0; &#xA0; &#xA0; &#xA0; if($var1!==$var2) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //Data mismatch ... They can&apos;t be the same ...
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; //To check for a reference of a variable with simple type
&#xA0; &#xA0; &#xA0; &#xA0; //simply store its old value and check against modifications of the second variable ;-)

&#xA0; &#xA0; &#xA0; &#xA0; do {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $key = uniqid(&quot;is_ref_&quot;, true);
&#xA0; &#xA0; &#xA0; &#xA0; } while($key === $var1);

&#xA0; &#xA0; &#xA0; &#xA0; $tmp = $var1; //WE NEED A COPY HERE!!!
&#xA0; &#xA0; &#xA0; &#xA0; $var1 = $key; //Set var1 to the value of $key (copy)
&#xA0; &#xA0; &#xA0; &#xA0; $same = $var1 === $var2; //Check if $var2 was modified too ...
&#xA0; &#xA0; &#xA0; &#xA0; $var1 = $tmp; //Undo our changes ...
&#xA0; &#xA0; }

&#xA0; &#xA0; return $same;
}

?>
```


Although this implementation is quite complete, it can&apos;t handle function references and some other minor stuff ATM.
This function is especially useful if you want to serialize a recursive array by hand.

The usage is something like:


```
<?php
$a = 5;
$b = 5;
var_dump(is_ref($a, $b)); //false

$a = 5;
$b = $a;
var_dump(is_ref($a, $b)); //false

$a = 5;
$b =&amp; $a;
var_dump(is_ref($a, $b)); //true
echo &quot;---\n&quot;;

$a = array();
var_dump(is_ref($a, $a)); //true

$a[] =&amp; $a;
var_dump(is_ref($a, $a[0])); // true
echo &quot;---\n&quot;;

$b = array(array());
var_dump(is_ref($b, $b)); //true
var_dump(is_ref($b, $b[0])); //false
echo &quot;---\n&quot;;

$b = array();
$b[] = $b;
var_dump(is_ref($b, $b)); //true
var_dump(is_ref($b, $b[0])); //false
var_dump(is_ref($b[0], $b[0][0])); //true*
echo &quot;---\n&quot;;

var_dump($a);
var_dump($b);

?>
```


* Please note the internal behaviour of PHP that seems to do the reference assignment BEFORE actually copying the variable!!! Thus you get an array containing a (different) recursive array for the last testcase, instead of an array containing an empty array as you could expect.

BenBE.

  

#

[Official documentation page](https://www.php.net/manual/en/language.references.spot.php)

**[To root](/README.md)**