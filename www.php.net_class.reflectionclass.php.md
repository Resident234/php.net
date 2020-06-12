# The ReflectionClass class




<div class="phpcode"><span class="html">
To reflect on a namespaced class in PHP 5.3, you must always specify the fully qualified name of the class - even if you&apos;ve aliased the containing namespace using a &quot;use&quot; statement.
<br>
<br>So instead of:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">use </span><span class="default">App</span><span class="keyword">\</span><span class="default">Core </span><span class="keyword">as </span><span class="default">Core</span><span class="keyword">;
<br></span><span class="default">$oReflectionClass </span><span class="keyword">= new </span><span class="default">ReflectionClass</span><span class="keyword">(</span><span class="string">&apos;Core\Singleton&apos;</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>You would type:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">use </span><span class="default">App</span><span class="keyword">\</span><span class="default">Core </span><span class="keyword">as </span><span class="default">Core</span><span class="keyword">;
<br></span><span class="default">$oReflectionClass </span><span class="keyword">= new </span><span class="default">ReflectionClass</span><span class="keyword">(</span><span class="string">&apos;App\Core\Singleton&apos;</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Reflecting an alias will give you a reflection of the resolved class.<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">X </span><span class="keyword">{<br>&#xA0; &#xA0; <br>}<br><br></span><span class="default">class_alias</span><span class="keyword">(</span><span class="string">&apos;X&apos;</span><span class="keyword">,</span><span class="string">&apos;Y&apos;</span><span class="keyword">);<br></span><span class="default">class_alias</span><span class="keyword">(</span><span class="string">&apos;Y&apos;</span><span class="keyword">,</span><span class="string">&apos;Z&apos;</span><span class="keyword">);<br></span><span class="default">$z </span><span class="keyword">= new </span><span class="default">ReflectionClass</span><span class="keyword">(</span><span class="string">&apos;Z&apos;</span><span class="keyword">);<br>echo </span><span class="default">$z</span><span class="keyword">-&gt;</span><span class="default">getName</span><span class="keyword">(); </span><span class="comment">// X<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Unserialized reflection class cause error.
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">/**
<br> * abc
<br> */
<br></span><span class="keyword">class </span><span class="default">a</span><span class="keyword">{}
<br>
<br></span><span class="default">$ref </span><span class="keyword">= new </span><span class="default">ReflectionClass</span><span class="keyword">(</span><span class="string">&apos;a&apos;</span><span class="keyword">);
<br></span><span class="default">$ref </span><span class="keyword">= </span><span class="default">unserialize</span><span class="keyword">(</span><span class="default">serialize</span><span class="keyword">(</span><span class="default">$ref</span><span class="keyword">));
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$ref</span><span class="keyword">);
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$ref</span><span class="keyword">-&gt;</span><span class="default">getDocComment</span><span class="keyword">());
<br>
<br></span><span class="comment">// object(ReflectionClass)#2 (1) {
<br>//&#xA0;&#xA0; [&quot;name&quot;]=&gt;
<br>//&#xA0;&#xA0; string(1) &quot;a&quot;
<br>// }
<br>// PHP Fatal error:&#xA0; ReflectionClass::getDocComment(): Internal error: Failed to retrieve the reflection object
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.reflectionclass.php)

**[⬆ to root](/)**