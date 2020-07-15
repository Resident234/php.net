# SoapClient::__getLastResponse



D&apos;oh!<br>That example needs:<br>$soapClient = new SoapClient($url, array(&apos;trace&apos;=&gt;1));<br>to turn ON tracing in the first place.  

#

You almost for sure will need to wrap a try/catch block around your SOAP call in order to use these to debug something that&apos;s not working.<br><br>Otherwise, PHP throws a fatal error before you can execute this function.<br><br>For example:<br>

```
<?php
    $soapClient = new SoapClient($url);
    echo htmlentities($soapClient->__getFunctions());
    //Assume that has output 'someFunction' (among others)
    try {
        $results = $soapClient->someFunction(...);
    }
    catch (SoapFault $soapFault) {
        var_dump($soapFault);
        echo "Request :&lt;br&gt;", htmlentities($soapClient->__getLastRequest()), "&lt;br&gt;";
        echo "Response :&lt;br&gt;", htmlentities($soapClient->__getLastResponse()), "&lt;br&gt;";
    }
?>
```
<br><br>Without try/catch, your just get the Fatal Error and PHP commits suicide before you can call __getLastRequest/__getLastResponse  

#

[Official documentation page](https://www.php.net/manual/en/soapclient.getlastresponse.php)

**[To root](/README.md)**