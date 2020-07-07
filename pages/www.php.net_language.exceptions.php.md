# Exceptions





When catching an exception inside a namespace it is important that you escape to the global space:



```
<?php
 namespace SomeNamespace;

 class SomeClass {

&#xA0; function SomeFunction() {
&#xA0;&#xA0; try {
&#xA0; &#xA0; throw new Exception(&apos;Some Error Message&apos;);
&#xA0;&#xA0; } catch (\Exception $e) {
&#xA0; &#xA0; var_dump($e-&gt;getMessage());
&#xA0;&#xA0; }
&#xA0; }

 }
?>
```



  

#



If a TRY has a FINALLY, a RETURN either in the TRY or a CATCH won&apos;t terminate the script. Code in the same block after the RETURN will not be executed, and the RETURN itself will be &quot;copied&quot; to the bottom of the FINALLY block to be executed.

a RETURN in the FINALLY block will override value(s) returned from the TRY or a CATCH block.

An EXIT or a DIE always terminate the script after themselves.

code 1



```
<?php
&#xA0; &#xA0; function foo(){
&#xA0; &#xA0; &#xA0; &#xA0; $bar = 1;
&#xA0; &#xA0; &#xA0; &#xA0; try{
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new Exception(&apos;I am Wu Xiancheng.&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; }catch(Exception $e){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $bar;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bar--; // this line will be ignored
&#xA0; &#xA0; &#xA0; &#xA0; }finally{
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bar++;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; echo foo(); // 2
?>
```


code 2



```
<?php
&#xA0; &#xA0; function foo(){
&#xA0; &#xA0; &#xA0; &#xA0; $bar = 1;
&#xA0; &#xA0; &#xA0; &#xA0; try{
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new Exception(&apos;I am Wu Xiancheng.&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; }catch(Exception $e){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $bar;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bar--; // this line will be ignored
&#xA0; &#xA0; &#xA0; &#xA0; }finally{
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bar++;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $bar;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; echo foo(); //2 
?>
```


code 3



```
<?php
&#xA0; &#xA0; function foo(){
&#xA0; &#xA0; &#xA0; &#xA0; $bar = 1;
&#xA0; &#xA0; &#xA0; &#xA0; try{
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new Exception(&apos;I am Wu Xiancheng.&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; }catch(Exception $e){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $bar;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bar--; // this line will be ignored
&#xA0; &#xA0; &#xA0; &#xA0; }finally{
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return 100;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; echo foo(); //100
?>
```



  

#



If you intend on creating a lot of custom exceptions, you may find this code useful.&#xA0; I&apos;ve created an interface and an abstract exception class that ensures that all parts of the built-in Exception class are preserved in child classes.&#xA0; It also properly pushes all information back to the parent constructor ensuring that nothing is lost.&#xA0; This allows you to quickly create new exceptions on the fly.&#xA0; It also overrides the default __toString method with a more thorough one.



```
<?php
interface IException
{
&#xA0; &#xA0; /* Protected methods inherited from Exception class */
&#xA0; &#xA0; public function getMessage();&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // Exception message 
&#xA0; &#xA0; public function getCode();&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // User-defined Exception code
&#xA0; &#xA0; public function getFile();&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Source filename
&#xA0; &#xA0; public function getLine();&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Source line
&#xA0; &#xA0; public function getTrace();&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // An array of the backtrace()
&#xA0; &#xA0; public function getTraceAsString();&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // Formated string of trace
&#xA0; &#xA0; 
&#xA0; &#xA0; /* Overrideable methods inherited from Exception class */
&#xA0; &#xA0; public function __toString();&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // formated string for display
&#xA0; &#xA0; public function __construct($message = null, $code = 0);
}

abstract class CustomException extends Exception implements IException
{
&#xA0; &#xA0; protected $message = &apos;Unknown exception&apos;;&#xA0; &#xA0;&#xA0; // Exception message
&#xA0; &#xA0; private&#xA0;&#xA0; $string;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Unknown
&#xA0; &#xA0; protected $code&#xA0; &#xA0; = 0;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // User-defined exception code
&#xA0; &#xA0; protected $file;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Source filename of exception
&#xA0; &#xA0; protected $line;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Source line of exception
&#xA0; &#xA0; private&#xA0;&#xA0; $trace;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // Unknown

&#xA0; &#xA0; public function __construct($message = null, $code = 0)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if (!$message) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new $this(&apos;Unknown &apos;. get_class($this));
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; parent::__construct($message, $code);
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; public function __toString()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; return get_class($this) . &quot; &apos;{$this-&gt;message}&apos; in {$this-&gt;file}({$this-&gt;line})\n&quot;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; . &quot;{$this-&gt;getTraceAsString()}&quot;;
&#xA0; &#xA0; }
}
?>
```


Now you can create new exceptions in one line:



```
<?php
class TestException extends CustomException {}
?>
```


Here&apos;s a test that shows that all information is properly preserved throughout the backtrace.



