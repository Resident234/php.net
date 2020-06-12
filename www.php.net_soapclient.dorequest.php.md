# SoapClient::__doRequest




<div class="phpcode"><span class="html">
Note when extending __doRequest, calling __getLastRequest will probably report incorrect information unless you make sure to update the internal __last_request variable. Save yourself some headaches.<br><br>function __doRequest($request, $location, $action, $version) {<br>&#xA0; &#xA0; &#xA0; $request = preg_replace(&apos;/abc/&apos;, &apos;def&apos;, $request);<br>&#xA0; &#xA0; &#xA0; $ret = parent::__doRequest($request, $location, $action, $version);<br>&#xA0; &#xA0; &#xA0; $this-&gt;__last_request = $request;<br>&#xA0; &#xA0; &#xA0; return $ret;<br>}</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that the SoapClient.__doRequest() method circumvents the throwing of SoapFault exceptions.
<br>
<br>Specifically, if you call the __doRequest() method and it fails, it would normally throw a SoapFault exception.&#xA0; However, the __doRequest() method doesn&apos;t actually throw the exception. Instead, the exception is saved in a class attribute called SoapFault.__soap_fault, and is actually thrown AFTER the __doRequest method completes (but the call stack will show that the exception was created inside the __doRequest method.
<br>
<br>I successfully used the following code to query the locally cached exception object that was not thrown:
<br>
<br><span class="default">&lt;?php
<br>$exception </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">;
<br>try {
<br>&#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= </span><span class="default">parent</span><span class="keyword">::</span><span class="default">__doRequest</span><span class="keyword">(</span><span class="default">$request</span><span class="keyword">, </span><span class="default">$location</span><span class="keyword">, </span><span class="default">$action</span><span class="keyword">, </span><span class="default">$version</span><span class="keyword">, </span><span class="default">$one_way</span><span class="keyword">);
<br>}
<br>catch (</span><span class="default">SoapFault $sf</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="comment">//this code was not reached&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="default">$exception </span><span class="keyword">= </span><span class="default">$sf</span><span class="keyword">;
<br>}
<br>catch (</span><span class="default">Exception $e</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="comment">//nor was this code reached either
<br>&#xA0; &#xA0; </span><span class="default">$exception </span><span class="keyword">= </span><span class="default">$e</span><span class="keyword">;
<br>}
<br>if((isset(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">__soap_fault</span><span class="keyword">)) &amp;&amp; (</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">__soap_fault </span><span class="keyword">!= </span><span class="default">null</span><span class="keyword">)) {
<br>&#xA0; &#xA0; </span><span class="comment">//this is where the exception from __doRequest is stored
<br>&#xA0; &#xA0; </span><span class="default">$exception </span><span class="keyword">= </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">__soap_fault</span><span class="keyword">;
<br>}
<br>
<br></span><span class="comment">//decide what to do about the exception here
<br>// [enter code here]
<br>//or throw the exception
<br></span><span class="keyword">if(</span><span class="default">$exception </span><span class="keyword">!= </span><span class="default">null</span><span class="keyword">) {
<br>&#xA0; &#xA0; throw </span><span class="default">$exception</span><span class="keyword">;
<br>}
<br></span><span class="comment">//note: you may want to unset the __soap_fault value if you don&apos;t want it thrown again up the call stack
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to communicate with a default configured ASP.NET server with SOAP 1.1 support, override your __doRequest with the following code. Adjust the namespace parameter, and all is good to go.<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">MSSoapClient </span><span class="keyword">extends </span><span class="default">SoapClient </span><span class="keyword">{<br><br>&#xA0; &#xA0; function </span><span class="default">__doRequest</span><span class="keyword">(</span><span class="default">$request</span><span class="keyword">, </span><span class="default">$location</span><span class="keyword">, </span><span class="default">$action</span><span class="keyword">, </span><span class="default">$version</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$namespace </span><span class="keyword">= </span><span class="string">&quot;<a href="http://tempuri.com" rel="nofollow" target="_blank">http://tempuri.com</a>&quot;</span><span class="keyword">;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$request </span><span class="keyword">= </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;/&lt;ns1:(\w+)/&apos;</span><span class="keyword">, </span><span class="string">&apos;&lt;$1 xmlns=&quot;&apos;</span><span class="keyword">.</span><span class="default">$namespace</span><span class="keyword">.</span><span class="string">&apos;&quot;&apos;</span><span class="keyword">, </span><span class="default">$request</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$request </span><span class="keyword">= </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;/&lt;ns1:(\w+)/&apos;</span><span class="keyword">, </span><span class="string">&apos;&lt;$1&apos;</span><span class="keyword">, </span><span class="default">$request</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$request </span><span class="keyword">= </span><span class="default">str_replace</span><span class="keyword">(array(</span><span class="string">&apos;/ns1:&apos;</span><span class="keyword">, </span><span class="string">&apos;xmlns:ns1=&quot;&apos;</span><span class="keyword">.</span><span class="default">$namespace</span><span class="keyword">.</span><span class="string">&apos;&quot;&apos;</span><span class="keyword">), array(</span><span class="string">&apos;/&apos;</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">), </span><span class="default">$request</span><span class="keyword">);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// parent call<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="default">parent</span><span class="keyword">::</span><span class="default">__doRequest</span><span class="keyword">(</span><span class="default">$request</span><span class="keyword">, </span><span class="default">$location</span><span class="keyword">, </span><span class="default">$action</span><span class="keyword">, </span><span class="default">$version</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">$client </span><span class="keyword">= new </span><span class="default">MSSoapClient</span><span class="keyword">(...);<br></span><span class="default">?&gt;<br></span><br>Hope this will save people endless hours of fiddling...</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/soapclient.dorequest.php)

**[⬆ to root](/)**