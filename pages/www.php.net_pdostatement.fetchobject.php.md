# PDOStatement::fetchObject





Be warned of the rather unorthodox behavior of PDOStatement::fetchObject() which injects property-values BEFORE invoking the constructor - in other words, if your class&#xA0; initializes property-values to defaults in the constructor, you will be overwriting the values injected by fetchObject() !

A var_dump($this) in your __construct() method will reveal that property-values have been initialized prior to calling your constructor, so be careful.

For this reason, I strongly recommend hydrating your objects manually, after retrieving the data as an array, rather than trying to have PDO apply properties directly to your objects.

Clearly somebody thought they were being clever here - allowing you to access hydrated property-values from the constructor. Unfortunately, this is just not how OOP works - the constructor, by definition, is the first method called upon construction. 

If you need to initialize your objects after they have been constructed and hydrated, I suggest your model types implement an interface with an init() method, and you data access layer invoke this method (if implemented) after hydrating.

  

#



It should be mentioned that this method can set even non-public properties. It may sound strange but it can actually be very useful when creating an object based on mysql result.
Consider a User class:



```
<?php
class User {
&#xA0;&#xA0; // Private properties
&#xA0;&#xA0; private $id, $name;

&#xA0;&#xA0; private function __construct () {}

&#xA0;&#xA0; public static function load_by_id ($id) {
&#xA0; &#xA0; &#xA0; $stmt = $pdo-&gt;prepare(&apos;SELECT id, name FROM users WHERE id=?&apos;);
&#xA0; &#xA0; &#xA0; $stmt-&gt;execute([$id]);
&#xA0; &#xA0; &#xA0; return $stmt-&gt;fetchObject(__CLASS__);
&#xA0;&#xA0; }
&#xA0;&#xA0; /* same method can be written with the &quot;name&quot; column/property */
}

$user = User::load_by_id(1);
var_dump($user);
?>
```


fetchObject() doesn&apos;t care about properties being public or not. It just passes the result to the object. Output is like:

object(User)#3 (2) {
&#xA0; [&quot;id&quot;:&quot;User&quot;:private]=&gt;
&#xA0; string(1) &quot;1&quot;
&#xA0; [&quot;name&quot;:&quot;User&quot;:private]=&gt;
&#xA0; string(10) &quot;John Smith&quot;
}

  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.fetchobject.php)

**[To root](/README.md)**