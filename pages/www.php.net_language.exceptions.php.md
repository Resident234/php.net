# Exceptions



When catching an exception inside a namespace it is important that you escape to the global space:<br><br>

```
<?php
 namespace SomeNamespace;

 class SomeClass {

  function SomeFunction() {
   try {
    throw new Exception('Some Error Message');
   } catch (\Exception $e) {
    var_dump($e->getMessage());
   }
  }

 }
?>
```
  

---

If a TRY has a FINALLY, a RETURN either in the TRY or a CATCH won&apos;t terminate the script. Code in the same block after the RETURN will not be executed, and the RETURN itself will be "copied" to the bottom of the FINALLY block to be executed.<br><br>a RETURN in the FINALLY block will override value(s) returned from the TRY or a CATCH block.<br><br>An EXIT or a DIE always terminate the script after themselves.<br><br>code 1<br><br>

```
<?php
    function foo(){
        $bar = 1;
        try{
            throw new Exception('I am Wu Xiancheng.');
        }catch(Exception $e){
            return $bar;
            $bar--; // this line will be ignored
        }finally{
            $bar++;
        }
    }
    echo foo(); // 2
?>
```


code 2



```
<?php
    function foo(){
        $bar = 1;
        try{
            throw new Exception('I am Wu Xiancheng.');
        }catch(Exception $e){
            return $bar;
            $bar--; // this line will be ignored
        }finally{
            $bar++;
            return $bar;
        }
    }
    echo foo(); //2 
?>
```


code 3



```
<?php
    function foo(){
        $bar = 1;
        try{
            throw new Exception('I am Wu Xiancheng.');
        }catch(Exception $e){
            return $bar;
            $bar--; // this line will be ignored
        }finally{
            return 100;
        }
    }
    echo foo(); //100
?>
```
  

---

If you intend on creating a lot of custom exceptions, you may find this code useful.  I&apos;ve created an interface and an abstract exception class that ensures that all parts of the built-in Exception class are preserved in child classes.  It also properly pushes all information back to the parent constructor ensuring that nothing is lost.  This allows you to quickly create new exceptions on the fly.  It also overrides the default __toString method with a more thorough one.<br><br>

```
<?php
interface IException
{
    /* Protected methods inherited from Exception class */
    public function getMessage();                 // Exception message 
    public function getCode();                    // User-defined Exception code
    public function getFile();                    // Source filename
    public function getLine();                    // Source line
    public function getTrace();                   // An array of the backtrace()
    public function getTraceAsString();           // Formated string of trace
    
    /* Overrideable methods inherited from Exception class */
    public function __toString();                 // formated string for display
    public function __construct($message = null, $code = 0);
}

abstract class CustomException extends Exception implements IException
{
    protected $message = 'Unknown exception';     // Exception message
    private   $string;                            // Unknown
    protected $code    = 0;                       // User-defined exception code
    protected $file;                              // Source filename of exception
    protected $line;                              // Source line of exception
    private   $trace;                             // Unknown

    public function __construct($message = null, $code = 0)
    {
        if (!$message) {
            throw new $this('Unknown '. get_class($this));
        }
        parent::__construct($message, $code);
    }
    
    public function __toString()
    {
        return get_class($this) . " '{$this->message}' in {$this->file}({$this->line})\n"
                                . "{$this->getTraceAsString()}";
    }
}
?>
```


Now you can create new exceptions in one line:



```
<?php
class TestException extends CustomException {}
?>
```


Here's a test that shows that all information is properly preserved throughout the backtrace.



```
<?php
function exceptionTest()
{
    try {
        throw new TestException();
    }
    catch (TestException $e) {
        echo "Caught TestException ('{$e->getMessage()}')\n{$e}\n";
    }
    catch (Exception $e) {
        echo "Caught Exception ('{$e->getMessage()}')\n{$e}\n";
    }
}

echo '<pre>' . exceptionTest() . '</pre>';
?>
```
<br><br>Here&apos;s a sample output:<br><br>Caught TestException (&apos;Unknown TestException&apos;)<br>TestException &apos;Unknown TestException&apos; in C:\xampp\htdocs\CustomException\CustomException.php(31)<br>#0 C:\xampp\htdocs\CustomException\ExceptionTest.php(19): CustomException-&gt;__construct()<br>#1 C:\xampp\htdocs\CustomException\ExceptionTest.php(43): exceptionTest()<br>#2 {main}  

---

catch doesn&apos;t check for the existence of the Exception class, so avoid typo.<br><br>

```
<?php
   class MyException extends Exception
   {
      ...
   }

   try
   {
      throw new MyException(...);
   }
   catch (MuException $e) // <--- typo
   {
      ...
   }
?>
```
 <br><br>You WON&apos;T get<br>   Fatal error: Class MuException could not be loaded ...<br><br>You WILL get<br>   Fatal error: Uncaught exception &apos;MyException&apos; ...  

---

Custom error handling on entire pages can avoid half rendered pages for the users:<br><br>

```
<?php
ob_start();
try {
    /*contains all page logic 
    and throws error if needed*/
    ...
} catch (Exception $e) {
  ob_end_clean();
  displayErrorPage($e->getMessage());
}
?>
```
  

---

