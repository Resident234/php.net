# SoapClient::__getTypes





```
<?php
// to see formated types

$soap = new SoapClient('http://domain.com/ws.php?WSDL');

echo '&lt;pre&gt;';
echo '&lt;h2&gt;Types:&lt;/h2&gt;';
$types = $soap->__getTypes();
foreach ($types as $type) {
    $type = preg_replace(
        array('/(\w+) ([a-zA-Z0-9]+)/', '/\n /'),
        array('&lt;font color="green"&gt;${1}&lt;/font&gt; &lt;font color="blue"&gt;${2}&lt;/font&gt;', "\n\t"),
        $type
    );
    echo $type;
    echo "\n\n";
}
echo '&lt;/pre&gt;';?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/soapclient.gettypes.php)

**[To root](/README.md)**