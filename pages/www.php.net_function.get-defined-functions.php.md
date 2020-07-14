# get_defined_functions



You can list all arguments using ReflectionFunction class. It&apos;s not necessary to parse selected files/files as suggested by Nguyet.Duc.<br><br>http://php.net/manual/pl/class.reflectionfunction.php<br><br>Example:<br>

```
<?php
function foo(&amp;$bar, $big, $small = 1) {}
function bar($foo) {}
function noparams() {}
function byrefandopt(&amp;$the = &apos;one&apos;) {}

$functions = get_defined_functions();
$functions_list = array();
foreach ($functions[&apos;user&apos;] as $func) {
        $f = new ReflectionFunction($func);
        $args = array();
        foreach ($f-&gt;getParameters() as $param) {
                $tmparg = &apos;&apos;;
                if ($param-&gt;isPassedByReference()) $tmparg = &apos;&amp;&apos;;
                if ($param-&gt;isOptional()) {
                        $tmparg = &apos;[&apos; . $tmparg . &apos;

```
<?php<br>function foo(&amp;$bar, $big, $small = 1) {}<br>function bar($foo) {}<br>function noparams() {}<br>function byrefandopt(&amp;$the = &apos;one&apos;) {}<br><br>$functions = get_defined_functions();<br>$functions_list = array();<br>foreach ($functions[&apos;user&apos;] as $func) {<br>        $f = new ReflectionFunction($func);<br>        $args = array();<br>        foreach ($f-&gt;getParameters() as $param) {<br>                $tmparg = &apos;&apos;;<br>                if ($param-&gt;isPassedByReference()) $tmparg = &apos;&amp;&apos;;<br>                if ($param-&gt;isOptional()) {<br>                        $tmparg = &apos;[&apos; . $tmparg . &apos;$&apos; . $param-&gt;getName() . &apos; = &apos; . $param-&gt;getDefaultValue() . &apos;]&apos;;<br>                } else {<br>                        $tmparg.= &apos;&amp;&apos; . $param-&gt;getName();<br>                }<br>                $args[] = $tmparg;<br>                unset ($tmparg);<br>        }<br>        $functions_list[] = &apos;function &apos; . $func . &apos; ( &apos; . implode(&apos;, &apos;, $args) . &apos; )&apos; . PHP_EOL;<br>}<br>print_r($functions_list);<br>?>
```
apos; . $param-&gt;getName() . &apos; = &apos; . $param-&gt;getDefaultValue() . &apos;]&apos;;
                } else {
                        $tmparg.= &apos;&amp;&apos; . $param-&gt;getName();
                }
                $args[] = $tmparg;
                unset ($tmparg);
        }
        $functions_list[] = &apos;function &apos; . $func . &apos; ( &apos; . implode(&apos;, &apos;, $args) . &apos; )&apos; . PHP_EOL;
}
print_r($functions_list);
?>
```
<br><br>Output:<br>Array<br>(<br>    [0] =&gt; function foo ( &amp;&amp;bar, &amp;big, [$small = 1] )<br><br>    [1] =&gt; function bar ( &amp;foo )<br><br>    [2] =&gt; function noparams (  )<br><br>    [3] =&gt; function byrefandopt ( [&amp;$the = one] )<br><br>)  

#

[Official documentation page](https://www.php.net/manual/en/function.get-defined-functions.php)

**[To root](/README.md)**