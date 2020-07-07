# debug_backtrace





Here&apos;s a function I just wrote for getting a nice and comprehensible call trace. It is probably more resource-intensive than some other alternatives but it is short, understandable, and gives nice output (Exception-&gt;getTraceAsString()).



```
<?php
function generateCallTrace()
{
&#xA0; &#xA0; $e = new Exception();
&#xA0; &#xA0; $trace = explode(&quot;\n&quot;, $e-&gt;getTraceAsString());
&#xA0; &#xA0; // reverse array to make steps line up chronologically
&#xA0; &#xA0; $trace = array_reverse($trace);
&#xA0; &#xA0; array_shift($trace); // remove {main}
&#xA0; &#xA0; array_pop($trace); // remove call to this method
&#xA0; &#xA0; $length = count($trace);
&#xA0; &#xA0; $result = array();
&#xA0; &#xA0; 
&#xA0; &#xA0; for ($i = 0; $i &lt; $length; $i++)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $result[] = ($i + 1)&#xA0; . &apos;)&apos; . substr($trace[$i], strpos($trace[$i], &apos; &apos;)); // replace &apos;#someNum&apos; with &apos;$i)&apos;, set the right ordering
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; return &quot;\t&quot; . implode(&quot;\n\t&quot;, $result);
}
?>
```


Example output:
&#xA0; &#xA0; 1) /var/www/test/test.php(15): SomeClass-&gt;__construct()
&#xA0; &#xA0; 2) /var/www/test/SomeClass.class.php(36): SomeClass-&gt;callSomething()

  

#



If you are using the backtrace function in an error handler, avoid using var_export() on the args, as you will cause fatal errors in some situations, preventing you from seeing your stack trace.&#xA0; Some structures will cause PHP to generate the fatal error &quot;Nesting level too deep - recursive dependency?&quot; This is a design feature of php, not a bug (see http://bugs.php.net/bug.php?id=30471)

  

#

[Official documentation page](https://www.php.net/manual/en/function.debug-backtrace.php)

**[To root](/README.md)**