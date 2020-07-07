# PDOStatement::bindValue





What the bindValue() docs fail to explain without reading them _very_ carefully is that bindParam() is passed to PDO byref - whereas bindValue() isn&apos;t.

Thus with bindValue() you can do something like $stmt-&gt;bindValue(&quot;:something&quot;, &quot;bind this&quot;); whereas with bindParam() it will fail because you can&apos;t pass a string by reference, for example.

  

#



When binding parameters, apparently you can&apos;t use a placeholder twice (e.g. &quot;select * from mails where sender=:me or recipient=:me&quot;), you&apos;ll have to give them different names otherwise your query will return empty handed (but not fail, unfortunately).&#xA0; Just in case you&apos;re struggling with something like this.

  

#



Be careful when trying to validate using PDO::PARAM_INT. 

Take this sample into account:



```
<?php
/* php --version
 * PHP 5.6.25 (cli) (built: Aug 24 2016 09:50:46)
 * Copyright (c) 1997-2016 The PHP Group
 * Zend Engine v2.6.0, Copyright (c) 1998-2016 Zend Technologies
 */

$id = &apos;1a&apos;; 
$stm = $pdo-&gt;prepare(&apos;select * from author where id = :id&apos;);
$bind = $stm-&gt;bindValue(&apos;:id&apos;, $id, PDO::PARAM_INT);

$stm-&gt;execute();
$authors = $stm-&gt;fetchAll();

var_dump($id);&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // string(2)
var_dump($bind);&#xA0; &#xA0; &#xA0;&#xA0; // true
var_dump((int)$id);&#xA0; &#xA0; // int(1)
var_dump(is_int($id)); // false
var_dump($authors);&#xA0; &#xA0; // the author id=1&#xA0; =(

// remember
var_dump(1 == &apos;1&apos;);&#xA0; &#xA0; // true
var_dump(1 === &apos;1&apos;);&#xA0;&#xA0; // false
var_dump(1 === &apos;1a&apos;);&#xA0; // false
var_dump(1 == &apos;1a&apos;);&#xA0;&#xA0; // true
?>
```


My opinion: bindValue() should test is_int() internaly first of anything, 
It is a bug? I&apos;m not sure.

  

#



Although bindValue() escapes quotes it does not escape &quot;%&quot; and &quot;_&quot;, so be careful when using LIKE. A malicious parameter full of %%% can dump your entire database if you don&apos;t escape the parameter yourself. PDO does not provide any other escape method to handle it.

  

#



Note that the third parameter ($data_type) in the majority of cases will not type cast the value into anything else to be used in the query, nor will it throw any sort of error if the type does not match up with the value provided. This parameter essentially has no effect whatsoever except throwing an error if it is set and is not a float, so do not think that it is adding any extra level of security to the queries.

The two exceptions where type casting is performed:

- if you use PDO::PDO_PARAM_INT and provide a boolean, it will be converted to a long
- if you use PDO::PDO_PARAM_BOOL and provide a long, it will be converted to a boolean



```
<?php

$query = &apos;SELECT * FROM `users` WHERE username = :username AND `password` = ENCRYPT( :password, `crypt_password`)&apos;;

$sth= $dbh-&gt;prepare($query);

// First try passing a random numerical value as the third parameter
var_dump($sth-&gt;bindValue(&apos;:username&apos;,&apos;bob&apos;, 12345.67)); // bool(true)

// Next try passing a string using the boolean type
var_dump($sth-&gt;bindValue(&apos;:password&apos;,&apos;topsecret_pw&apos;, PDO::PARAM_BOOL)); // bool(true)

$sth-&gt;execute(); // Query is executed successfully
$result = $sth-&gt;fetchAll(); // Returns the result of the query

?>
```



  

#



This function is useful for bind value on an array. You can specify the type of the value in advance with $typeArray.



```
<?php
/**
 * @param string $req : the query on which link the values
 * @param array $array : associative array containing the values &#x200B;&#x200B;to bind
 * @param array $typeArray : associative array with the desired value for its corresponding key in $array
 * */
function bindArrayValue($req, $array, $typeArray = false)
{
&#xA0; &#xA0; if(is_object($req) &amp;&amp; ($req instanceof PDOStatement))
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; foreach($array as $key =&gt; $value)
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if($typeArray)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $req-&gt;bindValue(&quot;:$key&quot;,$value,$typeArray[$key]);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(is_int($value))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $param = PDO::PARAM_INT;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; elseif(is_bool($value))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $param = PDO::PARAM_BOOL;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; elseif(is_null($value))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $param = PDO::PARAM_NULL;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; elseif(is_string($value))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $param = PDO::PARAM_STR;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $param = FALSE;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if($param)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $req-&gt;bindValue(&quot;:$key&quot;,$value,$param);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
}

/**
 * ## EXEMPLE ##
 * $array = array(&apos;language&apos; =&gt; &apos;php&apos;,&apos;lines&apos; =&gt; 254, &apos;publish&apos; =&gt; true);
 * $typeArray = array(&apos;language&apos; =&gt; PDO::PARAM_STR,&apos;lines&apos; =&gt; PDO::PARAM_INT,&apos;publish&apos; =&gt; PDO::PARAM_BOOL);
 * $req = &apos;SELECT * FROM code WHERE language = :language AND lines = :lines AND publish = :publish&apos;;
 * You can bind $array like that :
 * bindArrayValue($array,$req,$typeArray);
 * The function is more useful when you use limit clause because they need an integer.
 * */
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.bindvalue.php)

**[To root](/README.md)**