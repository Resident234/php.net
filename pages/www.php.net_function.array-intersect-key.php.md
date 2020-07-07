# array_intersect_key




<div class="phpcode"><span class="html">
Simple key white-list filter:
<br>
<br><span class="default">&lt;?php
<br>$arr </span><span class="keyword">= array(</span><span class="string">&apos;a&apos; </span><span class="keyword">=&gt; </span><span class="default">123</span><span class="keyword">, </span><span class="string">&apos;b&apos; </span><span class="keyword">=&gt; </span><span class="default">213</span><span class="keyword">, </span><span class="string">&apos;c&apos; </span><span class="keyword">=&gt; </span><span class="default">321</span><span class="keyword">);
<br></span><span class="default">$allowed </span><span class="keyword">= array(</span><span class="string">&apos;b&apos;</span><span class="keyword">, </span><span class="string">&apos;c&apos;</span><span class="keyword">);
<br>
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">array_intersect_key</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">, </span><span class="default">array_flip</span><span class="keyword">(</span><span class="default">$allowed</span><span class="keyword">)));
<br></span><span class="default">?&gt;
<br></span>
<br>Will return:
<br>Array
<br>(
<br>&#xA0; &#xA0; [b] =&gt; 213
<br>&#xA0; &#xA0; [c] =&gt; 321
<br>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
[Editor&apos;s note: changed array_merge_recursive() to array_replace_recursive() to fix the script]
<br>
<br>Here is a better way to merge settings using some defaults as a whitelist.
<br>
<br><span class="default">&lt;?php
<br>
<br>$defaults </span><span class="keyword">= [
<br>&#xA0; &#xA0; </span><span class="string">&apos;id&apos;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">=&gt; </span><span class="default">123456</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&apos;client_id&apos;&#xA0; &#xA0;&#xA0; </span><span class="keyword">=&gt; </span><span class="default">null</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&apos;client_secret&apos; </span><span class="keyword">=&gt; </span><span class="default">null</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&apos;options&apos;&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">=&gt; [
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;trusted&apos; </span><span class="keyword">=&gt; </span><span class="default">false</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;active&apos;&#xA0; </span><span class="keyword">=&gt; </span><span class="default">false
<br>&#xA0; &#xA0; </span><span class="keyword">]
<br>];
<br>
<br></span><span class="default">$options </span><span class="keyword">= [
<br>&#xA0; &#xA0; </span><span class="string">&apos;client_id&apos;&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">=&gt; </span><span class="default">789</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&apos;client_secret&apos;&#xA0;&#xA0; </span><span class="keyword">=&gt; </span><span class="string">&apos;5ebe2294ecd0e0f08eab7690d2a6ee69&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&apos;client_password&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;5f4dcc3b5aa765d61d8327deb882cf99&apos;</span><span class="keyword">, </span><span class="comment">// ignored
<br>&#xA0; &#xA0; </span><span class="string">&apos;client_name&apos;&#xA0; &#xA0;&#xA0; </span><span class="keyword">=&gt; </span><span class="string">&apos;IGNORED&apos;</span><span class="keyword">,&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// ignored
<br>&#xA0; &#xA0; </span><span class="string">&apos;options&apos;&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">=&gt; [
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;active&apos; </span><span class="keyword">=&gt; </span><span class="default">true
<br>&#xA0; &#xA0; </span><span class="keyword">]
<br>];
<br>
<br></span><span class="default">var_dump</span><span class="keyword">(
<br>&#xA0; &#xA0; </span><span class="default">array_replace_recursive</span><span class="keyword">(</span><span class="default">$defaults</span><span class="keyword">, 
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_intersect_key</span><span class="keyword">(
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$options</span><span class="keyword">, </span><span class="default">$defaults
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">)
<br>&#xA0; &#xA0; )
<br>);
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>Output:
<br>
<br>array (size=4)
<br>&#xA0; &#xA0; &apos;id&apos;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; =&gt; int 123456
<br>&#xA0; &#xA0; &apos;client_id&apos;&#xA0; &#xA0;&#xA0; =&gt; int 789
<br>&#xA0; &#xA0; &apos;client_secret&apos; =&gt; string &apos;5ebe2294ecd0e0f08eab7690d2a6ee69&apos; (length=32)
<br>&#xA0; &#xA0; &apos;options&apos;&#xA0; &#xA0; &#xA0;&#xA0; =&gt; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; array (size=2)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;trusted&apos; =&gt; boolean false
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;active&apos;&#xA0; =&gt; boolean true</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-intersect-key.php)

**[To root](/README.md)**