# The PDOException class



Here is something interesting regarding a PDOException and it involves some of the annoyances that can be associated with PHP&apos;s dynamic nature.<br><br>PDOException extends from RuntimeException, which in return extends from Exception. As such, it has access to the $code Protected Class Variable, which represents the Exception&apos;s code as an Integer (duh!) and can be accessed externally using the Exception::getCode Method.<br><br>Here is the interesting part. PDOException actually redefines $code as a String and not an Integer because for its case, $code actually contains the Exception&apos;s SQL State, which is composed of characters and numbers.<br><br>It is actually documented in the manual that $code is a String and not an Integer but it might not be immedietley clear because it is hidden by the fact that PDOException::getCode is documented to return an Integer and not a String!<br><br>Some developers like to catch a PDOException and rethrow it as a different Exception if they wrap their database calls in an external library. For example, consider the following code:<br><br>

```
<?php

try {
    $PDO = new PDO( '...' ); // PDO Driver DSN. Throws A PDOException.
}
catch( PDOException $Exception ) {
    // PHP Fatal Error. Second Argument Has To Be An Integer, But PDOException::getCode Returns A
    // String.
    throw new MyDatabaseException( $Exception->getMessage( ) , $Exception->getCode( ) );
}

?>
```


Be careful in that you have to typecast the value returned by PDOException::getCode to an Integer BEFORE you pass it as an Argument to your Exception's Constructor. The following will work:



```
<?php

try {
    $PDO = new PDO( '...' ); // PDO Driver DSN. Throws A PDOException.
}
catch( PDOException $Exception ) {
    // Note The Typecast To An Integer!
    throw new MyDatabaseException( $Exception->getMessage( ) , (int)$Exception->getCode( ) );
}

?>
```
<br><br>Hope this will save some developers some frustrating hours from an otherwise enjoyable job :)<br><br>Good Luck,  

---

[Official documentation page](https://www.php.net/manual/en/class.pdoexception.php)

**[To root](/README.md)**