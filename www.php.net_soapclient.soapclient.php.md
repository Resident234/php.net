# SoapClient::SoapClient




<div class="phpcode"><span class="html">
It took me longer than a week to figure out how to implement WSSE (Web Service Security) headers in native PHP SOAP. There are no much resource available on this, so thought to add this here for community benefit.
<br>
<br>Step1: Create two classes to create a structure for WSSE headers
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">class </span><span class="default">clsWSSEAuth </span><span class="keyword">{
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; private </span><span class="default">$Username</span><span class="keyword">; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; private </span><span class="default">$Password</span><span class="keyword">;&#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; function </span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$username</span><span class="keyword">, </span><span class="default">$password</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">Username</span><span class="keyword">=</span><span class="default">$username</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">Password</span><span class="keyword">=</span><span class="default">$password</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>}
<br>
<br>class </span><span class="default">clsWSSEToken </span><span class="keyword">{
<br>&#xA0; &#xA0; &#xA0; &#xA0; private </span><span class="default">$UsernameToken</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; function </span><span class="default">__construct </span><span class="keyword">(</span><span class="default">$innerVal</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">UsernameToken </span><span class="keyword">= </span><span class="default">$innerVal</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Step2: Create Soap Variables for UserName and Password
<br>
<br><span class="default">&lt;?php
<br>$username </span><span class="keyword">= </span><span class="default">1111</span><span class="keyword">;
<br></span><span class="default">$password </span><span class="keyword">= </span><span class="default">1111</span><span class="keyword">;
<br>
<br></span><span class="comment">//Check with your provider which security name-space they are using.
<br></span><span class="default">$strWSSENS </span><span class="keyword">= </span><span class="string">&quot;<a href="http://schemas.xmlsoap.org/ws/2002/07/secext" rel="nofollow" target="_blank">http://schemas.xmlsoap.org/ws/2002/07/secext</a>&quot;</span><span class="keyword">;
<br>
<br></span><span class="default">$objSoapVarUser </span><span class="keyword">= new </span><span class="default">SoapVar</span><span class="keyword">(</span><span class="default">$username</span><span class="keyword">, </span><span class="default">XSD_STRING</span><span class="keyword">, </span><span class="default">NULL</span><span class="keyword">, </span><span class="default">$strWSSENS</span><span class="keyword">, </span><span class="default">NULL</span><span class="keyword">, </span><span class="default">$strWSSENS</span><span class="keyword">);
<br></span><span class="default">$objSoapVarPass </span><span class="keyword">= new </span><span class="default">SoapVar</span><span class="keyword">(</span><span class="default">$password</span><span class="keyword">, </span><span class="default">XSD_STRING</span><span class="keyword">, </span><span class="default">NULL</span><span class="keyword">, </span><span class="default">$strWSSENS</span><span class="keyword">, </span><span class="default">NULL</span><span class="keyword">, </span><span class="default">$strWSSENS</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>Step3: Create Object for Auth Class and pass in soap var
<br>
<br><span class="default">&lt;?php
<br>$objWSSEAuth </span><span class="keyword">= new </span><span class="default">clsWSSEAuth</span><span class="keyword">(</span><span class="default">$objSoapVarUser</span><span class="keyword">, </span><span class="default">$objSoapVarPass</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>Step4: Create SoapVar out of object of Auth class
<br>
<br><span class="default">&lt;?php
<br>$objSoapVarWSSEAuth </span><span class="keyword">= new </span><span class="default">SoapVar</span><span class="keyword">(</span><span class="default">$objWSSEAuth</span><span class="keyword">, </span><span class="default">SOAP_ENC_OBJECT</span><span class="keyword">, </span><span class="default">NULL</span><span class="keyword">, </span><span class="default">$strWSSENS</span><span class="keyword">, </span><span class="string">&apos;UsernameToken&apos;</span><span class="keyword">, </span><span class="default">$strWSSENS</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>Step5: Create object for Token Class
<br>
<br><span class="default">&lt;?php
<br>$objWSSEToken </span><span class="keyword">= new </span><span class="default">clsWSSEToken</span><span class="keyword">(</span><span class="default">$objSoapVarWSSEAuth</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>Step6: Create SoapVar out of object of Token class
<br>
<br><span class="default">&lt;?php
<br>$objSoapVarWSSEToken </span><span class="keyword">= new </span><span class="default">SoapVar</span><span class="keyword">(</span><span class="default">$objWSSEToken</span><span class="keyword">, </span><span class="default">SOAP_ENC_OBJECT</span><span class="keyword">, </span><span class="default">NULL</span><span class="keyword">, </span><span class="default">$strWSSENS</span><span class="keyword">, </span><span class="string">&apos;UsernameToken&apos;</span><span class="keyword">, </span><span class="default">$strWSSENS</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>Step7: Create SoapVar for &apos;Security&apos; node
<br>
<br><span class="default">&lt;?php
<br>$objSoapVarHeaderVal</span><span class="keyword">=new </span><span class="default">SoapVar</span><span class="keyword">(</span><span class="default">$objSoapVarWSSEToken</span><span class="keyword">, </span><span class="default">SOAP_ENC_OBJECT</span><span class="keyword">, </span><span class="default">NULL</span><span class="keyword">, </span><span class="default">$strWSSENS</span><span class="keyword">, </span><span class="string">&apos;Security&apos;</span><span class="keyword">, </span><span class="default">$strWSSENS</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>Step8: Create header object out of security soapvar
<br>
<br><span class="default">&lt;?php
<br>$objSoapVarWSSEHeader </span><span class="keyword">= new </span><span class="default">SoapHeader</span><span class="keyword">(</span><span class="default">$strWSSENS</span><span class="keyword">, </span><span class="string">&apos;Security&apos;</span><span class="keyword">, </span><span class="default">$objSoapVarHeaderVal</span><span class="keyword">,</span><span class="default">true</span><span class="keyword">, </span><span class="string">&apos;<a href="http://abce.com" rel="nofollow" target="_blank">http://abce.com</a>&apos;</span><span class="keyword">);
<br>
<br></span><span class="comment">//Third parameter here makes &apos;mustUnderstand=1
<br>//Forth parameter generates &apos;actor=&quot;<a href="http://abce.com" rel="nofollow" target="_blank">http://abce.com</a>&quot;&apos;
<br></span><span class="default">?&gt;
<br></span>
<br>Step9: Create object of Soap Client
<br>
<br><span class="default">&lt;?php
<br>$objClient </span><span class="keyword">= new </span><span class="default">SoapClient</span><span class="keyword">(</span><span class="default">$WSDL</span><span class="keyword">, </span><span class="default">$arrOptions</span><span class="keyword">); 
<br></span><span class="default">?&gt;
<br></span>
<br>Step10: Set headers for soapclient object
<br>
<br><span class="default">&lt;?php
<br>$objClient</span><span class="keyword">-&gt;</span><span class="default">__setSoapHeaders</span><span class="keyword">(array(</span><span class="default">$objSoapVarWSSEHeader</span><span class="keyword">));
<br></span><span class="default">?&gt;
<br></span>
<br>Step 11: Final call to method
<br>
<br><span class="default">&lt;?php
<br>$objResponse </span><span class="keyword">= </span><span class="default">$objClient</span><span class="keyword">-&gt;</span><span class="default">__soapCall</span><span class="keyword">(</span><span class="default">$strMethod</span><span class="keyword">, </span><span class="default">$requestPayloadString</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
As noted in the bug report <a href="http://bugs.php.net/bug.php?id=36226," rel="nofollow" target="_blank">http://bugs.php.net/bug.php?id=36226,</a> it is considered a feature that sequences with a single element do not come out as arrays. To override this &quot;feature&quot; you can do the following:<br><br>$x = new SoapClient($wsdl, array(&apos;features&apos; =&gt;<br>SOAP_SINGLE_ELEMENT_ARRAYS));</span>
</div>
  

#


<div class="phpcode"><span class="html">
if you need to use ws-security with a nonce and a timestamp, you can use this :<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">WsseAuthHeader </span><span class="keyword">extends </span><span class="default">SoapHeader </span><span class="keyword">{<br><br>private </span><span class="default">$wss_ns </span><span class="keyword">= </span><span class="string">&apos;<a href="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" rel="nofollow" target="_blank">http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd</a>&apos;</span><span class="keyword">;<br>private </span><span class="default">$wsu_ns </span><span class="keyword">= </span><span class="string">&apos;<a href="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd" rel="nofollow" target="_blank">http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd</a>&apos;</span><span class="keyword">;<br><br>function </span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$user</span><span class="keyword">, </span><span class="default">$pass</span><span class="keyword">) {<br><br>&#xA0; &#xA0; </span><span class="default">$created </span><span class="keyword">= </span><span class="default">gmdate</span><span class="keyword">(</span><span class="string">&apos;Y-m-d\TH:i:s\Z&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$nonce </span><span class="keyword">= </span><span class="default">mt_rand</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">$passdigest </span><span class="keyword">= </span><span class="default">base64_encode</span><span class="keyword">( </span><span class="default">pack</span><span class="keyword">(</span><span class="string">&apos;H*&apos;</span><span class="keyword">, </span><span class="default">sha1</span><span class="keyword">( </span><span class="default">pack</span><span class="keyword">(</span><span class="string">&apos;H*&apos;</span><span class="keyword">, </span><span class="default">$nonce</span><span class="keyword">) . </span><span class="default">pack</span><span class="keyword">(</span><span class="string">&apos;a*&apos;</span><span class="keyword">,</span><span class="default">$created</span><span class="keyword">).&#xA0; </span><span class="default">pack</span><span class="keyword">(</span><span class="string">&apos;a*&apos;</span><span class="keyword">,</span><span class="default">$pass</span><span class="keyword">))));<br><br>&#xA0; &#xA0; </span><span class="default">$auth </span><span class="keyword">= new </span><span class="default">stdClass</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">$auth</span><span class="keyword">-&gt;</span><span class="default">Username </span><span class="keyword">= new </span><span class="default">SoapVar</span><span class="keyword">(</span><span class="default">$user</span><span class="keyword">, </span><span class="default">XSD_STRING</span><span class="keyword">, </span><span class="default">NULL</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">wss_ns</span><span class="keyword">, </span><span class="default">NULL</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">wss_ns</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$auth</span><span class="keyword">-&gt;</span><span class="default">Password </span><span class="keyword">= new </span><span class="default">SoapVar</span><span class="keyword">(</span><span class="default">$pass</span><span class="keyword">, </span><span class="default">XSD_STRING</span><span class="keyword">, </span><span class="default">NULL</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">wss_ns</span><span class="keyword">, </span><span class="default">NULL</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">wss_ns</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$auth</span><span class="keyword">-&gt;</span><span class="default">Nonce </span><span class="keyword">= new </span><span class="default">SoapVar</span><span class="keyword">(</span><span class="default">$passdigest</span><span class="keyword">, </span><span class="default">XSD_STRING</span><span class="keyword">, </span><span class="default">NULL</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">wss_ns</span><span class="keyword">, </span><span class="default">NULL</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">wss_ns</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$auth</span><span class="keyword">-&gt;</span><span class="default">Created </span><span class="keyword">= new </span><span class="default">SoapVar</span><span class="keyword">(</span><span class="default">$created</span><span class="keyword">, </span><span class="default">XSD_STRING</span><span class="keyword">, </span><span class="default">NULL</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">wss_ns</span><span class="keyword">, </span><span class="default">NULL</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">wsu_ns</span><span class="keyword">);<br><br>&#xA0; &#xA0; </span><span class="default">$username_token </span><span class="keyword">= new </span><span class="default">stdClass</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">$username_token</span><span class="keyword">-&gt;</span><span class="default">UsernameToken </span><span class="keyword">= new </span><span class="default">SoapVar</span><span class="keyword">(</span><span class="default">$auth</span><span class="keyword">, </span><span class="default">SOAP_ENC_OBJECT</span><span class="keyword">, </span><span class="default">NULL</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">wss_ns</span><span class="keyword">, </span><span class="string">&apos;UsernameToken&apos;</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">wss_ns</span><span class="keyword">);<br><br>&#xA0; &#xA0; </span><span class="default">$security_sv </span><span class="keyword">= new </span><span class="default">SoapVar</span><span class="keyword">(<br>&#xA0; &#xA0; &#xA0; &#xA0; new </span><span class="default">SoapVar</span><span class="keyword">(</span><span class="default">$username_token</span><span class="keyword">, </span><span class="default">SOAP_ENC_OBJECT</span><span class="keyword">, </span><span class="default">NULL</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">wss_ns</span><span class="keyword">, </span><span class="string">&apos;UsernameToken&apos;</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">wss_ns</span><span class="keyword">),<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">SOAP_ENC_OBJECT</span><span class="keyword">, </span><span class="default">NULL</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">wss_ns</span><span class="keyword">, </span><span class="string">&apos;Security&apos;</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">wss_ns</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">parent</span><span class="keyword">::</span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">wss_ns</span><span class="keyword">, </span><span class="string">&apos;Security&apos;</span><span class="keyword">, </span><span class="default">$security_sv</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br>}<br>}<br><br></span><span class="default">?&gt;<br></span><br>and with your SoapClient do :<br><br><span class="default">&lt;?php<br>$client </span><span class="keyword">= new </span><span class="default">SoapClient</span><span class="keyword">(</span><span class="string">&quot;<a href="http://host/path" rel="nofollow" target="_blank">http://host/path</a>&quot;</span><span class="keyword">);<br></span><span class="default">$client</span><span class="keyword">-&gt;</span><span class="default">__setSoapHeaders</span><span class="keyword">(Array(new </span><span class="default">WsseAuthHeader</span><span class="keyword">(</span><span class="string">&quot;user&quot;</span><span class="keyword">, </span><span class="string">&quot;pass&quot;</span><span class="keyword">)));<br></span><span class="default">?&gt;<br></span><br>works for me. based on a stackoverlfow post which only did the username and password, not the nonce and the timestamp</span>
</div>
  

#


<div class="phpcode"><span class="html">
This doesn&apos;t seem to be documented, but when you want to use compression for your outgoing requests, you have to OR with the compression level:<br><br><span class="default">&lt;?php<br> $client </span><span class="keyword">= new </span><span class="default">SoapClient</span><span class="keyword">(</span><span class="string">&quot;some.wsdl&quot;</span><span class="keyword">, <br>&#xA0; array(</span><span class="string">&apos;compression&apos; </span><span class="keyword">=&gt; </span><span class="default">SOAP_COMPRESSION_ACCEPT </span><span class="keyword">| </span><span class="default">SOAP_COMPRESSION_GZIP </span><span class="keyword">| </span><span class="default">9</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>This can be really usefull if you want to send large amounts of data.<br><br>Source: <a href="https://bugs.php.net/bug.php?id=36283" rel="nofollow" target="_blank">https://bugs.php.net/bug.php?id=36283</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
The documentation is wrong (version 7.1.12): A SoapFault exception will be thrown if the wsdl URI cannot be loaded.<br><br>What actually occurs -- even if wrapped in a try...catch(\Throwable $t) -- is an UNCATCHABLE fatal error is thrown.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Hello folks!<br><br>A hint for developers:<br><br>When programming some soap server set the &quot;soap.wsdl_cache_enabled&quot; directive in php.ini file to 0:<br><br>soap.wsdl_cache_enabled=0<br><br>Otherwise it will give a bunch of strange errors saying that your wsdl is incorrect or is missing.<br><br>Doing that will save you from a lot of useless pain.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Example for a soap client with HTTP authentication over a proxy:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">new </span><span class="default">SoapClient</span><span class="keyword">(
<br>&#xA0; &#xA0; </span><span class="string">&apos;service.wsdl&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; array(
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Stuff for development.
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;trace&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;exceptions&apos; </span><span class="keyword">=&gt; </span><span class="default">true</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;cache_wsdl&apos; </span><span class="keyword">=&gt; </span><span class="default">WSDL_CACHE_NONE</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;features&apos; </span><span class="keyword">=&gt; </span><span class="default">SOAP_SINGLE_ELEMENT_ARRAYS</span><span class="keyword">,
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Auth credentials for the SOAP request.
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;login&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;username&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;password&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;password&apos;</span><span class="keyword">,
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Proxy url.
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;proxy_host&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;example.com&apos;</span><span class="keyword">, </span><span class="comment">// Do not add the schema here (http or https). It won&apos;t work.
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;proxy_port&apos; </span><span class="keyword">=&gt; </span><span class="default">44300</span><span class="keyword">,
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Auth credentials for the proxy.
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;proxy_login&apos; </span><span class="keyword">=&gt; </span><span class="default">NULL</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;proxy_password&apos; </span><span class="keyword">=&gt; </span><span class="default">NULL</span><span class="keyword">,
<br>&#xA0; &#xA0; )
<br>);
<br></span><span class="default">?&gt;
<br></span>
<br>Providing an URL to a WSDL file on the remote server (which as well is protected with HTTP authentication) didn&apos;t work. I downloaded the WSDL and stored it on the local server.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/soapclient.soapclient.php)

**[⬆ to root](/)**