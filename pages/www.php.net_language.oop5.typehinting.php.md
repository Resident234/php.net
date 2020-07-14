# Type Hinting



People often ask about scalar/basic typehints.  Here is a drop in class that I use in my MVC framework that will enable typehints through the use of a custom error handler.<br><br>Note: You should include this code above all other code in your include headers and if you are the using set_error_handler() function you should be aware that this uses it as well.  You may need to chain your set_error_handlers()<br><br>Why?<br>1) Because people are sick of using the is_* functions to validate parameters.<br>2) Reduction of redundant coding for defensive coders.<br>3) Functions/Methods are self defining/documenting as to required input.<br><br>Also..<br>Follow the discussion for typehints in PHP 6.0 on the PHP Internals boards.<br><br>

```
<?php

define('TYPEHINT_PCRE'              ,'/^Argument (\d)+ passed to (?:(\w+)::)?(\w+)\(\) must be an instance of (\w+), (\w+) given/');

class Typehint
{

    private static $Typehints = array(
        'boolean'   => 'is_bool',
        'integer'   => 'is_int',
        'float'     => 'is_float',
        'string'    => 'is_string',
        'resrouce'  => 'is_resource'
    );

    private function __Constrct() {}

    public static function initializeHandler()
    {

        set_error_handler('Typehint::handleTypehint');

        return TRUE;
    }

    private static function getTypehintedArgument($ThBackTrace, $ThFunction, $ThArgIndex, &amp;$ThArgValue)
    {

        foreach ($ThBackTrace as $ThTrace)
        {

            // Match the function; Note we could do more defensive error checking.
            if (isset($ThTrace['function']) &amp;&amp; $ThTrace['function'] == $ThFunction)
            {

                $ThArgValue = $ThTrace['args'][$ThArgIndex - 1];

                return TRUE;
            }
        }

        return FALSE;
    }

    public static function handleTypehint($ErrLevel, $ErrMessage)
    {

        if ($ErrLevel == E_RECOVERABLE_ERROR)
        {

            if (preg_match(TYPEHINT_PCRE, $ErrMessage, $ErrMatches))
            {

                list($ErrMatch, $ThArgIndex, $ThClass, $ThFunction, $ThHint, $ThType) = $ErrMatches;

                if (isset(self::$Typehints[$ThHint]))
                {

                    $ThBacktrace = debug_backtrace();
                    $ThArgValue  = NULL;

                    if (self::getTypehintedArgument($ThBacktrace, $ThFunction, $ThArgIndex, $ThArgValue))
                    {

                        if (call_user_func(self::$Typehints[$ThHint], $ThArgValue))
                        {

                            return TRUE;
                        }
                    }
                }
            }
        }

        return FALSE;
    }
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
<br><br>You get the picture..  

#

The scalar type hinting solutions are all overthinking it. I provided the optimized regex version, as well as the fastest implementation I&apos;ve come up with, which just uses strpos. Then I benchmark both against the TypeHint class.<br><br>

```
<?php
function optimized_strpos($ErrLevel, $ErrMessage) {
        if ($ErrLevel == E_RECOVERABLE_ERROR
            // order this according to what your app uses most
            return strpos($ErrMessage, 'must be an instance of string, string')
                || strpos($ErrMessage, 'must be an instance of integer, integer')
                || strpos($ErrMessage, 'must be an instance of float, double')
                || strpos($ErrMessage, 'must be an instance of boolean, boolean')
                || strpos($ErrMessage, 'must be an instance of resource, resource');
}

function optimized_regex($ErrLevel, $ErrMessage) {
        if ($ErrLevel == E_RECOVERABLE_ERROR) {
            if(preg_match('/^Argument \d+ passed to (?:\w+::)?\w+\(\) must be an instance of (\w+), (\w+) given/', $ErrMessage, $matches))
                return $matches[1] == ($matches[2] == 'double' ? 'float' : $matches[2]);
        }
}
?>
```
<br><br>BENCHMARKING Typehint::handleTypehint()<br>string type hint.....[function it(string $var) {}]............2.1588530540466 seconds<br>float type hint......[function it(float $var) {}].............2.1563150882721 seconds<br>integer type hint....[function it(integer $var) {}]...........2.1579530239105 seconds<br>boolean type hint....[function it(boolean $var) {}]...........2.1590459346771 seconds<br><br>BENCHMARKING optimized_regex()<br>string type hint.....[function it(string $var) {}]............0.88872504234314 seconds<br>float type hint......[function it(float $var) {}].............0.88528990745544 seconds<br>integer type hint....[function it(integer $var) {}]...........0.89038777351379 seconds<br>boolean type hint....[function it(boolean $var) {}]...........0.89061188697815 seconds<br><br>BENCHMARKING optimized_strpos()<br>string type hint.....[function it(string $var) {}]............0.52635812759399 seconds<br>float type hint......[function it(float $var) {}].............0.74228310585022 seconds<br>integer type hint....[function it(integer $var) {}]...........0.63721108436584 seconds<br>boolean type hint....[function it(boolean $var) {}]...........0.8429491519928 seconds  

#

Daniel&apos;s typehint implementation was just what I was looking for but performance in production wasn&apos;t going to cut it. Calling a backtrace every time hurts performance. For my implementation I didn&apos;t use it, after all, PHP tells us what the data type is in the error message, I don&apos;t feel I need to evaluate the argument where I am using typehinting. Here is the cut down version I use in my error handling class:<br><br>

```
<?php
        public static function typehint($level, $message)
        {
            if($level == E_RECOVERABLE_ERROR)
            {
                if(preg_match('/^Argument (\d)+ passed to (?:(\w+)::)?(\w+)\(\) must be an instance of (\w+), (\w+) given/', $message, $match))
                {
                    if($match[4] == $match[5])
                        return true;
                }
            }
            
            return false;
        }
?>
```
<br><br>Hope this can be of use to somebody.  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.typehinting.php)

**[To root](/README.md)**