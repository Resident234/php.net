# PDOStatement::bindValue



What the bindValue() docs fail to explain without reading them _very_ carefully is that bindParam() is passed to PDO byref - whereas bindValue() isn&apos;t.<br><br>Thus with bindValue() you can do something like $stmt-&gt;bindValue(":something", "bind this"); whereas with bindParam() it will fail because you can&apos;t pass a string by reference, for example.  

#

When binding parameters, apparently you can&apos;t use a placeholder twice (e.g. "select * from mails where sender=:me or recipient=:me"), you&apos;ll have to give them different names otherwise your query will return empty handed (but not fail, unfortunately).  Just in case you&apos;re struggling with something like this.  

#

Be careful when trying to validate using PDO::PARAM_INT. <br><br>Take this sample into account:<br><br>

```
<?php
/* php --version
 * PHP 5.6.25 (cli) (built: Aug 24 2016 09:50:46)
 * Copyright (c) 1997-2016 The PHP Group
 * Zend Engine v2.6.0, Copyright (c) 1998-2016 Zend Technologies
 */

$id = '1a'; 
$stm = $pdo->prepare('select * from author where id = :id');
$bind = $stm->bindValue(':id', $id, PDO::PARAM_INT);

$stm->execute();
$authors = $stm->fetchAll();

var_dump($id);         // string(2)
var_dump($bind);       // true
var_dump((int)$id);    // int(1)
var_dump(is_int($id)); // false
var_dump($authors);    // the author id=1  =(

// remember
var_dump(1 == '1');    // true
var_dump(1 === '1');   // false
var_dump(1 === '1a');  // false
var_dump(1 == '1a');   // true
?>
```
<br><br>My opinion: bindValue() should test is_int() internaly first of anything, <br>It is a bug? I&apos;m not sure.  

#

Although bindValue() escapes quotes it does not escape "%" and "_", so be careful when using LIKE. A malicious parameter full of %%% can dump your entire database if you don&apos;t escape the parameter yourself. PDO does not provide any other escape method to handle it.  

#

Note that the third parameter ($data_type) in the majority of cases will not type cast the value into anything else to be used in the query, nor will it throw any sort of error if the type does not match up with the value provided. This parameter essentially has no effect whatsoever except throwing an error if it is set and is not a float, so do not think that it is adding any extra level of security to the queries.<br><br>The two exceptions where type casting is performed:<br><br>- if you use PDO::PDO_PARAM_INT and provide a boolean, it will be converted to a long<br>- if you use PDO::PDO_PARAM_BOOL and provide a long, it will be converted to a boolean<br><br>

```
<?php

$query = 'SELECT * FROM `users` WHERE username = :username AND `password` = ENCRYPT( :password, `crypt_password`)';

$sth= $dbh->prepare($query);

// First try passing a random numerical value as the third parameter
var_dump($sth->bindValue(':username','bob', 12345.67)); // bool(true)

// Next try passing a string using the boolean type
var_dump($sth->bindValue(':password','topsecret_pw', PDO::PARAM_BOOL)); // bool(true)

$sth->execute(); // Query is executed successfully
$result = $sth->fetchAll(); // Returns the result of the query

?>
```
  

#

This function is useful for bind value on an array. You can specify the type of the value in advance with $typeArray.<br><br>

```
<?php
/**
 * @param string $req : the query on which link the values
 * @param array $array : associative array containing the values &#x200B;&#x200B;to bind
 * @param array $typeArray : associative array with the desired value for its corresponding key in $array
 * */
function bindArrayValue($req, $array, $typeArray = false)
{
    if(is_object($req) &amp;&amp; ($req instanceof PDOStatement))
    {
        foreach($array as $key => $value)
        {
            if($typeArray)
                $req->bindValue(":$key",$value,$typeArray[$key]);
            else
            {
                if(is_int($value))
                    $param = PDO::PARAM_INT;
                elseif(is_bool($value))
                    $param = PDO::PARAM_BOOL;
                elseif(is_null($value))
                    $param = PDO::PARAM_NULL;
                elseif(is_string($value))
                    $param = PDO::PARAM_STR;
                else
                    $param = FALSE;
                    
                if($param)
                    $req->bindValue(":$key",$value,$param);
            }
        }
    }
}

/**
 * ## EXEMPLE ##
 * $array = array('language' => 'php','lines' => 254, 'publish' => true);
 * $typeArray = array('language' => PDO::PARAM_STR,'lines' => PDO::PARAM_INT,'publish' => PDO::PARAM_BOOL);
 * $req = 'SELECT * FROM code WHERE language = :language AND lines = :lines AND publish = :publish';
 * You can bind $array like that :
 * bindArrayValue($array,$req,$typeArray);
 * The function is more useful when you use limit clause because they need an integer.
 * */
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.bindvalue.php)

**[To root](/README.md)**