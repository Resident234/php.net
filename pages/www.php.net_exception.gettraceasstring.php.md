# Exception::getTraceAsString





Honestly, Exception::getTraceAsString() simply sucks, listing only the called method (below, for example, on line 89 function fail2() gets called, but there&apos;s no information that you have the originator is fail1()). The fact that, in the example below, the exception gets thrown on line 78, is completely omitted from the trace and only available within the exception. Chained exceptions are not supported as well.

Example:
#0 /var/htdocs/websites/sbdevel/public/index.php(70): seabird\test\C-&gt;exc()
#1 /var/htdocs/websites/sbdevel/public/index.php(85): seabird\test\C-&gt;doexc()
#2 /var/htdocs/websites/sbdevel/public/index.php(89): seabird\test\fail2()
#3 /var/htdocs/websites/sbdevel/public/index.php(93): seabird\test\fail1()
#4 {main}

jTraceEx() provides a much better java-like stack trace that includes support for chained exceptions:
Exception: Thrown from class C
 at seabird.test.C.exc(index.php:78)
 at seabird.test.C.doexc(index.php:70)
 at seabird.test.fail2(index.php:85)
 at seabird.test.fail1(index.php:89)
 at (main)(index.php:93)
Caused by: Exception: Thrown from class B
 at seabird.test.B.exc(index.php:64)
 at seabird.test.C.exc(index.php:75)
 ... 4 more
Caused by: Exception: Thrown from class A
 at seabird.test.A.exc(index.php:46)
 at seabird.test.B.exc(index.php:61)
 ... 5 more

(see at the end for the example code)
 
 

```
<?php
 /**
 * jTraceEx() - provide a Java style exception trace
 * @param $exception
 * @param $seen&#xA0; &#xA0; &#xA0; - array passed to recursive calls to accumulate trace lines already seen
 *&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; leave as NULL when calling this function
 * @return array of strings, one entry per trace line
 */
function jTraceEx($e, $seen=null) {
&#xA0; &#xA0; $starter = $seen ? &apos;Caused by: &apos; : &apos;&apos;;
&#xA0; &#xA0; $result = array();
&#xA0; &#xA0; if (!$seen) $seen = array();
&#xA0; &#xA0; $trace&#xA0; = $e-&gt;getTrace();
&#xA0; &#xA0; $prev&#xA0;&#xA0; = $e-&gt;getPrevious();
&#xA0; &#xA0; $result[] = sprintf(&apos;%s%s: %s&apos;, $starter, get_class($e), $e-&gt;getMessage());
&#xA0; &#xA0; $file = $e-&gt;getFile();
&#xA0; &#xA0; $line = $e-&gt;getLine();
&#xA0; &#xA0; while (true) {
&#xA0; &#xA0; &#xA0; &#xA0; $current = &quot;$file:$line&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; if (is_array($seen) &amp;&amp; in_array($current, $seen)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $result[] = sprintf(&apos; ... %d more&apos;, count($trace)+1);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; $result[] = sprintf(&apos; at %s%s%s(%s%s%s)&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; count($trace) &amp;&amp; array_key_exists(&apos;class&apos;, $trace[0]) ? str_replace(&apos;\\&apos;, &apos;.&apos;, $trace[0][&apos;class&apos;]) : &apos;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; count($trace) &amp;&amp; array_key_exists(&apos;class&apos;, $trace[0]) &amp;&amp; array_key_exists(&apos;function&apos;, $trace[0]) ? &apos;.&apos; : &apos;&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; count($trace) &amp;&amp; array_key_exists(&apos;function&apos;, $trace[0]) ? str_replace(&apos;\\&apos;, &apos;.&apos;, $trace[0][&apos;function&apos;]) : &apos;(main)&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $line === null ? $file : basename($file),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $line === null ? &apos;&apos; : &apos;:&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $line === null ? &apos;&apos; : $line);
&#xA0; &#xA0; &#xA0; &#xA0; if (is_array($seen))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $seen[] = &quot;$file:$line&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; if (!count($trace))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; $file = array_key_exists(&apos;file&apos;, $trace[0]) ? $trace[0][&apos;file&apos;] : &apos;Unknown Source&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; $line = array_key_exists(&apos;file&apos;, $trace[0]) &amp;&amp; array_key_exists(&apos;line&apos;, $trace[0]) &amp;&amp; $trace[0][&apos;line&apos;] ? $trace[0][&apos;line&apos;] : null;
&#xA0; &#xA0; &#xA0; &#xA0; array_shift($trace);
&#xA0; &#xA0; }
&#xA0; &#xA0; $result = join(&quot;\n&quot;, $result);
&#xA0; &#xA0; if ($prev)
&#xA0; &#xA0; &#xA0; &#xA0; $result&#xA0; .= &quot;\n&quot; . jTraceEx($prev, $seen);

&#xA0; &#xA0; return $result;
}
?>
```


Here&apos;s the example code:


```
<?php
class A {
&#xA0; &#xA0; public function exc() {
&#xA0; &#xA0; &#xA0; &#xA0; throw new \Exception(&apos;Thrown from class A&apos;);&#xA0; &#xA0; // &lt;-- line 46
&#xA0; &#xA0; }
}

class B {
&#xA0; &#xA0; public function exc() {
&#xA0; &#xA0; &#xA0; &#xA0; try {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $a = new A;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $a-&gt;exc();&#xA0; &#xA0; // &lt;-- line 61
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; catch(\Exception $e1) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new \Exception(&apos;Thrown from class B&apos;, 0, $e1);&#xA0; &#xA0; // &lt;-- line 64
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
}
class C {
&#xA0; &#xA0; public function doexc() {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;exc();&#xA0; &#xA0; // &lt;-- line 70
&#xA0; &#xA0; }
&#xA0; &#xA0; public function exc() {
&#xA0; &#xA0; &#xA0; &#xA0; try {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $b = new B;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $b-&gt;exc();&#xA0; &#xA0; // &lt;-- line 75
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; catch(\Exception $e1) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new \Exception(&apos;Thrown from class C&apos;, 0, $e1);&#xA0; &#xA0; // &lt;-- line 78
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
}

function fail2() {
&#xA0; &#xA0; $c = new C;
&#xA0; &#xA0; $c-&gt;doexc();&#xA0; &#xA0; // &lt;-- line 85
}

function fail1() {
&#xA0; &#xA0; fail2();&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // &lt;-- line 89
}

try {
&#xA0; &#xA0; fail1();&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // &lt;-- line 93
}
catch(\Exception $e) {
&#xA0; &#xA0; echo jTraceEx($e);
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/exception.gettraceasstring.php)

**[To root](/README.md)**