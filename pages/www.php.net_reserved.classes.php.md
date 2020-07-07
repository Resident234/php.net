# Predefined Classes




<div class="phpcode"><span class="html">
If you call var_export() on an instance of stdClass, it attempts to export it using ::__set_state(), which, for some reason, is not implemented in stdClass.<br><br>However, casting an associative array to an object usually produces the same effect (at least, it does in my case). So I wrote an improved_var_export() function to convert instances of stdClass to (object) array () calls. If you choose to export objects of any other class, I&apos;d advise you to implement ::__set_state().<br><br><span class="default">&lt;?php<br></span><span class="comment">/**<br> * An implementation of var_export() that is compatible with instances<br> * of stdClass.<br> * @param mixed $variable The variable you want to export<br> * @param bool $return If used and set to true, improved_var_export()<br> *&#xA0; &#xA0;&#xA0; will return the variable representation instead of outputting it.<br> * @return mixed|null Returns the variable representation when the<br> *&#xA0; &#xA0;&#xA0; return parameter is used and evaluates to TRUE. Otherwise, this<br> *&#xA0; &#xA0;&#xA0; function will return NULL.<br> */<br></span><span class="keyword">function </span><span class="default">improved_var_export </span><span class="keyword">(</span><span class="default">$variable</span><span class="keyword">, </span><span class="default">$return </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">) {<br>&#xA0; &#xA0; if (</span><span class="default">$variable </span><span class="keyword">instanceof </span><span class="default">stdClass</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= </span><span class="string">&apos;(object) &apos;</span><span class="keyword">.</span><span class="default">improved_var_export</span><span class="keyword">(</span><span class="default">get_object_vars</span><span class="keyword">(</span><span class="default">$variable</span><span class="keyword">), </span><span class="default">true</span><span class="keyword">);<br>&#xA0; &#xA0; } else if (</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$variable</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$array </span><span class="keyword">= array ();<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$variable </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$array</span><span class="keyword">[] = </span><span class="default">var_export</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">).</span><span class="string">&apos; =&gt; &apos;</span><span class="keyword">.</span><span class="default">improved_var_export</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= </span><span class="string">&apos;array (&apos;</span><span class="keyword">.</span><span class="default">implode</span><span class="keyword">(</span><span class="string">&apos;, &apos;</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">).</span><span class="string">&apos;)&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= </span><span class="default">var_export</span><span class="keyword">(</span><span class="default">$variable</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; if (!</span><span class="default">$return</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; print </span><span class="default">$result</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">null</span><span class="keyword">;<br>&#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$result</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="comment">// Example usage:<br></span><span class="default">$obj </span><span class="keyword">= new </span><span class="default">stdClass</span><span class="keyword">;<br></span><span class="default">$obj</span><span class="keyword">-&gt;</span><span class="default">test </span><span class="keyword">= </span><span class="string">&apos;abc&apos;</span><span class="keyword">;<br></span><span class="default">$obj</span><span class="keyword">-&gt;</span><span class="default">other </span><span class="keyword">= </span><span class="default">6.2</span><span class="keyword">;<br></span><span class="default">$obj</span><span class="keyword">-&gt;</span><span class="default">arr </span><span class="keyword">= array (</span><span class="default">1</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">, </span><span class="default">3</span><span class="keyword">);<br><br></span><span class="default">improved_var_export</span><span class="keyword">((object) array (<br>&#xA0; &#xA0; </span><span class="string">&apos;prop1&apos; </span><span class="keyword">=&gt; </span><span class="default">true</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&apos;prop2&apos; </span><span class="keyword">=&gt; </span><span class="default">$obj</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&apos;assocArray&apos; </span><span class="keyword">=&gt; array (<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;apple&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;good&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;orange&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;great&apos;<br>&#xA0; &#xA0; </span><span class="keyword">)<br>));<br><br></span><span class="comment">/* Output:<br>(object) array (&apos;prop1&apos; =&gt; true, &apos;prop2&apos; =&gt; (object) array (&apos;test&apos; =&gt; &apos;abc&apos;, &apos;other&apos; =&gt; 6.2, &apos;arr&apos; =&gt; array (0 =&gt; 1, 1 =&gt; 2, 2 =&gt; 3)), &apos;assocArray&apos; =&gt; array (&apos;apple&apos; =&gt; &apos;good&apos;, &apos;orange&apos; =&gt; &apos;great&apos;))<br>*/<br></span><span class="default">?&gt;<br></span><br>Note: This function spits out a single line of code, which is useful to save in a cache file to include/eval. It isn&apos;t formatted for readability. If you want to print a readable version for debugging purposes, then I would suggest print_r() or var_dump().</span>
</div>
  

#


<div class="phpcode"><span class="html">
if you want a Dynamic class you can extend from, add atributes AND methods on the fly you can use this:<br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">class </span><span class="default">Dynamic </span><span class="keyword">extends </span><span class="default">stdClass</span><span class="keyword">{<br>&#xA0; &#xA0; &#xA0; &#xA0; public function </span><span class="default">__call</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">,</span><span class="default">$params</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(!isset(</span><span class="default">$this</span><span class="keyword">-&gt;{</span><span class="default">$key</span><span class="keyword">})) throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&quot;Call to undefined method &quot;</span><span class="keyword">.</span><span class="default">get_class</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">).</span><span class="string">&quot;::&quot;</span><span class="keyword">.</span><span class="default">$key</span><span class="keyword">.</span><span class="string">&quot;()&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$subject </span><span class="keyword">= </span><span class="default">$this</span><span class="keyword">-&gt;{</span><span class="default">$key</span><span class="keyword">};<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">call_user_func_array</span><span class="keyword">(</span><span class="default">$subject</span><span class="keyword">,</span><span class="default">$params</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;<br></span><br>this will accept both arrays, strings and Closures:<br><span class="default">&lt;?php<br>&#xA0; &#xA0; $dynamic</span><span class="keyword">-&gt;</span><span class="default">myMethod </span><span class="keyword">= </span><span class="string">&quot;thatFunction&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">$dynamic</span><span class="keyword">-&gt;</span><span class="default">hisMethod </span><span class="keyword">= array(</span><span class="default">$instance</span><span class="keyword">,</span><span class="string">&quot;aMethod&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$dynamic</span><span class="keyword">-&gt;</span><span class="default">newMethod </span><span class="keyword">= array(</span><span class="default">SomeClass</span><span class="keyword">,</span><span class="string">&quot;staticMethod&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$dynamic</span><span class="keyword">-&gt;</span><span class="default">anotherMethod </span><span class="keyword">= function(){<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;Hey there&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; };<br></span><span class="default">?&gt;<br></span><br>then call them away =D</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/reserved.classes.php)

**[To root](/README.md)**