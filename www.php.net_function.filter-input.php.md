# filter_input




<div class="phpcode"><span class="html">
This function provides us the extremely simple solution for type filtering.<br><br>Without this function...<br><span class="default">&lt;?php<br></span><span class="keyword">if (!isset(</span><span class="default">$_GET</span><span class="keyword">[</span><span class="string">&apos;a&apos;</span><span class="keyword">])) {<br>&#xA0; &#xA0; </span><span class="default">$a </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">;<br>} elseif (!</span><span class="default">is_string</span><span class="keyword">(</span><span class="default">$_GET</span><span class="keyword">[</span><span class="string">&apos;a&apos;</span><span class="keyword">])) {<br>&#xA0; &#xA0; </span><span class="default">$a </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">;<br>} else {<br>&#xA0; &#xA0; </span><span class="default">$a </span><span class="keyword">= </span><span class="default">$_GET</span><span class="keyword">[</span><span class="string">&apos;a&apos;</span><span class="keyword">];<br>}<br></span><span class="default">$b </span><span class="keyword">= isset(</span><span class="default">$_GET</span><span class="keyword">[</span><span class="string">&apos;b&apos;</span><span class="keyword">]) &amp;&amp; </span><span class="default">is_string</span><span class="keyword">(</span><span class="default">$_GET</span><span class="keyword">[</span><span class="string">&apos;b&apos;</span><span class="keyword">]) ? </span><span class="default">$_GET</span><span class="keyword">[</span><span class="string">&apos;b&apos;</span><span class="keyword">] : </span><span class="string">&apos;&apos;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>With this function...<br><span class="default">&lt;?php<br>$a </span><span class="keyword">= </span><span class="default">filter_input</span><span class="keyword">(</span><span class="default">INPUT_GET</span><span class="keyword">, </span><span class="string">&apos;a&apos;</span><span class="keyword">);<br></span><span class="default">$b </span><span class="keyword">= (string)</span><span class="default">filter_input</span><span class="keyword">(</span><span class="default">INPUT_GET</span><span class="keyword">, </span><span class="string">&apos;b&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Yes, FILTER_REQUIRE_SCALAR seems to be set as a default option. <br>It&apos;s very helpful for eliminating E_NOTICE, E_WARNING and E_ERROR. <br>This fact should be documented.</span>
</div>
  

#


<div class="phpcode"><span class="html">
FastCGI seems to cause strange side-effects with unexpected null values when using INPUT_SERVER and INPUT_ENV with this function. You can use this code to see if it affects your server:<br><span class="default">&lt;?php<br>var_dump</span><span class="keyword">(</span><span class="default">$_SERVER</span><span class="keyword">);<br>foreach ( </span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$_SERVER</span><span class="keyword">) as </span><span class="default">$b </span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">, </span><span class="default">filter_input</span><span class="keyword">(</span><span class="default">INPUT_SERVER</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">));<br>}<br>echo </span><span class="string">&apos;&lt;hr&gt;&apos;</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$_ENV</span><span class="keyword">);<br>foreach ( </span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$_ENV</span><span class="keyword">) as </span><span class="default">$b </span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">, </span><span class="default">filter_input</span><span class="keyword">(</span><span class="default">INPUT_ENV</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">));<br>}<br></span><span class="default">?&gt;<br></span>If you want to be on the safe side, using the superglobal $_SERVER and $_ENV variables will always work. You can still use the filter_* functions for Get/Post/Cookie without a problem, which is the important part!</span>
</div>
  

#


