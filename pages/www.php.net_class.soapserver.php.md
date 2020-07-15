# The SoapServer class



While there are plenty of mentions online that SoapServer doesn&apos;t support SOAP Headers, this isn&apos;t true.<br><br>In your class, if you declare a function with the name of the header, the function will be called when that header is received.<br><br>

```
<?php
class MySoapService {
  private $user_is_valid;

  function MyHeader($header) {
    if ((isset($header->Username)) &amp;&amp; (isset($header->Password))) {
      if (ValidateUser($header->Username, $header->Password)) {
        $user_is_valid = true;
      }
    }
  }

  function MySoapRequest($request) {
    if ($user_is_valid) {
      // process request
    }
    else {
      throw new MyFault("MySoapRequest", "User not valid.");
    }
  }
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/class.soapserver.php)

**[To root](/README.md)**