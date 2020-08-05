# session_reset



First of all you should execute this code :<br>

```
<?php
    session_start();
    $_SESSION["A"] = "Some Value";
?>
```


then you should execute this one : 



```
<?php
    start_session();
    $_SESSION["A"] = "Some New Value";  // set new value

    session_reset();  // old session value restored
    echo $_SESSION["A"];

    //Output: Some Value
?>
```
<br><br>That is because session_reset() is rolling back changes to the last saved session data, which is their values right after the session_start().  

---

[Official documentation page](https://www.php.net/manual/en/function.session-reset.php)

**[To root](/README.md)**