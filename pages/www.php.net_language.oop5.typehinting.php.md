# Type Hinting





People often ask about scalar/basic typehints.&#xA0; Here is a drop in class that I use in my MVC framework that will enable typehints through the use of a custom error handler.

Note: You should include this code above all other code in your include headers and if you are the using set_error_handler() function you should be aware that this uses it as well.&#xA0; You may need to chain your set_error_handlers()

Why?
1) Because people are sick of using the is_* functions to validate parameters.
2) Reduction of redundant coding for defensive coders.
3) Functions/Methods are self defining/documenting as to required input.

Also..
Follow the discussion for typehints in PHP 6.0 on the PHP Internals boards.



```
<?php

define(&apos;TYPEHINT_PCRE&apos;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ,&apos;/^Argument (\d)+ passed to (?:(\w+)::)?(\w+)\(\) must be an instance of (\w+), (\w+) given/&apos;);

class Typehint
{

&#xA0; &#xA0; private static $Typehints = array(
&#xA0; &#xA0; &#xA0; &#xA0; &apos;boolean&apos;&#xA0;&#xA0; =&gt; &apos;is_bool&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &apos;integer&apos;&#xA0;&#xA0; =&gt; &apos;is_int&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &apos;float&apos;&#xA0; &#xA0;&#xA0; =&gt; &apos;is_float&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &apos;string&apos;&#xA0; &#xA0; =&gt; &apos;is_string&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &apos;resrouce&apos;&#xA0; =&gt; &apos;is_resource&apos;
&#xA0; &#xA0; );

&#xA0; &#xA0; private function __Constrct() {}

&#xA0; &#xA0; public static function initializeHandler()
&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; set_error_handler(&apos;Typehint::handleTypehint&apos;);

&#xA0; &#xA0; &#xA0; &#xA0; return TRUE;
&#xA0; &#xA0; }

&#xA0; &#xA0; private static function getTypehintedArgument($ThBackTrace, $ThFunction, $ThArgIndex, &amp;$ThArgValue)
&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; foreach ($ThBackTrace as $ThTrace)
&#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Match the function; Note we could do more defensive error checking.
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (isset($ThTrace[&apos;function&apos;]) &amp;&amp; $ThTrace[&apos;function&apos;] == $ThFunction)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $ThArgValue = $ThTrace[&apos;args&apos;][$ThArgIndex - 1];

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return TRUE;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; return FALSE;
&#xA0; &#xA0; }

&#xA0; &#xA0; public static function handleTypehint($ErrLevel, $ErrMessage)
&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; if ($ErrLevel == E_RECOVERABLE_ERROR)
&#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (preg_match(TYPEHINT_PCRE, $ErrMessage, $ErrMatches))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; list($ErrMatch, $ThArgIndex, $ThClass, $ThFunction, $ThHint, $ThType) = $ErrMatches;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (isset(self::$Typehints[$ThHint]))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $ThBacktrace = debug_backtrace();
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $ThArgValue&#xA0; = NULL;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (self::getTypehintedArgument($ThBacktrace, $ThFunction, $ThArgIndex, $ThArgValue))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (call_user_func(self::$Typehints[$ThHint], $ThArgValue))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return TRUE;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; return FALSE;
&#xA0; &#xA0; }
}

Typehint::initializeHandler();

?>
```


An are some examples of the class in use:



```
<?php

function teststring(string $string) { echo $string; }
function testinteger(integer $integer) { echo $integer; }
function testfloat(float $float) { echo $float; }

// This will work for class methods as well.

?>
```


You get the picture..

  

#



The scalar type hinting solutions are all overthinking it. I provided the optimized regex version, as well as the fastest implementation I&apos;ve come up with, which just uses strpos. Then I benchmark both against the TypeHint class.



```
<?php
function optimized_strpos($ErrLevel, $ErrMessage) {
&#xA0; &#xA0; &#xA0; &#xA0; if ($ErrLevel == E_RECOVERABLE_ERROR
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // order this according to what your app uses most
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return strpos($ErrMessage, &apos;must be an instance of string, string&apos;)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; || strpos($ErrMessage, &apos;must be an instance of integer, integer&apos;)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; || strpos($ErrMessage, &apos;must be an instance of float, double&apos;)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; || strpos($ErrMessage, &apos;must be an instance of boolean, boolean&apos;)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; || strpos($ErrMessage, &apos;must be an instance of resource, resource&apos;);
}

function optimized_regex($ErrLevel, $ErrMessage) {
&#xA0; &#xA0; &#xA0; &#xA0; if ($ErrLevel == E_RECOVERABLE_ERROR) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(preg_match(&apos;/^Argument \d+ passed to (?:\w+::)?\w+\(\) must be an instance of (\w+), (\w+) given/&apos;, $ErrMessage, $matches))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $matches[1] == ($matches[2] == &apos;double&apos; ? &apos;float&apos; : $matches[2]);
&#xA0; &#xA0; &#xA0; &#xA0; }
}
?>
```


BENCHMARKING Typehint::handleTypehint()
string type hint.....[function it(string $var) {}]............2.1588530540466 seconds
float type hint......[function it(float $var) {}].............2.1563150882721 seconds
integer type hint....[function it(integer $var) {}]...........2.1579530239105 seconds
boolean type hint....[function it(boolean $var) {}]...........2.1590459346771 seconds

BENCHMARKING optimized_regex()
string type hint.....[function it(string $var) {}]............0.88872504234314 seconds
float type hint......[function it(float $var) {}].............0.88528990745544 seconds
integer type hint....[function it(integer $var) {}]...........0.89038777351379 seconds
boolean type hint....[function it(boolean $var) {}]...........0.89061188697815 seconds

BENCHMARKING optimized_strpos()
string type hint.....[function it(string $var) {}]............0.52635812759399 seconds
float type hint......[function it(float $var) {}].............0.74228310585022 seconds
integer type hint....[function it(integer $var) {}]...........0.63721108436584 seconds
boolean type hint....[function it(boolean $var) {}]...........0.8429491519928 seconds

  

#



Daniel&apos;s typehint implementation was just what I was looking for but performance in production wasn&apos;t going to cut it. Calling a backtrace every time hurts performance. For my implementation I didn&apos;t use it, after all, PHP tells us what the data type is in the error message, I don&apos;t feel I need to evaluate the argument where I am using typehinting. Here is the cut down version I use in my error handling class:





```
<?php

&#xA0; &#xA0; &#xA0; &#xA0; public static function typehint($level, $message)

&#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if($level == E_RECOVERABLE_ERROR)

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(preg_match(&apos;/^Argument (\d)+ passed to (?:(\w+)::)?(\w+)\(\) must be an instance of (\w+), (\w+) given/&apos;, $message, $match))

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if($match[4] == $match[5])

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return true;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return false;

&#xA0; &#xA0; &#xA0; &#xA0; }

?>
```




Hope this can be of use to somebody.

  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.typehinting.php)

**[To root](/README.md)**