# geoip_asnum_by_name



The code example as stated has a missing &apos;.&apos;, causing a syntax error if it is pasted and executed without being checked.<br><br>Correct Code:<br><br>

```
<?php
$asn = geoip_asnum_by_name(&apos;www.example.com&apos;);

if ($asn) {
    echo &apos;The ASN is: &apos; . $asn;
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.geoip-asnum-by-name.php)

**[To root](/README.md)**