# isset



I, too, was dismayed to find that isset($foo) returns false if ($foo == null). Here&apos;s an (awkward) way around it.<br><br>unset($foo);<br>if (compact(&apos;foo&apos;) != array()) {<br>  do_your_thing();<br>}<br><br>Of course, that is very non-intuitive, long, hard-to-understand, and kludgy. Better to design your code so you don&apos;t depend on the difference between an unset variable and a variable with the value null. But "better" only because PHP has made this weird development choice.<br><br>In my thinking this was a mistake in the development of PHP. The name ("isset") should describe the function and not have the desciption be "is set AND is not null". If it was done properly a programmer could very easily do (isset($var) || is_null($var)) if they wanted to check for this!<br><br>A variable set to null is a different state than a variable not set - there should be some easy way to differentiate. Just my (pointless) $0.02.  

#

"empty() is the opposite of (boolean) var, except that no warning is generated when the variable is not set."<br><br>So essentially<br>

```
<?php
if (isset($var) &amp;&amp; $var)
?>
```

is the same as


```
<?php
if (!empty($var))
?>
```
<br>doesn&apos;t it? :)<br><br>!empty() mimics the chk() function posted before.  

#

You can safely use isset to check properties and subproperties of objects directly. So instead of writing<br><br>    isset($abc) &amp;&amp; isset($abc-&gt;def) &amp;&amp; isset($abc-&gt;def-&gt;ghi)<br><br>or in a shorter form<br><br>    isset($abc, $abc-&gt;def, $abc-&gt;def-&gt;ghi)<br><br>you can just write<br><br>    isset ($abc-&gt;def-&gt;ghi)<br><br>without raising any errors, warnings or notices.<br><br>Examples<br>

```
<?php
    $abc = (object) array("def" => 123);
    var_dump(isset($abc));                // bool(true)
    var_dump(isset($abc->def));           // bool(true)
    var_dump(isset($abc->def->ghi));      // bool(false)
    var_dump(isset($abc->def->ghi->jkl)); // bool(false)
    var_dump(isset($def));                // bool(false)
    var_dump(isset($def->ghi));           // bool(false)
    var_dump(isset($def->ghi->jkl));      // bool(false)

    var_dump($abc);                       // object(stdClass)#1 (1) { ["def"] => int(123) }
    var_dump($abc->def);                  // int(123)
    var_dump($abc->def->ghi);             // null / E_NOTICE: Trying to get property of non-object
    var_dump($abc->def->ghi->jkl);        // null / E_NOTICE: Trying to get property of non-object
    var_dump($def);                       // null / E_NOTICE: Trying to get property of non-object
    var_dump($def->ghi);                  // null / E_NOTICE: Trying to get property of non-object
    var_dump($def->ghi->jkl);             // null / E_NOTICE: Trying to get property of non-object
?>
```
  

#

How to test for a variable actually existing, including being set to null. This will prevent errors when passing to functions.<br><br>

```
<?php
// false
var_export(
  array_key_exists('myvar', get_defined_vars())
);

$myvar;
// false
var_export(
  array_key_exists('myvar', get_defined_vars())
);

$myvar = null;
// true
var_export(
  array_key_exists('myvar', get_defined_vars())
);

unset($myvar);
// false
var_export(
  array_key_exists('myvar', get_defined_vars())
);

if (array_key_exists('myvar', get_defined_vars())) {
  myfunction($myvar);
}
?>
```
<br><br>Note: you can&apos;t turn this into a function (e.g. is_defined($myvar)) because get_defined_vars() only gets the variables in the current scope and entering a function changes the scope.  

#

in PHP5, if you have <br><br>

```
<?php
class Foo
{
    protected $data = array('bar' => null);

    function __get($p)
    {
        if( isset($this->data[$p]) ) return $this->data[$p];
    }
}
?>
```


and


```
<?php
$foo = new Foo;
echo isset($foo->bar);
?>
```
<br>will always echo &apos;false&apos;. because the isset() accepts VARIABLES as it parameters, but in this case, $foo-&gt;bar is NOT a VARIABLE. it is a VALUE returned from the __get() method of the class Foo. thus the isset($foo-&gt;bar) expreesion will always equal &apos;false&apos;.  

#

The new (as of PHP7) &apos;null coalesce operator&apos; allows shorthand isset. You can use it like so:<br><br>

