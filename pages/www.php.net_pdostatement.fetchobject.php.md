# PDOStatement::fetchObject



Be warned of the rather unorthodox behavior of PDOStatement::fetchObject() which injects property-values BEFORE invoking the constructor - in other words, if your class  initializes property-values to defaults in the constructor, you will be overwriting the values injected by fetchObject() !<br><br>A var_dump($this) in your __construct() method will reveal that property-values have been initialized prior to calling your constructor, so be careful.<br><br>For this reason, I strongly recommend hydrating your objects manually, after retrieving the data as an array, rather than trying to have PDO apply properties directly to your objects.<br><br>Clearly somebody thought they were being clever here - allowing you to access hydrated property-values from the constructor. Unfortunately, this is just not how OOP works - the constructor, by definition, is the first method called upon construction. <br><br>If you need to initialize your objects after they have been constructed and hydrated, I suggest your model types implement an interface with an init() method, and you data access layer invoke this method (if implemented) after hydrating.  

---

It should be mentioned that this method can set even non-public properties. It may sound strange but it can actually be very useful when creating an object based on mysql result.<br>Consider a User class:<br><br>

```
<?php
class User {
   // Private properties
   private $id, $name;

   private function __construct () {}

   public static function load_by_id ($id) {
      $stmt = $pdo->prepare('SELECT id, name FROM users WHERE id=?');
      $stmt->execute([$id]);
      return $stmt->fetchObject(__CLASS__);
   }
   /* same method can be written with the "name" column/property */
}

$user = User::load_by_id(1);
var_dump($user);
?>
```
<br><br>fetchObject() doesn&apos;t care about properties being public or not. It just passes the result to the object. Output is like:<br><br>object(User)#3 (2) {<br>  ["id":"User":private]=&gt;<br>  string(1) "1"<br>  ["name":"User":private]=&gt;<br>  string(10) "John Smith"<br>}  

---

[Official documentation page](https://www.php.net/manual/en/pdostatement.fetchobject.php)

**[To root](/README.md)**