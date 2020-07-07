# The Closure class





This caused me some confusion a while back when I was still learning what closures were and how to use them, but what is referred to as a closure in PHP isn&apos;t the same thing as what they call closures in other languages (E.G. JavaScript).

In JavaScript, a closure can be thought of as a scope, when you define a function, it silently inherits the scope it&apos;s defined in, which is called its closure, and it retains that no matter where it&apos;s used.&#xA0; It&apos;s possible for multiple functions to share the same closure, and they can have access to multiple closures as long as they are within their accessible scope.

In PHP,&#xA0; a closure is a callable class, to which you&apos;ve bound your parameters manually.

It&apos;s a slight distinction but one I feel bears mentioning.

  

#



Small little trick. You can use a closures in itself via reference.

Example to delete a directory with all subdirectories and files:



```
<?php
$deleteDirectory = null;
$deleteDirectory = function($path) use (&amp;$deleteDirectory) {
&#xA0; &#xA0; $resource = opendir($path);
&#xA0; &#xA0; while (($item = readdir($resource)) !== false) {
&#xA0; &#xA0; &#xA0; &#xA0; if ($item !== &quot;.&quot; &amp;&amp; $item !== &quot;..&quot;) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (is_dir($path . &quot;/&quot; . $item)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $deleteDirectory($path . &quot;/&quot; . $item);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; unlink($path . &quot;/&quot; . $item);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; closedir($resource);
&#xA0; &#xA0; rmdir($path);
};
$deleteDirectory(&quot;path/to/directoy&quot;);
?>
```



  

#



Scope
A closure encapsulates its scope, meaning that it has no access to the scope in which it is defined or executed. It is, however, possible to inherit variables from the parent scope (where the closure is defined) into the closure with the use keyword:

function createGreeter($who) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return function() use ($who) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo &quot;Hello $who&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; };
}

$greeter = createGreeter(&quot;World&quot;);
$greeter(); // Hello World

This inherits the variables by-value, that is, a copy is made available inside the closure using its original name.
font: Zend Certification Study Guide.

  

#

[Official documentation page](https://www.php.net/manual/en/class.closure.php)

**[To root](/README.md)**