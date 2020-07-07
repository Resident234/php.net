# Magic Methods





The __toString() method is extremely useful for converting class attribute names and values into common string representations of data (of which there are many choices). I mention this as previous references to __toString() refer only to debugging uses.

I have previously used the __toString() method in the following ways:

 - representing a data-holding object as:
&#xA0;&#xA0; - XML
&#xA0;&#xA0; - raw POST data
&#xA0;&#xA0; - a GET query string
&#xA0;&#xA0; - header name:value pairs

 - representing a custom mail object as an actual email (headers then body, all correctly represented)

When creating a class, consider what possible standard string representations are available and, of those, which would be the most relevant with respect to the purpose of the class.

Being able to represent data-holding objects in standardised string forms makes it much easier for your internal representations of data to be shared in an interoperable way with other applications.

  

#



Be very careful to define __set_state() in classes which inherit from a parent using it, as the static __set_state() call will be called for any children.&#xA0; If you are not careful, you will end up with an object of the wrong type.&#xA0; Here is an example:





```
<?php

class A

{

&#xA0; &#xA0; public $var1; 



&#xA0; &#xA0; public static function __set_state($an_array)

&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; $obj = new A;

&#xA0; &#xA0; &#xA0; &#xA0; $obj-&gt;var1 = $an_array[&apos;var1&apos;];&#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; return $obj;

&#xA0; &#xA0; }

}



class B extends A {

}



$b = new B;

$b-&gt;var1 = 5;



eval(&apos;$new_b = &apos; . var_export($b, true) . &apos;;&apos;); 

var_dump($new_b);

/*

object(A)#2 (1) {

&#xA0; [&quot;var1&quot;]=&gt;

&#xA0; int(5)

}

*/

?>
```



  

#



Ever wondered why you can&apos;t throw exceptions from __toString()? Yeah me too. 

Well now you can! This trick allows you to throw any type of exception from within a __toString(), with a full &amp; correct backtrace.

How does it work? Well PHP __toString() handling is not as strict in every case: throwing an Exception from __toString() triggers a fatal E_ERROR, but returning a non-string value from a __toString() triggers a non-fatal E_RECOVERABLE_ERROR. 
Add a little bookkeeping, and can circumvented this PHP deficiency!
(tested to work PHP 5.3+)



```
<?php

set_error_handler(array(&apos;My_ToStringFixer&apos;, &apos;errorHandler&apos;));
error_reporting(E_ALL | E_STRICT);

class My_ToStringFixer
{
&#xA0; &#xA0; protected static $_toStringException;

&#xA0; &#xA0; public static function errorHandler($errorNumber, $errorMessage, $errorFile, $errorLine)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if (isset(self::$_toStringException))
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $exception = self::$_toStringException;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Always unset &apos;_toStringException&apos;, we don&apos;t want a straggler to be found later if something came between the setting and the error
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; self::$_toStringException = null;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (preg_match(&apos;~^Method .*::__toString\(\) must return a string value$~&apos;, $errorMessage))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw $exception;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; public static function throwToStringException($exception)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; // Should not occur with prescribed usage, but in case of recursion: clean out exception, return a valid string, and weep
&#xA0; &#xA0; &#xA0; &#xA0; if (isset(self::$_toStringException))
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; self::$_toStringException = null;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; self::$_toStringException = $exception;

&#xA0; &#xA0; &#xA0; &#xA0; return null;
&#xA0; &#xA0; }
}

class My_Class
{
&#xA0; &#xA0; public function doComplexStuff()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; throw new Exception(&apos;Oh noes!&apos;);
&#xA0; &#xA0; }

&#xA0; &#xA0; public function __toString()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; try
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // do your complex thing which might trigger an exception
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;doComplexStuff();
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; catch (Exception $e)
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // The &apos;return&apos; is required to trigger the trick
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return My_ToStringFixer::throwToStringException($e);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
}

$x = new My_Class();

try
{
&#xA0; &#xA0; echo $x;
}
catch (Exception $e)
{
&#xA0; &#xA0; echo &apos;Caught Exception! : &apos;. $e;
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.magic.php)

**[To root](/README.md)**