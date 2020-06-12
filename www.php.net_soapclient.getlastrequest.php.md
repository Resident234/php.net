# SoapClient::__getLastRequest




<div class="phpcode"><span class="html">
Adding htmlentities() can be helpful since it makes the XML visible in your browser without needing to view the source.<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">echo </span><span class="string">&quot;REQUEST:\n&quot; </span><span class="keyword">. </span><span class="default">htmlentities</span><span class="keyword">(</span><span class="default">$client</span><span class="keyword">-&gt;</span><span class="default">__getLastRequest</span><span class="keyword">()) . </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that when you create SoapClient with option &quot;trace&quot; set to FALSE or omit it, than &quot;__getLastRequest()&quot; always returns NULL.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I guess many peoples calls getLastRequest and it returns nothing. &quot;Heey where is the my last request&quot;. Now we will see our request,&#xA0; when you created a SoapClient instance, you should give a option parameter as below :
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">// below $option=array(&apos;trace&apos;,1);
<br>// correct one is below
<br></span><span class="default">$option</span><span class="keyword">=array(</span><span class="string">&apos;trace&apos;</span><span class="keyword">=&gt;</span><span class="default">1</span><span class="keyword">);
<br>
<br></span><span class="default">$client</span><span class="keyword">=new </span><span class="default">SoapClient</span><span class="keyword">(</span><span class="string">&apos;some.wsdl&apos;</span><span class="keyword">,</span><span class="default">$option</span><span class="keyword">);
<br>
<br>try{
<br>&#xA0; </span><span class="default">$client</span><span class="keyword">-&gt;</span><span class="default">aMethodAtRemote</span><span class="keyword">();
<br>}catch(</span><span class="default">SoapFault $fault</span><span class="keyword">){
<br>&#xA0; </span><span class="comment">// &lt;xmp&gt; tag displays xml output in html
<br>&#xA0; </span><span class="keyword">echo </span><span class="string">&apos;Request : &lt;br/&gt;&lt;xmp&gt;&apos;</span><span class="keyword">,
<br>&#xA0; </span><span class="default">$client</span><span class="keyword">-&gt;</span><span class="default">__getLastRequest</span><span class="keyword">(),
<br>&#xA0; </span><span class="string">&apos;&lt;/xmp&gt;&lt;br/&gt;&lt;br/&gt; Error Message : &lt;br/&gt;&apos;</span><span class="keyword">,
<br>&#xA0; </span><span class="default">$fault</span><span class="keyword">-&gt;</span><span class="default">getMessage</span><span class="keyword">();
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>&quot;trace&quot; parameter enables output of request. Now, you should see SOAP request.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/soapclient.getlastrequest.php)

**[⬆ to root](/)**