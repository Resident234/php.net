# The SoapHeader class




<div class="phpcode"><span class="html">
None of the examples really do it for me.<br>Note: you should NOT need to hard-code any XML.<br><br>Here is an example of creating a nested header and including a parameter.<br><br>$client = new SoapClient(WSDL,array());<br><br>$auth = array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;UserName&apos;=&gt;&apos;USERNAME&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;Password&apos;=&gt;&apos;PASSWORD&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;SystemId&apos;=&gt; array(&apos;_&apos;=&gt;&apos;DATA&apos;,&apos;Param&apos;=&gt;&apos;PARAM&apos;),<br>&#xA0; &#xA0; &#xA0; &#xA0; );<br>&#xA0; $header = new SoapHeader(&apos;NAMESPACE&apos;,&apos;Auth&apos;,$auth,false);<br>&#xA0; $client-&gt;__setSoapHeaders($header);<br><br>Gives the following header XML:<br><br>&#xA0; &lt;SOAP-ENV:Header&gt;<br>&#xA0; &#xA0; &lt;ns1:Auth&gt;<br>&#xA0; &#xA0; &#xA0; &lt;ns1:SystemId Param=&quot;PARAM&quot;&gt;DATA&lt;/ns1:SystemId&gt;<br>&#xA0; &#xA0; &#xA0; &lt;ns1:UserName&gt;USERNAME&lt;/ns1:UserName&gt;<br>&#xA0; &#xA0; &#xA0; &lt;ns1:Password&gt;PASSWORD&lt;/ns1:Password&gt;<br>&#xA0; &#xA0; &lt;/ns1:Auth&gt;<br>&#xA0; &lt;/SOAP-ENV:Header&gt;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.soapheader.php)

**[To root](/README.md)**