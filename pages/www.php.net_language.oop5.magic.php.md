# Magic Methods



The __toString() method is extremely useful for converting class attribute names and values into common string representations of data (of which there are many choices). I mention this as previous references to __toString() refer only to debugging uses.<br><br>I have previously used the __toString() method in the following ways:<br><br> - representing a data-holding object as:<br>   - XML<br>   - raw POST data<br>   - a GET query string<br>   - header name:value pairs<br><br> - representing a custom mail object as an actual email (headers then body, all correctly represented)<br><br>When creating a class, consider what possible standard string representations are available and, of those, which would be the most relevant with respect to the purpose of the class.<br><br>Being able to represent data-holding objects in standardised string forms makes it much easier for your internal representations of data to be shared in an interoperable way with other applications.  

---

Be very careful to define __set_state() in classes which inherit from a parent using it, as the static __set_state() call will be called for any children.  If you are not careful, you will end up with an object of the wrong type.  Here is an example:<br><br>

```
<?php
class A
{
    public $var1; 

    public static function __set_state($an_array)
    {
        $obj = new A;
        $obj->var1 = $an_array['var1'];  
        return $obj;
    }
}

class B extends A {
}

$b = new B;
$b->var1 = 5;

eval('$new_b = ' . var_export($b, true) . ';'); 
var_dump($new_b);
/*
object(A)#2 (1) {
  ["var1"]=>
  int(5)
}
*/
?>
```
  

---

Ever wondered why you can&apos;t throw exceptions from __toString()? Yeah me too. <br><br>Well now you can! This trick allows you to throw any type of exception from within a __toString(), with a full &amp; correct backtrace.<br><br>How does it work? Well PHP __toString() handling is not as strict in every case: throwing an Exception from __toString() triggers a fatal E_ERROR, but returning a non-string value from a __toString() triggers a non-fatal E_RECOVERABLE_ERROR. <br>Add a little bookkeeping, and can circumvented this PHP deficiency!<br>(tested to work PHP 5.3+)<br><br>

```
<?php

set_error_handler(array('My_ToStringFixer', 'errorHandler'));
error_reporting(E_ALL | E_STRICT);

class My_ToStringFixer
{
    protected static $_toStringException;

    public static function errorHandler($errorNumber, $errorMessage, $errorFile, $errorLine)
    {
        if (isset(self::$_toStringException))
        {
            $exception = self::$_toStringException;
            // Always unset '_toStringException', we don't want a straggler to be found later if something came between the setting and the error
            self::$_toStringException = null;
            if (preg_match('~^Method .*::__toString\(\) must return a string value$~', $errorMessage))
                throw $exception;
        }
        return false;
    }
    
    public static function throwToStringException($exception)
    {
        // Should not occur with prescribed usage, but in case of recursion: clean out exception, return a valid string, and weep
        if (isset(self::$_toStringException))
        {
            self::$_toStringException = null;
            return '';
        }

        self::$_toStringException = $exception;

        return null;
    }
}

class My_Class
{
    public function doComplexStuff()
    {
        throw new Exception('Oh noes!');
    }

    public function __toString()
    {
        try
        {
            // do your complex thing which might trigger an exception
            return $this->doComplexStuff();
        }
        catch (Exception $e)
        {
            // The 'return' is required to trigger the trick
            return My_ToStringFixer::throwToStringException($e);
        }
    }
}

$x = new My_Class();

try
{
    echo $x;
}
catch (Exception $e)
{
    echo 'Caught Exception! : '. $e;
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/language.oop5.magic.php)

**[To root](/README.md)**