```
<?php
function exceptionTest()
{
&#xA0; &#xA0; try {
&#xA0; &#xA0; &#xA0; &#xA0; throw new TestException();
&#xA0; &#xA0; }
&#xA0; &#xA0; catch (TestException $e) {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;Caught TestException (&apos;{$e-&gt;getMessage()}&apos;)\n{$e}\n&quot;;
&#xA0; &#xA0; }
&#xA0; &#xA0; catch (Exception $e) {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;Caught Exception (&apos;{$e-&gt;getMessage()}&apos;)\n{$e}\n&quot;;
&#xA0; &#xA0; }
}

echo &apos;&lt;pre&gt;&apos; . exceptionTest() . &apos;&lt;/pre&gt;&apos;;
?>
```


Here&apos;s a sample output:

Caught TestException (&apos;Unknown TestException&apos;)
TestException &apos;Unknown TestException&apos; in C:\xampp\htdocs\CustomException\CustomException.php(31)
#0 C:\xampp\htdocs\CustomException\ExceptionTest.php(19): CustomException-&gt;__construct()
#1 C:\xampp\htdocs\CustomException\ExceptionTest.php(43): exceptionTest()
#2 {main}

  

#



catch doesn&apos;t check for the existence of the Exception class, so avoid typo.



```
<?php
&#xA0;&#xA0; class MyException extends Exception
&#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; ...
&#xA0;&#xA0; }

&#xA0;&#xA0; try
&#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; throw new MyException(...);
&#xA0;&#xA0; }
&#xA0;&#xA0; catch (MuException $e) // &lt;--- typo
&#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; ...
&#xA0;&#xA0; }
?>
```
 

You WON&apos;T get
&#xA0;&#xA0; Fatal error: Class MuException could not be loaded ...

You WILL get
&#xA0;&#xA0; Fatal error: Uncaught exception &apos;MyException&apos; ...

  

#



Custom error handling on entire pages can avoid half rendered pages for the users:



```
<?php
ob_start();
try {
&#xA0; &#xA0; /*contains all page logic 
&#xA0; &#xA0; and throws error if needed*/
&#xA0; &#xA0; ...
} catch (Exception $e) {
&#xA0; ob_end_clean();
&#xA0; displayErrorPage($e-&gt;getMessage());
}
?>
```



  

#



Using a return statement inside a finally block will override any other return statement or thrown exception from the try block and all defined catch blocks.&#xA0;&#xA0; Code execution in the parent stack will continue as if the exception was never thrown.&#xA0; 

Frankly this is a good design decision because it means I can optionally dismiss all thrown exceptions from 1 or more catch blocks in one place, without having to nest my whole try block inside an additional (and otherwise needless) try/catch block.

This is the same behavior as Java, whereas C# throws a compile time error when a return statement exists inside a finally block.&#xA0; So I figured it was worth pointing out to PHP devs who may not have any exposure to finally blocks or how other languages do it.



```
<?php

function asdf()
{
&#xA0; &#xA0; try {
&#xA0; &#xA0; &#xA0; &#xA0; throw new Exception(&apos;error&apos;);
&#xA0; &#xA0; }
&#xA0; &#xA0; catch(Exception $e) {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;An error occurred&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; throw $e;
&#xA0; &#xA0; }
&#xA0; &#xA0; finally {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //This overrides the exception as if it were never thrown
&#xA0; &#xA0; &#xA0; &#xA0; return &quot;\nException erased&quot;;
&#xA0; &#xA0; }
}

try {
&#xA0; &#xA0; echo asdf();
}
catch(Exception $e) {
&#xA0; &#xA0; echo &quot;\nResult: &quot; . $e-&gt;getMessage();
}
?>
```


The output from above will look like this:

&#xA0; &#xA0; An error occurred
&#xA0; &#xA0; Exception erased

Without the return statement in the finally block it would look like this:

&#xA0; &#xA0; An error occurred
&#xA0; &#xA0; Result: error

  

#



Type declarations will trigger Uncaught TypeError when a different type is passed. And it cannot be caught with the Exception class.


```
<?php
&#xA0; &#xA0; function xc(array $a){&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; }&#xA0; &#xA0; 
&#xA0; &#xA0; try{
&#xA0; &#xA0; &#xA0; &#xA0; xc(4);
&#xA0; &#xA0; }catch (Exception $e){ // failed to catch it
&#xA0; &#xA0; &#xA0; &#xA0; echo $e-&gt;getMessage();
&#xA0; &#xA0; }
?>
```


You should use TypeError instead for PHP 7+,



```
<?php
&#xA0; &#xA0; function xc(array $a){&#xA0; &#xA0; 
&#xA0; &#xA0; }&#xA0; &#xA0; 
&#xA0; &#xA0; try{
&#xA0; &#xA0; &#xA0; &#xA0; xc(4);
&#xA0; &#xA0; }catch (TypeError $e){
&#xA0; &#xA0; &#xA0; &#xA0; echo $e-&gt;getMessage();
&#xA0; &#xA0; }
?>
```


In php version prior to 7.0, you should translate Catchable fatal errors to an exception and then catch it.



