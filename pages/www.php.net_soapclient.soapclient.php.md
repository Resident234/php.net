# SoapClient::SoapClient



It took me longer than a week to figure out how to implement WSSE (Web Service Security) headers in native PHP SOAP. There are no much resource available on this, so thought to add this here for community benefit.<br><br>Step1: Create two classes to create a structure for WSSE headers<br><br>

```
<?php
class clsWSSEAuth {
          private $Username; 
          private $Password;  
        function __construct($username, $password) {
                 $this-&gt;Username=$username;
                 $this-&gt;Password=$password;
              }
}

class clsWSSEToken {
        private $UsernameToken;
        function __construct ($innerVal){
            $this-&gt;UsernameToken = $innerVal;
        }
}
?>
```


Step2: Create Soap Variables for UserName and Password



```
<?php
$username = 1111;
$password = 1111;

//Check with your provider which security name-space they are using.
$strWSSENS = "http://schemas.xmlsoap.org/ws/2002/07/secext";

$objSoapVarUser = new SoapVar($username, XSD_STRING, NULL, $strWSSENS, NULL, $strWSSENS);
$objSoapVarPass = new SoapVar($password, XSD_STRING, NULL, $strWSSENS, NULL, $strWSSENS);
?>
```


Step3: Create Object for Auth Class and pass in soap var



```
<?php
$objWSSEAuth = new clsWSSEAuth($objSoapVarUser, $objSoapVarPass);
?>
```


Step4: Create SoapVar out of object of Auth class



```
<?php
$objSoapVarWSSEAuth = new SoapVar($objWSSEAuth, SOAP_ENC_OBJECT, NULL, $strWSSENS, &apos;UsernameToken&apos;, $strWSSENS);
?>
```


Step5: Create object for Token Class



```
<?php
$objWSSEToken = new clsWSSEToken($objSoapVarWSSEAuth);
?>
```


Step6: Create SoapVar out of object of Token class



```
<?php
$objSoapVarWSSEToken = new SoapVar($objWSSEToken, SOAP_ENC_OBJECT, NULL, $strWSSENS, &apos;UsernameToken&apos;, $strWSSENS);
?>
```


Step7: Create SoapVar for &apos;Security&apos; node



```
<?php
$objSoapVarHeaderVal=new SoapVar($objSoapVarWSSEToken, SOAP_ENC_OBJECT, NULL, $strWSSENS, &apos;Security&apos;, $strWSSENS);
?>
```


Step8: Create header object out of security soapvar



```
<?php
$objSoapVarWSSEHeader = new SoapHeader($strWSSENS, &apos;Security&apos;, $objSoapVarHeaderVal,true, &apos;http://abce.com&apos;);

//Third parameter here makes &apos;mustUnderstand=1
//Forth parameter generates &apos;actor="http://abce.com"&apos;
?>
```


Step9: Create object of Soap Client



```
<?php
$objClient = new SoapClient($WSDL, $arrOptions); 
?>
```


Step10: Set headers for soapclient object



```
<?php
$objClient-&gt;__setSoapHeaders(array($objSoapVarWSSEHeader));
?>
```


Step 11: Final call to method



```
<?php
$objResponse = $objClient-&gt;__soapCall($strMethod, $requestPayloadString);
?>
```
  

#

As noted in the bug report http://bugs.php.net/bug.php?id=36226, it is considered a feature that sequences with a single element do not come out as arrays. To override this "feature" you can do the following:<br><br>$x = new SoapClient($wsdl, array(&apos;features&apos; =&gt;<br>SOAP_SINGLE_ELEMENT_ARRAYS));  

#

if you need to use ws-security with a nonce and a timestamp, you can use this :<br><br>

