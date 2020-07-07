# OpenSSL Functions





Currently, all OpenSSL Functions defined in PHP only utilize the PEM format.&#xA0; Use the following code to convert from DER to PEM and PEM to DER.



```
<?php
$pem_data = file_get_contents($cert_path.$pem_file);
$pem2der = pem2der($pem_data);

$der_data = file_get_contents($cert_path.$der_file);
$der2pem = der2pem($der_data);

function pem2der($pem_data) {
&#xA0;&#xA0; $begin = &quot;CERTIFICATE-----&quot;;
&#xA0;&#xA0; $end&#xA0;&#xA0; = &quot;-----END&quot;;
&#xA0;&#xA0; $pem_data = substr($pem_data, strpos($pem_data, $begin)+strlen($begin));&#xA0; &#xA0; 
&#xA0;&#xA0; $pem_data = substr($pem_data, 0, strpos($pem_data, $end));
&#xA0;&#xA0; $der = base64_decode($pem_data);
&#xA0;&#xA0; return $der;
}

function der2pem($der_data) {
&#xA0;&#xA0; $pem = chunk_split(base64_encode($der_data), 64, &quot;\n&quot;);
&#xA0;&#xA0; $pem = &quot;-----BEGIN CERTIFICATE-----\n&quot;.$pem.&quot;-----END CERTIFICATE-----\n&quot;;
&#xA0;&#xA0; return $pem;
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/ref.openssl.php)

**[To root](/README.md)**