# SoapClient::__setSoapHeaders



To create complex SOAP Headers, you can do something like this:<br><br>Required SOAP Header:<br><br>&lt;soap:Header&gt;<br>    &lt;RequestorCredentials xmlns="http://namespace.example.com/"&gt;<br>      &lt;Token&gt;string&lt;/Token&gt;<br>      &lt;Version&gt;string&lt;/Version&gt;<br>      &lt;MerchantID&gt;string&lt;/MerchantID&gt;<br>      &lt;UserCredentials&gt;<br>        &lt;UserID&gt;string&lt;/UserID&gt;<br>        &lt;Password&gt;string&lt;/Password&gt;<br>      &lt;/UserCredentials&gt;<br>    &lt;/RequestorCredentials&gt;<br>&lt;/soap:Header&gt;<br><br>Corresponding PHP code:<br><br>

```
<?php

$ns = &apos;http://namespace.example.com/&apos;; //Namespace of the WS.

//Body of the Soap Header.
$headerbody = array(&apos;Token&apos; =&gt; $someToken,
                    &apos;Version&apos; =&gt; $someVersion,
                    &apos;MerchantID&apos;=&gt;$someMerchantId,
                      &apos;UserCredentials&apos;=&gt;array(&apos;UserID&apos;=&gt;$UserID,
                                             &apos;Password&apos;=&gt;$Pwd));

//Create Soap Header.        
$header = new SOAPHeader($ns, &apos;RequestorCredentials&apos;, $headerbody);        
        
//set the Headers of Soap Client.
$soap_client-&gt;__setSoapHeaders($header);

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/soapclient.setsoapheaders.php)

**[To root](/README.md)**