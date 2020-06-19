# Classes and Objects




<div class="phpcode"><span class="html">
PHP 5 is very very flexible in accessing member variables and member functions. These access methods maybe look unusual and unnecessary at first glance; but they are very useful sometimes; specially when you work with SimpleXML classes and objects. I have posted a similar comment in SimpleXML function reference section, but this one is more comprehensive.
<br>
<br>I use the following class as reference for all examples:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">class </span><span class="default">Foo </span><span class="keyword">{
<br>&#xA0; &#xA0; public </span><span class="default">$aMemberVar </span><span class="keyword">= </span><span class="string">&apos;aMemberVar Member Variable&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; public </span><span class="default">$aFuncName </span><span class="keyword">= </span><span class="string">&apos;aMemberFunc&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; function </span><span class="default">aMemberFunc</span><span class="keyword">() {
<br>&#xA0; &#xA0; &#xA0; &#xA0; print </span><span class="string">&apos;Inside `aMemberFunc()`&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>}
<br>
<br></span><span class="default">$foo </span><span class="keyword">= new </span><span class="default">Foo</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span>
<br>You can access member variables in an object using another variable as name:
<br>
<br><span class="default">&lt;?php
<br>$element </span><span class="keyword">= </span><span class="string">&apos;aMemberVar&apos;</span><span class="keyword">;
<br>print </span><span class="default">$foo</span><span class="keyword">-&gt;</span><span class="default">$element</span><span class="keyword">; </span><span class="comment">// prints &quot;aMemberVar Member Variable&quot;
<br></span><span class="default">?&gt;
<br></span>
<br>or use functions:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">getVarName</span><span class="keyword">()
<br>{ return </span><span class="string">&apos;aMemberVar&apos;</span><span class="keyword">; }
<br>
<br>print </span><span class="default">$foo</span><span class="keyword">-&gt;{</span><span class="default">getVarName</span><span class="keyword">()}; </span><span class="comment">// prints &quot;aMemberVar Member Variable&quot;
<br></span><span class="default">?&gt;
<br></span>
<br>Important Note: You must surround function name with { and } or PHP would think you are calling a member function of object &quot;foo&quot;.
<br>
<br>you can use a constant or literal as well:
<br>
<br><span class="default">&lt;?php
<br>define</span><span class="keyword">(</span><span class="default">MY_CONSTANT</span><span class="keyword">, </span><span class="string">&apos;aMemberVar&apos;</span><span class="keyword">);
<br>print </span><span class="default">$foo</span><span class="keyword">-&gt;{</span><span class="default">MY_CONSTANT</span><span class="keyword">}; </span><span class="comment">// Prints &quot;aMemberVar Member Variable&quot;
<br></span><span class="keyword">print </span><span class="default">$foo</span><span class="keyword">-&gt;{</span><span class="string">&apos;aMemberVar&apos;</span><span class="keyword">}; </span><span class="comment">// Prints &quot;aMemberVar Member Variable&quot;
<br></span><span class="default">?&gt;
<br></span>
<br>You can use members of other objects as well:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">print </span><span class="default">$foo</span><span class="keyword">-&gt;{</span><span class="default">$otherObj</span><span class="keyword">-&gt;</span><span class="default">var</span><span class="keyword">};
<br>print </span><span class="default">$foo</span><span class="keyword">-&gt;{</span><span class="default">$otherObj</span><span class="keyword">-&gt;</span><span class="default">func</span><span class="keyword">()};
<br></span><span class="default">?&gt;
<br></span>
<br>You can use mathods above to access member functions as well:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">print </span><span class="default">$foo</span><span class="keyword">-&gt;{</span><span class="string">&apos;aMemberFunc&apos;</span><span class="keyword">}(); </span><span class="comment">// Prints &quot;Inside `aMemberFunc()`&quot;
<br></span><span class="keyword">print </span><span class="default">$foo</span><span class="keyword">-&gt;{</span><span class="default">$foo</span><span class="keyword">-&gt;</span><span class="default">aFuncName</span><span class="keyword">}(); </span><span class="comment">// Prints &quot;Inside `aMemberFunc()`&quot;
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.php)

**[To root](/README.md)**