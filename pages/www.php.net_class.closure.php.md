# The Closure class



This caused me some confusion a while back when I was still learning what closures were and how to use them, but what is referred to as a closure in PHP isn&apos;t the same thing as what they call closures in other languages (E.G. JavaScript).<br><br>In JavaScript, a closure can be thought of as a scope, when you define a function, it silently inherits the scope it&apos;s defined in, which is called its closure, and it retains that no matter where it&apos;s used.  It&apos;s possible for multiple functions to share the same closure, and they can have access to multiple closures as long as they are within their accessible scope.<br><br>In PHP,  a closure is a callable class, to which you&apos;ve bound your parameters manually.<br><br>It&apos;s a slight distinction but one I feel bears mentioning.  

#

Small little trick. You can use a closures in itself via reference.<br><br>Example to delete a directory with all subdirectories and files:<br><br>

```
<?php
$deleteDirectory = null;
$deleteDirectory = function($path) use (&amp;$deleteDirectory) {
    $resource = opendir($path);
    while (($item = readdir($resource)) !== false) {
        if ($item !== "." &amp;&amp; $item !== "..") {
            if (is_dir($path . "/" . $item)) {
                $deleteDirectory($path . "/" . $item);
            } else {
                unlink($path . "/" . $item);
            }
        }
    }
    closedir($resource);
    rmdir($path);
};
$deleteDirectory("path/to/directoy");
?>
```
  

#

Scope<br>A closure encapsulates its scope, meaning that it has no access to the scope in which it is defined or executed. It is, however, possible to inherit variables from the parent scope (where the closure is defined) into the closure with the use keyword:<br><br>function createGreeter($who) {<br>              return function() use ($who) {<br>                  echo "Hello $who";<br>              };<br>}<br><br>$greeter = createGreeter("World");<br>$greeter(); // Hello World<br><br>This inherits the variables by-value, that is, a copy is made available inside the closure using its original name.<br>font: Zend Certification Study Guide.  

#

[Official documentation page](https://www.php.net/manual/en/class.closure.php)

**[To root](/README.md)**