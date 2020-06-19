# The SplEnum class




<div class="phpcode"><span class="html">
Source: <a href="https://stackoverflow.com/a/254543/508666" rel="nofollow" target="_blank">https://stackoverflow.com/a/254543/508666</a>
<br>
<br>if class SplEnum is not present, I would normally use something simple like the following:
<br>
<br>abstract class DaysOfWeek
<br>{
<br>&#xA0; &#xA0; const Sunday = 0;
<br>&#xA0; &#xA0; const Monday = 1;
<br>&#xA0; &#xA0; // etc.
<br>}
<br>
<br>$today = DaysOfWeek::Sunday;
<br>However, other use cases may require more validation of constants and values. 
<br>
<br>abstract class BasicEnum {
<br>&#xA0; &#xA0; private static $constCacheArray = NULL;
<br>
<br>private function __construct(){
<br>&#xA0; &#xA0; &#xA0; /*
<br>&#xA0; &#xA0; &#xA0; &#xA0; Preventing instance :)
<br>&#xA0; &#xA0; &#xA0; */
<br>&#xA0; &#xA0;&#xA0; }
<br>
<br>&#xA0; &#xA0; private static function getConstants() {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (self::$constCacheArray == NULL) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; self::$constCacheArray = [];
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; $calledClass = get_called_class();
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (!array_key_exists($calledClass, self::$constCacheArray)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $reflect = new ReflectionClass($calledClass);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; self::$constCacheArray[$calledClass] = $reflect-&gt;getConstants();
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; return self::$constCacheArray[$calledClass];
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; public static function isValidName($name, $strict = false) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; $constants = self::getConstants();
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; if ($strict) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return array_key_exists($name, $constants);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; $keys = array_map(&apos;strtolower&apos;, array_keys($constants));
<br>&#xA0; &#xA0; &#xA0; &#xA0; return in_array(strtolower($name), $keys);
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; public static function isValidValue($value) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; $values = array_values(self::getConstants());
<br>&#xA0; &#xA0; &#xA0; &#xA0; return in_array($value, $values, $strict = true);
<br>&#xA0; &#xA0; }
<br>}
<br>By creating a simple enum class that extends BasicEnum, you now have the ability to use methods thusly for simple input validation:
<br>
<br>abstract class DaysOfWeek extends BasicEnum {
<br>&#xA0; &#xA0; const Sunday = 0;
<br>&#xA0; &#xA0; const Monday = 1;
<br>&#xA0; &#xA0; const Tuesday = 2;
<br>&#xA0; &#xA0; const Wednesday = 3;
<br>&#xA0; &#xA0; const Thursday = 4;
<br>&#xA0; &#xA0; const Friday = 5;
<br>&#xA0; &#xA0; const Saturday = 6;
<br>}
<br>
<br>DaysOfWeek::isValidName(&apos;Humpday&apos;);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // false
<br>DaysOfWeek::isValidName(&apos;Monday&apos;);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // true
<br>DaysOfWeek::isValidName(&apos;monday&apos;);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // true
<br>DaysOfWeek::isValidName(&apos;monday&apos;, $strict = true);&#xA0;&#xA0; // false
<br>DaysOfWeek::isValidName(0);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // false
<br>
<br>DaysOfWeek::isValidValue(0);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // true
<br>DaysOfWeek::isValidValue(5);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // true
<br>DaysOfWeek::isValidValue(7);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // false
<br>DaysOfWeek::isValidValue(&apos;Friday&apos;);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // false
<br>As a side note, any time I use reflection at least once on a static/const class where the data won&apos;t change (such as in an enum), I cache the results of those reflection calls, since using fresh reflection objects each time will eventually have a noticeable performance impact (Stored in an assocciative array for multiple enums).</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here&apos;s a clearer example usage in case anyone else finds the<br>current documentation confusing (as I did).<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">Fruit </span><span class="keyword">extends </span><span class="default">SplEnum<br></span><span class="keyword">{<br>&#xA0; </span><span class="comment">// If no value is given during object construction this value is used<br>&#xA0; </span><span class="keyword">const </span><span class="default">__default </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;<br>&#xA0; </span><span class="comment">// Our enum values<br>&#xA0; </span><span class="keyword">const </span><span class="default">APPLE&#xA0; &#xA0;&#xA0; </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;<br>&#xA0; const </span><span class="default">ORANGE&#xA0; &#xA0; </span><span class="keyword">= </span><span class="default">2</span><span class="keyword">;<br>}<br><br></span><span class="default">$myApple&#xA0;&#xA0; </span><span class="keyword">= new </span><span class="default">Fruit</span><span class="keyword">();<br></span><span class="default">$myOrange&#xA0; </span><span class="keyword">= new </span><span class="default">Fruit</span><span class="keyword">(</span><span class="default">Fruit</span><span class="keyword">::</span><span class="default">ORANGE</span><span class="keyword">);<br></span><span class="default">$fail&#xA0; &#xA0; &#xA0; </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;<br><br>function </span><span class="default">eat</span><span class="keyword">(</span><span class="default">Fruit $aFruit</span><span class="keyword">)<br>{<br>&#xA0; if (</span><span class="default">Fruit</span><span class="keyword">::</span><span class="default">APPLE </span><span class="keyword">== </span><span class="default">$aFruit</span><span class="keyword">) {<br>&#xA0; &#xA0; echo </span><span class="string">&quot;Eating an apple.\n&quot;</span><span class="keyword">;<br>&#xA0; } elseif (</span><span class="default">Fruit</span><span class="keyword">::</span><span class="default">ORANGE </span><span class="keyword">== </span><span class="default">$aFruit</span><span class="keyword">) {<br>&#xA0; &#xA0; echo </span><span class="string">&quot;Eating an orange.\n&quot;</span><span class="keyword">;<br>&#xA0; }<br>}<br><br></span><span class="default">eat</span><span class="keyword">(</span><span class="default">$myApple</span><span class="keyword">);&#xA0; </span><span class="comment">// Eating an apple.<br></span><span class="default">eat</span><span class="keyword">(</span><span class="default">$myOrange</span><span class="keyword">); </span><span class="comment">// Eating an orange.<br><br></span><span class="default">eat</span><span class="keyword">(</span><span class="default">$fail</span><span class="keyword">); </span><span class="comment">// PHP Catchable fatal error:&#xA0; Argument 1 passed to eat() must be an instance of Fruit, integer given<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
SplEnum is a nice start for enumerated type support as an extension (making enum a part of the language would be much better), but it lacks a lot of features that enums have in other languages.&#xA0; I needed functionality like the hasKey() method supported by Java.&#xA0; I extended the class as SplEnumPlus and used that subclass in my code as follows:<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">SplEnumPlus </span><span class="keyword">extends </span><span class="default">SplEnum </span><span class="keyword">{<br>&#xA0; &#xA0; static function </span><span class="default">hasKey</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$foundKey </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; try {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$enumClassName </span><span class="keyword">= </span><span class="default">get_called_class</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; new </span><span class="default">$enumClassName</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$foundKey </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; } finally {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$foundKey</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>}<br><br>class </span><span class="default">Fruit </span><span class="keyword">extends </span><span class="default">SplEnumPlus </span><span class="keyword">{<br>&#xA0; const </span><span class="default">APPLE&#xA0; &#xA0;&#xA0; </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;<br>&#xA0; const </span><span class="default">ORANGE&#xA0; &#xA0; </span><span class="keyword">= </span><span class="default">2</span><span class="keyword">;<br>}<br><br>echo (</span><span class="default">Fruit</span><span class="keyword">::</span><span class="default">hasKey</span><span class="keyword">(</span><span class="default">Fruit</span><span class="keyword">::</span><span class="default">APPLE</span><span class="keyword">) ? </span><span class="string">&apos;yes&apos; </span><span class="keyword">: </span><span class="string">&apos;no&apos;</span><span class="keyword">) . </span><span class="default">PHP_EOL</span><span class="keyword">; </span><span class="comment">// yes<br></span><span class="keyword">echo (</span><span class="default">Fruit</span><span class="keyword">::</span><span class="default">hasKey</span><span class="keyword">(</span><span class="string">&apos;banana&apos;</span><span class="keyword">) ? </span><span class="string">&apos;yes&apos; </span><span class="keyword">: </span><span class="string">&apos;no&apos;</span><span class="keyword">) . </span><span class="default">PHP_EOL</span><span class="keyword">; </span><span class="comment">// no<br></span><span class="default">?&gt;<br></span><br>Other useful features, like reverse value-to-key lookups, could be done in this way.&#xA0; It would be helpful if this and other useful functionality were made part of SplEnum.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.splenum.php)

**[To root](/README.md)**