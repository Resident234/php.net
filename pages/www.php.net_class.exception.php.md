# Exception



Lists of Throwable and Exception tree as of 7.2.0<br><br>    Error<br>      ArithmeticError<br>        DivisionByZeroError<br>      AssertionError<br>      ParseError<br>      TypeError<br>        ArgumentCountError<br>    Exception<br>      ClosedGeneratorException<br>      DOMException<br>      ErrorException<br>      IntlException<br>      LogicException<br>        BadFunctionCallException<br>          BadMethodCallException<br>        DomainException<br>        InvalidArgumentException<br>        LengthException<br>        OutOfRangeException<br>      PharException<br>      ReflectionException<br>      RuntimeException<br>        OutOfBoundsException<br>        OverflowException<br>        PDOException<br>        RangeException<br>        UnderflowException<br>        UnexpectedValueException<br>      SodiumException <br><br>Find the script and output in the following links:<br>https://gist.github.com/mlocati/249f07b074a0de339d4d1ca980848e6a<br>https://3v4l.org/sDMsv<br><br>posted by someone here http://php.net/manual/en/class.throwable.php  

---

Note that an exception&apos;s properties are populated when the exception is *created*, not when it is thrown.  Throwing the exception does not seem to modify them.<br><br>Among other things, this means:<br><br>* The exception will blame the line that created it, not the line that threw it.<br><br>* Unlike in some other languages, rethrowing an exception doesn&apos;t muck up the trace.<br><br>* A thrown exception and an unthrown one look basically identical.  On my machine, the only visible difference is that a thrown exception has an `xdebug_message` property while an unthrown one doesn&apos;t.  Of course, if you don&apos;t have xdebug installed, you won&apos;t even get that.  

---

[Official documentation page](https://www.php.net/manual/en/class.exception.php)

**[To root](/README.md)**