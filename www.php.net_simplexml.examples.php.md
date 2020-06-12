# Examples




<div class="phpcode"><span class="html">
When using simplexml to access a element the returned object may be a SimpleXMLElement instead of a string.
<br>
<br>Example:
<br>
<br><span class="default">&lt;?php
<br>$string </span><span class="keyword">= &lt;&lt;&lt;XML
<br></span><span class="string">&lt;?xml version=&apos;1.0&apos;?&gt;
<br>&lt;document&gt;
<br>&#xA0; &#xA0; &lt;cmd&gt;login&lt;/cmd&gt;
<br>&#xA0; &#xA0; &lt;login&gt;Richard&lt;/login&gt;
<br>&lt;/document&gt;
<br></span><span class="keyword">XML;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
<br></span><span class="default">$xml </span><span class="keyword">= </span><span class="default">simplexml_load_string</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">);
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$xml</span><span class="keyword">);
<br></span><span class="default">$login </span><span class="keyword">= </span><span class="default">$xml</span><span class="keyword">-&gt;</span><span class="default">login</span><span class="keyword">;
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$login</span><span class="keyword">);
<br></span><span class="default">$login </span><span class="keyword">= (string) </span><span class="default">$xml</span><span class="keyword">-&gt;</span><span class="default">login</span><span class="keyword">;
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$login</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>Expected result:
<br>----------------
<br>SimpleXMLElement Object
<br>(
<br>&#xA0; &#xA0; [cmd] =&gt; login
<br>&#xA0; &#xA0; [login] =&gt; Richard
<br>)
<br>Richard
<br>Richard
<br>
<br>Actual result:
<br>--------------
<br>SimpleXMLElement Object
<br>(
<br>&#xA0; &#xA0; [cmd] =&gt; login
<br>&#xA0; &#xA0; [login] =&gt; Richard
<br>)
<br>SimpleXMLElement Object
<br>(
<br>&#xA0; &#xA0; [0] =&gt; Richard
<br>)
<br>Richard
<br>
<br>But this is an intended behavior. See <a href="http://bugs.php.net/bug.php?id=29500" rel="nofollow" target="_blank">http://bugs.php.net/bug.php?id=29500</a></span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/simplexml.examples.php)

**[⬆ to root](/)**