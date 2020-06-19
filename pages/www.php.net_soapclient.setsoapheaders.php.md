# SoapClient::__setSoapHeaders




<div class="phpcode"><span class="html">
To create complex SOAP Headers, you can do something like this:
<br>
<br>Required SOAP Header:
<br>
<br>&lt;soap:Header&gt;
<br>&#xA0; &#xA0; &lt;RequestorCredentials xmlns=&quot;<a href="http://namespace.example.com/" rel="nofollow" target="_blank">http://namespace.example.com/</a>&quot;&gt;
<br>&#xA0; &#xA0; &#xA0; &lt;Token&gt;string&lt;/Token&gt;
<br>&#xA0; &#xA0; &#xA0; &lt;Version&gt;string&lt;/Version&gt;
<br>&#xA0; &#xA0; &#xA0; &lt;MerchantID&gt;string&lt;/MerchantID&gt;
<br>&#xA0; &#xA0; &#xA0; &lt;UserCredentials&gt;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &lt;UserID&gt;string&lt;/UserID&gt;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &lt;Password&gt;string&lt;/Password&gt;
<br>&#xA0; &#xA0; &#xA0; &lt;/UserCredentials&gt;
<br>&#xA0; &#xA0; &lt;/RequestorCredentials&gt;
<br>&lt;/soap:Header&gt;
<br>
<br>Corresponding PHP code:
<br>
<br><span class="default">&lt;?php
<br>
<br>$ns </span><span class="keyword">= </span><span class="string">&apos;<a href="http://namespace.example.com/" rel="nofollow" target="_blank">http://namespace.example.com/</a>&apos;</span><span class="keyword">; </span><span class="comment">//Namespace of the WS.
<br>
<br>//Body of the Soap Header.
<br></span><span class="default">$headerbody </span><span class="keyword">= array(</span><span class="string">&apos;Token&apos; </span><span class="keyword">=&gt; </span><span class="default">$someToken</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;Version&apos; </span><span class="keyword">=&gt; </span><span class="default">$someVersion</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;MerchantID&apos;</span><span class="keyword">=&gt;</span><span class="default">$someMerchantId</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;UserCredentials&apos;</span><span class="keyword">=&gt;array(</span><span class="string">&apos;UserID&apos;</span><span class="keyword">=&gt;</span><span class="default">$UserID</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="string">&apos;Password&apos;</span><span class="keyword">=&gt;</span><span class="default">$Pwd</span><span class="keyword">));
<br>
<br></span><span class="comment">//Create Soap Header.&#xA0; &#xA0; &#xA0; &#xA0; 
<br></span><span class="default">$header </span><span class="keyword">= new </span><span class="default">SOAPHeader</span><span class="keyword">(</span><span class="default">$ns</span><span class="keyword">, </span><span class="string">&apos;RequestorCredentials&apos;</span><span class="keyword">, </span><span class="default">$headerbody</span><span class="keyword">);&#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; 
<br></span><span class="comment">//set the Headers of Soap Client.
<br></span><span class="default">$soap_client</span><span class="keyword">-&gt;</span><span class="default">__setSoapHeaders</span><span class="keyword">(</span><span class="default">$header</span><span class="keyword">);
<br>
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/soapclient.setsoapheaders.php)

**[To root](/README.md)**