# filter_var




<div class="phpcode"><span class="html">
Pay attention that the function will not validate &quot;not latin&quot; domains.<br><br>if (filter_var(&apos;&#x443;&#x43D;&#x438;&#x43A;&#x443;&#x43C;@&#x438;&#x437;.&#x440;&#x444;&apos;, FILTER_VALIDATE_EMAIL)) { <br>&#xA0; &#xA0; echo &apos;VALID&apos;; <br>} else {<br>&#xA0; &#xA0; echo &apos;NOT VALID&apos;;<br>}</span>
</div>
  

#


<div class="phpcode"><span class="html">
I found some addresses that FILTER_VALIDATE_EMAIL rejects, but RFC5321 permits:<br><span class="default">&lt;?php<br></span><span class="keyword">foreach (array(<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;localpart.ending.with.dot.@example.com&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;(comment)localpart@example.com&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;&quot;this is v@lid!&quot;@example.com&apos;</span><span class="keyword">, <br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;&quot;much.more unusual&quot;@example.com&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;postbox@com&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;admin@mailserver1&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;&quot;()&lt;&gt;[]:,;@\\&quot;\\\\!#$%&amp;\&apos;*+-/=?^_`{}| ~.a&quot;@example.org&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;&quot; &quot;@example.org&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; ) as </span><span class="default">$address</span><span class="keyword">) {<br>&#xA0; &#xA0; echo </span><span class="string">&quot;&lt;p&gt;</span><span class="default">$address</span><span class="string"> is &lt;b&gt;&quot;</span><span class="keyword">.(</span><span class="default">filter_var</span><span class="keyword">(</span><span class="default">$address</span><span class="keyword">, </span><span class="default">FILTER_VALIDATE_EMAIL</span><span class="keyword">) ? </span><span class="string">&apos;&apos; </span><span class="keyword">: </span><span class="string">&apos;not&apos;</span><span class="keyword">).</span><span class="string">&quot; valid&lt;/b&gt;&lt;/p&gt;&quot;</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span>Results:<br><br>localpart.ending.with.dot.@example.com is not valid<br>(comment)localpart@example.com is not valid<br>&quot;this is v@lid!&quot;@example.com is not valid<br>&quot;much.more unusual&quot;@example.com is not valid<br>postbox@com is not valid<br>admin@mailserver1 is not valid<br>&quot;()&lt;&gt;[]:,;@\&quot;\\!#$%&amp;&apos;*+-/=?^_`{}| ~.a&quot;@example.org is not valid<br>&quot; &quot;@example.org is not valid<br><br>The documentation does not saying that FILTER_VALIDATE_EMAIL should pass the RFC5321, however you can meet with these examples (especially with the first one). So this is a note, not a bug report.</span>
</div>
  

#


<div class="phpcode"><span class="html">
And this is also a valid url <br><br><a href="http://example.com/" rel="nofollow" target="_blank">http://example.com/</a>&quot;&gt;&lt;script&gt;alert(document.cookie)&lt;/script&gt;</span>
</div>
  

#


<div class="phpcode"><span class="html">
note that FILTER_VALIDATE_BOOLEAN tries to be smart, recognizing words like Yes, No, Off, On, both string and native types of true and false, and is not case-sensitive when validating strings.<br><br><span class="default">&lt;?php<br>$vals</span><span class="keyword">=array(</span><span class="string">&apos;on&apos;</span><span class="keyword">,</span><span class="string">&apos;On&apos;</span><span class="keyword">,</span><span class="string">&apos;ON&apos;</span><span class="keyword">,</span><span class="string">&apos;off&apos;</span><span class="keyword">,</span><span class="string">&apos;Off&apos;</span><span class="keyword">,</span><span class="string">&apos;OFF&apos;</span><span class="keyword">,</span><span class="string">&apos;yes&apos;</span><span class="keyword">,</span><span class="string">&apos;Yes&apos;</span><span class="keyword">,</span><span class="string">&apos;YES&apos;</span><span class="keyword">,<br></span><span class="string">&apos;no&apos;</span><span class="keyword">,</span><span class="string">&apos;No&apos;</span><span class="keyword">,</span><span class="string">&apos;NO&apos;</span><span class="keyword">,</span><span class="default">0</span><span class="keyword">,</span><span class="default">1</span><span class="keyword">,</span><span class="string">&apos;0&apos;</span><span class="keyword">,</span><span class="string">&apos;1&apos;</span><span class="keyword">,</span><span class="string">&apos;true&apos;</span><span class="keyword">,<br></span><span class="string">&apos;True&apos;</span><span class="keyword">,</span><span class="string">&apos;TRUE&apos;</span><span class="keyword">,</span><span class="string">&apos;false&apos;</span><span class="keyword">,</span><span class="string">&apos;False&apos;</span><span class="keyword">,</span><span class="string">&apos;FALSE&apos;</span><span class="keyword">,</span><span class="default">true</span><span class="keyword">,</span><span class="default">false</span><span class="keyword">,</span><span class="string">&apos;foo&apos;</span><span class="keyword">,</span><span class="string">&apos;bar&apos;</span><span class="keyword">);<br>foreach(</span><span class="default">$vals </span><span class="keyword">as </span><span class="default">$val</span><span class="keyword">){<br>&#xA0; &#xA0; echo </span><span class="default">var_export</span><span class="keyword">(</span><span class="default">$val</span><span class="keyword">,</span><span class="default">true</span><span class="keyword">).</span><span class="string">&apos;: &apos;</span><span class="keyword">;&#xA0;&#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">filter_var</span><span class="keyword">(</span><span class="default">$val</span><span class="keyword">,</span><span class="default">FILTER_VALIDATE_BOOLEAN</span><span class="keyword">,</span><span class="default">FILTER_NULL_ON_FAILURE</span><span class="keyword">));<br>}<br></span><span class="default">?&gt;<br></span><br>outputs:<br>&apos;on&apos;: bool(true)<br>&apos;On&apos;: bool(true)<br>&apos;ON&apos;: bool(true)<br>&apos;off&apos;: bool(false)<br>&apos;Off&apos;: bool(false)<br>&apos;OFF&apos;: bool(false)<br>&apos;yes&apos;: bool(true)<br>&apos;Yes&apos;: bool(true)<br>&apos;YES&apos;: bool(true)<br>&apos;no&apos;: bool(false)<br>&apos;No&apos;: bool(false)<br>&apos;NO&apos;: bool(false)<br>0: bool(false)<br>1: bool(true)<br>&apos;0&apos;: bool(false)<br>&apos;1&apos;: bool(true)<br>&apos;true&apos;: bool(true)<br>&apos;True&apos;: bool(true)<br>&apos;TRUE&apos;: bool(true)<br>&apos;false&apos;: bool(false)<br>&apos;False&apos;: bool(false)<br>&apos;FALSE&apos;: bool(false)<br>true: bool(true)<br>false: bool(false)<br>&apos;foo&apos;: NULL<br>&apos;bar&apos;: NULL</span>
</div>
  

#


<div class="phpcode"><span class="html">
FILTER_VALIDATE_URL allows:<br><br>filter_var(&apos;javascript://comment%0Aalert(1)&apos;, FILTER_VALIDATE_URL);<br><br>Where the %0A (URL encoded newline), in certain contexts, will split the comment from the JS code.<br><br>This can result in an XSS vulnerability.</span>
</div>
  

#


<div class="phpcode"><span class="html">
please note FILTER_VALIDATE_URL passes following url
<br>
<br><a href="http://example.ee/sdsf" rel="nofollow" target="_blank">http://example.ee/sdsf</a>&quot;f</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.filter-var.php)

**[To root](/README.md)**