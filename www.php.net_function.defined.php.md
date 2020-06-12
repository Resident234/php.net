# defined




<div class="phpcode"><span class="html">
My preferred way of checking if a constant is set, and if it isn&apos;t - setting it (could be used to set defaults in a file, where the user has already had the opportunity to set their own values in another.)<br><br><span class="default">&lt;?php<br><br>defined</span><span class="keyword">(</span><span class="string">&apos;CONSTANT&apos;</span><span class="keyword">) or </span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;CONSTANT&apos;</span><span class="keyword">, </span><span class="string">&apos;SomeDefaultValue&apos;</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>Dan.</span>
</div>
  

#


<div class="phpcode"><span class="html">
You can use the late static command &quot;static::&quot; withing defined as well. This example outputs - as expected - &quot;int (2)&quot;
<br>
<br><span class="default">&lt;?php
<br>&#xA0; </span><span class="keyword">abstract class </span><span class="default">class1
<br>&#xA0; </span><span class="keyword">{
<br>&#xA0; &#xA0; public function </span><span class="default">getConst</span><span class="keyword">()
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; return </span><span class="default">defined</span><span class="keyword">(</span><span class="string">&apos;static::SOME_CONST&apos;</span><span class="keyword">) ? static::</span><span class="default">SOME_CONST </span><span class="keyword">: </span><span class="default">false</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; }
<br>&#xA0; 
<br>&#xA0; final class </span><span class="default">class2 </span><span class="keyword">extends </span><span class="default">class1
<br>&#xA0; </span><span class="keyword">{
<br>&#xA0; &#xA0; const </span><span class="default">SOME_CONST </span><span class="keyword">= </span><span class="default">2</span><span class="keyword">;
<br>&#xA0; }
<br>&#xA0; 
<br>&#xA0; </span><span class="default">$class2 </span><span class="keyword">= new </span><span class="default">class2</span><span class="keyword">;
<br>&#xA0; 
<br>&#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$class2</span><span class="keyword">-&gt;</span><span class="default">getConst</span><span class="keyword">());
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Before using defined() have a look at the following benchmarks:<br><br>true&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 0.65ms<br>$true&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 0.69ms (1)<br>$config[&apos;true&apos;]&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 0.87ms<br>TRUE_CONST&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 1.28ms (2)<br>true&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 0.65ms<br>defined(&apos;TRUE_CONST&apos;)&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 2.06ms (3)<br>defined(&apos;UNDEF_CONST&apos;)&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 12.34ms (4)<br>isset($config[&apos;def_key&apos;])&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 0.91ms (5)<br>isset($config[&apos;undef_key&apos;])&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 0.79ms<br>isset($empty_hash[$good_key])&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 0.78ms<br>isset($small_hash[$good_key])&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 0.86ms<br>isset($big_hash[$good_key])&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 0.89ms<br>isset($small_hash[$bad_key])&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 0.78ms<br>isset($big_hash[$bad_key])&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 0.80ms<br><br>PHP Version 5.2.6, Apache 2.0, Windows XP<br><br>Each statement was executed 1000 times and while a 12ms overhead on 1000 calls isn&apos;t going to have the end users tearing their hair out, it does throw up some interesting results when comparing to if(true):<br><br>1) if($true) was virtually identical<br>2) if(TRUE_CONST) was almost twice as slow - I guess that the substitution isn&apos;t done at compile time (I had to double check this one!)<br>3) defined() is 3 times slower if the constant exists<br>4) defined() is 19 TIMES SLOWER if the constant doesn&apos;t exist!<br>5) isset() is remarkably efficient regardless of what you throw at it (great news for anyone implementing array driven event systems - me!)<br><br>May want to avoid if(defined(&apos;DEBUG&apos;))...</span>
</div>
  

#


