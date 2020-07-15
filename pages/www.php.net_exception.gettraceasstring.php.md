# Exception::getTraceAsString



Honestly, Exception::getTraceAsString() simply sucks, listing only the called method (below, for example, on line 89 function fail2() gets called, but there&apos;s no information that you have the originator is fail1()). The fact that, in the example below, the exception gets thrown on line 78, is completely omitted from the trace and only available within the exception. Chained exceptions are not supported as well.<br><br>Example:<br>#0 /var/htdocs/websites/sbdevel/public/index.php(70): seabird\test\C-&gt;exc()<br>#1 /var/htdocs/websites/sbdevel/public/index.php(85): seabird\test\C-&gt;doexc()<br>#2 /var/htdocs/websites/sbdevel/public/index.php(89): seabird\test\fail2()<br>#3 /var/htdocs/websites/sbdevel/public/index.php(93): seabird\test\fail1()<br>#4 {main}<br><br>jTraceEx() provides a much better java-like stack trace that includes support for chained exceptions:<br>Exception: Thrown from class C<br> at seabird.test.C.exc(index.php:78)<br> at seabird.test.C.doexc(index.php:70)<br> at seabird.test.fail2(index.php:85)<br> at seabird.test.fail1(index.php:89)<br> at (main)(index.php:93)<br>Caused by: Exception: Thrown from class B<br> at seabird.test.B.exc(index.php:64)<br> at seabird.test.C.exc(index.php:75)<br> ... 4 more<br>Caused by: Exception: Thrown from class A<br> at seabird.test.A.exc(index.php:46)<br> at seabird.test.B.exc(index.php:61)<br> ... 5 more<br><br>(see at the end for the example code)<br> <br> 

```
<?php
 /**
 * jTraceEx() - provide a Java style exception trace
 * @param $exception
 * @param $seen      - array passed to recursive calls to accumulate trace lines already seen
 *                     leave as NULL when calling this function
 * @return array of strings, one entry per trace line
 */
function jTraceEx($e, $seen=null) {
    $starter = $seen ? 'Caused by: ' : '';
    $result = array();
    if (!$seen) $seen = array();
    $trace  = $e->getTrace();
    $prev   = $e->getPrevious();
    $result[] = sprintf('%s%s: %s', $starter, get_class($e), $e->getMessage());
    $file = $e->getFile();
    $line = $e->getLine();
    while (true) {
        $current = "$file:$line";
        if (is_array($seen) &amp;&amp; in_array($current, $seen)) {
            $result[] = sprintf(' ... %d more', count($trace)+1);
            break;
        }
        $result[] = sprintf(' at %s%s%s(%s%s%s)',
                                    count($trace) &amp;&amp; array_key_exists('class', $trace[0]) ? str_replace('\\', '.', $trace[0]['class']) : '',
                                    count($trace) &amp;&amp; array_key_exists('class', $trace[0]) &amp;&amp; array_key_exists('function', $trace[0]) ? '.' : '',
                                    count($trace) &amp;&amp; array_key_exists('function', $trace[0]) ? str_replace('\\', '.', $trace[0]['function']) : '(main)',
                                    $line === null ? $file : basename($file),
                                    $line === null ? '' : ':',
                                    $line === null ? '' : $line);
        if (is_array($seen))
            $seen[] = "$file:$line";
        if (!count($trace))
            break;
        $file = array_key_exists('file', $trace[0]) ? $trace[0]['file'] : 'Unknown Source';
        $line = array_key_exists('file', $trace[0]) &amp;&amp; array_key_exists('line', $trace[0]) &amp;&amp; $trace[0]['line'] ? $trace[0]['line'] : null;
        array_shift($trace);
    }
    $result = join("\n", $result);
    if ($prev)
        $result  .= "\n" . jTraceEx($prev, $seen);

    return $result;
}
?>
```


Here's the example code:


```
<?php
class A {
    public function exc() {
        throw new \Exception('Thrown from class A');    // <-- line 46
    }
}

class B {
    public function exc() {
        try {
            $a = new A;
            $a->exc();    // <-- line 61
        }
        catch(\Exception $e1) {
            throw new \Exception('Thrown from class B', 0, $e1);    // <-- line 64
        }
    }
}
class C {
    public function doexc() {
        $this->exc();    // <-- line 70
    }
    public function exc() {
        try {
            $b = new B;
            $b->exc();    // <-- line 75
        }
        catch(\Exception $e1) {
            throw new \Exception('Thrown from class C', 0, $e1);    // <-- line 78
        }
    }
}

function fail2() {
    $c = new C;
    $c->doexc();    // <-- line 85
}

function fail1() {
    fail2();            // <-- line 89
}

try {
    fail1();            // <-- line 93
}
catch(\Exception $e) {
    echo jTraceEx($e);
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/exception.gettraceasstring.php)

**[To root](/README.md)**