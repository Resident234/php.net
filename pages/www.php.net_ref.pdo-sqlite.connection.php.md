# PDO_SQLITE DSN





In memory sqlite has some limitations. The memory space could be the request, the session, but no way seems documented to share a base in memory among users.



For a request, open your base with the code

$pdo = new PDO( &apos;sqlite::memory:&apos;);

and your base will disapear on next request.



For session persistency



```
<?php

$pdo = new PDO(

&#xA0; &#xA0; &apos;sqlite::memory:&apos;,

&#xA0; &#xA0; null,

&#xA0; &#xA0; null,

&#xA0; &#xA0; array(PDO::ATTR_PERSISTENT =&gt; true)

);

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/ref.pdo-sqlite.connection.php)

**[To root](/README.md)**