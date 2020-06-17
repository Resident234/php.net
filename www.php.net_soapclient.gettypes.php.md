# SoapClient::__getTypes




<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br></span><span class="comment">// to see formated types<br><br></span><span class="default">$soap </span><span class="keyword">= new </span><span class="default">SoapClient</span><span class="keyword">(</span><span class="string">&apos;<a href="http://domain.com/ws.php?WSDL" rel="nofollow" target="_blank">http://domain.com/ws.php?WSDL</a>&apos;</span><span class="keyword">);<br><br>echo </span><span class="string">&apos;&lt;pre&gt;&apos;</span><span class="keyword">;<br>echo </span><span class="string">&apos;&lt;h2&gt;Types:&lt;/h2&gt;&apos;</span><span class="keyword">;<br></span><span class="default">$types </span><span class="keyword">= </span><span class="default">$soap</span><span class="keyword">-&gt;</span><span class="default">__getTypes</span><span class="keyword">();<br>foreach (</span><span class="default">$types </span><span class="keyword">as </span><span class="default">$type</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$type </span><span class="keyword">= </span><span class="default">preg_replace</span><span class="keyword">(<br>&#xA0; &#xA0; &#xA0; &#xA0; array(</span><span class="string">&apos;/(\w+) ([a-zA-Z0-9]+)/&apos;</span><span class="keyword">, </span><span class="string">&apos;/\n /&apos;</span><span class="keyword">),<br>&#xA0; &#xA0; &#xA0; &#xA0; array(</span><span class="string">&apos;&lt;font color=&quot;green&quot;&gt;${1}&lt;/font&gt; &lt;font color=&quot;blue&quot;&gt;${2}&lt;/font&gt;&apos;</span><span class="keyword">, </span><span class="string">&quot;\n\t&quot;</span><span class="keyword">),<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$type<br>&#xA0; &#xA0; </span><span class="keyword">);<br>&#xA0; &#xA0; echo </span><span class="default">$type</span><span class="keyword">;<br>&#xA0; &#xA0; echo </span><span class="string">&quot;\n\n&quot;</span><span class="keyword">;<br>}<br>echo </span><span class="string">&apos;&lt;/pre&gt;&apos;</span><span class="keyword">;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/soapclient.gettypes.php)

**[To root](/README.md)**