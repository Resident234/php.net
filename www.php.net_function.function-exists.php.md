# function_exists




<div class="phpcode"><span class="html">
You can use this function to conditionally define functions, see: <a href="http://php.net/manual/en/functions.user-defined.php" rel="nofollow" target="_blank">http://php.net/manual/en/functions.user-defined.php</a>
<br>
<br>For instance Wordpress uses it to make functions &quot;pluggable.&quot; If a plugin has already defined a pluggable function, then the WP code knows not to try to redefine it.
<br>
<br>But function_exists() will always return true unless you wrap any later function definition in a conditional clause, like if(){...}. This is a subtle matter in PHP parsing. Some examples:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">if (</span><span class="default">function_exists</span><span class="keyword">(</span><span class="string">&apos;foo&apos;</span><span class="keyword">)) {
<br>&#xA0; print </span><span class="string">&quot;foo defined\\n&quot;</span><span class="keyword">;
<br>} else {
<br>&#xA0; print </span><span class="string">&quot;foo not defined\\n&quot;</span><span class="keyword">;
<br>}
<br>function </span><span class="default">foo</span><span class="keyword">() {}
<br>
<br>if (</span><span class="default">function_exists</span><span class="keyword">(</span><span class="string">&apos;bar&apos;</span><span class="keyword">)) {
<br>&#xA0; print </span><span class="string">&quot;bar defined\\n&quot;</span><span class="keyword">;
<br>} else {
<br>&#xA0; print </span><span class="string">&quot;defining bar\\n&quot;</span><span class="keyword">;
<br>&#xA0; function </span><span class="default">bar</span><span class="keyword">() {}
<br>}
<br>print </span><span class="string">&quot;calling bar\\n&quot;</span><span class="keyword">;
<br></span><span class="default">bar</span><span class="keyword">(); </span><span class="comment">// ok to call function conditionally defined earlier
<br>
<br></span><span class="keyword">print </span><span class="string">&quot;calling baz\\n&quot;</span><span class="keyword">;
<br></span><span class="default">baz</span><span class="keyword">(); </span><span class="comment">// ok to call function unconditionally defined later
<br></span><span class="keyword">function </span><span class="default">baz</span><span class="keyword">() {}
<br>
<br></span><span class="default">qux</span><span class="keyword">(); </span><span class="comment">// NOT ok to call function conditionally defined later
<br></span><span class="keyword">if (!</span><span class="default">function_exists</span><span class="keyword">(</span><span class="string">&apos;qux&apos;</span><span class="keyword">)) {
<br>&#xA0; function </span><span class="default">qux</span><span class="keyword">() {}
<br>}
<br></span><span class="default">?&gt;
<br></span>Prints:
<br>&#xA0; foo defined
<br>&#xA0; defining bar
<br>&#xA0; calling bar
<br>&#xA0; calling baz
<br>&#xA0; PHP Fatal error: Call to undefined function qux()
<br>
<br>Any oddities are probably due to the order in which you include/require files.</span>
</div>
  

#


<div class="phpcode"><span class="html">
It should be noted that the function_exists check is not relative to the root namespace. This means that the namespace should be appended to the check:<br><br><span class="default">&lt;?php<br><br>&#xA0; </span><span class="keyword">namespace </span><span class="default">test</span><span class="keyword">;<br><br>&#xA0; if (!</span><span class="default">function_exists</span><span class="keyword">(</span><span class="default">__NAMESPACE__ </span><span class="keyword">. </span><span class="string">&apos;\example&apos;</span><span class="keyword">))<br>&#xA0; {<br><br>&#xA0; &#xA0; function </span><span class="default">example</span><span class="keyword">()<br>&#xA0; &#xA0; {<br><br>&#xA0; &#xA0; }<br><br>&#xA0; }<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.function-exists.php)

**[⬆ to root](/)**