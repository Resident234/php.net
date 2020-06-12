# SoapVar::SoapVar




<div class="phpcode"><span class="html">
I spent hours trying to find how to send a request where an element is repeated. Here is how I managed to do it:<br><span class="default">&lt;?php<br>$parm </span><span class="keyword">= array();<br></span><span class="default">$parm</span><span class="keyword">[] = new </span><span class="default">SoapVar</span><span class="keyword">(</span><span class="string">&apos;123&apos;</span><span class="keyword">, </span><span class="default">XSD_STRING</span><span class="keyword">, </span><span class="default">null</span><span class="keyword">, </span><span class="default">null</span><span class="keyword">, </span><span class="string">&apos;customerNo&apos; </span><span class="keyword">);<br></span><span class="default">$parm</span><span class="keyword">[] = new </span><span class="default">SoapVar</span><span class="keyword">(</span><span class="string">&apos;THIS&apos;</span><span class="keyword">, </span><span class="default">XSD_STRING</span><span class="keyword">, </span><span class="default">null</span><span class="keyword">, </span><span class="default">null</span><span class="keyword">, </span><span class="string">&apos;selection&apos; </span><span class="keyword">);<br></span><span class="default">$parm</span><span class="keyword">[] = new </span><span class="default">SoapVar</span><span class="keyword">(</span><span class="string">&apos;THAT&apos;</span><span class="keyword">, </span><span class="default">XSD_STRING</span><span class="keyword">, </span><span class="default">null</span><span class="keyword">, </span><span class="default">null</span><span class="keyword">, </span><span class="string">&apos;selection&apos; </span><span class="keyword">);<br></span><span class="default">$resp </span><span class="keyword">= </span><span class="default">$client</span><span class="keyword">-&gt;</span><span class="default">getStuff</span><span class="keyword">( new </span><span class="default">SoapVar</span><span class="keyword">(</span><span class="default">$parm</span><span class="keyword">, </span><span class="default">SOAP_ENC_OBJECT</span><span class="keyword">) );<br></span><span class="default">?&gt;<br></span><br>This will send something like:<br>&lt;getStuff&gt;<br>&#xA0; &lt;customerNo&gt;123&lt;/customerNo&gt;<br>&#xA0; &lt;selection&gt;THIS&lt;/selection&gt;<br>&#xA0; &lt;selection&gt;THAT&lt;/selection&gt;<br>&lt;/getStuff&gt;<br><br>Hope this will save someone else&apos;s time.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I worked for hours to figure out how to create following structure:<br><br>&lt;ns:Foo attr=&apos;bar&apos;&gt;<br>&#xA0;&#xA0; &lt;ns:Baz attr=&apos;foobar&apos;&gt;barbaz&lt;/ns:Baz&gt;<br>&lt;/ns:Foo&gt;<br><br>It can be done with following array structure:<br><br>$arr = array(<br>&#xA0; &apos;Foo&apos; =&gt; array(<br>&#xA0; &#xA0;&#xA0; &apos;attr&apos;=&gt;&apos;bar&apos;,<br>&#xA0; &#xA0;&#xA0; &apos;Baz&apos;=&gt;array( &apos;_&apos; =&gt; &apos;barbaz&apos;, &apos;attr&apos;=&gt;&apos;foobar&apos;)<br>&#xA0; )<br>);</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/soapvar.soapvar.php)

**[⬆ to root](/)**