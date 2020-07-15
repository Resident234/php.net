# get_defined_functions



You can list all arguments using ReflectionFunction class. It&apos;s not necessary to parse selected files/files as suggested by Nguyet.Duc.<br><br>http://php.net/manual/pl/class.reflectionfunction.php<br><br>Example:<br>

```
<?php
function foo(&amp;$bar, $big, $small = 1) {}
function bar($foo) {}
function noparams() {}
function byrefandopt(&amp;$the = 'one') {}

$functions = get_defined_functions();
$functions_list = array();
foreach ($functions['user'] as $func) {
        $f = new ReflectionFunction($func);
        $args = array();
        foreach ($f->getParameters() as $param) {
                $tmparg = '';
                if ($param->isPassedByReference()) $tmparg = '&amp;';
                if ($param->isOptional()) {
                        $tmparg = '[' . $tmparg . '<br><br>Output:<br>Array<br>(<br>    [0] =&gt; function foo ( &amp;&amp;bar, &amp;big, [$small = 1] )<br><br>    [1] =&gt; function bar ( &amp;foo )<br><br>    [2] =&gt; function noparams (  )<br><br>    [3] =&gt; function byrefandopt ( [&amp;$the = one] )<br><br>)   . $param->getName() . ' = ' . $param->getDefaultValue() . ']';
                } else {
                        $tmparg.= '&amp;' . $param->getName();
                }
                $args[] = $tmparg;
                unset ($tmparg);
        }
        $functions_list[] = 'function ' . $func . ' ( ' . implode(', ', $args) . ' )' . PHP_EOL;
}
print_r($functions_list);
?>
```
<br><br>Output:<br>Array<br>(<br>    [0] =&gt; function foo ( &amp;&amp;bar, &amp;big, [$small = 1] )<br><br>    [1] =&gt; function bar ( &amp;foo )<br><br>    [2] =&gt; function noparams (  )<br><br>    [3] =&gt; function byrefandopt ( [&amp;$the = one] )<br><br>)  

#

[Official documentation page](https://www.php.net/manual/en/function.get-defined-functions.php)

**[To root](/README.md)**