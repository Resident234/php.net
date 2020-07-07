# Exception::getCode





The exception code can be used to categorize your errors. If you&apos;re wondering what the exception code can be used for, read on below. 

Let&apos;s say each time your application isn&apos;t able to connect to the database, you can save the error message under the error/exception code 214. At the end of the month, you can do a quick search on the error number &apos;214&apos; and find out how many times this error occurred. This makes life easier. Also, the error/exception message will give you details into what happened. 

The point is to use both the exception message and code. It&apos;s helpful in the long run.

Note: I added this note, because I was confused earlier as to the purpose of the exception code and it&apos;s use.

  

#



when raising an Exception with no error code explicitly defined, getCode() returns the integer 0 





```
<?php

try {

&#xA0; throw new Exception(&quot;no code!!&quot;);

} catch (Exception $e) {

&#xA0; print(&quot;Code=&apos;&quot; . $e-&gt;getCode() . &quot;&apos;&quot;);

}

?>
```




outputs 



Code=&apos;0&apos;

  

#

[Official documentation page](https://www.php.net/manual/en/exception.getcode.php)

**[To root](/README.md)**