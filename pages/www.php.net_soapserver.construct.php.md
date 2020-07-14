# SoapServer::__construct



// Workin Server with Client for localhost<br><br>// server.php<br><br>

```
<?php 
class MyClass {
  public function helloWorld() {

    return &apos;Hallo Welt &apos;. print_r(func_get_args(), true);
  }
}
 
try {
  $server = new SOAPServer(
    NULL,
    array(
     &apos;uri&apos; =&gt; &apos;http://localhost/soap/server.php&apos;
    )
  );
 
  $server-&gt;setClass(&apos;MyClass&apos;);
  $server-&gt;handle();
}
 
catch (SOAPFault $f) {
  print $f-&gt;faultstring;
}

?>
```


// client.php:



```
<?php
$client = new SoapClient(null, array(
      &apos;location&apos; =&gt; "http://localhost/soap/server.php",
      &apos;uri&apos;      =&gt; "http://localhost/soap/server.php",
      &apos;trace&apos;    =&gt; 1 ));

echo $return = $client-&gt;__soapCall("helloWorld",array("world"));
?>
```
<br><br>// Hope you like it  

#

[Official documentation page](https://www.php.net/manual/en/soapserver.construct.php)

**[To root](/README.md)**