```
<?php

class WsseAuthHeader extends SoapHeader {

private $wss_ns = &apos;http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd&apos;;
private $wsu_ns = &apos;http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd&apos;;

function __construct($user, $pass) {

    $created = gmdate(&apos;Y-m-d\TH:i:s\Z&apos;);
    $nonce = mt_rand();
    $passdigest = base64_encode( pack(&apos;H*&apos;, sha1( pack(&apos;H*&apos;, $nonce) . pack(&apos;a*&apos;,$created).  pack(&apos;a*&apos;,$pass))));

    $auth = new stdClass();
    $auth-&gt;Username = new SoapVar($user, XSD_STRING, NULL, $this-&gt;wss_ns, NULL, $this-&gt;wss_ns);
    $auth-&gt;Password = new SoapVar($pass, XSD_STRING, NULL, $this-&gt;wss_ns, NULL, $this-&gt;wss_ns);
    $auth-&gt;Nonce = new SoapVar($passdigest, XSD_STRING, NULL, $this-&gt;wss_ns, NULL, $this-&gt;wss_ns);
    $auth-&gt;Created = new SoapVar($created, XSD_STRING, NULL, $this-&gt;wss_ns, NULL, $this-&gt;wsu_ns);

    $username_token = new stdClass();
    $username_token-&gt;UsernameToken = new SoapVar($auth, SOAP_ENC_OBJECT, NULL, $this-&gt;wss_ns, &apos;UsernameToken&apos;, $this-&gt;wss_ns);

    $security_sv = new SoapVar(
        new SoapVar($username_token, SOAP_ENC_OBJECT, NULL, $this-&gt;wss_ns, &apos;UsernameToken&apos;, $this-&gt;wss_ns),
        SOAP_ENC_OBJECT, NULL, $this-&gt;wss_ns, &apos;Security&apos;, $this-&gt;wss_ns);
    parent::__construct($this-&gt;wss_ns, &apos;Security&apos;, $security_sv, true);
}
}

?>
```


and with your SoapClient do :



```
<?php
$client = new SoapClient("http://host/path");
$client-&gt;__setSoapHeaders(Array(new WsseAuthHeader("user", "pass")));
?>
```
<br><br>works for me. based on a stackoverlfow post which only did the username and password, not the nonce and the timestamp  

#

This doesn&apos;t seem to be documented, but when you want to use compression for your outgoing requests, you have to OR with the compression level:<br><br>

```
<?php
 $client = new SoapClient("some.wsdl", 
  array(&apos;compression&apos; =&gt; SOAP_COMPRESSION_ACCEPT | SOAP_COMPRESSION_GZIP | 9));
?>
```
<br><br>This can be really usefull if you want to send large amounts of data.<br><br>Source: https://bugs.php.net/bug.php?id=36283  

#

The documentation is wrong (version 7.1.12): A SoapFault exception will be thrown if the wsdl URI cannot be loaded.<br><br>What actually occurs -- even if wrapped in a try...catch(\Throwable $t) -- is an UNCATCHABLE fatal error is thrown.  

#

Hello folks!<br><br>A hint for developers:<br><br>When programming some soap server set the "soap.wsdl_cache_enabled" directive in php.ini file to 0:<br><br>soap.wsdl_cache_enabled=0<br><br>Otherwise it will give a bunch of strange errors saying that your wsdl is incorrect or is missing.<br><br>Doing that will save you from a lot of useless pain.  

#

Example for a soap client with HTTP authentication over a proxy:<br><br>

```
<?php
new SoapClient(
    &apos;service.wsdl&apos;,
    array(
        // Stuff for development.
        &apos;trace&apos; =&gt; 1,
        &apos;exceptions&apos; =&gt; true,
        &apos;cache_wsdl&apos; =&gt; WSDL_CACHE_NONE,
        &apos;features&apos; =&gt; SOAP_SINGLE_ELEMENT_ARRAYS,

        // Auth credentials for the SOAP request.
        &apos;login&apos; =&gt; &apos;username&apos;,
        &apos;password&apos; =&gt; &apos;password&apos;,

        // Proxy url.
        &apos;proxy_host&apos; =&gt; &apos;example.com&apos;, // Do not add the schema here (http or https). It won&apos;t work.
        &apos;proxy_port&apos; =&gt; 44300,

        // Auth credentials for the proxy.
        &apos;proxy_login&apos; =&gt; NULL,
        &apos;proxy_password&apos; =&gt; NULL,
    )
);
?>
```
<br><br>Providing an URL to a WSDL file on the remote server (which as well is protected with HTTP authentication) didn&apos;t work. I downloaded the WSDL and stored it on the local server.  

#

[Official documentation page](https://www.php.net/manual/en/soapclient.soapclient.php)

**[To root](/README.md)**