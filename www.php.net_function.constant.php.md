# constant




<div class="phpcode"><span class="html">
The constant name can be an empty string.<br><br>Code:<br><br>define(&quot;&quot;, &quot;foo&quot;);<br>echo constant(&quot;&quot;);<br><br>Output:<br><br>foo</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you are referencing class constant (either using namespaces or not, because one day you may want to start using them), you&apos;ll have the least headaches when doing it like this:<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">Foo </span><span class="keyword">{<br>&#xA0; &#xA0; const </span><span class="default">BAR </span><span class="keyword">= </span><span class="default">42</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br>&lt;?php<br></span><span class="keyword">namespace </span><span class="default">Baz</span><span class="keyword">;<br>use \</span><span class="default">Foo </span><span class="keyword">as </span><span class="default">F</span><span class="keyword">;<br><br>echo </span><span class="default">constant</span><span class="keyword">(</span><span class="default">F</span><span class="keyword">::class.</span><span class="string">&apos;::BAR&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>since F::class will be dereferenced to whatever namespace shortcuts you are using (and those are way easier to refactor for IDE than just plain strings with hardcoded namespaces in string literals)</span>
</div>
  

#


<div class="phpcode"><span class="html">
As of PHP 5.4.6 constant() pays no attention to any namespace aliases that might be defined in the file in which it&apos;s used. I.e. constant() always behaves as if it is called from the global namespace. This means that the following will not work:<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">Foo </span><span class="keyword">{<br>&#xA0; &#xA0; const </span><span class="default">BAR </span><span class="keyword">= </span><span class="default">42</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span><br><span class="default">&lt;?php<br></span><span class="keyword">namespace </span><span class="default">Baz</span><span class="keyword">;<br><br>use \</span><span class="default">Foo </span><span class="keyword">as </span><span class="default">F</span><span class="keyword">;<br><br>echo </span><span class="default">constant</span><span class="keyword">(</span><span class="string">&apos;F::BAR&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>However, calling constant(&apos;Foo::BAR&apos;) will work as expected.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.constant.php)

**[⬆ to root](/)**