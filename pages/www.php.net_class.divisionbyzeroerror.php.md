# DivisionByZeroError



Note that on division by zero 1/0 and module by zero 1%0 an E_WARNING is triggered first (probably for backward compatibility with PHP5), then the DivisionByZeroError exception is thrown next.<br><br>The result is, for example, that if you set the maximum level of error detection with error_level(-1) and you also map errors to exception, say ErrorException, then on division by zero only this latter exception is thrown reporting "Division by zero". The result is that a code like this:<br><br>

```
<?php
// Set a safe environment:
error_reporting(-1);

// Maps errors to ErrorException.
function my_error_handler($errno, $message)
{ throw new ErrorException($message); }

try {
    echo 1/0;
}
catch(ErrorException $e){
    echo "got $e";
}
?>
```
<br><br>allows to detect such error in the same way under PHP5 and PHP7, although the DivisionByZeroError exception is masked off by ErrorException.  

---

[Official documentation page](https://www.php.net/manual/en/class.divisionbyzeroerror.php)

**[To root](/README.md)**