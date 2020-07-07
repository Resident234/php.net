# Exception





Lists of Throwable and Exception tree as of 7.2.0

&#xA0; &#xA0; Error
&#xA0; &#xA0; &#xA0; ArithmeticError
&#xA0; &#xA0; &#xA0; &#xA0; DivisionByZeroError
&#xA0; &#xA0; &#xA0; AssertionError
&#xA0; &#xA0; &#xA0; ParseError
&#xA0; &#xA0; &#xA0; TypeError
&#xA0; &#xA0; &#xA0; &#xA0; ArgumentCountError
&#xA0; &#xA0; Exception
&#xA0; &#xA0; &#xA0; ClosedGeneratorException
&#xA0; &#xA0; &#xA0; DOMException
&#xA0; &#xA0; &#xA0; ErrorException
&#xA0; &#xA0; &#xA0; IntlException
&#xA0; &#xA0; &#xA0; LogicException
&#xA0; &#xA0; &#xA0; &#xA0; BadFunctionCallException
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; BadMethodCallException
&#xA0; &#xA0; &#xA0; &#xA0; DomainException
&#xA0; &#xA0; &#xA0; &#xA0; InvalidArgumentException
&#xA0; &#xA0; &#xA0; &#xA0; LengthException
&#xA0; &#xA0; &#xA0; &#xA0; OutOfRangeException
&#xA0; &#xA0; &#xA0; PharException
&#xA0; &#xA0; &#xA0; ReflectionException
&#xA0; &#xA0; &#xA0; RuntimeException
&#xA0; &#xA0; &#xA0; &#xA0; OutOfBoundsException
&#xA0; &#xA0; &#xA0; &#xA0; OverflowException
&#xA0; &#xA0; &#xA0; &#xA0; PDOException
&#xA0; &#xA0; &#xA0; &#xA0; RangeException
&#xA0; &#xA0; &#xA0; &#xA0; UnderflowException
&#xA0; &#xA0; &#xA0; &#xA0; UnexpectedValueException
&#xA0; &#xA0; &#xA0; SodiumException 

Find the script and output in the following links:
https://gist.github.com/mlocati/249f07b074a0de339d4d1ca980848e6a
https://3v4l.org/sDMsv

posted by someone here http://php.net/manual/en/class.throwable.php

  

#



Note that an exception&apos;s properties are populated when the exception is *created*, not when it is thrown.&#xA0; Throwing the exception does not seem to modify them.

Among other things, this means:

* The exception will blame the line that created it, not the line that threw it.

* Unlike in some other languages, rethrowing an exception doesn&apos;t muck up the trace.

* A thrown exception and an unthrown one look basically identical.&#xA0; On my machine, the only visible difference is that a thrown exception has an `xdebug_message` property while an unthrown one doesn&apos;t.&#xA0; Of course, if you don&apos;t have xdebug installed, you won&apos;t even get that.

  

#

[Official documentation page](https://www.php.net/manual/en/class.exception.php)

**[To root](/README.md)**