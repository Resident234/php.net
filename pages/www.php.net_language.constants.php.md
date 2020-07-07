# Constants





11/14/2016 - note updated by sobak

-----



CONSTANTS and PHP Class Definitions



Using &quot;define(&apos;MY_VAR&apos;, &apos;default value&apos;)&quot; INSIDE a class definition does not work as expected. You have to use the PHP keyword &apos;const&apos; and initialize it with a scalar value -- boolean, int, float, string (or array in PHP 5.6+) -- right away.





```
<?php



define(&apos;MIN_VALUE&apos;, &apos;0.0&apos;);&#xA0;&#xA0; // RIGHT - Works OUTSIDE of a class definition.

define(&apos;MAX_VALUE&apos;, &apos;1.0&apos;);&#xA0;&#xA0; // RIGHT - Works OUTSIDE of a class definition.



//const MIN_VALUE = 0.0;&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; RIGHT - Works both INSIDE and OUTSIDE of a class definition.

//const MAX_VALUE = 1.0;&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; RIGHT - Works both INSIDE and OUTSIDE of a class definition.



class Constants

{

&#xA0; //define(&apos;MIN_VALUE&apos;, &apos;0.0&apos;);&#xA0; WRONG - Works OUTSIDE of a class definition.

&#xA0; //define(&apos;MAX_VALUE&apos;, &apos;1.0&apos;);&#xA0; WRONG - Works OUTSIDE of a class definition.



&#xA0; const MIN_VALUE = 0.0;&#xA0; &#xA0; &#xA0; // RIGHT - Works INSIDE of a class definition.

&#xA0; const MAX_VALUE = 1.0;&#xA0; &#xA0; &#xA0; // RIGHT - Works INSIDE of a class definition.



&#xA0; public static function getMinValue()

&#xA0; {

&#xA0; &#xA0; return self::MIN_VALUE;

&#xA0; }



&#xA0; public static function getMaxValue()

&#xA0; {

&#xA0; &#xA0; return self::MAX_VALUE;

&#xA0; }

}



?>
```




#Example 1:

You can access these constants DIRECTLY like so:

 * type the class name exactly.

 * type two (2) colons.

 * type the const name exactly.



#Example 2:

Because our class definition provides two (2) static functions, you can also access them like so:

 * type the class name exactly.

 * type two (2) colons.

 * type the function name exactly (with the parentheses).





```
<?php



#Example 1:

$min = Constants::MIN_VALUE;

$max = Constants::MAX_VALUE;



#Example 2:

$min = Constants::getMinValue();

$max = Constants::getMaxValue();



?>
```




Once class constants are declared AND initialized, they cannot be set to different values -- that is why there are no setMinValue() and setMaxValue() functions in the class definition -- which means they are READ-ONLY and STATIC (shared by all instances of the class).

  

#



class constant are by default public in nature but they cannot be assigned visibility factor and in turn gives syntax error



```
<?php

class constants {

&#xA0; &#xA0; const MAX_VALUE = 10;
&#xA0; &#xA0; &#xA0; &#xA0; public const MIN_VALUE =1;

}

// This will work
echo constants::MAX_VALUE;

// This will return syntax error 
echo constants::MIN_VALUE; 
?>
```



  

#



Lets expand comment of &apos;storm&apos; about usage of undefined constants. His claim that &apos;An undefined constant evaluates as true...&apos; is wrong and right at same time. As said further in documentation &apos; If you use an undefined constant, PHP assumes that you mean the name of the constant itself, just as if you called it as a string...&apos;. So yeah, undefined global constant when accessed directly will be resolved as string equal to name of sought constant (as thought PHP supposes that programmer had forgot apostrophes and autofixes it) and non-zero non-empty string converts to True.

There are two ways to prevent this:
1. always use function constant(&apos;CONST_NAME&apos;) to get constant value (BTW it also works for class constants - constant(&apos;CLASS_NAME::CONST_NAME&apos;) );
2. use only class constants (that are defined inside of class using keyword const) because they are not converted to string when not found but throw exception instead (Fatal error: Undefined class constant).

  

#



I find using the concatenation operator helps disambiguate value assignments with constants. For example, setting constants in a global configuration file:





```
<?php

define(&apos;LOCATOR&apos;,&#xA0;&#xA0; &quot;/locator&quot;);

define(&apos;CLASSES&apos;,&#xA0;&#xA0; LOCATOR.&quot;/code/classes&quot;);

define(&apos;FUNCTIONS&apos;, LOCATOR.&quot;/code/functions&quot;);

define(&apos;USERDIR&apos;,&#xA0;&#xA0; LOCATOR.&quot;/user&quot;);

?>
```




Later, I can use the same convention when invoking a constant&apos;s value for static constructs such as require() calls:





```
<?php

require_once(FUNCTIONS.&quot;/database.fnc&quot;);

require_once(FUNCTIONS.&quot;/randchar.fnc&quot;);

?>
```




as well as dynamic constructs, typical of value assignment to variables:





```
<?php

$userid&#xA0; = randchar(8,&apos;anc&apos;,&apos;u&apos;);

$usermap = USERDIR.&quot;/&quot;.$userid.&quot;.png&quot;;

?>
```




The above convention works for me, and helps produce self-documenting code.



-- Erich

  

#



PHP Modules also define constants.&#xA0; Make sure to avoid constant name collisions.&#xA0; There are two ways to do this that I can think of.
First: in your code make sure that the constant name is not already used.&#xA0; ex. 

```
<?php if (! defined(&quot;CONSTANT_NAME&quot;)) { Define(&quot;CONSTANT_NAME&quot;,&quot;Some Value&quot;); } ?>
```
&#xA0; This can get messy when you start thinking about collision handling, and the implications of this.
Second: Use some off prepend to all your constant names without exception&#xA0; ex. 

```
<?php Define(&quot;SITE_CONSTANT_NAME&quot;,&quot;Some Value&quot;); ?>
```


Perhaps the developers or documentation maintainers could recommend a good prepend and ask module writers to avoid that prepend in modules.

  

#



Warning, constants used within the heredoc syntax (http://www.php.net/manual/en/language.types.string.php) are not interpreted!



Editor&apos;s Note: This is true. PHP has no way of recognizing the constant from any other string of characters within the heredoc block.

  

#

[Official documentation page](https://www.php.net/manual/en/language.constants.php)

**[To root](/README.md)**