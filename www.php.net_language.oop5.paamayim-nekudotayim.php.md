# Scope Resolution Operator (::)




<div class="phpcode"><span class="html">
A class constant, class property (static), and class function (static) can all share the same name and be accessed using the double-colon.<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">A </span><span class="keyword">{<br><br>&#xA0; &#xA0; public static </span><span class="default">$B </span><span class="keyword">= </span><span class="string">&apos;1&apos;</span><span class="keyword">; </span><span class="comment"># Static class variable.<br><br>&#xA0; &#xA0; </span><span class="keyword">const </span><span class="default">B </span><span class="keyword">= </span><span class="string">&apos;2&apos;</span><span class="keyword">; </span><span class="comment"># Class constant.<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="keyword">public static function </span><span class="default">B</span><span class="keyword">() { </span><span class="comment"># Static class function.<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="string">&apos;3&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>}<br><br>echo </span><span class="default">A</span><span class="keyword">::</span><span class="default">$B </span><span class="keyword">. </span><span class="default">A</span><span class="keyword">::</span><span class="default">B </span><span class="keyword">. </span><span class="default">A</span><span class="keyword">::</span><span class="default">B</span><span class="keyword">(); </span><span class="comment"># Outputs: 123<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
In PHP, you use the self keyword to access static properties and methods.<br><br>The problem is that you can replace $this-&gt;method() with self::method() anywhere, regardless if method() is declared static or not. So which one should you use?<br><br>Consider this code:<br><br>class ParentClass {<br>&#xA0; &#xA0; function test() {<br>&#xA0; &#xA0; &#xA0; &#xA0; self::who();&#xA0; &#xA0; // will output &apos;parent&apos;<br>&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;who();&#xA0; &#xA0; // will output &apos;child&apos;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; function who() {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;parent&apos;;<br>&#xA0; &#xA0; }<br>}<br><br>class ChildClass extends ParentClass {<br>&#xA0; &#xA0; function who() {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;child&apos;;<br>&#xA0; &#xA0; }<br>}<br><br>$obj = new ChildClass();<br>$obj-&gt;test();<br>In this example, self::who() will always output &#x2018;parent&#x2019;, while $this-&gt;who() will depend on what class the object has.<br><br>Now we can see that self refers to the class in which it is called, while $this refers to the class of the current object.<br><br>So, you should use self only when $this is not available, or when you don&#x2019;t want to allow descendant classes to overwrite the current method.</span>
</div>
  

#


<div class="phpcode"><span class="html">
It seems as though you can use more than the class name to reference the static variables, constants, and static functions of a class definition from outside that class using the :: . The language appears to allow you to use the object itself. <br><br>For example:<br>class horse <br>{<br>&#xA0;&#xA0; static $props = {&apos;order&apos;=&gt;&apos;mammal&apos;};<br>}<br>$animal = new horse();<br>echo $animal::$props[&apos;order&apos;];<br><br>// yields &apos;mammal&apos;<br><br>This does not appear to be documented but I see it as an important convenience in the language. I would like to see it documented and supported as valid. <br><br>If it weren&apos;t supported officially, the alternative would seem to be messy, something like this:<br><br>$animalClass = get_class($animal);<br>echo $animalClass::$props[&apos;order&apos;];</span>
</div>
  

#


<div class="phpcode"><span class="html">
Just found out that using the class name may also work to call similar function of anchestor class.<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">Anchestor </span><span class="keyword">{<br>&#xA0;&#xA0; <br>&#xA0;&#xA0; public </span><span class="default">$Prefix </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;<br><br>&#xA0;&#xA0; private </span><span class="default">$_string </span><span class="keyword">=&#xA0; </span><span class="string">&apos;Bar&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; public function </span><span class="default">Foo</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">Prefix</span><span class="keyword">.</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">_string</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br>class </span><span class="default">MyParent </span><span class="keyword">extends </span><span class="default">Anchestor </span><span class="keyword">{<br>&#xA0; &#xA0; public function </span><span class="default">Foo</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">Prefix </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">parent</span><span class="keyword">::</span><span class="default">Foo</span><span class="keyword">().</span><span class="string">&apos;Baz&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br>class </span><span class="default">Child </span><span class="keyword">extends </span><span class="default">MyParent </span><span class="keyword">{<br>&#xA0; &#xA0; public function </span><span class="default">Foo</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">Prefix </span><span class="keyword">= </span><span class="string">&apos;Foo&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">Anchestor</span><span class="keyword">::</span><span class="default">Foo</span><span class="keyword">();<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">$c </span><span class="keyword">= new </span><span class="default">Child</span><span class="keyword">();<br>echo </span><span class="default">$c</span><span class="keyword">-&gt;</span><span class="default">Foo</span><span class="keyword">(); </span><span class="comment">//return FooBar, because Prefix, as in Anchestor::Foo()<br><br></span><span class="default">?&gt;<br></span><br>The Child class calls at Anchestor::Foo(), and therefore MyParent::Foo() is never run.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.paamayim-nekudotayim.php)

**[To root](/)**