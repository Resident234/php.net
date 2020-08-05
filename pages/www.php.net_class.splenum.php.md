# The SplEnum class



Source: https://stackoverflow.com/a/254543/508666<br><br>if class SplEnum is not present, I would normally use something simple like the following:<br><br>abstract class DaysOfWeek<br>{<br>    const Sunday = 0;<br>    const Monday = 1;<br>    // etc.<br>}<br><br>$today = DaysOfWeek::Sunday;<br>However, other use cases may require more validation of constants and values. <br><br>abstract class BasicEnum {<br>    private static $constCacheArray = NULL;<br><br>private function __construct(){<br>      /*<br>        Preventing instance :)<br>      */<br>     }<br><br>    private static function getConstants() {<br>        if (self::$constCacheArray == NULL) {<br>            self::$constCacheArray = [];<br>        }<br>        $calledClass = get_called_class();<br>        if (!array_key_exists($calledClass, self::$constCacheArray)) {<br>            $reflect = new ReflectionClass($calledClass);<br>            self::$constCacheArray[$calledClass] = $reflect-&gt;getConstants();<br>        }<br>        return self::$constCacheArray[$calledClass];<br>    }<br><br>    public static function isValidName($name, $strict = false) {<br>        $constants = self::getConstants();<br><br>        if ($strict) {<br>            return array_key_exists($name, $constants);<br>        }<br><br>        $keys = array_map(&apos;strtolower&apos;, array_keys($constants));<br>        return in_array(strtolower($name), $keys);<br>    }<br><br>    public static function isValidValue($value) {<br>        $values = array_values(self::getConstants());<br>        return in_array($value, $values, $strict = true);<br>    }<br>}<br>By creating a simple enum class that extends BasicEnum, you now have the ability to use methods thusly for simple input validation:<br><br>abstract class DaysOfWeek extends BasicEnum {<br>    const Sunday = 0;<br>    const Monday = 1;<br>    const Tuesday = 2;<br>    const Wednesday = 3;<br>    const Thursday = 4;<br>    const Friday = 5;<br>    const Saturday = 6;<br>}<br><br>DaysOfWeek::isValidName(&apos;Humpday&apos;);                  // false<br>DaysOfWeek::isValidName(&apos;Monday&apos;);                   // true<br>DaysOfWeek::isValidName(&apos;monday&apos;);                   // true<br>DaysOfWeek::isValidName(&apos;monday&apos;, $strict = true);   // false<br>DaysOfWeek::isValidName(0);                          // false<br><br>DaysOfWeek::isValidValue(0);                         // true<br>DaysOfWeek::isValidValue(5);                         // true<br>DaysOfWeek::isValidValue(7);                         // false<br>DaysOfWeek::isValidValue(&apos;Friday&apos;);                  // false<br>As a side note, any time I use reflection at least once on a static/const class where the data won&apos;t change (such as in an enum), I cache the results of those reflection calls, since using fresh reflection objects each time will eventually have a noticeable performance impact (Stored in an assocciative array for multiple enums).  

---

Here&apos;s a clearer example usage in case anyone else finds the<br>current documentation confusing (as I did).<br><br>

```
<?php
class Fruit extends SplEnum
{
  // If no value is given during object construction this value is used
  const __default = 1;
  // Our enum values
  const APPLE     = 1;
  const ORANGE    = 2;
}

$myApple   = new Fruit();
$myOrange  = new Fruit(Fruit::ORANGE);
$fail      = 1;

function eat(Fruit $aFruit)
{
  if (Fruit::APPLE == $aFruit) {
    echo "Eating an apple.\n";
  } elseif (Fruit::ORANGE == $aFruit) {
    echo "Eating an orange.\n";
  }
}

eat($myApple);  // Eating an apple.
eat($myOrange); // Eating an orange.

eat($fail); // PHP Catchable fatal error:  Argument 1 passed to eat() must be an instance of Fruit, integer given

?>
```
  

---

SplEnum is a nice start for enumerated type support as an extension (making enum a part of the language would be much better), but it lacks a lot of features that enums have in other languages.  I needed functionality like the hasKey() method supported by Java.  I extended the class as SplEnumPlus and used that subclass in my code as follows:<br><br>

```
<?php
class SplEnumPlus extends SplEnum {
    static function hasKey($key) {
        $foundKey = false;
        
        try {
            $enumClassName = get_called_class();
            new $enumClassName($key);
            $foundKey = true;
        } finally {
            return $foundKey;
        }
    }
}

class Fruit extends SplEnumPlus {
  const APPLE     = 1;
  const ORANGE    = 2;
}

echo (Fruit::hasKey(Fruit::APPLE) ? 'yes' : 'no') . PHP_EOL; // yes
echo (Fruit::hasKey('banana') ? 'yes' : 'no') . PHP_EOL; // no
?>
```
<br><br>Other useful features, like reverse value-to-key lookups, could be done in this way.  It would be helpful if this and other useful functionality were made part of SplEnum.  

---

[Official documentation page](https://www.php.net/manual/en/class.splenum.php)

**[To root](/README.md)**