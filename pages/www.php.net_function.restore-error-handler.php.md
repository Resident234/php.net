# restore_error_handler





Isolde is kind of wrong. The error handlers are stacked with set_error_handler(), and popped with restore_error_handler(). Here i put an example:



```
<?php
&#xA0; &#xA0; mysql_connect(&quot;inexistent&quot;); //Generate an error. The actual error handler is set by default

&#xA0; &#xA0; function foo1() {echo &quot;&lt;br&gt;Error foo1&lt;br&gt;&quot;;}
&#xA0; &#xA0; function foo2() {echo &quot;&lt;br&gt;Error foo2&lt;br&gt;&quot;;}
&#xA0; &#xA0; function foo3() {echo &quot;&lt;br&gt;Error foo3&lt;br&gt;&quot;;}
&#xA0; &#xA0; 
&#xA0; &#xA0; set_error_handler(&quot;foo1&quot;);&#xA0; &#xA0; //current error handler: foo1
&#xA0; &#xA0; set_error_handler(&quot;foo2&quot;);&#xA0; &#xA0; //current error handler: foo2
&#xA0; &#xA0; set_error_handler(&quot;foo3&quot;);&#xA0; &#xA0; //current error handler: foo3
&#xA0; &#xA0; 
&#xA0; &#xA0; mysql_connect(&quot;inexistent&quot;);&#xA0; &#xA0; 
&#xA0; &#xA0; restore_error_handler();&#xA0; &#xA0; &#xA0; &#xA0; //now, current error handler: foo2
&#xA0; &#xA0; mysql_connect(&quot;inexistent&quot;);&#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; restore_error_handler();&#xA0; &#xA0; &#xA0; &#xA0; //now, current error handler: foo1
&#xA0; &#xA0; mysql_connect(&quot;inexistent&quot;); 
&#xA0; &#xA0; restore_error_handler();&#xA0; &#xA0; &#xA0; &#xA0; //now current error handler: default handler
&#xA0; &#xA0; mysql_connect(&quot;inexistent&quot;);
&#xA0; &#xA0; restore_error_handler();&#xA0; &#xA0; &#xA0; &#xA0; //now current error handler: default handler (The stack can&apos;t pop more)
php?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.restore-error-handler.php)

**[To root](/README.md)**