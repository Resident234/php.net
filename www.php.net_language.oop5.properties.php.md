# Properties




<div class="phpcode"><span class="html">
In case this saves anyone any time, I spent ages working out why the following didn&apos;t work:<br><br>class MyClass<br>{<br>&#xA0; &#xA0; private $foo = FALSE;<br><br>&#xA0; &#xA0; public function __construct()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;$foo = TRUE;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; echo($this-&gt;$foo);<br>&#xA0; &#xA0; }<br>}<br><br>$bar = new MyClass();<br><br>giving &quot;Fatal error: Cannot access empty property in ...test_class.php on line 8&quot;<br><br>The subtle change of removing the $ before accesses of $foo fixes this:<br><br>class MyClass<br>{<br>&#xA0; &#xA0; private $foo = FALSE;<br><br>&#xA0; &#xA0; public function __construct()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;foo = TRUE;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; echo($this-&gt;foo);<br>&#xA0; &#xA0; }<br>}<br><br>$bar = new MyClass();<br><br>I guess because it&apos;s treating $foo like a variable in the first example, so trying to call $this-&gt;FALSE (or something along those lines) which makes no sense. It&apos;s obvious once you&apos;ve realised, but there aren&apos;t any examples of accessing on this page that show that.</span>
</div>
  

#


<div class="phpcode"><span class="html">
You can access property names with dashes in them (for example, because you converted an XML file to an object) in the following way:<br><br><span class="default">&lt;?php<br>$ref </span><span class="keyword">= new </span><span class="default">StdClass</span><span class="keyword">();<br></span><span class="default">$ref</span><span class="keyword">-&gt;{</span><span class="string">&apos;ref-type&apos;</span><span class="keyword">} = </span><span class="string">&apos;Journal Article&apos;</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$ref</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
$this can be cast to array.&#xA0; But when doing so, it prefixes the property names/new array keys with certain data depending on the property classification.&#xA0; Public property names are not changed.&#xA0; Protected properties are prefixed with a space-padded &apos;*&apos;.&#xA0; Private properties are prefixed with the space-padded class name...
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">class </span><span class="default">test
<br></span><span class="keyword">{
<br>&#xA0; &#xA0; public </span><span class="default">$var1 </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;
<br>&#xA0; &#xA0; protected </span><span class="default">$var2 </span><span class="keyword">= </span><span class="default">2</span><span class="keyword">;
<br>&#xA0; &#xA0; private </span><span class="default">$var3 </span><span class="keyword">= </span><span class="default">3</span><span class="keyword">;
<br>&#xA0; &#xA0; static </span><span class="default">$var4 </span><span class="keyword">= </span><span class="default">4</span><span class="keyword">;
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; public function </span><span class="default">toArray</span><span class="keyword">()
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; return (array) </span><span class="default">$this</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>}
<br>
<br></span><span class="default">$t </span><span class="keyword">= new </span><span class="default">test</span><span class="keyword">;
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$t</span><span class="keyword">-&gt;</span><span class="default">toArray</span><span class="keyword">());
<br>
<br></span><span class="comment">/* outputs:
<br>
<br>Array
<br>(
<br>&#xA0; &#xA0; [var1] =&gt; 1
<br>&#xA0; &#xA0; [ * var2] =&gt; 2
<br>&#xA0; &#xA0; [ test var3] =&gt; 3
<br>)
<br>
<br>*/
<br></span><span class="default">?&gt;
<br></span>
<br>This is documented behavior when converting any object to an array (see &lt;/language.types.array.php#language.types.array.casting&gt; PHP manual page).&#xA0; All properties regardless of visibility will be shown when casting an object to array (with exceptions of a few built-in objects).
<br>
<br>To get an array with all property names unaltered, use the &apos;get_object_vars($this)&apos; function in any method within class scope to retrieve an array of all properties regardless of external visibility, or &apos;get_object_vars($object)&apos; outside class scope to retrieve an array of only public properties (see: &lt;/function.get-object-vars.php&gt; PHP manual page).</span>
</div>
  

#


<div class="phpcode"><span class="html">
Heredoc IS valid as of PHP 5.3 and this is documented in the manual at <a href="http://php.net/manual/en/language.types.string.php#language.types.string.syntax.heredoc" rel="nofollow" target="_blank">http://php.net/manual/en/language.types.string.php#language.types.string.syntax.heredoc</a><br><br>Only heredocs containing variables are invalid because then it becomes dynamic.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Do not confuse php&apos;s version of properties with properties in other languages (C++ for example).&#xA0; In php, properties are the same as attributes, simple variables without functionality.&#xA0; They should be called attributes, not properties.<br><br>Properties have implicit accessor and mutator functionality.&#xA0; I&apos;ve created an abstract class that allows implicit property functionality.<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">abstract class </span><span class="default">PropertyObject<br></span><span class="keyword">{<br>&#xA0; public function </span><span class="default">__get</span><span class="keyword">(</span><span class="default">$name</span><span class="keyword">)<br>&#xA0; {<br>&#xA0; &#xA0; if (</span><span class="default">method_exists</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">, (</span><span class="default">$method </span><span class="keyword">= </span><span class="string">&apos;get_&apos;</span><span class="keyword">.</span><span class="default">$name</span><span class="keyword">)))<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">$method</span><span class="keyword">();<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; else return;<br>&#xA0; }<br>&#xA0; <br>&#xA0; public function </span><span class="default">__isset</span><span class="keyword">(</span><span class="default">$name</span><span class="keyword">)<br>&#xA0; {<br>&#xA0; &#xA0; if (</span><span class="default">method_exists</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">, (</span><span class="default">$method </span><span class="keyword">= </span><span class="string">&apos;isset_&apos;</span><span class="keyword">.</span><span class="default">$name</span><span class="keyword">)))<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">$method</span><span class="keyword">();<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; else return;<br>&#xA0; }<br>&#xA0; <br>&#xA0; public function </span><span class="default">__set</span><span class="keyword">(</span><span class="default">$name</span><span class="keyword">, </span><span class="default">$value</span><span class="keyword">)<br>&#xA0; {<br>&#xA0; &#xA0; if (</span><span class="default">method_exists</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">, (</span><span class="default">$method </span><span class="keyword">= </span><span class="string">&apos;set_&apos;</span><span class="keyword">.</span><span class="default">$name</span><span class="keyword">)))<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">$method</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>&#xA0; }<br>&#xA0; <br>&#xA0; public function </span><span class="default">__unset</span><span class="keyword">(</span><span class="default">$name</span><span class="keyword">)<br>&#xA0; {<br>&#xA0; &#xA0; if (</span><span class="default">method_exists</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">, (</span><span class="default">$method </span><span class="keyword">= </span><span class="string">&apos;unset_&apos;</span><span class="keyword">.</span><span class="default">$name</span><span class="keyword">)))<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">$method</span><span class="keyword">();<br>&#xA0; &#xA0; }<br>&#xA0; }<br>}<br><br></span><span class="default">?&gt;<br></span><br>after extending this class, you can create accessors and mutators that will be called automagically, using php&apos;s magic methods, when the corresponding property is accessed.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.properties.php)

**[To root](/)**