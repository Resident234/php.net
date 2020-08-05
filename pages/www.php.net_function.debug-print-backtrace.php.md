# debug_print_backtrace



Another way to manipulate and print a backtrace, without using output buffering:<br><br>

```
<?php
// print backtrace, getting rid of repeated absolute path on each file
$e = new Exception();
print_r(str_replace('/path/to/code/', '', $e->getTraceAsString()));
?>
```
  

---

I like the output of debug_print_backtrace() but I sometimes want it as a string.<br><br>bortuzar&apos;s solution to use output buffering is great, but I&apos;d like to factorize that into a function.  Doing that however always results in whatever function name I use appearing at the top of the stack which is redundant.<br><br>Below is my noddy (simple) solution.  If you don&apos;t care for renumbering the call stack, omit the second preg_replace().<br><br>

```
<?php
    function debug_string_backtrace() {
        ob_start();
        debug_print_backtrace();
        $trace = ob_get_contents();
        ob_end_clean();

        // Remove first item from backtrace as it's this function which
        // is redundant.
        $trace = preg_replace ('/^#0\s+' . __FUNCTION__ . "[^\n]*\n/", '', $trace, 1);

        // Renumber backtrace items.
        $trace = preg_replace ('/^#(\d+)/me', '\'#\' . ($1 - 1)', $trace);

        return $trace;
    }
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.debug-print-backtrace.php)

**[To root](/README.md)**