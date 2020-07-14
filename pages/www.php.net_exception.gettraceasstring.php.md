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
    $starter = $seen ? &apos;Caused by: &apos; : &apos;&apos;;
    $result = array();
    if (!$seen) $seen = array();
    $trace  = $e-&gt;getTrace();
    $prev   = $e-&gt;getPrevious();
    $result[] = sprintf(&apos;%s%s: %s&apos;, $starter, get_class($e), $e-&gt;getMessage());
    $file = $e-&gt;getFile();
    $line = $e-&gt;getLine();
    while (true) {
        $current = "$file:$line";
        if (is_array($seen) &amp;&amp; in_array($current, $seen)) {
            $result[] = sprintf(&apos; ... %d more&apos;, count($trace)+1);
            break;
        }
        $result[] = sprintf(&apos; at %s%s%s(%s%s%s)&apos;,
                                    count($trace) &amp;&amp; array_key_exists(&apos;class&apos;, $trace[0]) ? str_replace(&apos;\\&apos;, &apos;.&apos;, $trace[0][&apos;class&apos;]) : &apos;&apos;,
                                    count($trace) &amp;&amp; array_key_exists(&apos;class&apos;, $trace[0]) &amp;&amp; array_key_exists(&apos;function&apos;, $trace[0]) ? &apos;.&apos; : &apos;&apos;,
                                    count($trace) &amp;&amp; array_key_exists(&apos;function&apos;, $trace[0]) ? str_replace(&apos;\\&apos;, &apos;.&apos;, $trace[0][&apos;function&apos;]) : &apos;(main)&apos;,
                                    $line === null ? $file : basename($file),
                                    $line === null ? &apos;&apos; : &apos;:&apos;,
                                    $line === null ? &apos;&apos; : $line);
        if (is_array($seen))
            $seen[] = "$file:$line";
        if (!count($trace))
            break;
        $file = array_key_exists(&apos;file&apos;, $trace[0]) ? $trace[0][&apos;file&apos;] : &apos;Unknown Source&apos;;
        $line = array_key_exists(&apos;file&apos;, $trace[0]) &amp;&amp; array_key_exists(&apos;line&apos;, $trace[0]) &amp;&amp; $trace[0][&apos;line&apos;] ? $trace[0][&apos;line&apos;] : null;
        array_shift($trace);
    }
    $result = join("\n", $result);
    if ($prev)
        $result  .= "\n" . jTraceEx($prev, $seen);

    return $result;
}
?>
```


Here&apos;s the example code:


```
<?php
class A {
    public function exc() {
        throw new \Exception(&apos;Thrown from class A&apos;);    // &lt;-- line 46
    }
}

class B {
    public function exc() {
        try {
            $a = new A;
            $a-&gt;exc();    // &lt;-- line 61
        }
        catch(\Exception $e1) {
            throw new \Exception(&apos;Thrown from class B&apos;, 0, $e1);    // &lt;-- line 64
        }
    }
}
class C {
    public function doexc() {
        $this-&gt;exc();    // &lt;-- line 70
    }
    public function exc() {
        try {
            $b = new B;
            $b-&gt;exc();    // &lt;-- line 75
        }
        catch(\Exception $e1) {
            throw new \Exception(&apos;Thrown from class C&apos;, 0, $e1);    // &lt;-- line 78
        }
    }
}

function fail2() {
    $c = new C;
    $c-&gt;doexc();    // &lt;-- line 85
}

function fail1() {
    fail2();            // &lt;-- line 89
}

try {
    fail1();            // &lt;-- line 93
}
catch(\Exception $e) {
    echo jTraceEx($e);
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/exception.gettraceasstring.php)

**[To root](/README.md)**