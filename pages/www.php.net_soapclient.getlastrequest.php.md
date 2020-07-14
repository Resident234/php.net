# SoapClient::__getLastRequest



Adding htmlentities() can be helpful since it makes the XML visible in your browser without needing to view the source.<br><br>

```
<?php

echo "REQUEST:\n" . htmlentities($client-&gt;__getLastRequest()) . "\n";

?>
```
  

#

Note that when you create SoapClient with option "trace" set to FALSE or omit it, than "__getLastRequest()" always returns NULL.  

#

I guess many peoples calls getLastRequest and it returns nothing. "Heey where is the my last request". Now we will see our request,  when you created a SoapClient instance, you should give a option parameter as below :<br><br>

```
<?php
// below $option=array(&apos;trace&apos;,1);
// correct one is below
$option=array(&apos;trace&apos;=&gt;1);

$client=new SoapClient(&apos;some.wsdl&apos;,$option);

try{
  $client-&gt;aMethodAtRemote();
}catch(SoapFault $fault){
  // &lt;xmp&gt; tag displays xml output in html
  echo &apos;Request : &lt;br/&gt;&lt;xmp&gt;&apos;,
  $client-&gt;__getLastRequest(),
  &apos;&lt;/xmp&gt;&lt;br/&gt;&lt;br/&gt; Error Message : &lt;br/&gt;&apos;,
  $fault-&gt;getMessage();
}
?>
```
<br><br>"trace" parameter enables output of request. Now, you should see SOAP request.  

#

[Official documentation page](https://www.php.net/manual/en/soapclient.getlastrequest.php)

**[To root](/README.md)**