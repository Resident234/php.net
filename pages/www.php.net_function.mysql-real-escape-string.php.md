# mysql_real_escape_string





Just a little function which mimics the original mysql_real_escape_string but which doesn&apos;t need an active mysql connection. Could be implemented as a static function in a database class. Hope it helps someone.





```
<?php

function mysql_escape_mimic($inp) {

&#xA0; &#xA0; if(is_array($inp))

&#xA0; &#xA0; &#xA0; &#xA0; return array_map(__METHOD__, $inp);



&#xA0; &#xA0; if(!empty($inp) &amp;&amp; is_string($inp)) {

&#xA0; &#xA0; &#xA0; &#xA0; return str_replace(array(&apos;\\&apos;, &quot;\0&quot;, &quot;\n&quot;, &quot;\r&quot;, &quot;&apos;&quot;, &apos;&quot;&apos;, &quot;\x1a&quot;), array(&apos;\\\\&apos;, &apos;\\0&apos;, &apos;\\n&apos;, &apos;\\r&apos;, &quot;\\&apos;&quot;, &apos;\\&quot;&apos;, &apos;\\Z&apos;), $inp);

&#xA0; &#xA0; }



&#xA0; &#xA0; return $inp;

}

?>
```



  

#



Note that mysql_real_escape_string doesn&apos;t prepend backslashes to \x00, \n, \r, and and \x1a as mentionned in the documentation, but actually replaces the character with a MySQL acceptable representation for queries (e.g. \n is replaced with the &apos;\n&apos; litteral). (\, &apos;, and &quot; are escaped as documented) This doesn&apos;t change how you should use this function, but I think it&apos;s good to know.

  

#



For further information:
http://dev.mysql.com/doc/refman/5.5/en/mysql-real-escape-string.html
(replace your MySQL version in the URL)

  

#

[Official documentation page](https://www.php.net/manual/en/function.mysql-real-escape-string.php)

**[To root](/README.md)**