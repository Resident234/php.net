# The SoapFault class



You may use undocumented and invisible property $e-&gt;faultcode to access string version of $code. Because standard $e-&gt;getCode() does not work:<br><br>

```
<?php
$e = new SoapFault("test", "msg");
var_dump($e->getCode()); // prints "0"
var_dump($e->faultcode); // prints "test"
?>
```


Also you may use namespaced fault codes:



```
<?php
$e = new SoapFault(array("namespace", "test"), "msg");
?>
```
<br><br>- see ext/soap/soap.php, PHP_METHOD(SoapFault, SoapFault). To access the namespace, use $e-&gt;faultcodens  

---

A bit more digging in ext/soap/soap.c and the set_soap_fault function reveals the other undocumented properties from the constructor:<br><br>

```
<?php
try {
    throw new SoapFault('code', 'string', 'actor', 'detail', 'name', 'header');
} catch (Exception $ex) {
    var_dump($ex->faultcode, $ex->faultstring, $ex->faultactor, $ex->detail, $ex->_name, $ex->headerfault);
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/class.soapfault.php)

**[To root](/README.md)**