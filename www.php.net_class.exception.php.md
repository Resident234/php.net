# Exception




<div class="phpcode"><span class="html">
Lists of Throwable and Exception tree as of 7.2.0<br><br>&#xA0; &#xA0; Error<br>&#xA0; &#xA0; &#xA0; ArithmeticError<br>&#xA0; &#xA0; &#xA0; &#xA0; DivisionByZeroError<br>&#xA0; &#xA0; &#xA0; AssertionError<br>&#xA0; &#xA0; &#xA0; ParseError<br>&#xA0; &#xA0; &#xA0; TypeError<br>&#xA0; &#xA0; &#xA0; &#xA0; ArgumentCountError<br>&#xA0; &#xA0; Exception<br>&#xA0; &#xA0; &#xA0; ClosedGeneratorException<br>&#xA0; &#xA0; &#xA0; DOMException<br>&#xA0; &#xA0; &#xA0; ErrorException<br>&#xA0; &#xA0; &#xA0; IntlException<br>&#xA0; &#xA0; &#xA0; LogicException<br>&#xA0; &#xA0; &#xA0; &#xA0; BadFunctionCallException<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; BadMethodCallException<br>&#xA0; &#xA0; &#xA0; &#xA0; DomainException<br>&#xA0; &#xA0; &#xA0; &#xA0; InvalidArgumentException<br>&#xA0; &#xA0; &#xA0; &#xA0; LengthException<br>&#xA0; &#xA0; &#xA0; &#xA0; OutOfRangeException<br>&#xA0; &#xA0; &#xA0; PharException<br>&#xA0; &#xA0; &#xA0; ReflectionException<br>&#xA0; &#xA0; &#xA0; RuntimeException<br>&#xA0; &#xA0; &#xA0; &#xA0; OutOfBoundsException<br>&#xA0; &#xA0; &#xA0; &#xA0; OverflowException<br>&#xA0; &#xA0; &#xA0; &#xA0; PDOException<br>&#xA0; &#xA0; &#xA0; &#xA0; RangeException<br>&#xA0; &#xA0; &#xA0; &#xA0; UnderflowException<br>&#xA0; &#xA0; &#xA0; &#xA0; UnexpectedValueException<br>&#xA0; &#xA0; &#xA0; SodiumException <br><br>Find the script and output in the following links:<br><a href="https://gist.github.com/mlocati/249f07b074a0de339d4d1ca980848e6a" rel="nofollow" target="_blank">https://gist.github.com/mlocati/249f07b074a0de339d4d1ca980848e6a</a><br><a href="https://3v4l.org/sDMsv" rel="nofollow" target="_blank">https://3v4l.org/sDMsv</a><br><br>posted by someone here <a href="http://php.net/manual/en/class.throwable.php" rel="nofollow" target="_blank">http://php.net/manual/en/class.throwable.php</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that an exception&apos;s properties are populated when the exception is *created*, not when it is thrown.&#xA0; Throwing the exception does not seem to modify them.<br><br>Among other things, this means:<br><br>* The exception will blame the line that created it, not the line that threw it.<br><br>* Unlike in some other languages, rethrowing an exception doesn&apos;t muck up the trace.<br><br>* A thrown exception and an unthrown one look basically identical.&#xA0; On my machine, the only visible difference is that a thrown exception has an `xdebug_message` property while an unthrown one doesn&apos;t.&#xA0; Of course, if you don&apos;t have xdebug installed, you won&apos;t even get that.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.exception.php)

**[⬆ to root](/)**