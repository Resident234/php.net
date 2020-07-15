# SoapServer::__construct



// Workin Server with Client for localhost<br><br>// server.php<br><br>

```
<?php 
class MyClass {
  public function helloWorld() {

    return 'Hallo Welt '. print_r(func_get_args(), true);
  }
}
 
try {
  $server = new SOAPServer(
    NULL,
    array(
     'uri' => 'http://localhost/soap/server.php'
    )
  );
 
  $server->setClass('MyClass');
  $server->handle();
}
 
catch (SOAPFault $f) {
  print $f->faultstring;
}

?>
```


// client.php:



```
<?php
$client = new SoapClient(null, array(
      'location' => "http://localhost/soap/server.php",
      'uri'      => "http://localhost/soap/server.php",
      'trace'    => 1 ));

echo $return = $client->__soapCall("helloWorld",array("world"));
?>
```
<br><br>// Hope you like it  

#

[Official documentation page](https://www.php.net/manual/en/soapserver.construct.php)

**[To root](/README.md)**