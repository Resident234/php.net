# SoapClient::__getLastResponse




<div class="phpcode"><span class="html">
D&apos;oh!<br>That example needs:<br>$soapClient = new SoapClient($url, array(&apos;trace&apos;=&gt;1));<br>to turn ON tracing in the first place.</span>
</div>
  

#


<div class="phpcode"><span class="html">
You almost for sure will need to wrap a try/catch block around your SOAP call in order to use these to debug something that&apos;s not working.<br><br>Otherwise, PHP throws a fatal error before you can execute this function.<br><br>For example:<br><span class="default">&lt;?php<br>&#xA0; &#xA0; $soapClient </span><span class="keyword">= new </span><span class="default">SoapClient</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">);<br>&#xA0; &#xA0; echo </span><span class="default">htmlentities</span><span class="keyword">(</span><span class="default">$soapClient</span><span class="keyword">-&gt;</span><span class="default">__getFunctions</span><span class="keyword">());<br>&#xA0; &#xA0; </span><span class="comment">//Assume that has output &apos;someFunction&apos; (among others)<br>&#xA0; &#xA0; </span><span class="keyword">try {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$results </span><span class="keyword">= </span><span class="default">$soapClient</span><span class="keyword">-&gt;</span><span class="default">someFunction</span><span class="keyword">(...);<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; catch (</span><span class="default">SoapFault $soapFault</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$soapFault</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;Request :&lt;br&gt;&quot;</span><span class="keyword">, </span><span class="default">htmlentities</span><span class="keyword">(</span><span class="default">$soapClient</span><span class="keyword">-&gt;</span><span class="default">__getLastRequest</span><span class="keyword">()), </span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;Response :&lt;br&gt;&quot;</span><span class="keyword">, </span><span class="default">htmlentities</span><span class="keyword">(</span><span class="default">$soapClient</span><span class="keyword">-&gt;</span><span class="default">__getLastResponse</span><span class="keyword">()), </span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;<br></span><br>Without try/catch, your just get the Fatal Error and PHP commits suicide before you can call __getLastRequest/__getLastResponse</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/soapclient.getlastresponse.php)

**[⬆ to root](/)**