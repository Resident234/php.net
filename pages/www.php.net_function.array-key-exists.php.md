# array_key_exists



If you want to take the performance advantage of isset() while keeping the NULL element correctly detected, use this:<br><br>if (isset(..) || array_key_exists(...))<br>{<br>...<br>}<br><br>Benchmark (100000 runs):<br>array_key_exists() : 205 ms<br>is_set() : 35ms<br>isset() || array_key_exists() : 48ms<br><br>Note: <br>The code for this check is very fast, so you shouldn&apos;t warp the code into a single function like below, because the overhead of calling a function dominates the overall performance.<br><br>function array_check(...)<br>{<br>    return (isset(..) || array_key_exists(...))<br>}  

#

You&apos;ll notice several notes on this page stating that isset() is significantly faster than array_key_exists(). This may be true except for one small hitch. isset() will return false for arrays keys that have there value set to NULL, which is therefore not entirely accurate.<br><br>Example:<br><br>

```
<?php
$foo = array();
$foo['bar'] = NULL;

var_dump(isset($foo['bar']));
var_dump(array_key_exists('bar', $foo));
?>
```
<br><br>will output:<br>bool(false)<br>bool(true)<br><br>Be aware of this!  

#

Beware that if the array passed to array_key_exists is NULL, the return value will also be NULL. <br><br>This is undocumented behaviour, moreover the documentation (and return typehint) suggest that the array_key_exists function only returns boolean value. But that&apos;s not the case.  

#

The way array_key_exists handles null, float, boolean, and &apos;integer-representing string&apos; keys is inconsistent in itself and, in the case of bool and float, with the way these are converted when used as array offset.<br><br>

```
<?php
$array = array(null => 1, false => 2, true => 3, 4.6 => 4, "08" => 5, "8" => 6);
var_export($array);

echo "\nnull is " . (array_key_exists(null, $array) ? '' : 'not ') . "a key.\n";
echo 'false is ' . (array_key_exists(false, $array) ? '' : 'not ') . "a key.\n";
echo 'true is ' . (array_key_exists(true, $array) ? '' : 'not ') . "a key.\n";
echo '4.6 is ' . (array_key_exists(4.6, $array) ? '' : 'not ') . "a key.\n";
echo '"08" is ' . (array_key_exists("08", $array) ? '' : 'not ') . "a key.\n";
echo '"8" is ' . (array_key_exists("8", $array) ? '' : 'not ') . "a key.\n";
?>
```
<br><br>Output:<br><br>array (<br>  &apos;&apos; =&gt; 1,<br>  0 =&gt; 2,<br>  1 =&gt; 3,<br>  4 =&gt; 4,<br>  &apos;08&apos; =&gt; 5,<br>  8 =&gt; 6,<br>)<br>null is a key.<br>false is not a key.<br>true is not a key.<br>4.6 is not a key.<br>"08" is a key.<br>"8" is a key.<br><br>Well, and you get this warning three times (on the bools and the float, but not on the null):<br><br>Warning:  array_key_exists() [function.array-key-exists]: The first argument should be either a string or an integer in /var/www/php/test.php on line 6  

#

Very simple case-insensitive array_key_exists:<br><br>bool (in_array(strtolower($needle), array_map(&apos;strtolower&apos;, array_keys($haystack))))  

#

The argument of array_key_exists() vs. isset() came up in the workplace today, so I conducted a little benchmark to see which is faster:<br><br>

```
<?php
    // one-dimensional arrays
    $array = array_fill(0,50000,'tommy is the best!');
    $arraykeyexists_result = array();

    $start = microtime(true);
    for ($i = 0; $i &lt; 100000; $i++) {
        if (array_key_exists($i,$array)) {
            $arraykeyexists_result[] = 1;
        }
        else {
            $arraykeyexists_result[] = 0;
        }
    }
    $arrtime = round(microtime(true)-$start,3);
    
    $start = microtime(true);
    for ($i = 0; $i &lt; 100000; $i++) {
        if (isset($array[$i])) {
            $arraykeyexists_result[] = 1;
        }
        else {
            $arraykeyexists_result[] = 0;
        }
    }
    $istime = round(microtime(true)-$start,3);
    
    $totaltime = $arrtime+$istime;
    $arrpercentage = round(100*$arrtime/$totaltime,3);
    $ispercentage = round(100*$istime/$totaltime,3);    
    
    echo "array_key_exists(): $arrtime [$arrpercentage%] seconds\n";
    echo "isset():            $istime [$ispercentage%] seconds\n";

?>
```
<br><br>On Windows, the output is similar to<br><br>array_key_exists(): 0.504 [82.895%] seconds<br>isset():            0.104 [17.105%] seconds<br><br>On Mac or Linux, isset() is faster but only by a factor of approximately 1.5.  

#

[Official documentation page](https://www.php.net/manual/en/function.array-key-exists.php)

**[To root](/README.md)**