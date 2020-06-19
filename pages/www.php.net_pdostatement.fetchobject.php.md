# PDOStatement::fetchObject




<div class="phpcode"><span class="html">
Be warned of the rather unorthodox behavior of PDOStatement::fetchObject() which injects property-values BEFORE invoking the constructor - in other words, if your class&#xA0; initializes property-values to defaults in the constructor, you will be overwriting the values injected by fetchObject() !<br><br>A var_dump($this) in your __construct() method will reveal that property-values have been initialized prior to calling your constructor, so be careful.<br><br>For this reason, I strongly recommend hydrating your objects manually, after retrieving the data as an array, rather than trying to have PDO apply properties directly to your objects.<br><br>Clearly somebody thought they were being clever here - allowing you to access hydrated property-values from the constructor. Unfortunately, this is just not how OOP works - the constructor, by definition, is the first method called upon construction. <br><br>If you need to initialize your objects after they have been constructed and hydrated, I suggest your model types implement an interface with an init() method, and you data access layer invoke this method (if implemented) after hydrating.</span>
</div>
  

#


<div class="phpcode"><span class="html">
It should be mentioned that this method can set even non-public properties. It may sound strange but it can actually be very useful when creating an object based on mysql result.<br>Consider a User class:<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">User </span><span class="keyword">{<br>&#xA0;&#xA0; </span><span class="comment">// Private properties<br>&#xA0;&#xA0; </span><span class="keyword">private </span><span class="default">$id</span><span class="keyword">, </span><span class="default">$name</span><span class="keyword">;<br><br>&#xA0;&#xA0; private function </span><span class="default">__construct </span><span class="keyword">() {}<br><br>&#xA0;&#xA0; public static function </span><span class="default">load_by_id </span><span class="keyword">(</span><span class="default">$id</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$stmt </span><span class="keyword">= </span><span class="default">$pdo</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="string">&apos;SELECT id, name FROM users WHERE id=?&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">([</span><span class="default">$id</span><span class="keyword">]);<br>&#xA0; &#xA0; &#xA0; return </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">fetchObject</span><span class="keyword">(</span><span class="default">__CLASS__</span><span class="keyword">);<br>&#xA0;&#xA0; }<br>&#xA0;&#xA0; </span><span class="comment">/* same method can be written with the &quot;name&quot; column/property */<br></span><span class="keyword">}<br><br></span><span class="default">$user </span><span class="keyword">= </span><span class="default">User</span><span class="keyword">::</span><span class="default">load_by_id</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$user</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>fetchObject() doesn&apos;t care about properties being public or not. It just passes the result to the object. Output is like:<br><br>object(User)#3 (2) {<br>&#xA0; [&quot;id&quot;:&quot;User&quot;:private]=&gt;<br>&#xA0; string(1) &quot;1&quot;<br>&#xA0; [&quot;name&quot;:&quot;User&quot;:private]=&gt;<br>&#xA0; string(10) &quot;John Smith&quot;<br>}</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.fetchobject.php)

**[To root](/README.md)**