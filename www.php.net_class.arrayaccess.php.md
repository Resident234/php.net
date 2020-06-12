# The ArrayAccess interface




<div class="phpcode"><span class="html">
It bit me today, so putting it here in the hope it will help others:<br>If you call array_key_exists() on an object of a class that implements ArrayAccess, ArrayAccess::offsetExists() wil NOT be called.</span>
</div>
  

#


<div class="phpcode"><span class="html">
The indexes used in an ArrayAccess object are not limited to strings and integers as they are for arrays: you can use any type for the index as long as you write your implementation to handle them. This fact is exploited by the SplObjectStorage class.</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br><br></span><span class="comment">/**<br> * ArrayAndObjectAccess<br> * Yes you can access class as array and the same time as object<br> *<br> * @author Yousef Ismaeil &lt;cliprz@gmail.com&gt;<br> */<br><br></span><span class="keyword">class </span><span class="default">ArrayAndObjectAccess </span><span class="keyword">implements </span><span class="default">ArrayAccess </span><span class="keyword">{<br><br>&#xA0; &#xA0; </span><span class="comment">/**<br>&#xA0; &#xA0;&#xA0; * Data<br>&#xA0; &#xA0;&#xA0; *<br>&#xA0; &#xA0;&#xA0; * @var array<br>&#xA0; &#xA0;&#xA0; * @access private<br>&#xA0; &#xA0;&#xA0; */<br>&#xA0; &#xA0; </span><span class="keyword">private </span><span class="default">$data </span><span class="keyword">= [];<br><br>&#xA0; &#xA0; </span><span class="comment">/**<br>&#xA0; &#xA0;&#xA0; * Get a data by key<br>&#xA0; &#xA0;&#xA0; *<br>&#xA0; &#xA0;&#xA0; * @param string The key data to retrieve<br>&#xA0; &#xA0;&#xA0; * @access public<br>&#xA0; &#xA0;&#xA0; */<br>&#xA0; &#xA0; </span><span class="keyword">public function &amp;</span><span class="default">__get </span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">data</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">];<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; </span><span class="comment">/**<br>&#xA0; &#xA0;&#xA0; * Assigns a value to the specified data<br>&#xA0; &#xA0;&#xA0; * <br>&#xA0; &#xA0;&#xA0; * @param string The data key to assign the value to<br>&#xA0; &#xA0;&#xA0; * @param mixed&#xA0; The value to set<br>&#xA0; &#xA0;&#xA0; * @access public <br>&#xA0; &#xA0;&#xA0; */<br>&#xA0; &#xA0; </span><span class="keyword">public function </span><span class="default">__set</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">,</span><span class="default">$value</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">data</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">$value</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; </span><span class="comment">/**<br>&#xA0; &#xA0;&#xA0; * Whether or not an data exists by key<br>&#xA0; &#xA0;&#xA0; *<br>&#xA0; &#xA0;&#xA0; * @param string An data key to check for<br>&#xA0; &#xA0;&#xA0; * @access public<br>&#xA0; &#xA0;&#xA0; * @return boolean<br>&#xA0; &#xA0;&#xA0; * @abstracting ArrayAccess<br>&#xA0; &#xA0;&#xA0; */<br>&#xA0; &#xA0; </span><span class="keyword">public function </span><span class="default">__isset </span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; return isset(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">data</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">]);<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; </span><span class="comment">/**<br>&#xA0; &#xA0;&#xA0; * Unsets an data by key<br>&#xA0; &#xA0;&#xA0; *<br>&#xA0; &#xA0;&#xA0; * @param string The key to unset<br>&#xA0; &#xA0;&#xA0; * @access public<br>&#xA0; &#xA0;&#xA0; */<br>&#xA0; &#xA0; </span><span class="keyword">public function </span><span class="default">__unset</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; unset(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">data</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">]);<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; </span><span class="comment">/**<br>&#xA0; &#xA0;&#xA0; * Assigns a value to the specified offset<br>&#xA0; &#xA0;&#xA0; *<br>&#xA0; &#xA0;&#xA0; * @param string The offset to assign the value to<br>&#xA0; &#xA0;&#xA0; * @param mixed&#xA0; The value to set<br>&#xA0; &#xA0;&#xA0; * @access public<br>&#xA0; &#xA0;&#xA0; * @abstracting ArrayAccess<br>&#xA0; &#xA0;&#xA0; */<br>&#xA0; &#xA0; </span><span class="keyword">public function </span><span class="default">offsetSet</span><span class="keyword">(</span><span class="default">$offset</span><span class="keyword">,</span><span class="default">$value</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">is_null</span><span class="keyword">(</span><span class="default">$offset</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">data</span><span class="keyword">[] = </span><span class="default">$value</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">data</span><span class="keyword">[</span><span class="default">$offset</span><span class="keyword">] = </span><span class="default">$value</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; </span><span class="comment">/**<br>&#xA0; &#xA0;&#xA0; * Whether or not an offset exists<br>&#xA0; &#xA0;&#xA0; *<br>&#xA0; &#xA0;&#xA0; * @param string An offset to check for<br>&#xA0; &#xA0;&#xA0; * @access public<br>&#xA0; &#xA0;&#xA0; * @return boolean<br>&#xA0; &#xA0;&#xA0; * @abstracting ArrayAccess<br>&#xA0; &#xA0;&#xA0; */<br>&#xA0; &#xA0; </span><span class="keyword">public function </span><span class="default">offsetExists</span><span class="keyword">(</span><span class="default">$offset</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; return isset(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">data</span><span class="keyword">[</span><span class="default">$offset</span><span class="keyword">]);<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; </span><span class="comment">/**<br>&#xA0; &#xA0;&#xA0; * Unsets an offset<br>&#xA0; &#xA0;&#xA0; *<br>&#xA0; &#xA0;&#xA0; * @param string The offset to unset<br>&#xA0; &#xA0;&#xA0; * @access public<br>&#xA0; &#xA0;&#xA0; * @abstracting ArrayAccess<br>&#xA0; &#xA0;&#xA0; */<br>&#xA0; &#xA0; </span><span class="keyword">public function </span><span class="default">offsetUnset</span><span class="keyword">(</span><span class="default">$offset</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">offsetExists</span><span class="keyword">(</span><span class="default">$offset</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; unset(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">data</span><span class="keyword">[</span><span class="default">$offset</span><span class="keyword">]);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; </span><span class="comment">/**<br>&#xA0; &#xA0;&#xA0; * Returns the value at specified offset<br>&#xA0; &#xA0;&#xA0; *<br>&#xA0; &#xA0;&#xA0; * @param string The offset to retrieve<br>&#xA0; &#xA0;&#xA0; * @access public<br>&#xA0; &#xA0;&#xA0; * @return mixed<br>&#xA0; &#xA0;&#xA0; * @abstracting ArrayAccess<br>&#xA0; &#xA0;&#xA0; */<br>&#xA0; &#xA0; </span><span class="keyword">public function </span><span class="default">offsetGet</span><span class="keyword">(</span><span class="default">$offset</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">offsetExists</span><span class="keyword">(</span><span class="default">$offset</span><span class="keyword">) ? </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">data</span><span class="keyword">[</span><span class="default">$offset</span><span class="keyword">] : </span><span class="default">null</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>}<br><br></span><span class="default">?&gt;<br></span><br>Usage<br><br><span class="default">&lt;?php<br>$foo </span><span class="keyword">= new </span><span class="default">ArrayAndObjectAccess</span><span class="keyword">();<br></span><span class="comment">// Set data as array and object<br></span><span class="default">$foo</span><span class="keyword">-&gt;</span><span class="default">fname </span><span class="keyword">= </span><span class="string">&apos;Yousef&apos;</span><span class="keyword">;<br></span><span class="default">$foo</span><span class="keyword">-&gt;</span><span class="default">lname </span><span class="keyword">= </span><span class="string">&apos;Ismaeil&apos;</span><span class="keyword">;<br></span><span class="comment">// Call as object<br></span><span class="keyword">echo </span><span class="string">&apos;fname as object &apos;</span><span class="keyword">.</span><span class="default">$foo</span><span class="keyword">-&gt;</span><span class="default">fname</span><span class="keyword">.</span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br></span><span class="comment">// Call as array<br></span><span class="keyword">echo </span><span class="string">&apos;lname as array &apos;</span><span class="keyword">.</span><span class="default">$foo</span><span class="keyword">[</span><span class="string">&apos;lname&apos;</span><span class="keyword">].</span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br></span><span class="comment">// Reset as array<br></span><span class="default">$foo</span><span class="keyword">[</span><span class="string">&apos;fname&apos;</span><span class="keyword">] = </span><span class="string">&apos;Cliprz&apos;</span><span class="keyword">;<br>echo </span><span class="default">$foo</span><span class="keyword">[</span><span class="string">&apos;fname&apos;</span><span class="keyword">].</span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br><br></span><span class="comment">/** Outputs<br>fname as object Yousef<br>lname as array Ismaeil<br>Cliprz<br>*/<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Objects implementing ArrayAccess may return objects by references in PHP 5.3.0.<br><br>You can implement your ArrayAccess object like this:<br><br>&#xA0; &#xA0; class Reflectable implements ArrayAccess {<br><br>&#xA0; &#xA0; &#xA0; &#xA0; public function set($name, $value) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;{$name} = $value;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; public function &amp;get($name) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;{$name};<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; public function offsetGet($offset) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;get($offset);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; public function offsetSet($offset, $value) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;set($offset, $value);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; ...<br><br>&#xA0; &#xA0; }<br><br>This base class allows you to get / set your object properties using the [] operator just like in Javascript:<br><br>&#xA0; &#xA0; class Boo extends Reflectable {<br>&#xA0; &#xA0; &#xA0; &#xA0; public $name;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; $obj = new Boo();<br>&#xA0; &#xA0; $obj[&apos;name&apos;] = &quot;boo&quot;;<br>&#xA0; &#xA0; echo $obj[&apos;name&apos;]; // prints boo</span>
</div>
  

#


<div class="phpcode"><span class="html">
Conclusion: Type hints \ArrayAccess and array are not compatible.
<br>
<br><span class="default">&lt;?php
<br>
<br>&#xA0; &#xA0;&#xA0; </span><span class="keyword">class </span><span class="default">MyArrayAccess </span><span class="keyword">implements \</span><span class="default">ArrayAccess
<br>&#xA0; &#xA0;&#xA0; </span><span class="keyword">{
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; public function </span><span class="default">offsetExists</span><span class="keyword">(</span><span class="default">$offset</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; public function </span><span class="default">offsetSet</span><span class="keyword">(</span><span class="default">$offset</span><span class="keyword">, </span><span class="default">$value</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; public function </span><span class="default">offsetGet</span><span class="keyword">(</span><span class="default">$offset</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; public function </span><span class="default">offsetUnset</span><span class="keyword">(</span><span class="default">$offset</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
<br>&#xA0; &#xA0;&#xA0; }
<br>
<br>
<br>&#xA0; &#xA0;&#xA0; function </span><span class="default">test</span><span class="keyword">(array </span><span class="default">$arr</span><span class="keyword">)
<br>&#xA0; &#xA0;&#xA0; {
<br>&#xA0; &#xA0;&#xA0; }
<br>
<br>&#xA0; &#xA0;&#xA0; function </span><span class="default">test2</span><span class="keyword">(\</span><span class="default">ArrayAccess $arr</span><span class="keyword">)
<br>&#xA0; &#xA0;&#xA0; {
<br>
<br>&#xA0; &#xA0;&#xA0; }
<br>
<br>
<br>&#xA0; &#xA0;&#xA0; </span><span class="default">$arrObj </span><span class="keyword">= new </span><span class="default">MyArrayAccess</span><span class="keyword">();
<br>&#xA0; &#xA0;&#xA0; </span><span class="default">test</span><span class="keyword">([]); </span><span class="comment">//result: works!
<br>&#xA0; &#xA0;&#xA0; </span><span class="default">test</span><span class="keyword">(</span><span class="default">$arrObj</span><span class="keyword">); </span><span class="comment">//result: does NOT work
<br>&#xA0; &#xA0;&#xA0; </span><span class="default">test2</span><span class="keyword">([]); </span><span class="comment">//result: does NOT work
<br>&#xA0; &#xA0;&#xA0; </span><span class="default">test2</span><span class="keyword">(</span><span class="default">$arrObj</span><span class="keyword">); </span><span class="comment">// result: works!
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.arrayaccess.php)

**[To root](/)**