# debug_backtrace



Here&apos;s a function I just wrote for getting a nice and comprehensible call trace. It is probably more resource-intensive than some other alternatives but it is short, understandable, and gives nice output (Exception-&gt;getTraceAsString()).<br><br>

```
<?php
function generateCallTrace()
{
    $e = new Exception();
    $trace = explode("\n", $e-&gt;getTraceAsString());
    // reverse array to make steps line up chronologically
    $trace = array_reverse($trace);
    array_shift($trace); // remove {main}
    array_pop($trace); // remove call to this method
    $length = count($trace);
    $result = array();
    
    for ($i = 0; $i &lt; $length; $i++)
    {
        $result[] = ($i + 1)  . &apos;)&apos; . substr($trace[$i], strpos($trace[$i], &apos; &apos;)); // replace &apos;#someNum&apos; with &apos;$i)&apos;, set the right ordering
    }
    
    return "\t" . implode("\n\t", $result);
}
?>
```
<br><br>Example output:<br>    1) /var/www/test/test.php(15): SomeClass-&gt;__construct()<br>    2) /var/www/test/SomeClass.class.php(36): SomeClass-&gt;callSomething()  

#

If you are using the backtrace function in an error handler, avoid using var_export() on the args, as you will cause fatal errors in some situations, preventing you from seeing your stack trace.  Some structures will cause PHP to generate the fatal error "Nesting level too deep - recursive dependency?" This is a design feature of php, not a bug (see http://bugs.php.net/bug.php?id=30471)  

#

[Official documentation page](https://www.php.net/manual/en/function.debug-backtrace.php)

**[To root](/README.md)**