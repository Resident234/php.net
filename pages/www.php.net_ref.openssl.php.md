# OpenSSL Functions



Currently, all OpenSSL Functions defined in PHP only utilize the PEM format.  Use the following code to convert from DER to PEM and PEM to DER.<br><br>

```
<?php
$pem_data = file_get_contents($cert_path.$pem_file);
$pem2der = pem2der($pem_data);

$der_data = file_get_contents($cert_path.$der_file);
$der2pem = der2pem($der_data);

function pem2der($pem_data) {
   $begin = "CERTIFICATE-----";
   $end   = "-----END";
   $pem_data = substr($pem_data, strpos($pem_data, $begin)+strlen($begin));    
   $pem_data = substr($pem_data, 0, strpos($pem_data, $end));
   $der = base64_decode($pem_data);
   return $der;
}

function der2pem($der_data) {
   $pem = chunk_split(base64_encode($der_data), 64, "\n");
   $pem = "-----BEGIN CERTIFICATE-----\n".$pem."-----END CERTIFICATE-----\n";
   return $pem;
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/ref.openssl.php)

**[To root](/README.md)**