# class_uses




<div class="phpcode"><span class="html">
To get ALL traits including those used by parent classes and other traits, use this function:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">class_uses_deep</span><span class="keyword">(</span><span class="default">$class</span><span class="keyword">, </span><span class="default">$autoload </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$traits </span><span class="keyword">= [];
<br>&#xA0; &#xA0; do {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$traits </span><span class="keyword">= </span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">class_uses</span><span class="keyword">(</span><span class="default">$class</span><span class="keyword">, </span><span class="default">$autoload</span><span class="keyword">), </span><span class="default">$traits</span><span class="keyword">);
<br>&#xA0; &#xA0; } while(</span><span class="default">$class </span><span class="keyword">= </span><span class="default">get_parent_class</span><span class="keyword">(</span><span class="default">$class</span><span class="keyword">));
<br>&#xA0; &#xA0; foreach (</span><span class="default">$traits </span><span class="keyword">as </span><span class="default">$trait </span><span class="keyword">=&gt; </span><span class="default">$same</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$traits </span><span class="keyword">= </span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">class_uses</span><span class="keyword">(</span><span class="default">$trait</span><span class="keyword">, </span><span class="default">$autoload</span><span class="keyword">), </span><span class="default">$traits</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; return </span><span class="default">array_unique</span><span class="keyword">(</span><span class="default">$traits</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
A slighly modified version from Stealz that also checks all the &quot;parent&quot; traits used by the traits:<br><br><span class="default">&lt;?php<br></span><span class="keyword">public static function </span><span class="default">class_uses_deep</span><span class="keyword">(</span><span class="default">$class</span><span class="keyword">, </span><span class="default">$autoload </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$traits </span><span class="keyword">= [];<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Get traits of all parent classes<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">do {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$traits </span><span class="keyword">= </span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">class_uses</span><span class="keyword">(</span><span class="default">$class</span><span class="keyword">, </span><span class="default">$autoload</span><span class="keyword">), </span><span class="default">$traits</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; } while (</span><span class="default">$class </span><span class="keyword">= </span><span class="default">get_parent_class</span><span class="keyword">(</span><span class="default">$class</span><span class="keyword">));<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Get traits of all parent traits<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$traitsToSearch </span><span class="keyword">= </span><span class="default">$traits</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; while (!empty(</span><span class="default">$traitsToSearch</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$newTraits </span><span class="keyword">= </span><span class="default">class_uses</span><span class="keyword">(</span><span class="default">array_pop</span><span class="keyword">(</span><span class="default">$traitsToSearch</span><span class="keyword">), </span><span class="default">$autoload</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$traits </span><span class="keyword">= </span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">$newTraits</span><span class="keyword">, </span><span class="default">$traits</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$traitsToSearch </span><span class="keyword">= </span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">$newTraits</span><span class="keyword">, </span><span class="default">$traitsToSearch</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; };<br><br>&#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$traits </span><span class="keyword">as </span><span class="default">$trait </span><span class="keyword">=&gt; </span><span class="default">$same</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$traits </span><span class="keyword">= </span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">class_uses</span><span class="keyword">(</span><span class="default">$trait</span><span class="keyword">, </span><span class="default">$autoload</span><span class="keyword">), </span><span class="default">$traits</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">array_unique</span><span class="keyword">(</span><span class="default">$traits</span><span class="keyword">);<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.class-uses.php)

**[⬆ to root](/)**