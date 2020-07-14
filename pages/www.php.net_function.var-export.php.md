# var_export



/**<br> * var_export() with square brackets and indented 4 spaces.<br> */<br>

```
<?php<br>function varexport($expression, $return=FALSE) {<br>    $export = var_export($expression, TRUE);<br>    $export = preg_replace("/^([ ]*)(.*)/m", &apos;$1$1$2&apos;, $export);<br>    $array = preg_split("/\r\n|\n|\r/", $export);<br>    $array = preg_replace(["/\s*array\s\($/", "/\)(,)?$/", "/\s=&gt;\s$/"], [NULL, &apos;]$1&apos;, &apos; =&gt; [&apos;], $array);<br>    $export = join(PHP_EOL, array_filter(["["] + $array));<br>    if ((bool)$return) return $export; else echo $export;<br>}  

#

Looks like since version 5.4.22 var_export uses the serialize_precision ini setting, rather than the precision one used for normal output of floating-point numbers.<br>As a consequence since version 5.4.22 for example var_export(1.1) will output 1.1000000000000001 (17 is default precision value) and not 1.1 as before. <br><br>

```
<?php 
//ouput 1.1000000000000001
var_export(1.1)
 ?>
```
  

#

It doesn&apos;t appear to be documented, but the behaviour of `var_export()` changed in PHP 7.<br><br>Previously, `var_export(3.)` returned "3", now it returns "3.0".  

#

I found that my complex type was exporting with <br>  stdClass::__set_state()<br>in places. Not only was that strange and messy, it cannot be eval()-ed back in at all. Fatal error. Doh!<br><br>However a quick string-replace tidy-up of the result rendered it valid again.<br><br>    $macro = var_export($data, TRUE);<br>    $macro = str_replace("stdClass::__set_state", "(object)", $macro);<br>    $macro = &apos;$data = &apos; . $macro . &apos;;&apos;;<br><br>And now the string I output *can* be evaluated back in again.  

#

&lt;roman at DIESPAM dot feather dot org dot ru&gt;, your function has inefficiencies and problems. I probably speak for everyone when I ask you to test code before you add to the manual.<br><br>Since the issue of whitespace only comes up when exporting arrays, you can use the original var_export() for all other variable types. This function does the job, and, from the outside, works the same as var_export().<br><br>

```
<?php

function var_export_min($var, $return = false) {
    if (is_array($var)) {
        $toImplode = array();
        foreach ($var as $key =&gt; $value) {
            $toImplode[] = var_export($key, true).&apos;=&gt;&apos;.var_export_min($value, true);
        }
        $code = &apos;array(&apos;.implode(&apos;,&apos;, $toImplode).&apos;)&apos;;
        if ($return) return $code;
        else echo $code;
    } else {
        return var_export($var, $return);
    }
}

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.var-export.php)

**[To root](/README.md)**