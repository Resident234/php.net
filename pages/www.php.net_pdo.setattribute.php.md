# PDO::setAttribute





Because no examples are provided, and to alleviate any confusion as a result, the setAttribute() method is invoked like so:

setAttribute(ATTRIBUTE, OPTION);

So, if I wanted to ensure that the column names returned from a query were returned in the case the database driver returned them (rather than having them returned in all upper case [as is the default on some of the PDO extensions]), I would do the following:



```
<?php
// Create a new database connection.
$dbConnection = new PDO($dsn, $user, $pass);

// Set the case in which to return column_names.
$dbConnection-&gt;setAttribute(PDO::ATTR_CASE, PDO::CASE_NATURAL);
?>
```


Hope this helps some of you who learn by example (as is the case with me).

.Colin

  

#



This is an update to a note I wrote earlier concerning how to set multiple attributes when you create you PDO connection string.

You can put all the attributes you want in an associative array and pass that array as the fourth parameter in your connection string. So it goes like this: 


```
<?php
$options = [
&#xA0; &#xA0; PDO::ATTR_ERRMODE =&gt; PDO::ERRMODE_EXCEPTION,
&#xA0; &#xA0; PDO::ATTR_CASE =&gt; PDO::CASE_NATURAL,
&#xA0; &#xA0; PDO::ATTR_ORACLE_NULLS =&gt; PDO::NULL_EMPTY_STRING
];

// Now you create your connection string
try {
&#xA0; &#xA0; // Then pass the options as the last parameter in the connection string
&#xA0; &#xA0; $connection = new PDO(&quot;mysql:host=$host; dbname=$dbname&quot;, $user, $password, $options);

&#xA0; &#xA0; // That&apos;s how you can set multiple attributes
} catch(PDOException $e) {
&#xA0; &#xA0; die(&quot;Database connection failed: &quot; . $e-&gt;getMessage());
}
?>
```



  

#



Well, I have not seen it mentioned anywhere and thought its worth mentioning. It might help someone. If you are wondering whether you can set multiple attributes then the answer is yes.

You can do it like this:
try {
&#xA0; &#xA0; $connection = new PDO(&quot;mysql:host=$host; dbname=$dbname&quot;, $user, $password);
&#xA0; &#xA0; // You can begin setting all the attributes you want.
&#xA0; &#xA0; $connection-&gt;setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
&#xA0; &#xA0; $connection-&gt;setAttribute(PDO::ATTR_CASE, PDO::CASE_NATURAL);
&#xA0; &#xA0; $connection-&gt;setAttribute(PDO::ATTR_ORACLE_NULLS, PDO::NULL_EMPTY_STRING);

&#xA0; &#xA0; // That&apos;s how you can set multiple attributes
}
catch(PDOException $e)
{
&#xA0; &#xA0; die(&quot;Database connection failed: &quot; . $e-&gt;getMessage());
}

I hope this helps somebody. :)

  

#



It is worth noting that not all attributes may be settable via setAttribute().&#xA0; For example, PDO::MYSQL_ATTR_MAX_BUFFER_SIZE is only settable in PDO::__construct().&#xA0; You must pass PDO::MYSQL_ATTR_MAX_BUFFER_SIZE as part of the optional 4th parameter to the constructor.&#xA0; This is detailed in http://bugs.php.net/bug.php?id=38015

  

#



There is also a way to specifie the default fetch mode :



```
<?php

$connection = new PDO($connection_string);

$connection-&gt;setAttribute(PDO::ATTR_DEFAULT_FETCH_MODE, PDO::FETCH_OBJ);

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/pdo.setattribute.php)

**[To root](/README.md)**