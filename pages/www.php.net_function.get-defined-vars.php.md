# get_defined_vars



dirty code sample:<br><br>print_r(compact(array_keys(get_defined_vars())));  

#

A little gotcha to watch out for:<br><br>If you turn off RegisterGlobals and related, then use get_defined_vars(), you may see something like the following:<br><br>

```
<?php
Array
(
    [GLOBALS] => Array
        (
            [GLOBALS] => Array
 *RECURSION*
            [_POST] => Array()
            [_GET] => Array()
            [_COOKIE] => Array()
            [_FILES] => Array()
        )

    [_POST] => Array()
    [_GET] => Array()
    [_COOKIE] => Array()
    [_FILES] => Array()

)
?>
```


Notice that $_SERVER isn't there.  It seems that php only loads the superglobal $_SERVER if it is used somewhere.  You could do this:



```
<?php
print '&lt;pre&gt;' . htmlspecialchars(print_r(get_defined_vars(), true)) . '&lt;/pre&gt;';
print '&lt;pre&gt;' . htmlspecialchars(print_r($_SERVER, true)) . '&lt;/pre&gt;';
?>
```
<br><br>And then $_SERVER will appear in both lists.  I guess it&apos;s not really a gotcha, because nothing bad will happen either way, but it&apos;s an interesting curiosity nonetheless.  

#

Since get_defined_vars() only gets the variables at the point you call the function, there is a simple way to get the variables defined within the current scope.<br><br>

```
<?php
// The very top of your php script
$vars = get_defined_vars();

// Now do your stuff
$foo = 'foo';
$bar = 'bar';

// Get all the variables defined in current scope
$vars = array_diff(get_defined_vars(),$vars);

echo '&lt;pre&gt;';
print_r($vars);
echo '&lt;/pre&gt;';
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.get-defined-vars.php)

**[To root](/README.md)**