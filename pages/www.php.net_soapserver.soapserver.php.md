# SoapServer::SoapServer




<div class="phpcode"><span class="html">
Here&apos;s my solution to make SOAP-headers based authentication.<br><br>1). First of all we define the decorator class for our service class:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">SOAP_Service_Secure<br></span><span class="keyword">{<br>&#xA0; &#xA0; protected </span><span class="default">$class_name&#xA0; &#xA0; </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; protected </span><span class="default">$authenticated </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">;<br><br>&#xA0; &#xA0; </span><span class="comment">// -----<br><br>&#xA0; &#xA0; </span><span class="keyword">public function </span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$class_name</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">class_name </span><span class="keyword">= </span><span class="default">$class_name</span><span class="keyword">;<br><br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; public function </span><span class="default">AuthHeader</span><span class="keyword">(</span><span class="default">$Header</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$Header</span><span class="keyword">-&gt;</span><span class="default">username </span><span class="keyword">== </span><span class="string">&apos;foo&apos; </span><span class="keyword">&amp;&amp; </span><span class="default">$Header</span><span class="keyword">-&gt;</span><span class="default">password </span><span class="keyword">== </span><span class="string">&apos;bar&apos;</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">authenticated </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;<br><br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; public function </span><span class="default">__call</span><span class="keyword">(</span><span class="default">$method_name</span><span class="keyword">, </span><span class="default">$arguments</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; if(!</span><span class="default">method_exists</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">class_name</span><span class="keyword">, </span><span class="default">$method_name</span><span class="keyword">))<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&apos;method not found&apos;</span><span class="keyword">);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">checkAuth</span><span class="keyword">();<br><br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">call_user_func_array</span><span class="keyword">(array(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">class_name</span><span class="keyword">, </span><span class="default">$method_name</span><span class="keyword">), </span><span class="default">$arguments</span><span class="keyword">);<br><br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; </span><span class="comment">// -----<br><br>&#xA0; &#xA0; </span><span class="keyword">protected function </span><span class="default">checkAuth</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; if(!</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">authenticated</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">HTML_Output</span><span class="keyword">::</span><span class="default">error</span><span class="keyword">(</span><span class="default">403</span><span class="keyword">);<br><br>&#xA0; &#xA0; }<br><br>}<br><br></span><span class="default">?&gt;<br></span><br>2). Then we pass an instance of it to the SoapServer object.<br><br><span class="default">&lt;?php<br><br>&#xA0; &#xA0; $Service </span><span class="keyword">= new </span><span class="default">SOAP_Service_Secure</span><span class="keyword">(</span><span class="string">&apos;My_Service&apos;</span><span class="keyword">);<br><br>&#xA0; &#xA0; </span><span class="default">$Server </span><span class="keyword">= new </span><span class="default">SoapServer</span><span class="keyword">(</span><span class="string">&apos;my-service.wsdl&apos;</span><span class="keyword">);<br><br>&#xA0; &#xA0; </span><span class="default">$Server</span><span class="keyword">-&gt;</span><span class="default">setObject</span><span class="keyword">(</span><span class="default">$Service</span><span class="keyword">);<br><br>&#xA0; &#xA0; </span><span class="default">$Server</span><span class="keyword">-&gt;</span><span class="default">handle</span><span class="keyword">();<br><br></span><span class="default">?&gt;<br></span><br>3). Implementing a client:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">AuthHeader<br></span><span class="keyword">{<br>&#xA0; &#xA0; public </span><span class="default">$username</span><span class="keyword">;<br>&#xA0; &#xA0; public </span><span class="default">$password</span><span class="keyword">;&#xA0; &#xA0; <br>&#xA0; &#xA0; <br>}<br><br></span><span class="comment">// -----<br><br></span><span class="default">$Client </span><span class="keyword">= new </span><span class="default">SoapClient</span><span class="keyword">(</span><span class="string">&apos;<a href="http://example.com/my-service.wsdl" rel="nofollow" target="_blank">http://example.com/my-service.wsdl</a>&apos;</span><span class="keyword">, array(<br>&#xA0; &#xA0; </span><span class="string">&apos;exceptions&apos; </span><span class="keyword">=&gt; </span><span class="default">true<br></span><span class="keyword">));<br><br></span><span class="comment">// -----<br><br></span><span class="default">$AuthHeader </span><span class="keyword">= new </span><span class="default">AuthHeader</span><span class="keyword">();<br><br></span><span class="default">$AuthHeader</span><span class="keyword">-&gt;</span><span class="default">username </span><span class="keyword">= </span><span class="string">&apos;foo&apos;</span><span class="keyword">;<br></span><span class="default">$AuthHeader</span><span class="keyword">-&gt;</span><span class="default">password </span><span class="keyword">= </span><span class="string">&apos;bar&apos;</span><span class="keyword">;<br><br></span><span class="default">$Headers</span><span class="keyword">[] = new </span><span class="default">SoapHeader</span><span class="keyword">(</span><span class="string">&apos;<a href="http://example.com" rel="nofollow" target="_blank">http://example.com</a>&apos;</span><span class="keyword">, </span><span class="string">&apos;AuthHeader&apos;</span><span class="keyword">, </span><span class="default">$AuthHeader</span><span class="keyword">);<br><br></span><span class="default">$Client</span><span class="keyword">-&gt;</span><span class="default">__setSoapHeaders</span><span class="keyword">(</span><span class="default">$Headers</span><span class="keyword">);<br><br></span><span class="comment">// -----<br><br></span><span class="default">$Result </span><span class="keyword">= </span><span class="default">$Client</span><span class="keyword">-&gt;</span><span class="default">someMethod</span><span class="keyword">();<br><br></span><span class="default">?&gt;<br></span><br>As you can see SoapServer uses our decorator class to answer client requests. If request contains &quot;AuthHeader&quot; SOAP-header then our wrapper checks the credentials and sets $authenticated parameter to TRUE (on success). Then SoapServer calls requested method on our wrapper, we are intercepting it with __call(), checking authentication and if everything is allright we are calling real method on the &quot;My_Service&quot; class.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/soapserver.soapserver.php)

**[To root](/README.md)**