<div class="phpcode"><span class="html">
if you want to check id a class constant is defined use self:: before the constant name:<br><br><span class="default">&lt;?php<br>defined</span><span class="keyword">(</span><span class="string">&apos;self::CONSTANT_NAME&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I saw that PHP doesn&apos;t have an enum function so I created my own. It&apos;s not necessary, but can come in handy from time to time.<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">enum</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$args </span><span class="keyword">= </span><span class="default">func_get_args</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$args </span><span class="keyword">as </span><span class="default">$key</span><span class="keyword">=&gt;</span><span class="default">$arg</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">defined</span><span class="keyword">(</span><span class="default">$arg</span><span class="keyword">))<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; die(</span><span class="string">&apos;Redefinition of defined constant &apos; </span><span class="keyword">. </span><span class="default">$arg</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">define</span><span class="keyword">(</span><span class="default">$arg</span><span class="keyword">, </span><span class="default">$key</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">enum</span><span class="keyword">(</span><span class="string">&apos;ONE&apos;</span><span class="keyword">,</span><span class="string">&apos;TWO&apos;</span><span class="keyword">,</span><span class="string">&apos;THREE&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; echo </span><span class="default">ONE</span><span class="keyword">, </span><span class="string">&apos; &apos;</span><span class="keyword">, </span><span class="default">TWO</span><span class="keyword">, </span><span class="string">&apos; &apos;</span><span class="keyword">, </span><span class="default">THREE</span><span class="keyword">;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
This function, along with constant(), is namespace sensitive. And it might help if you imagine them always running under the &quot;root namespace&quot;:<br><br><span class="default">&lt;?php<br></span><span class="keyword">namespace </span><span class="default">FOO</span><span class="keyword">\</span><span class="default">BAR<br></span><span class="keyword">{<br>&#xA0; &#xA0; const </span><span class="default">WMP</span><span class="keyword">=</span><span class="string">&quot;wmp&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; function </span><span class="default">test</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">defined</span><span class="keyword">(</span><span class="string">&quot;WMP&quot;</span><span class="keyword">)) echo </span><span class="string">&quot;direct: &quot;</span><span class="keyword">.</span><span class="default">constant</span><span class="keyword">(</span><span class="string">&quot;WMP&quot;</span><span class="keyword">); </span><span class="comment">//doesn&apos;t work;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">elseif(</span><span class="default">defined</span><span class="keyword">(</span><span class="string">&quot;FOO\\BAR\\WMP&quot;</span><span class="keyword">)) echo </span><span class="string">&quot;namespace: &quot;</span><span class="keyword">.</span><span class="default">constant</span><span class="keyword">(</span><span class="string">&quot;FOO\\BAR\\WMP&quot;</span><span class="keyword">); </span><span class="comment">//works<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">echo </span><span class="default">WMP</span><span class="keyword">; </span><span class="comment">//works<br>&#xA0; &#xA0; </span><span class="keyword">}<br>}<br>namespace<br>{<br>&#xA0; &#xA0; \</span><span class="default">FOO</span><span class="keyword">\</span><span class="default">BAR</span><span class="keyword">\</span><span class="default">test</span><span class="keyword">();<br>}</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you wish to protect files from direct access I normally use this:<br><br>index.php:<br><br><span class="default">&lt;?php<br></span><span class="comment">// Main stuff here<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;START&apos;</span><span class="keyword">,</span><span class="default">microtime</span><span class="keyword">());<br><br>include </span><span class="string">&quot;x.php&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>x.php:<br><br><span class="default">&lt;?php<br>defined</span><span class="keyword">(</span><span class="string">&apos;START&apos;</span><span class="keyword">)||(</span><span class="default">header</span><span class="keyword">(</span><span class="string">&quot;HTTP/1.1 403 Forbidden&quot;</span><span class="keyword">)&amp;die(</span><span class="string">&apos;403.14 - Directory listing denied.&apos;</span><span class="keyword">));<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.defined.php)

**[⬆ to root](/)**