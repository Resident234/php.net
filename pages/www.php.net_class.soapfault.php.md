# The SoapFault class



You may use undocumented and invisible property $e-&gt;faultcode to access string version of $code. Because standard $e-&gt;getCode() does not work:<br><br>

```
<?php
$e = new SoapFault("test", "msg");
var_dump($e-&gt;getCode()); // prints "0"
var_dump($e-&gt;faultcode); // prints "test"
?>
```


Also you may use namespaced fault codes:



```
<?php
$e = new SoapFault(array("namespace", "test"), "msg");
?>
```
<br><br>- see ext/soap/soap.php, PHP_METHOD(SoapFault, SoapFault). To access the namespace, use $e-&gt;faultcodens  

#

A bit more digging in ext/soap/soap.c and the set_soap_fault function reveals the other undocumented properties from the constructor:<br><br>

```
<?php
try {
    throw new SoapFault(&apos;code&apos;, &apos;string&apos;, &apos;actor&apos;, &apos;detail&apos;, &apos;name&apos;, &apos;header&apos;);
} catch (Exception $ex) {
    var_dump($ex-&gt;faultcode, $ex-&gt;faultstring, $ex-&gt;faultactor, $ex-&gt;detail, $ex-&gt;_name, $ex-&gt;headerfault);
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/class.soapfault.php)

**[To root](/README.md)**