<div class="phpcode"><span class="html">
If your $_POST contains an array value:<br><span class="default">&lt;?php<br>$_POST&#xA0; </span><span class="keyword">= array(<br>&#xA0; &#xA0; </span><span class="string">&apos;var&apos; </span><span class="keyword">=&gt; array(</span><span class="string">&apos;more&apos;</span><span class="keyword">, </span><span class="string">&apos;than&apos;</span><span class="keyword">, </span><span class="string">&apos;one&apos;</span><span class="keyword">, </span><span class="string">&apos;values&apos;</span><span class="keyword">)<br>);<br></span><span class="default">?&gt;<br></span>you should use FILTER_REQUIRE_ARRAY option:<br><span class="default">&lt;?php<br>var_dump</span><span class="keyword">(</span><span class="default">filter_input</span><span class="keyword">(</span><span class="default">INPUT_POST</span><span class="keyword">, </span><span class="string">&apos;var&apos;</span><span class="keyword">, </span><span class="default">FILTER_DEFAULT </span><span class="keyword">, </span><span class="default">FILTER_REQUIRE_ARRAY</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span>Otherwise it returns false.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that this function doesn&apos;t (or at least doesn&apos;t seem to) actually filter based on the current values of $_GET etc. Instead, it seems to filter based off the original values.<br><span class="default">&lt;?php<br>$_GET</span><span class="keyword">[</span><span class="string">&apos;search&apos;</span><span class="keyword">] = </span><span class="string">&apos;foo&apos;</span><span class="keyword">; </span><span class="comment">// This has no effect on the filter_input<br><br></span><span class="default">$search_html </span><span class="keyword">= </span><span class="default">filter_input</span><span class="keyword">(</span><span class="default">INPUT_GET</span><span class="keyword">, </span><span class="string">&apos;search&apos;</span><span class="keyword">, </span><span class="default">FILTER_SANITIZE_SPECIAL_CHARS</span><span class="keyword">);<br></span><span class="default">$search_url </span><span class="keyword">= </span><span class="default">filter_input</span><span class="keyword">(</span><span class="default">INPUT_GET</span><span class="keyword">, </span><span class="string">&apos;search&apos;</span><span class="keyword">, </span><span class="default">FILTER_SANITIZE_ENCODED</span><span class="keyword">);<br>echo </span><span class="string">&quot;You have searched for </span><span class="default">$search_html</span><span class="string">.\n&quot;</span><span class="keyword">;<br>echo </span><span class="string">&quot;&lt;a href=&apos;?search=</span><span class="default">$search_url</span><span class="string">&apos;&gt;Search again.&lt;/a&gt;&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>If you need to set a default input value and filter that, use filter_var on your required input variable instead</span>
</div>
  

#


<div class="phpcode"><span class="html">
To use a class method for a callback function, as usual, provide an array with an instance of the class and the method name.<br>Example:<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">myValidator<br></span><span class="keyword">{<br>&#xA0; public function </span><span class="default">username</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">)<br>&#xA0; {<br>&#xA0; &#xA0; </span><span class="comment">// return username or boolean false<br>&#xA0; </span><span class="keyword">}<br>}<br><br></span><span class="default">$myValidator </span><span class="keyword">= new </span><span class="default">myValidator</span><span class="keyword">;<br></span><span class="default">$options </span><span class="keyword">= array(</span><span class="string">&apos;options&apos; </span><span class="keyword">=&gt; array(</span><span class="default">$myValidator</span><span class="keyword">, </span><span class="string">&apos;username&apos;</span><span class="keyword">));<br></span><span class="default">$username </span><span class="keyword">= </span><span class="default">filter_input</span><span class="keyword">(</span><span class="default">INPUT_GET</span><span class="keyword">, </span><span class="string">&apos;username&apos;</span><span class="keyword">, </span><span class="default">FILTER_CALLBACK</span><span class="keyword">, </span><span class="default">$options</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$username</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here is an example how to work with the options-parameter. Notice the &apos;options&apos; in the &apos;options&apos;-Parameter!<br><br><span class="default">&lt;?php<br>$options</span><span class="keyword">=array(</span><span class="string">&apos;options&apos;</span><span class="keyword">=&gt;array(</span><span class="string">&apos;default&apos;</span><span class="keyword">=&gt;</span><span class="default">5</span><span class="keyword">, </span><span class="string">&apos;min_range&apos;</span><span class="keyword">=&gt;</span><span class="default">0</span><span class="keyword">, </span><span class="string">&apos;max_range&apos;</span><span class="keyword">=&gt;</span><span class="default">9</span><span class="keyword">));<br><br></span><span class="default">$priority</span><span class="keyword">=</span><span class="default">filter_input</span><span class="keyword">(</span><span class="default">INPUT_GET</span><span class="keyword">, </span><span class="string">&apos;priority&apos;</span><span class="keyword">, </span><span class="default">FILTER_VALIDATE_INT</span><span class="keyword">, </span><span class="default">$options</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>$priority will be 5 if the priority-Parameter isn&apos;t set or out the given range.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.filter-input.php)

**[To root](/README.md)**