Using a return statement inside a finally block will override any other return statement or thrown exception from the try block and all defined catch blocks.   Code execution in the parent stack will continue as if the exception was never thrown.  <br><br>Frankly this is a good design decision because it means I can optionally dismiss all thrown exceptions from 1 or more catch blocks in one place, without having to nest my whole try block inside an additional (and otherwise needless) try/catch block.<br><br>This is the same behavior as Java, whereas C# throws a compile time error when a return statement exists inside a finally block.  So I figured it was worth pointing out to PHP devs who may not have any exposure to finally blocks or how other languages do it.<br><br>

```
<?php

function asdf()
{
    try {
        throw new Exception('error');
    }
    catch(Exception $e) {
        echo "An error occurred";
        throw $e;
    }
    finally {
                //This overrides the exception as if it were never thrown
        return "\nException erased";
    }
}

try {
    echo asdf();
}
catch(Exception $e) {
    echo "\nResult: " . $e->getMessage();
}
?>
```
<br><br>The output from above will look like this:<br><br>    An error occurred<br>    Exception erased<br><br>Without the return statement in the finally block it would look like this:<br><br>    An error occurred<br>    Result: error  

---

Type declarations will trigger Uncaught TypeError when a different type is passed. And it cannot be caught with the Exception class.<br>

```
<?php
    function xc(array $a){        
    }    
    try{
        xc(4);
    }catch (Exception $e){ // failed to catch it
        echo $e->getMessage();
    }
?>
```


You should use TypeError instead for PHP 7+,



```
<?php
    function xc(array $a){    
    }    
    try{
        xc(4);
    }catch (TypeError $e){
        echo $e->getMessage();
    }
?>
```


In php version prior to 7.0, you should translate Catchable fatal errors to an exception and then catch it.



```
<?php
    function exceptionErrorHandler($errNumber, $errStr, $errFile, $errLine ) {
        throw new ErrorException($errStr, 0, $errNumber, $errFile, $errLine);
    }
    set_error_handler('exceptionErrorHandler');
    function s(array $a){        
    }
    try{
        s(4);
    }catch (Exception $e){
        echo $e->getMessage();
    }
?>
```
  

---

Sometimes you want a single catch() to catch multiple types of Exception. In a language like Python, you can specify multiple types in a catch(), but in PHP you can only specify one. This can be annoying when you want handle many different Exceptions with the same catch() block.<br><br>However, you can replicate the functionality somewhat, because catch(&lt;classname&gt; $var) will match the given &lt;classname&gt; *or any of it&apos;s sub-classes*.<br><br>For example:<br><br>

```
<?php
class DisplayException extends Exception {};
class FileException extends Exception {};
class AccessControl extends FileException {}; // Sub-class of FileException
class IOError extends FileException {}; // Sub-class of FileException

try {
  if(!is_readable($somefile))
     throw new IOError("File is not readable!");
  if(!user_has_access_to_file($someuser, $somefile))
     throw new AccessControl("Permission denied!");
  if(!display_file($somefile))
     throw new DisplayException("Couldn't display file!");

} catch (FileException $e) {
  // This block will catch FileException, AccessControl or IOError exceptions, but not Exceptions or DisplayExceptions.
  echo "File error: ".$e->getMessage();
  exit(1);
}
?>
```
<br><br>Corollary: If you want to catch *any* exception, no matter what the type, just use "catch(Exception $var)", because all exceptions are sub-classes of the built-in Exception.  

---

If you are using a namespace, you must indicate the global namespace when using Exceptions.<br>

```
<?php
namespace alpha;
function foo(){
    throw new \Exception("Something is wrong!");
    // throw new Exception(""); will fail
}

try {
    foo();
} catch( \Exception $e ) {
    // catch( Exception $e ) will give no warning, but will not catch Exception
    echo "ERROR: $e";
}

?>
```
  

---

&#x2018;Normal execution (when no exception is thrown within the try block, *or when a catch matching the thrown exception&#x2019;s class is not present*) will continue after that last catch block defined in sequence.&#x2019;<br><br>&#x2018;If an exception is not caught, a PHP Fatal Error will be issued with an &#x201C;Uncaught Exception &#x2026;&#x201D; message, unless a handler has been defined with set_exception_handler().&#x2019;<br><br>These two sentences seem a bit contradicting about what happens &#x2018;when a catch matching the thrown exception&#x2019;s class is not present&#x2019; (and the second sentence is actually correct).  

---

Just an example why finally blocks are usefull (5.5)<br><br>

```
<?php

//without catch
function example() {
  try {
    //do something that throws an exeption
  }
  finally {
    //this code will be executed even when the exception is executed
  }
}

function example2() {
  try {
     //open sql connection check user as example
     if(condition) { 
        return false;
     }
  }
  finally {
    //close the sql connection, this will be executed even if the return is called.
  }
}

?>
```
  

---

The "finally" block can change the exception that has been throw by the catch block.<br><br>

```
<?php
try{
        try {
                throw new \Exception("Hello");
        } catch(\Exception $e) {
                echo $e->getMessage()." catch in\n";
                throw $e;
        } finally {
                echo $e->getMessage()." finally \n";
                throw new \Exception("Bye");
        }
} catch (\Exception $e) {
        echo $e->getMessage()." catch out\n";
}
?>
```
<br><br>The output is:<br><br>Hello catch in<br>Hello finally <br>Bye catch out  

---

[Official documentation page](https://www.php.net/manual/en/language.exceptions.php)

**[To root](/README.md)**