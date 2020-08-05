# Spotting References



Hi,<br><br>If you want to check if two variables are referencing each other (i.e. point to the same memory) you can use a function like this:<br><br>

```
<?php

function same_type(&amp;$var1, &amp;$var2){
    return gettype($var1) === gettype($var2);
}

function is_ref(&amp;$var1, &amp;$var2) {
    //If a reference exists, the type IS the same
    if(!same_type($var1, $var2)) {
        return false;
    }

    $same = false;

    //We now only need to ask for var1 to be an array ;-)
    if(is_array($var1)) {
        //Look for an unused index in $var1
        do {
            $key = uniqid("is_ref_", true);
        } while(array_key_exists($key, $var1));

        //The two variables differ in content ... They can't be the same
        if(array_key_exists($key, $var2)) {
            return false;
        }

        //The arrays point to the same data if changes are reflected in $var2
        $data = uniqid("is_ref_data_", true);
        $var1[$key] =&amp; $data;
        //There seems to be a modification ...
        if(array_key_exists($key, $var2)) {
            if($var2[$key] === $data) {
                $same = true;
            }
        }

        //Undo our changes ...
        unset($var1[$key]);
    } elseif(is_object($var1)) {
        //The same objects are required to have equal class names ;-)
        if(get_class($var1) !== get_class($var2)) {
            return false;
        }

        $obj1 = array_keys(get_object_vars($var1));
        $obj2 = array_keys(get_object_vars($var2));

        //Look for an unused index in $var1
        do {
            $key = uniqid("is_ref_", true);
        } while(in_array($key, $obj1));

        //The two variables differ in content ... They can't be the same
        if(in_array($key, $obj2)) {
            return false;
        }

        //The arrays point to the same data if changes are reflected in $var2
        $data = uniqid("is_ref_data_", true);
        $var1->$key =&amp; $data;
        //There seems to be a modification ...
        if(isset($var2->$key)) {
            if($var2[$key] === $data) {
                $same = true;
            }
        }

        //Undo our changes ...
        unset($var1->$key);
    } elseif (is_resource($var1)) {
        if(get_resource_type($var1) !== get_resource_type($var2)) {
            return false;
        }

        return ((string) $var1) === ((string) $var2);
    } else {
        //Simple variables ...
        if($var1!==$var2) {
            //Data mismatch ... They can't be the same ...
            return false;
        }

        //To check for a reference of a variable with simple type
        //simply store its old value and check against modifications of the second variable ;-)

        do {
            $key = uniqid("is_ref_", true);
        } while($key === $var1);

        $tmp = $var1; //WE NEED A COPY HERE!!!
        $var1 = $key; //Set var1 to the value of $key (copy)
        $same = $var1 === $var2; //Check if $var2 was modified too ...
        $var1 = $tmp; //Undo our changes ...
    }

    return $same;
}

?>
```


Although this implementation is quite complete, it can't handle function references and some other minor stuff ATM.
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
echo "---\n";

$a = array();
var_dump(is_ref($a, $a)); //true

$a[] =&amp; $a;
var_dump(is_ref($a, $a[0])); // true
echo "---\n";

$b = array(array());
var_dump(is_ref($b, $b)); //true
var_dump(is_ref($b, $b[0])); //false
echo "---\n";

$b = array();
$b[] = $b;
var_dump(is_ref($b, $b)); //true
var_dump(is_ref($b, $b[0])); //false
var_dump(is_ref($b[0], $b[0][0])); //true*
echo "---\n";

var_dump($a);
var_dump($b);

?>
```
<br><br>* Please note the internal behaviour of PHP that seems to do the reference assignment BEFORE actually copying the variable!!! Thus you get an array containing a (different) recursive array for the last testcase, instead of an array containing an empty array as you could expect.<br><br>BenBE.  

---

[Official documentation page](https://www.php.net/manual/en/language.references.spot.php)

**[To root](/README.md)**