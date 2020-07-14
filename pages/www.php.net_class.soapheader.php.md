# The SoapHeader class



None of the examples really do it for me.<br>Note: you should NOT need to hard-code any XML.<br><br>Here is an example of creating a nested header and including a parameter.<br><br>$client = new SoapClient(WSDL,array());<br><br>$auth = array(<br>        &apos;UserName&apos;=&gt;&apos;USERNAME&apos;,<br>        &apos;Password&apos;=&gt;&apos;PASSWORD&apos;,<br>        &apos;SystemId&apos;=&gt; array(&apos;_&apos;=&gt;&apos;DATA&apos;,&apos;Param&apos;=&gt;&apos;PARAM&apos;),<br>        );<br>  $header = new SoapHeader(&apos;NAMESPACE&apos;,&apos;Auth&apos;,$auth,false);<br>  $client-&gt;__setSoapHeaders($header);<br><br>Gives the following header XML:<br><br>  &lt;SOAP-ENV:Header&gt;<br>    &lt;ns1:Auth&gt;<br>      &lt;ns1:SystemId Param="PARAM"&gt;DATA&lt;/ns1:SystemId&gt;<br>      &lt;ns1:UserName&gt;USERNAME&lt;/ns1:UserName&gt;<br>      &lt;ns1:Password&gt;PASSWORD&lt;/ns1:Password&gt;<br>    &lt;/ns1:Auth&gt;<br>  &lt;/SOAP-ENV:Header&gt;  

#

[Official documentation page](https://www.php.net/manual/en/class.soapheader.php)

**[To root](/README.md)**