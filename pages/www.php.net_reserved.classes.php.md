# Predefined Classes



If you call var_export() on an instance of stdClass, it attempts to export it using ::__set_state(), which, for some reason, is not implemented in stdClass.<br><br>However, casting an associative array to an object usually produces the same effect (at least, it does in my case). So I wrote an improved_var_export() function to convert instances of stdClass to (object) array () calls. If you choose to export objects of any other class, I&apos;d advise you to implement ::__set_state().<br><br>

```
<?php
/**
 * An implementation of var_export() that is compatible with instances
 * of stdClass.
 * @param mixed $variable The variable you want to export
 * @param bool $return If used and set to true, improved_var_export()
 *     will return the variable representation instead of outputting it.
 * @return mixed|null Returns the variable representation when the
 *     return parameter is used and evaluates to TRUE. Otherwise, this
 *     function will return NULL.
 */
function improved_var_export ($variable, $return = false) {
    if ($variable instanceof stdClass) {
        $result = '(object) '.improved_var_export(get_object_vars($variable), true);
    } else if (is_array($variable)) {
        $array = array ();
        foreach ($variable as $key => $value) {
            $array[] = var_export($key, true).' => '.improved_var_export($value, true);
        }
        $result = 'array ('.implode(', ', $array).')';
    } else {
        $result = var_export($variable, true);
    }

    if (!$return) {
        print $result;
        return null;
    } else {
        return $result;
    }
}

// Example usage:
$obj = new stdClass;
$obj->test = 'abc';
$obj->other = 6.2;
$obj->arr = array (1, 2, 3);

improved_var_export((object) array (
    'prop1' => true,
    'prop2' => $obj,
    'assocArray' => array (
        'apple' => 'good',
        'orange' => 'great'
    )
));

/* Output:
(object) array ('prop1' => true, 'prop2' => (object) array ('test' => 'abc', 'other' => 6.2, 'arr' => array (0 => 1, 1 => 2, 2 => 3)), 'assocArray' => array ('apple' => 'good', 'orange' => 'great'))
*/
?>
```
<br><br>Note: This function spits out a single line of code, which is useful to save in a cache file to include/eval. It isn&apos;t formatted for readability. If you want to print a readable version for debugging purposes, then I would suggest print_r() or var_dump().  

---

if you want a Dynamic class you can extend from, add atributes AND methods on the fly you can use this:<br>

```
<?php
    class Dynamic extends stdClass{
        public function __call($key,$params){
            if(!isset($this->{$key})) throw new Exception("Call to undefined method ".get_class($this)."::".$key."()");
            $subject = $this->{$key};
            call_user_func_array($subject,$params);
        }
    }
?>
```


this will accept both arrays, strings and Closures:


```
<?php
    $dynamic->myMethod = "thatFunction";
    $dynamic->hisMethod = array($instance,"aMethod");
    $dynamic->newMethod = array(SomeClass,"staticMethod");
    $dynamic->anotherMethod = function(){
        echo "Hey there";
    };
?>
```
<br><br>then call them away =D  

---

[Official documentation page](https://www.php.net/manual/en/reserved.classes.php)

**[To root](/README.md)**