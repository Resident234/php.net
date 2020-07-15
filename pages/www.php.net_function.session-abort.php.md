# session_abort



To better understand this function you should execute this code first :<br><br>

```
<?php
    // First of all choose your path , For e.g. C:/session
    session_save_path('Your Path here !');
    
    session_start();
    
    // Define a Session Variable
    $_SESSION['Key'] = 'value' ;
    
    Var_dump(session_status() == PHP_SESSION_ACTIVE);
    
    // Output : bool(True) , it means you have an open session !
?>
```


Then you should execute this code :



```
<?php
    // Choose the path that you used it in first part  
    session_save_path('Your path here');
    
    session_start();
    
    // If you want to close session and keep your original data in your path , you should use session_abort()
    session_abort();
    
    var_dump(session_status()== PHP_SESSION_ACTIVE);
    
    // Output : bool(False) , it means your session closed .
?>
```
<br><br>So if you have an open session , session_abort() will simply close it without effecting the external session data , so you can reload your data again from your path that you chose .  

#

[Official documentation page](https://www.php.net/manual/en/function.session-abort.php)

**[To root](/README.md)**