# dns_get_record



This method has no error handling, it simply puts out "false" and it is impossible to check for NXDOMAIN, SERVFAIL, TIMEOUT or any other error...  

#

Get more than one type at once like this:<br>

```
<?php
$dnsr = dns_get_record('php.net', DNS_A + DNS_NS);
print_r($dnsr);
?>
```


Using DNS_ALL fails on some domains where DNS_ANY works. I noticed the function getting stuck on the DNS_PTR record, which caused it to return FALSE with this error:
PHP Warning:  dns_get_record(): res_nsend() failed in ....

This gets all records except DNS_PTR:


```
<?php
$dnsr = dns_get_record('php.net', DNS_ALL - DNS_PTR);
print_r($dnsr);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.dns-get-record.php)

**[To root](/README.md)**