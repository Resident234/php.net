# session_register




<div class="phpcode"><span class="html">
Below is a fix that may be included in older code to make it work with PHP6. 
<br>When needed it recreates the functions
<br>- session_register()
<br>- session_is_registered()
<br>- session_unregister()
<br>
<br>The functions inside the function fix_session_register() are only available&#xA0; after fix_session_register() has run.
<br>Therefore in PHP&lt;6 where there already is a session_register() nothing happens.
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">// Fix for removed Session functions
<br></span><span class="keyword">function </span><span class="default">fix_session_register</span><span class="keyword">(){
<br>&#xA0; &#xA0; function </span><span class="default">session_register</span><span class="keyword">(){
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$args </span><span class="keyword">= </span><span class="default">func_get_args</span><span class="keyword">();
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$args </span><span class="keyword">as </span><span class="default">$key</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">]=</span><span class="default">$GLOBALS</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">];
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; function </span><span class="default">session_is_registered</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; return isset(</span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">]);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; function </span><span class="default">session_unregister</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; unset(</span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">]);
<br>&#xA0; &#xA0; }
<br>}
<br>if (!</span><span class="default">function_exists</span><span class="keyword">(</span><span class="string">&apos;session_register&apos;</span><span class="keyword">)) </span><span class="default">fix_session_register</span><span class="keyword">();
<br></span><span class="default">?&gt;
<br></span>
<br>
<br>[EDIT BY danbrown AT php DOT net: Bugfix provided by &quot;dr3w&quot; on 02-APR-2010: &quot;its [sic] function_exists with an S at the end&quot;.]</span>
</div>
  

#


<div class="phpcode"><span class="html">
if you remove session_register() calls and replace with $_SESSION assignments, make sure that it wasn&apos;t being used in place of session_start(). If it was, you&apos;ll need to add a call to session_start() too, before you assign to $_SESSION.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.session-register.php)

**[⬆ to root](/)**