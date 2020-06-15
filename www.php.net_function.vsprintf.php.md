# vsprintf




<div class="phpcode"><span class="html">
Instead of inventing own functions in case you&apos;d like to use array keys as placeholder names and replace corresponding array values in a string, just use the str_replace:<br><br>$string = &apos;Hello %name!&apos;;<br>$data = array(<br>&#xA0; &apos;%name&apos; =&gt; &apos;John&apos;<br>);<br><br>$greeting = str_replace(array_keys($data), array_values($data), $string);</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php
<br></span><span class="comment">/**
<br> * Like vsprintf, but accepts $args keys instead of order index.
<br> * Both numeric and strings matching /[a-zA-Z0-9_-]+/ are allowed.
<br> *
<br> * Example: vskprintf(&apos;y = %y$d, x = %x$1.1f&apos;, array(&apos;x&apos; =&gt; 1, &apos;y&apos; =&gt; 2))
<br> * Result:&#xA0; &apos;y = 2, x = 1.0&apos;
<br> *
<br> * $args also can be object, then it&apos;s properties are retrieved
<br> * using get_object_vars().
<br> *
<br> * &apos;%s&apos; without argument name works fine too. Everything vsprintf() can do
<br> * is supported.
<br> *
<br> * @author Josef Kufner &lt;jkufner(at)gmail.com&gt;
<br> */
<br></span><span class="keyword">function </span><span class="default">vksprintf</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">, </span><span class="default">$args</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; if (</span><span class="default">is_object</span><span class="keyword">(</span><span class="default">$args</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$args </span><span class="keyword">= </span><span class="default">get_object_vars</span><span class="keyword">(</span><span class="default">$args</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; </span><span class="default">$map </span><span class="keyword">= </span><span class="default">array_flip</span><span class="keyword">(</span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$args</span><span class="keyword">));
<br>&#xA0; &#xA0; </span><span class="default">$new_str </span><span class="keyword">= </span><span class="default">preg_replace_callback</span><span class="keyword">(</span><span class="string">&apos;/(^|[^%])%([a-zA-Z0-9_-]+)\$/&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; function(</span><span class="default">$m</span><span class="keyword">) use (</span><span class="default">$map</span><span class="keyword">) { return </span><span class="default">$m</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">].</span><span class="string">&apos;%&apos;</span><span class="keyword">.(</span><span class="default">$map</span><span class="keyword">[</span><span class="default">$m</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">]] + </span><span class="default">1</span><span class="keyword">).</span><span class="string">&apos;$&apos;</span><span class="keyword">; },
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$str</span><span class="keyword">);
<br>&#xA0; &#xA0; return </span><span class="default">vsprintf</span><span class="keyword">(</span><span class="default">$new_str</span><span class="keyword">, </span><span class="default">$args</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.vsprintf.php)

**[To root](/README.md)**