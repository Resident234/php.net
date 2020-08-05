# SoapClient::__setSoapHeaders



To create complex SOAP Headers, you can do something like this:<br><br>Required SOAP Header:<br><br>&lt;soap:Header&gt;<br>    &lt;RequestorCredentials xmlns="http://namespace.example.com/"&gt;<br>      &lt;Token&gt;string&lt;/Token&gt;<br>      &lt;Version&gt;string&lt;/Version&gt;<br>      &lt;MerchantID&gt;string&lt;/MerchantID&gt;<br>      &lt;UserCredentials&gt;<br>        &lt;UserID&gt;string&lt;/UserID&gt;<br>        &lt;Password&gt;string&lt;/Password&gt;<br>      &lt;/UserCredentials&gt;<br>    &lt;/RequestorCredentials&gt;<br>&lt;/soap:Header&gt;<br><br>Corresponding PHP code:<br><br>

```
<?php

$ns = 'http://namespace.example.com/'; //Namespace of the WS.

//Body of the Soap Header.
$headerbody = array('Token' => $someToken,
                    'Version' => $someVersion,
                    'MerchantID'=>$someMerchantId,
                      'UserCredentials'=>array('UserID'=>$UserID,
                                             'Password'=>$Pwd));

//Create Soap Header.        
$header = new SOAPHeader($ns, 'RequestorCredentials', $headerbody);        
        
//set the Headers of Soap Client.
$soap_client->__setSoapHeaders($header);

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/soapclient.setsoapheaders.php)

**[To root](/README.md)**