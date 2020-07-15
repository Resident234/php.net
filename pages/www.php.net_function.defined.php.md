# defined



My preferred way of checking if a constant is set, and if it isn&apos;t - setting it (could be used to set defaults in a file, where the user has already had the opportunity to set their own values in another.)<br><br>

```
<?php

defined('CONSTANT') or define('CONSTANT', 'SomeDefaultValue');

?>
```
<br><br>Dan.  

#

You can use the late static command "static::" withing defined as well. This example outputs - as expected - "int (2)"<br><br>

```
<?php
  abstract class class1
  {
    public function getConst()
    {
      return defined('static::SOME_CONST') ? static::SOME_CONST : false;
    }
  }
  
  final class class2 extends class1
  {
    const SOME_CONST = 2;
  }
  
  $class2 = new class2;
  
  var_dump($class2->getConst());
?>
```
  

#

Before using defined() have a look at the following benchmarks:<br><br>true                                       0.65ms<br>$true                                      0.69ms (1)<br>$config[&apos;true&apos;]                            0.87ms<br>TRUE_CONST                                 1.28ms (2)<br>true                                       0.65ms<br>defined(&apos;TRUE_CONST&apos;)                      2.06ms (3)<br>defined(&apos;UNDEF_CONST&apos;)                    12.34ms (4)<br>isset($config[&apos;def_key&apos;])                  0.91ms (5)<br>isset($config[&apos;undef_key&apos;])                0.79ms<br>isset($empty_hash[$good_key])              0.78ms<br>isset($small_hash[$good_key])              0.86ms<br>isset($big_hash[$good_key])                0.89ms<br>isset($small_hash[$bad_key])               0.78ms<br>isset($big_hash[$bad_key])                 0.80ms<br><br>PHP Version 5.2.6, Apache 2.0, Windows XP<br><br>Each statement was executed 1000 times and while a 12ms overhead on 1000 calls isn&apos;t going to have the end users tearing their hair out, it does throw up some interesting results when comparing to if(true):<br><br>1) if($true) was virtually identical<br>2) if(TRUE_CONST) was almost twice as slow - I guess that the substitution isn&apos;t done at compile time (I had to double check this one!)<br>3) defined() is 3 times slower if the constant exists<br>4) defined() is 19 TIMES SLOWER if the constant doesn&apos;t exist!<br>5) isset() is remarkably efficient regardless of what you throw at it (great news for anyone implementing array driven event systems - me!)<br><br>May want to avoid if(defined(&apos;DEBUG&apos;))...  

#

if you want to check id a class constant is defined use self:: before the constant name:<br><br>

```
<?php
defined('self::CONSTANT_NAME');
?>
```
  

#

I saw that PHP doesn&apos;t have an enum function so I created my own. It&apos;s not necessary, but can come in handy from time to time.<br><br>

```
<?php
    function enum()
    {
        $args = func_get_args();
        foreach($args as $key=>$arg)
        {
            if(defined($arg))
            {
                 die('Redefinition of defined constant ' . $arg);
            }

            define($arg, $key);
        }
    }
    
    enum('ONE','TWO','THREE');
    echo ONE, ' ', TWO, ' ', THREE;
?>
```
  

#

This function, along with constant(), is namespace sensitive. And it might help if you imagine them always running under the "root namespace":<br><br>

```
<?php
namespace FOO\BAR
{
    const WMP="wmp";
    function test()
    {
        if(defined("WMP")) echo "direct: ".constant("WMP"); //doesn't work;
        elseif(defined("FOO\\BAR\\WMP")) echo "namespace: ".constant("FOO\\BAR\\WMP"); //works
        echo WMP; //works
    }
}
namespace
{
    \FOO\BAR\test();
}?>
```
  

#

If you wish to protect files from direct access I normally use this:<br><br>index.php:<br><br>

```
<?php
// Main stuff here
define('START',microtime());

include "x.php";
?>
```


x.php:



```
<?php
defined('START')||(header("HTTP/1.1 403 Forbidden")&amp;die('403.14 - Directory listing denied.'));
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.defined.php)

**[To root](/README.md)**