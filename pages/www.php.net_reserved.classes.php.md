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
        $result = &apos;(object) &apos;.improved_var_export(get_object_vars($variable), true);
    } else if (is_array($variable)) {
        $array = array ();
        foreach ($variable as $key =&gt; $value) {
            $array[] = var_export($key, true).&apos; =&gt; &apos;.improved_var_export($value, true);
        }
        $result = &apos;array (&apos;.implode(&apos;, &apos;, $array).&apos;)&apos;;
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
$obj-&gt;test = &apos;abc&apos;;
$obj-&gt;other = 6.2;
$obj-&gt;arr = array (1, 2, 3);

improved_var_export((object) array (
    &apos;prop1&apos; =&gt; true,
    &apos;prop2&apos; =&gt; $obj,
    &apos;assocArray&apos; =&gt; array (
        &apos;apple&apos; =&gt; &apos;good&apos;,
        &apos;orange&apos; =&gt; &apos;great&apos;
    )
));

/* Output:
(object) array (&apos;prop1&apos; =&gt; true, &apos;prop2&apos; =&gt; (object) array (&apos;test&apos; =&gt; &apos;abc&apos;, &apos;other&apos; =&gt; 6.2, &apos;arr&apos; =&gt; array (0 =&gt; 1, 1 =&gt; 2, 2 =&gt; 3)), &apos;assocArray&apos; =&gt; array (&apos;apple&apos; =&gt; &apos;good&apos;, &apos;orange&apos; =&gt; &apos;great&apos;))
*/
?>
```
<br><br>Note: This function spits out a single line of code, which is useful to save in a cache file to include/eval. It isn&apos;t formatted for readability. If you want to print a readable version for debugging purposes, then I would suggest print_r() or var_dump().  

#

if you want a Dynamic class you can extend from, add atributes AND methods on the fly you can use this:<br>

```
<?php
    class Dynamic extends stdClass{
        public function __call($key,$params){
            if(!isset($this-&gt;{$key})) throw new Exception("Call to undefined method ".get_class($this)."::".$key."()");
            $subject = $this-&gt;{$key};
            call_user_func_array($subject,$params);
        }
    }
?>
```


this will accept both arrays, strings and Closures:


```
<?php
    $dynamic-&gt;myMethod = "thatFunction";
    $dynamic-&gt;hisMethod = array($instance,"aMethod");
    $dynamic-&gt;newMethod = array(SomeClass,"staticMethod");
    $dynamic-&gt;anotherMethod = function(){
        echo "Hey there";
    };
?>
```
<br><br>then call them away =D  

#

[Official documentation page](https://www.php.net/manual/en/reserved.classes.php)

**[To root](/README.md)**