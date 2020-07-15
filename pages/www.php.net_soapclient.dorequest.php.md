# SoapClient::__doRequest



Note when extending __doRequest, calling __getLastRequest will probably report incorrect information unless you make sure to update the internal __last_request variable. Save yourself some headaches.<br><br>function __doRequest($request, $location, $action, $version) {<br>      $request = preg_replace(&apos;/abc/&apos;, &apos;def&apos;, $request);<br>      $ret = parent::__doRequest($request, $location, $action, $version);<br>      $this-&gt;__last_request = $request;<br>      return $ret;<br>}  

#

Note that the SoapClient.__doRequest() method circumvents the throwing of SoapFault exceptions.<br><br>Specifically, if you call the __doRequest() method and it fails, it would normally throw a SoapFault exception.  However, the __doRequest() method doesn&apos;t actually throw the exception. Instead, the exception is saved in a class attribute called SoapFault.__soap_fault, and is actually thrown AFTER the __doRequest method completes (but the call stack will show that the exception was created inside the __doRequest method.<br><br>I successfully used the following code to query the locally cached exception object that was not thrown:<br><br>

```
<?php
$exception = null;
try {
    $result = parent::__doRequest($request, $location, $action, $version, $one_way);
}
catch (SoapFault $sf) {
    //this code was not reached    
    $exception = $sf;
}
catch (Exception $e) {
    //nor was this code reached either
    $exception = $e;
}
if((isset($this->__soap_fault)) &amp;&amp; ($this->__soap_fault != null)) {
    //this is where the exception from __doRequest is stored
    $exception = $this->__soap_fault;
}

//decide what to do about the exception here
// [enter code here]
//or throw the exception
if($exception != null) {
    throw $exception;
}
//note: you may want to unset the __soap_fault value if you don't want it thrown again up the call stack
?>
```
  

#

If you want to communicate with a default configured ASP.NET server with SOAP 1.1 support, override your __doRequest with the following code. Adjust the namespace parameter, and all is good to go.<br><br>

```
<?php
class MSSoapClient extends SoapClient {

    function __doRequest($request, $location, $action, $version) {
        $namespace = "http://tempuri.com";

        $request = preg_replace('/&lt;ns1:(\w+)/', '&lt;$1 xmlns="'.$namespace.'"', $request, 1);
        $request = preg_replace('/&lt;ns1:(\w+)/', '&lt;$1', $request);
        $request = str_replace(array('/ns1:', 'xmlns:ns1="'.$namespace.'"'), array('/', ''), $request);

        // parent call
        return parent::__doRequest($request, $location, $action, $version);
    }
}

$client = new MSSoapClient(...);
?>
```
<br><br>Hope this will save people endless hours of fiddling...  

#

[Official documentation page](https://www.php.net/manual/en/soapclient.dorequest.php)

**[To root](/README.md)**