# Late Static Bindings




<div class="phpcode"><span class="html">
Finally we can implement some ActiveRecord methods:
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">class </span><span class="default">Model
<br></span><span class="keyword">{
<br>&#xA0; &#xA0; public static function </span><span class="default">find</span><span class="keyword">()
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo static::</span><span class="default">$name</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>}
<br>
<br>class </span><span class="default">Product </span><span class="keyword">extends </span><span class="default">Model
<br></span><span class="keyword">{
<br>&#xA0; &#xA0; protected static </span><span class="default">$name </span><span class="keyword">= </span><span class="string">&apos;Product&apos;</span><span class="keyword">;
<br>}
<br>
<br></span><span class="default">Product</span><span class="keyword">::</span><span class="default">find</span><span class="keyword">();
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>Output: &apos;Product&apos;</span>
</div>
  

#


<div class="phpcode"><span class="html">
For abstract classes with static factory method, you can use the static keyword instead of self like the following:<br><span class="default">&lt;?php<br><br></span><span class="keyword">abstract class </span><span class="default">A</span><span class="keyword">{<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; static function </span><span class="default">create</span><span class="keyword">(){<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//return new self();&#xA0; //Fatal error: Cannot instantiate abstract class A<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return new static(); </span><span class="comment">//this is the correct way<br><br>&#xA0; &#xA0; </span><span class="keyword">}<br>&#xA0; &#xA0; <br>}<br><br>class </span><span class="default">B </span><span class="keyword">extends </span><span class="default">A</span><span class="keyword">{<br>}<br><br></span><span class="default">$obj</span><span class="keyword">=</span><span class="default">B</span><span class="keyword">::</span><span class="default">create</span><span class="keyword">();<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$obj</span><span class="keyword">);<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.late-static-bindings.php)

**[To root](/)**