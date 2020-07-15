# restore_error_handler



Isolde is kind of wrong. The error handlers are stacked with set_error_handler(), and popped with restore_error_handler(). Here i put an example:<br><br>

```
<?php
    mysql_connect("inexistent"); //Generate an error. The actual error handler is set by default

    function foo1() {echo "<br>Error foo1<br>";}
    function foo2() {echo "<br>Error foo2<br>";}
    function foo3() {echo "<br>Error foo3<br>";}
    
    set_error_handler("foo1");    //current error handler: foo1
    set_error_handler("foo2");    //current error handler: foo2
    set_error_handler("foo3");    //current error handler: foo3
    
    mysql_connect("inexistent");    
    restore_error_handler();        //now, current error handler: foo2
    mysql_connect("inexistent");     
    restore_error_handler();        //now, current error handler: foo1
    mysql_connect("inexistent"); 
    restore_error_handler();        //now current error handler: default handler
    mysql_connect("inexistent");
    restore_error_handler();        //now current error handler: default handler (The stack can't pop more)
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.restore-error-handler.php)

**[To root](/README.md)**