```
<?php
// Fetches the value of $_GET['user'] and returns 'nobody'
// if it does not exist.
$username = $_GET['user'] ?? 'nobody';
// This is equivalent to:
$username = isset($_GET['user']) ? $_GET['user'] : 'nobody';

// Coalescing can be chained: this will return the first
// defined value out of $_GET['user'], $_POST['user'], and
// 'nobody'.
$username = $_GET['user'] ?? $_POST['user'] ?? 'nobody';
?>
```
<br><br>Quoted from http://php.net/manual/en/migration70.new-features.php#migration70.new-features.null-coalesce-op  

#

I tried the example posted previously by Slawek:<br><br>$foo = &apos;a little string&apos;;<br>echo isset($foo)?&apos;yes &apos;:&apos;no &apos;, isset($foo[&apos;aaaa&apos;])?&apos;yes &apos;:&apos;no &apos;;<br><br>He got yes yes, but he didn&apos;t say what version of PHP he was using.<br><br>I tried this on PHP 5.0.5 and got:  yes no<br><br>But on PHP 4.3.5 I got:  yes yes<br><br>Apparently, PHP4 converts the the string &apos;aaaa&apos; to zero and then returns the string character at that position within the string $foo, when $foo is not an array. That means you can&apos;t assume you are dealing with an array, even if you used an expression such as isset($foo[&apos;aaaa&apos;][&apos;bbb&apos;][&apos;cc&apos;][&apos;d&apos;]), because it will return true also if any part is a string.<br><br>PHP5 does not do this. If $foo is a string, the index must actually be numeric (e.g. $foo[0]) for it to return the indexed character.  

#

Careful with this function "ifsetfor" by soapergem, passing by reference means that if, like the example $_GET[&apos;id&apos;], the argument is an array index, it will be created in the original array (with a null value), thus causing posible trouble with the following code. At least in PHP 5.<br><br>For example:<br><br>

```
<?php
$a = array();
print_r($a);
ifsetor($a["unsetindex"], 'default');
print_r($a);
?>
```
<br><br>will print <br><br>Array<br>(<br>)<br>Array<br>(<br>    [unsetindex] =&gt; <br>)<br><br>Any foreach or similar will be different before and after the call.  

#

1) Note that isset($var) doesn&apos;t distinguish the two cases when $var is undefined, or is null. Evidence is in the following code.<br><br>

```
<?php
unset($undefined);
$null = null;
if (true === isset($undefined)){echo 'isset($undefined) === true'} else {echo 'isset($undefined) === false'); // 'isset($undefined) === false'
if (true === isset($null)){echo 'isset($null) === true'} else {echo 'isset($null) === false');              // 'isset($null)      === false'
?>
```


2) If you want to distinguish undefined variable with a defined variable with a null value, then use array_key_exist



```
<?php
unset($undefined);
$null = null;

if (true !== array_key_exists('undefined', get_defined_vars())) {echo '$undefined does not exist';} else {echo '$undefined exists';} // '$undefined does not exist'
if (true === array_key_exists('null', get_defined_vars())) {echo '$null exists';} else {echo '$null does not exist';}                // '$null exists'
?>
```
  

#

To organize some of the frequently used functions..<br><br>

```
<?php

/**
 * Returns field of variable (arr[key] or obj->prop), otherwise the third parameter
 * @param array/object $arr_or_obj
 * @param string $key_or_prop
 * @param mixed $else
 */
function nz($arr_or_obj, $key_or_prop, $else){
  $result = $else;
  if(isset($arr_or_obj)){
    if(is_array($arr_or_obj){
      if(isset($arr_or_obj[$key_or_prop]))
        $result = $arr_or_obj[$key_or_prop];
    }elseif(is_object($arr_or_object))
      if(isset($arr_or_obj->$key_or_prop))
        $result = $arr_or_obj->$key_or_prop;
    }
  }
  return $result;
}

/**
 * Returns integer value using nz()
 */
function nz_int($arr_or_obj, $key_or_prop, $else){
  return intval(nz($arr_or_obj, $key_or_prop, $else));
}

$my_id = nz_int($_REQUEST, 'id', 0);
if($my_id > 0){
  //why?
}
?>
```
  

#

Sometimes you have to check if an array has some keys. To achieve it you can use "isset" like this: isset($array[&apos;key1&apos;], $array[&apos;key2&apos;], $array[&apos;key3&apos;], $array[&apos;key4&apos;])<br>You have to write $array all times and it is reiterative if you use same array each time.<br><br>With this simple function you can check if an array has some keys:<br><br>

