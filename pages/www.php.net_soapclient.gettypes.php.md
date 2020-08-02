# SoapClient::__getTypes





```
<?php
// to see formated types

$soap = new SoapClient('http://domain.com/ws.php?WSDL');

echo '';
echo '<h2>Types:</h2>';
$types = $soap->__getTypes();
foreach ($types as $type) {
    $type = preg_replace(
        array('/(\w+) ([a-zA-Z0-9]+)/', '/\n /'),
        array('<font color="green">${1}</font> <font color="blue">${2}</font>', "\n\t"),
        $type
    );
    echo $type;
    echo "\n\n";
}
echo '';?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/soapclient.gettypes.php)

**[To root](/README.md)**