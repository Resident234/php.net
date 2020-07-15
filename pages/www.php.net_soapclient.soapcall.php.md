# SoapClient::__soapCall



Note that calling __soapCall and calling the generated method from WSDL requires specifying parameters in two different ways.<br><br>For example, if you have a web service with method login that takes username and password, you can call it the following way:<br>

```
<?php
$params = array('username'=>'name', 'password'=>'secret');
$client->login($params);
?>
```


If you want to call __soapCall, you must wrap the arguments in another array as follows:


```
<?php
$client->__soapCall('login', array($params));
?>
```
  

#

One thing to note.<br><br>This happened to me and it took a while until I discovered what the problem was.<br><br>I was trying to get .NET objects from a provided web service, however it always seemed to return empty objects. It did return the backbone, but nothing within the objects that made up the structure.<br><br>Anyhow, it seems that you have to be very precise with the arrays when calling these functions. Par example, do this:<br><br>

```
<?php
$obj = $client->__soapCall($SOAPCall, array('parameters'=>$SoapCallParameters));
?>
```


meaning that you must put an array as the second argument with 'parameters' as the key and the soap call parameters as the value.

Also make sure that the parameter variable, in my case $SoapCallParameters is in the form of what is requested by the webservice.

So, don't just make an array of the form:


```
<?php

(
   [0] => 'Mary',
   [1] => 1983
)

?>
```


but if the webservice requests a 'muid' variable as 'Mary' and a 'birthyear' as 1983, then make your array like this:



```
<?php

(
   [muid] => 'Mary',
   [birthyear] => 1983
)

?>
```
<br><br>The above arrays refer to the $SoapCallParameters variable.<br><br>Hope this helps somebody, not having to spend too much time figuring out the problems.  

#

[Official documentation page](https://www.php.net/manual/en/soapclient.soapcall.php)

**[To root](/README.md)**