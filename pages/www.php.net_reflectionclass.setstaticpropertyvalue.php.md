# ReflectionClass::setStaticPropertyValue



Calling this method on a static property that is not public will return a ReflectionException stating the property does not exist. This is quite misleading as the property is valid.<br><br>class test {<br>    public static $publicProperty = &apos;public&apos;;<br>    private static $privateProperty = &apos;private&apos;;<br><br>    public static function printProperties() {<br>        echo self::$publicProperty . "\n";<br>        echo self::$privateProperty . "\n";<br>    }<br>} <br><br>$reflectedClass = new \ReflectionClass(&apos;test&apos;);<br>$reflectedClass-&gt;setStaticPropertyValue(&apos;publicProperty&apos;, &apos;foo&apos;);<br>$reflectedClass-&gt;setStaticPropertyValue( &apos;privateProperty&apos;, &apos;bar&apos; );<br><br>PHP Fatal error:  Uncaught exception &apos;ReflectionException&apos; with message &apos;Class test does not have a property named privateProperty&apos;<br><br>If you retrieve the method using the reflection class getProperty method you can circumnavigate this issue<br><br>$reflectedProperty = $reflectedClass-&gt;getProperty(&apos;privateProperty&apos;);<br>$reflectedProperty-&gt;setAccessible(true);<br>$reflectedProperty = $reflectedProperty-&gt;setValue(&apos;bar&apos;);<br><br>test::printProperties(); <br>will echo<br>foo<br>bar  

---

It is important to note that calling ReflectionClass::setStaticPropertyValue will not allow you to add new static properties to a class.  

---

[Official documentation page](https://www.php.net/manual/en/reflectionclass.setstaticpropertyvalue.php)

**[To root](/README.md)**