```
<?php
function isset_array() {
    if (func_num_args() < 2) return true;
    $args = func_get_args();
    $array = array_shift($args);
    if (!is_array($array)) return false;
    foreach ($args as $n) if (!isset($array[$n])) return false;
    return true;
}
?>
```
<br><br>Use: isset_array($array, &apos;key1&apos;, &apos;key2&apos;, &apos;key3&apos;, &apos;key4&apos;)<br>First parameter has the array; following parameters has the keys you want to check.  

#

Note that isset() is not recursive as of the 5.4.8 I have available here to test with: if you use it on a multidimensional array or an object it will not check isset() on each dimension as it goes.<br><br>Imagine you have a class with a normal __isset and a __get that fatals for non-existant properties. isset($object-&gt;nosuch) will behave normally but isset($object-&gt;nosuch-&gt;foo) will crash. Rather harsh IMO but still possible.<br><br>

```
<?php

class FatalOnGet {

    // pretend that the methods have implementations that actually try to do work
    // in this example I only care about the worst case conditions

    public function __get($name) {
        echo "(getting {$name}) ";

        // if property does not exist {
            echo "Property does not exist!";
            exit;
        // }
    }

    public function __isset($name) {
        echo "(isset {$name}?) ";
        // return whether the property exists
        return false;
    }

}

$obj = new FatalOnGet();

// works
echo "Testing if ->nosuch exists: ";
if (isset($obj->nosuch)) echo "Yes"; else echo "No";

// fatals
echo "\nTesting if ->nosuch->foo exists: ";
if (isset($obj->nosuch->foo)) echo "Yes"; else echo "No";

// not executed
echo "\nTesting if ->irrelevant exists: ";
if (isset($obj->irrelevant)) echo "Yes"; else echo "No";

?>
```
<br><br>    Testing if -&gt;nosuch exists: No<br>    Testing if -&gt;nosuch-&gt;foo exists: Property does not exist!<br><br>Uncomment the echos in the methods and you&apos;ll see exactly what happened:<br><br>    Testing if -&gt;nosuch exists: (isset nosuch?) No<br>    Testing if -&gt;nosuch-&gt;foo exists: (getting nosuch) Property does not exist!<br><br>On a similar note, if __get always returns but instead issues warnings or notices then those will surface.  

#

isset expects the variable sign first, so you can&apos;t add parentheses or anything.<br><br>

```
<?php
    $foo = 1;
    if(isset(($foo))) { // Syntax error at isset((
        $foo = 2;
    }
?>
```
  

#

The following is an example of how to test if a variable is set, whether or not it is NULL. It makes use of the fact that an unset variable will throw an E_NOTICE error, but one initialized as NULL will not. <br><br>

```
<?php

function var_exists($var){
    if (empty($GLOBALS['var_exists_err'])) {
        return true;
    } else {
        unset($GLOBALS['var_exists_err']);
        return false;
    }
}

function var_existsHandler($errno, $errstr, $errfile, $errline) {
   $GLOBALS['var_exists_err'] = true;
}

$l = NULL;
set_error_handler("var_existsHandler", E_NOTICE);
echo (var_exists($l)) ? "True " : "False ";
echo (var_exists($k)) ? "True " : "False ";
restore_error_handler();

?>
```


Outputs:
True False

The problem is, the set_error_handler and restore_error_handler calls can not be inside the function, which means you need 2 extra lines of code every time you are testing. And if you have any E_NOTICE errors caused by other code between the set_error_handler and restore_error_handler they will not be dealt with properly. One solution:



```
<?php

function var_exists($var){
   if (empty($GLOBALS['var_exists_err'])) {
       return true;
   } else {
       unset($GLOBALS['var_exists_err']);
       return false;
   }
}

function var_existsHandler($errno, $errstr, $errfile, $errline) {
    $filearr = file($errfile);
    if (strpos($filearr[$errline-1], 'var_exists') !== false) {
        $GLOBALS['var_exists_err'] = true;
        return true;
    } else {
        return false;
    }
}

$l = NULL;
set_error_handler("var_existsHandler", E_NOTICE);
echo (var_exists($l)) ? "True " : "False ";
echo (var_exists($k)) ? "True " : "False ";
is_null($j);
restore_error_handler();

?>
```
<br><br>Outputs:<br>True False<br>Notice: Undefined variable: j in filename.php on line 26<br><br>This will make the handler only handle var_exists, but it adds a lot of overhead. Everytime an E_NOTICE error happens, the file it originated from will be loaded into an array.  

#

[Official documentation page](https://www.php.net/manual/en/function.isset.php)

**[To root](/README.md)**