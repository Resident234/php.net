# set_exception_handler





Things you should be aware of:

An exception handler handles exceptions that were not caught before. It is the nature of an exception that it discontinues execution of your program - since it declares an exceptional situation in which the program cannot continue (except you catch it).

Since it has not been catched your code signals it is not being aware of the situation and cant go on.

This implies: returning to the script is simply impossible when the exception handler has already been called, since an uncaught exception is not a notice. use your own debug- or notice-log-system for things like that.

Furthermore: While is is still possible to call functions from your script, since the exception handler has already been called exceptions bubbling from that piece of code won&apos;t trigger the exception handler again. php will die without leaving any information apart form &quot;uncaught exception with unknown stack frame&quot;. So if you call functions from your script, make sure that you catch any exceptions that possibly occur via try..catch inside the exception handler.

For those of you who misinterpreted the essential meaning of the exception handler: it&apos;s only use is to handle the abortion of your script gracefully, e.g. in a project like facebook or wikipedia: render a nice error page, eventually hiding information which shall not leak into the public (instead you may want to write to your log or mail the sys-admin or stuff like that).

In other words: Redirecting all php-errors form an error-handler using exceptions - including notices - is a very dumb idea, if you do not intend having your script aborted everytime you didn&apos;t set a variable (for example).

my 2 cents.

  

#



If you want a class instance to handle the exception, this is how you do it :



```
<?php
class example {
&#xA0;&#xA0; public function __construct() {
&#xA0; &#xA0; &#xA0;&#xA0; @set_exception_handler(array($this, &apos;exception_handler&apos;));
&#xA0; &#xA0; &#xA0;&#xA0; throw new Exception(&apos;DOH!!&apos;);
&#xA0;&#xA0; }

&#xA0;&#xA0; public function exception_handler($exception) {
&#xA0; &#xA0; &#xA0;&#xA0; print &quot;Exception Caught: &quot;. $exception-&gt;getMessage() .&quot;\n&quot;;
&#xA0;&#xA0; }
}

$example = new example;

php?>
```


See the first post (Sean&apos;s) for a static example.&#xA0; As Sean points out, the exception_handler function must be declared public.

  

#



A behaviour not documented or discussed enough, yet pretty common is that is that if an exception is thrown from the global exception handler then a fatal error occurs (Exception thrown without a stack frame). That is, if you define your own global exception handler by calling set_exception_handler() and you throw an exception from inside it then this fatal error occurs. It is only natural though, as the callback defined by set_exception_handler() is only called on uncaught (unhandled) exceptions so if you throw one from there then you get this fatal error as there is no exception handler left (you override the php internal one by calling set_exception_handler()), hence no stack frame for it.

Example:



```
<?php

function myExceptionHandler (Exception $ex)
{
&#xA0; &#xA0; throw $ex;
}

set_exception_handler(&quot;myExceptionHandler&quot;);

throw new Exception(&quot;This should cause a fatal error and this message will be lost&quot;);

php?>
```


Will cause a Fatal error: Exception thrown without a stack frame

If you skip/comment the set_exception_handler(&quot;...&quot;) line then the internal PHP global handler will catch the exception and output the exception message and trace (as string) to the browser, allowing you to at least see the exception message.

While it is a very good idea to always define your own global exception handler by using the set_exception_handler() function, you should pay attention and never throw an exception from it (or if you do then catch it).

Finally, every serious coder should use an IDE with debugging capabilities. Tracking down an error like this becomes a trivial matter by using simple debugging &quot;Step into&quot; commands (I for one recommend Zend IDE v5.2 at the moment of this writing). I have seen numerous messages on the internet with people wondering why this message pops up.

Cheers

p.s. Other causes for this error which are somehow unrelated to this is when you throw an exception from a destructor (the reasons behind that are similar though, the global handler might no longer exist due to the php engine shutting the page down).

  

#

[Official documentation page](https://www.php.net/manual/en/function.set-exception-handler.php)

**[To root](/README.md)**