```
<?php
&#xA0; &#xA0; function exceptionErrorHandler($errNumber, $errStr, $errFile, $errLine ) {
&#xA0; &#xA0; &#xA0; &#xA0; throw new ErrorException($errStr, 0, $errNumber, $errFile, $errLine);
&#xA0; &#xA0; }
&#xA0; &#xA0; set_error_handler(&apos;exceptionErrorHandler&apos;);
&#xA0; &#xA0; function s(array $a){&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; }
&#xA0; &#xA0; try{
&#xA0; &#xA0; &#xA0; &#xA0; s(4);
&#xA0; &#xA0; }catch (Exception $e){
&#xA0; &#xA0; &#xA0; &#xA0; echo $e-&gt;getMessage();
&#xA0; &#xA0; }
?>
```



  

#



Sometimes you want a single catch() to catch multiple types of Exception. In a language like Python, you can specify multiple types in a catch(), but in PHP you can only specify one. This can be annoying when you want handle many different Exceptions with the same catch() block.

However, you can replicate the functionality somewhat, because catch(&lt;classname&gt; $var) will match the given &lt;classname&gt; *or any of it&apos;s sub-classes*.

For example:



```
<?php
class DisplayException extends Exception {};
class FileException extends Exception {};
class AccessControl extends FileException {}; // Sub-class of FileException
class IOError extends FileException {}; // Sub-class of FileException

try {
&#xA0; if(!is_readable($somefile))
&#xA0; &#xA0;&#xA0; throw new IOError(&quot;File is not readable!&quot;);
&#xA0; if(!user_has_access_to_file($someuser, $somefile))
&#xA0; &#xA0;&#xA0; throw new AccessControl(&quot;Permission denied!&quot;);
&#xA0; if(!display_file($somefile))
&#xA0; &#xA0;&#xA0; throw new DisplayException(&quot;Couldn&apos;t display file!&quot;);

} catch (FileException $e) {
&#xA0; // This block will catch FileException, AccessControl or IOError exceptions, but not Exceptions or DisplayExceptions.
&#xA0; echo &quot;File error: &quot;.$e-&gt;getMessage();
&#xA0; exit(1);
}
?>
```


Corollary: If you want to catch *any* exception, no matter what the type, just use &quot;catch(Exception $var)&quot;, because all exceptions are sub-classes of the built-in Exception.

  

#



If you are using a namespace, you must indicate the global namespace when using Exceptions.


```
<?php
namespace alpha;
function foo(){
&#xA0; &#xA0; throw new \Exception(&quot;Something is wrong!&quot;);
&#xA0; &#xA0; // throw new Exception(&quot;&quot;); will fail
}

try {
&#xA0; &#xA0; foo();
} catch( \Exception $e ) {
&#xA0; &#xA0; // catch( Exception $e ) will give no warning, but will not catch Exception
&#xA0; &#xA0; echo &quot;ERROR: $e&quot;;
}

?>
```



  

#



&#x2018;Normal execution (when no exception is thrown within the try block, *or when a catch matching the thrown exception&#x2019;s class is not present*) will continue after that last catch block defined in sequence.&#x2019;

&#x2018;If an exception is not caught, a PHP Fatal Error will be issued with an &#x201C;Uncaught Exception &#x2026;&#x201D; message, unless a handler has been defined with set_exception_handler().&#x2019;

These two sentences seem a bit contradicting about what happens &#x2018;when a catch matching the thrown exception&#x2019;s class is not present&#x2019; (and the second sentence is actually correct).

  

#



Just an example why finally blocks are usefull (5.5)



```
<?php

//without catch
function example() {
&#xA0; try {
&#xA0; &#xA0; //do something that throws an exeption
&#xA0; }
&#xA0; finally {
&#xA0; &#xA0; //this code will be executed even when the exception is executed
&#xA0; }
}

function example2() {
&#xA0; try {
&#xA0; &#xA0;&#xA0; //open sql connection check user as example
&#xA0; &#xA0;&#xA0; if(condition) { 
&#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0;&#xA0; }
&#xA0; }
&#xA0; finally {
&#xA0; &#xA0; //close the sql connection, this will be executed even if the return is called.
&#xA0; }
}

?>
```



  

#



The &quot;finally&quot; block can change the exception that has been throw by the catch block.



```
<?php
try{
&#xA0; &#xA0; &#xA0; &#xA0; try {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new \Exception(&quot;Hello&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; } catch(\Exception $e) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo $e-&gt;getMessage().&quot; catch in\n&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw $e;
&#xA0; &#xA0; &#xA0; &#xA0; } finally {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo $e-&gt;getMessage().&quot; finally \n&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new \Exception(&quot;Bye&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; }
} catch (\Exception $e) {
&#xA0; &#xA0; &#xA0; &#xA0; echo $e-&gt;getMessage().&quot; catch out\n&quot;;
}
?>
```


The output is:

Hello catch in
Hello finally 
Bye catch out

  

#

[Official documentation page](https://www.php.net/manual/en/language.exceptions.php)

**[To root](/README.md)**