# The PDOException class





Here is something interesting regarding a PDOException and it involves some of the annoyances that can be associated with PHP&apos;s dynamic nature.

PDOException extends from RuntimeException, which in return extends from Exception. As such, it has access to the $code Protected Class Variable, which represents the Exception&apos;s code as an Integer (duh!) and can be accessed externally using the Exception::getCode Method.

Here is the interesting part. PDOException actually redefines $code as a String and not an Integer because for its case, $code actually contains the Exception&apos;s SQL State, which is composed of characters and numbers.

It is actually documented in the manual that $code is a String and not an Integer but it might not be immedietley clear because it is hidden by the fact that PDOException::getCode is documented to return an Integer and not a String!

Some developers like to catch a PDOException and rethrow it as a different Exception if they wrap their database calls in an external library. For example, consider the following code:



```
<?php

try {
&#xA0; &#xA0; $PDO = new PDO( &apos;...&apos; ); // PDO Driver DSN. Throws A PDOException.
}
catch( PDOException $Exception ) {
&#xA0; &#xA0; // PHP Fatal Error. Second Argument Has To Be An Integer, But PDOException::getCode Returns A
&#xA0; &#xA0; // String.
&#xA0; &#xA0; throw new MyDatabaseException( $Exception-&gt;getMessage( ) , $Exception-&gt;getCode( ) );
}

?>
```


Be careful in that you have to typecast the value returned by PDOException::getCode to an Integer BEFORE you pass it as an Argument to your Exception&apos;s Constructor. The following will work:



```
<?php

try {
&#xA0; &#xA0; $PDO = new PDO( &apos;...&apos; ); // PDO Driver DSN. Throws A PDOException.
}
catch( PDOException $Exception ) {
&#xA0; &#xA0; // Note The Typecast To An Integer!
&#xA0; &#xA0; throw new MyDatabaseException( $Exception-&gt;getMessage( ) , (int)$Exception-&gt;getCode( ) );
}

?>
```


Hope this will save some developers some frustrating hours from an otherwise enjoyable job :)

Good Luck,

  

#

[Official documentation page](https://www.php.net/manual/en/class.pdoexception.php)

**[To root](/README.md)**